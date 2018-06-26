---
id: 235
title: 'Value type och Reference type i C#'
date: 2014-08-09T15:12:16+00:00
author: jonte
layout: post
guid: http://www.mourtada.se/?p=235
permalink: /value-type-och-reference-type-c/
categories:
  - 'C#'
  - Grundläggande
tags:
  - 'C#'
  - Reference type
  - Value type
---
Vad innebär Value-Type och Reference-Type i C# ?

En Value type exempelvis en integer(struct) innehåller själva värdet. I detta fallet ett numeriskt heltalsvärde.

En Reference type exempelvis en klass innehåller ett referensvärde till vart objektet är sparat i minnet.

Nedan ger jag lite exempel för att förtydliga:

<pre class="brush: csharp; title: ; notranslate" title="">class Program
    {
        
        static void Main(string[] args)
        {
            // Value type, struct
            int firstNumber = 10;

            // Reference type of class Car
            Car firstCar = new Car() { Name = "Volvo", NumberOfWheels = 4};

            // Endast värdet kopieras till secondNumber
            int secondNumber = firstNumber;

            // Referensvärdet för firstCar kopieras till secondCar.
            // Dom har nu en referens till samma objekt i minnet
            Car secondCar = firstCar;

            // Båda raderna under kommer skriva ut värdet 10.
            Console.WriteLine("firstnumber: {0}", firstNumber);
            Console.WriteLine("secondNumber: {0}", secondNumber);

            // Båda raderna under kommer skriva ut värdet 10.
            Console.WriteLine("firstCar.NumberOfWheels: {0}", firstCar.NumberOfWheels);
            Console.WriteLine("secondCar.NumberOfWheels: {0}", secondCar.NumberOfWheels);

            firstNumber = 20;
            firstCar.NumberOfWheels = 8;

            // Första raden kommer att skriva ut 20. Andra skriver ut 10.
            Console.WriteLine("firstnumber: {0}", firstNumber);
            Console.WriteLine("secondNumber: {0}", secondNumber);

            // Båda raderna under kommer skriva ut värdet 8.
            Console.WriteLine("firstCar.NumberOfWheels: {0}", firstCar.NumberOfWheels);
            Console.WriteLine("secondCar.NumberOfWheels: {0}", secondCar.NumberOfWheels);

            PassParameterTest(firstNumber, firstCar);

            // Första raden kommer att skriva ut 20. Andra skriver ut 10.
            Console.WriteLine("firstnumber: {0}", firstNumber);
            Console.WriteLine("secondNumber: {0}", secondNumber);

            // Båda raderna under kommer skriva ut värdet 16.
            Console.WriteLine("firstCar.NumberOfWheels: {0}", firstCar.NumberOfWheels);
            Console.WriteLine("secondCar.NumberOfWheels: {0}", secondCar.NumberOfWheels);

            firstCar = new Car() { Name = "Volvo", NumberOfWheels = 2 };

            // Först raden kommer att skriva ut 2. Andra kommer att skriva ut 16
            Console.WriteLine("firstCar.NumberOfWheels: {0}", firstCar.NumberOfWheels);
            Console.WriteLine("secondCar.NumberOfWheels: {0}", secondCar.NumberOfWheels);

        }

        public static void PassParameterTest(int number, Car car) 
        {
            number = 30;

            car.NumberOfWheels = 16;
        }
    }

    class Car
    {
        public string Name { get; set; }
        public int NumberOfWheels { get; set; }
    }
</pre>

Varför får vi olika värden på NumberOfWheels på sista exemplet? För att vi instansierar ett nytt objekt och får tillbaka ett nytt referensvärde. Vilket i sin tur gör att firstCar pekar på ett annat objekt i minnet.