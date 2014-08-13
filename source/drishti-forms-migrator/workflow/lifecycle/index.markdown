---
layout: page
title: "Drishti Forms Migrator"
---

# Dristhi Forms Migrator: Life-cycle

* Even before the application is deployed and started, we need to make sure that the postgres server is up and running.

* The postgres should have at-least one database created with the username and password.

* This information: the database name, username and password has to be provided to the application. This can be done by
entering them in the application's config file which in this case is a `.yml` file.

* We should run the database migration for the application. All the tables are created/modified are part of this migration
and should be created prior the application launch. Hence any new forms or new columns or deletion of the columns should be
done via the migration script.

* Once the application is started the batch job is also started.

* The batch job first tries to make a http request to the Drishti Application
	
	Example:	
	`http://host/all-form-submissions?timestamp=0&batch-size=100`

	* `timestamp` : The server would return all the form entries which have the server version value greater than this.
	* `batch-size` : The server would return as many forms as mentioned here. Usually its 100.

* When it fetches, say, 100 form entries, it processes them and saves it to the repository. 

* Once all the form entries are processed and saved to the repository. There are few other tasks:
	
	1. The form entry that has the max `serverVersion` is picked. And this value is then persisted in a table called `migration_audit` against 
	   the column `timestamp`.

	   During the next poll from the batch process, `migration_audit` table is queried for the entry which has max timestamp value. And 
   	   this value is used for the `timestamp` value in the query parameter for the http request. 

   	2. The total number of form entries and total number of successfully processed form entries is also persisted along with the timestamp in 
   	   the `migration_audit` row.

   	3. If in any case the processing of the form fails which might be because of invalid data or unknown form name etc., an another entry is made
   	   in a table called `error_audit` for each form failures.  
   	   This table would contain a user-friendly message and also a detailed message of the actual exception caused. 