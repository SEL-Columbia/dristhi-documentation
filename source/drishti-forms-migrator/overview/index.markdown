---
layout: page
title: "Drishti Forms Migrator"
---

# Dristhi Forms Migrator: Overview

## What is this application?

The data created through the Drishti application is stored in the couchDb which is no-sql database. This application
fetches the data from the couchDb and imports to the postgres(relational db).

## What's in it?

* The application has a batch job which runs over an interval that can be configured by an application property.
* A rest end point which can take a list of form entries and saves them in the database.


## How does it work?

The batch job is responsible for the data conversion. The batch job runs periodically and fetches the data from a rest endpoint of the Drishti application.
It then processes the form entries, converts it to row data and saves it to the database.

## What does it require?

1. Java 8
2. The application uses [Dropwizard framework][1]
3. Customized version of [dropwizard jobs] [2]
4. Postgres database server
5. Ruby


[1]:https://dropwizard.github.io/dropwizard/]
[2]:https://github.com/spinscale/dropwizard-jobs