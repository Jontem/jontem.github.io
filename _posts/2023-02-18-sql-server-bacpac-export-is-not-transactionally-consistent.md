---
title: SQL server bacpac export is not transactionally consistent
date: 2023-02-18T00:00:00+00:00
layout: post
permalink: /sql-server-bacpac-export-is-not-transactionally-consistent/
categories:
  - sql-server
tags:
  - Azure
  - backpac
  - backup
  - sql server
---

The other day I needed a copy of a production database which was running in Azure SQL. The only simple way to export data from Azure SQL is through a bacpac export. Azure manages the ordinary backups and they cannot be accessed for download. The can only be restored within Azure.  

I exported the backup with Azure cli `az sql db export` command and exported directly to Azure blob storage. Everything went fine and I could then download the export to my local computer where I was running the SQL server as container on linux. 

But when restoring the bacpac file with SQL server management studio the import operation never completed. It got stuck on of the biggest tables in the database when creating the clustered index which was also the primary key. Nothing was shown in the UI. I then tried using the `sqlpackage` command line tool which was kind to tell me that there was a duplicate key that was preventing the creation of the index. But it didn't fail it just tried again...and again...and again. 

This seemed very strange. After a short search on the web I realized that bacpac exports are not transactionally consistent. It is very easy to find this if you read the documentation https://learn.microsoft.com/en-us/azure/azure-sql/database/database-export?view=azuresql.

So I made a copy of the database in Azure SQL and exported that one instead. Now the bacpac file imported without problems!