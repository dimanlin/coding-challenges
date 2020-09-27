# Application Challenge #

* Create a small PORO application that will fetch sample data from API, convert it to given format and save to SQL. 
* Write tests for each component of your application. 
* For testing - create a fake API server that will respond with API samples provided. Please don't use external services.

### Business Specification

For this challenge, we want to collect statistics of web site visits, that is provided by an API server, and store it to a MySQL database for later usage. 
We also want to store raw data in database.

A sample API server response is located at `samples/**/*`

## Part 1
Each file represents a partial response from API. Create a fake REST API server that will respond with this data.
Then create a client that will fetch this data (through network) and save it to database as is.
Please think about edge cases that could happen on remote side, on network level etc, handle them and cover with tests.

## Part 2
Create an aggregation query for previously saved data aggregating it by session. The aggregation should be done in SQL.
Save sessions and page visits into 2 tables. Sessions should have many page visits. 

**Models**

We need the data to be saved into 2 different tables: `sessions` and `pagevisits`.  

* Create migrations to create those tables.
* Create models `Session` and `Pagevisit`. `Session` is as associated with `Pagevisit` with a one-to-many relation.
* The `userId` field has unique user identifier, and `timestamp` - page visit timestamp. We assume that if user was inactive more then for 90 minutes - it's a new session.
* Create additional `position` column in page visits table and fill it with actual position in session.

`Pagevisit` should store:

```ruby
    t.string "url"
    t.bigint "session_id"
    t.datetime "timestamp"
    t.string "title"
    t.integer "position"
``` 


`Session` should store:
```ruby
   t.string "userId"
```


* `userId` must validate with `/\A[A-z0-9]{8}-[A-z0-9]{4}-[A-z0-9]{4}-[A-z0-9]{4}-[A-z0-9]{12}\z/`
* Each session should not have duplicate page visits.
* Please think about edge cases that can occur on incorrect data, data duplication, etc 

### Requirements

+ Ruby MRI 2.5+
+ 100% coverage
+ Rubocop for linting ruby code
+ MySQL for database
+ Docker + docker-compose for containerization
