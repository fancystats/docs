About FancyStats
================

The FancyStats project aims to provide a high quality, flexible source of traditional and advanced NHL statistics to the intarwebs.  Born out of a frustration with a lack of easily digestible NHL stats, the project hopes to remove the repeated effort of individuals having to screen scrape existing sources.

The project also seeks to allow advanced users the felxibility to explore the data directly, with the ability to create custom stats and answer questions more easily.

If you are just looking to make use of the stats, you should head over to [fancystats.org](fancystats.org) and start exploring today!  If you are a developer looking to contribute to the site or learn more about how it works, you can find more information on the code that drives the site below.

Development
===========

FancyStats is an opensource project, and we welcome pull requests.  To get started developing, we recommend you read through this document to learn how to set up a development environment and learn a bit more about the architecture of the site.

Site Architecture
-----------------

FancyStats is broken up into three major parts:

* [nhlstats](https://github.com/fancystats/nhlstats): handles gathering data from the NHL and adding it to our database at regular intervals.
* [API](https://github.com/fancystats/api): used by the site's front-end as well as users looking to get access to the raw data.
* [Web Site](https://github.com/fancystats/www): presents data to casual visitors of the site.

In addition to these pieces, a database is required to store the results.  For our production instance, we run PostgreSQL, but our ORM allows the use of other database backends.

###Scheduled Tasks via the nhlstats Library

Our scheduled tasks consist of the screen scraping tools that run regularly on the server, collecting data from the NHL.  The tasks check our database on start up for active games, and will check the games for updates to data, entering any new information in the database.  Additionally, we run less frequent checks for corrections to past games.

The test suite for this code includes static assets we've downloaded from the NHL, to represent various issues we need to test for in the code (missing information, incorrect information, etc.)

The code is written in Python and makes use of [Scrapy](http://scrapy.org) for web scraping and [Peewee ORM](https://peewee.readthedocs.org/en/latest/) for interacting with the database.

###API

The API layer exists to present the data in the database to clients on the web.  It is written in Python and makes use of [Flask-API](http://www.flaskapi.org) to provide a simple web API.

The test suite for this code focuses on encoding issues and dealing with bad data.

###Front-End

The front end is a static website, compiled from [EJS](http://www.embeddedjs.com) templates via [Harp](http://harpjs.com) which is intended to be hosted on a service such as S3 for maximum uptime.  The site connects to the API to display data to the user.

The test suite for this code focuses on dealing with poor connections, bad data, and encoding issues.
