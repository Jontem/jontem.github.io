---
id: 256
title: Hur enhetstestar man en Web Api-controller
date: 2015-01-26T22:44:21+00:00
author: jonte
layout: post
guid: http://www.mourtada.se/?p=256
permalink: /hur-enhetstestar-man-en-web-api-controller/
categories:
  - ASP .NET Web Api
  - 'C#'
  - TDD
---
Hur enhetstestar man en Web Api-controller?

Vi har nedanstående controller med en Action metod.

<pre lang="csharp">public class MovieApiController : ApiController
    {
        private IRepository _movieRepository; 

        public MovieApiController()
        {
            _movieRepository = new Repository();
        }

        public MovieApiController(IRepository movieRepository)
        {
            _movieRepository = movieRepository;
        }

        public IHttpActionResult Get()
        {
            var movies = _movieRepository.Get().ToList();

            if (!movies.Any())
            {
                return NotFound();
            }

            return Ok(movies);
        }
    }
</pre>

Vad är det vi vill testa? Jag tycker vi ska testa att vi får tillbaka det som förväntas och en korrekt HttpStatus-kod. Som ni ser ovan så hämtar jag alla filmer från mitt filmrepository. Ifall jag inte hittar några filmer så returnerar jag en 404. Ifall jag hittar något så returnerar jag en en status 200 tillsammans med mina objekt. Observera att min action returnerar en IHttpActionResult som är nytt för Web Api 2.

För att se vilka klasser som ärver av IHttpActionResult: [System.Web.Http.Results](https://msdn.microsoft.com/en-us/library/system.web.http.results.aspx)

<pre lang="csharp">[TestClass]
    public class MovieApiControllerTest
    {
        private Mock&lt;IRepository&gt; _movieRepository;
        private List _movies;

        [TestInitialize]
        public void Setup()
        {
            _movieRepository = new Mock&lt;IRepository&gt;();
            _movies = new List
            {
                new Movie
                {
                    Id = 1,
                    Title = "Batman",
                    Year = 1999,
                    Rating = 7
                },
                new Movie
                {
                    Id = 2,
                    Title = "Sagan om ringen",
                    Year = 2001,
                    Rating = 8
                }
            };
        }

        [TestMethod]
        public void MovieApiControllerTest_Get_FoundMovies_ReturnsStatusOk()
        {
            // Arrange
            _movieRepository.Setup(x =&gt; x.Get()).Returns(_movies);

            var controller = new MovieApiController(_movieRepository.Object);

            // Act
            var result = controller.Get();
            var contentResult = result as OkNegotiatedContentResult&lt;List&gt;;

            // Assert
            Assert.IsNotNull(contentResult);
            Assert.AreEqual(2, contentResult.Content.Count);
        }

        [TestMethod]
        public void MovieApiControllerTest_Get_NoMovies_ReturnsStatusNotFound()
        {
            // Arrange
            _movieRepository.Setup(x =&gt; x.Get()).Returns(new List());

            var controller = new MovieApiController(_movieRepository.Object);

            // Act
            IHttpActionResult result = controller.Get();

            // Assert
            Assert.IsInstanceOfType(result, typeof(NotFoundResult));
        }
    }
</pre>

I första testet får jag först tillbaka ett IHttpActionResult. Jag castar det sedan till ett OkNegotiatedContentResult. Kollar så det inte är null och sedan kollar jag att får tillbaka rätt antal filmer.

I andra testet så kollar jag att vi får tillbaka en 404. Observera att jag använder mig av Assert.IsInstanceOfType. Detta är för att resultatet från controllern är av IHttpActionResult vilket inte är en konkret klass.