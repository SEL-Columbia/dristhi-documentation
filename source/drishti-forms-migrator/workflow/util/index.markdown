---
layout: page
title: "Drishti Forms Migrator"
---

# Dristhi Forms Migrator: Util

* We need to have the java classes created for every form entry types. And also the corresponding
  table created in postgres through the migration script. 
  
* There is a ruby utility that will generate the java class and the corresponding migration script for a given sample form data.

* This utility can be run when there is any new forms that needs to be added.

* The sample json file that contains the form sample is place under `util/samples` directory

* Once the sample json is placed, you can run

`$ruby generator.rb`

* This will create java class file into `util/generated/classes` folder and migration script into `util/generated/migrations` folder