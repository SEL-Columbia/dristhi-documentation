---
layout: page
title: "SQLCipher and Encryption"
---

# SQLCipher and Encryption

The data that is stored in the app contains the medical and personal information of the clients, so it is necessary to encrypt and store the data in the local storage/database. [SQLCipher](http://sqlcipher.net) is an open source SQLite extension that provides 256 bit AES encryption of database files. It has a small foot print therefore does not impact the performance.

In Dristhi app, SQLCipher is used to encrypt the local database with a specific password which is usually the password of the ANM who logs into the app.

The following are the sequence that happens when the ANM logs in:

* When the ANM logs in, the password of the ANM is used as the key to encrypt the database
* The password of the ANM shall be stored in the session
* After the ANM logins, any of the actions that the ANM does, boils down to the corresponding repository classes
* These repository classes call the
  [Repository](https://github.com/SEL-Columbia/dristhi-app/blob/master/dristhi-app/src/main/java/org/ei/drishti/repository/Repository.java) classes to get the [readable] database
* When the readable or writable database is requested, the repository class tries to decrypt the database with the ANM password from the Session and then returns the readable/writable [database](https://github.com/SEL-Columbia/dristhi-app/blob/master/dristhi-app/src/main/java/org/ei/drishti/repository/Repository.java#L42-54)
* This corresponding database is used to do all the read and write operations
* The database is cleared when all the Dristhi application data is cleared through the Settings application

