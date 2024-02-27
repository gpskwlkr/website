+++
title = 'Zero Cost Database Magic'
date = 2023-09-29T16:46:30+04:00
draft = false
showPublicationDate = true
showReadingTime = true
toc = true
comments = true
categories = ["Database development"]
keywords = ["Whether you’re a hobbyist or a professional developer", "database", "AWS", "Azure", "Linode", "DigitalOcean", "Docker", "PlanetScale", "MySQL", "Cloudflare D1", "SQLite database", "Cloudflare Workers", "Xata", "PostgreSQL", "ElasticSearch", "Fauna", "document databases", "MongoDB", "JOIN operations", "FQL", "GraphQL API", "Neon", "Rust", "PostgreSQL", "server location", "serverless platform for MySQL", "SQLite database on the edge", "serverless relational database based on PostgreSQL and ElasticSearch", "serverless database created by ex Twitter engineers", "scaling PostgreSQL with a generous free tier"]
summary = "5 Free Database Services for Testing Your Ideas: PlanetScale, Cloudflare D1, Xata, Fauna, Neon - Choose the Best Fit for Your Project!"
description = "Explore 5 free database services ideal for testing your projects, from PlanetScale's MySQL platform to Cloudflare D1's SQLite edge database. Discover Xata's PostgreSQL with ElasticSearch, Fauna's serverless option, and Neon's Rust-based PostgreSQL scaling. Make the right choice for your needs! 🚀"
coverImage = "/images/database.webp"
coverImageAltText = "A general photo of a database setup, possibly showing servers, storage units, and networking equipment"
+++

{{<img src="/images/database.webp" align="center" alt="A general photo of a database setup, possibly showing servers, storage units, and networking equipment">}} <br>

Whether you’re a hobbyist or a professional developer, at some point you’re going to need a database. We all know the famous solutions like AWS, Azure, or maybe something that’s more affordable like Linode or DigitalOcean, but what if you only need a database to test some of your ideas? Spinning up your own instance with Docker or just installing it locally may not always be an option, I wish there were some database services that would be performant enough for testing your ideas and also were free… Well, there are and in this article, I’m going to show off 5 of them, so you can choose the best one for your project!

## PlanetScale

{{<img src="/images/planetscale.webp" align="center" alt="PlanetScale logo">}} <br>

**PlanetScale** is a serverless platform for **MySQL** which offers really good options on a free tier. I think it’d suit most of the developers who want to use **MySQL** and to save up some money as well, here’s a short brief of what the free tier has to offer:

- **Storage - 5GB**
- **Reads per month - 1 Billion**
- **Writes per month - 10 Million**
- **1 Production Branch**
- **1 Development Branch**

As you see, [**PlanetScale**](https://planetscale.com/) has a pretty good free tier, but there’s one more service that has even more in theirs, so make sure to read to the end!

## Cloudflare D1

{{<img src="/images/cloudflare_d1.webp" align="center" alt="Cloudflare D1 logo">}} <br>

**Cloudflare D1** is basically a SQLite database which runs on the edge and is used with **Cloudflare Workers**. So, if you have your web service deployed to **Workers** which is also running on the edge, it means that your users will be able to always connect to the closest server and get the least amount of latency for them. Here’s a short brief of what the free tier has to offer:

- **Serverless SQL Database**
- **Reads per day - 5 Million**
- **Writes per day - 100K**

If you’re someone who enjoys using **Cloudflare Workers, D1** might be the best choice for you, make sure to check it out at - **[Cloudflare D1](https://developers.cloudflare.com/d1/)**.

## Xata

{{<img src="/images/xata.webp" align="center" alt="Xata logo">}} <br>

**Xata** is another serverless relational database that is based on **PostgreSQL** and **ElasticSearch** under the hood. It treats your tables like a spreadsheet and what’s really cool about it is that it has full text search built in, so you don’t need to have your data duplicated to **ElasticSearch** instance. It has **SDK**s available for many programming languages as well as visual schema editor which makes it really easy to use. Here’s a short brief of what the free tier has to offer:

- **15 GB Storage for your data**
- **Total records - 750K**
- **Branches per database - 15**
- **Requests per second - 75**

Make sure to check it out at - [**Xata**](https://xata.io/).

## Fauna

{{<img src="/images/fauna.webp" align="center" alt="Fauna logo">}}

[**Fauna**](https://fauna.com/) is yet another serverless database which was created by ex **Twitter** engineers. In some terms it’s similar to document databases such as **MongoDB** but at the same time supports features like native **JOIN** operations which many document databases are missing. **Fauna** got it’s own query language called **FQL** but also comes with **GraphQL API**. Here’s a short brief of what the free tier has to offer:

- **100K TROs (Total Read Operations)**
- **50K TWOs (Total Write Operations)**
- **1GB Storage**
- **5 Databases**

It certainly sounds not as good as previous ones, but brace yourselves, next one coming is my personal favourite!

## Neon

{{<img src="/images/neon.webp" align="center" alt="Neon logo">}}

**Neon** is a serverless option to scaling **PostgreSQL** that comes with a very generous free tier. It’s also written in **Rust**, comes with good documentation and code samples for your framework of choice. Most importantly, it supports data branching in your database for various environments like production, staging or development. Here’s a short overview of what the free tier has to offer:

- **Storage - 3GiB**
- **Branches - 10**
- **Active time per month - 100 hours and even if you surpass the limit, primary branch is still staying active.**
- **History retention - 7 days**
- **Ability to choose PostgreSQL version and server location**

By far, **Neon** is my favourite and make sure to check it for yourself as well at - [**Neon**](https://neon.tech/).

<hr class="border-gray-300 dark:border-gray-600 my-4">

Thank you for taking the time to read :)

If you enjoyed the article, you're welcome to join the [Discord](https://discord.gg/9SNyRghp) community, or [Telegram](https://t.me/anakidzedev) channel.
