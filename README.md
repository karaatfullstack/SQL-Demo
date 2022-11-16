# SQL-Demo
## 0 - Running Postgres
1. Starting the DB conection in terminal (likely already done):
`brew services start postgresql`
2. Opening postgres connection: 
`psql postgres`

## 1 - Creating a DB and table
1. CREATE DATABASE fsa_demo
 - Show \l -> lists all DBs I've made
 - -q  -> quits the connection to the DB
 - `psql -d database_name` will reconnect you OR `psql` and then `\c databse_name`
 
2. Create a table
 - Show there are no tables in this yet by doing \d    -> "did not find any relations"
  `  CREATE TABLE people(name VARCHAR(100)); `  // creates the table 
 - Do \d to show your schema (which contains that database table)
 - Then do ` \d table_name ` (in this case, ` \d people ` )
 
## 2 - Adding Data
1. Add a person to the people table
 -  `INSERT INTO people(name) VALUES ('Amy'); `
   - Show that it returns: ` SELECT * from people; `
 - ` INSERT INTO people(name) VALUES ('Dan'); `
   - Show alias: ` SELECT name AS friend FROM people; `
 
2. Count rows of data
 - ` SELECT count(*) FROM people;`
 - ` SELECT count(*) FROM people WHERE name = 'Dan'; `  // returns 1
 
3. Drop table and make a new one
 - start over with: `DROP TABLE people`
 - [Share this XKCD Comic](https://xkcd.com/327/)
 - Create new table: 
    ` CREATE TABLE people(id INTEGER PRIMARY KEY, name VARCHAR(100)); ` // mention primary key
 -  ` INSERT INTO people(name) VALUES('Kevin'); ` // THIS WILL FAIL because I haven't added in an ID
 - Can put in any order as long as they match: 
   - ` INSERT INTO people(name, id) VALUES('Kevin', 1); `

4. Serial - when you don't want to keep track of your unique IDs
    ` CREATE TABLE emails(id SERIAL PRIMARY KEY, email VARCHAR(100) UNIQUE, dept_id INTEGER); ` // Serial and Unique are both important keywords here
    ` INSERT INTO emails(email, dept_id) VALUES('billingemail.com', 2); `
    ` INSERT INTO emails(email, dept_id) VALUES('developeremail.com', 3); `
   
## 3 - Joining tables
1. For JOINs, what relationship would we like to model? Let's say we have a company and want to join a departments table to our emails table
 - ` CREATE TABLE departments( id SERIAL PRIMARY KEY, dept_name VARCHAR(100));
 - ` INSERT INTO departments(dept_name) VALUES('HR'); ` 
 - `  ""  "" departments(dept_name) VALUES('developers'); ` 
 - `  ""  "" departments(dept_name) VALUES('billing'); ` 

2. Create table with a reference to the departments table
 - ` CREATE TABLE employees(id SERIAL PRIMARY KEY, name VARCHAR(100), department_id INTEGER REFERENCES departments(id)); `
   - Why do we use the "REFERENCES"? It tells the DB that there is a relation, so it will only accept department IDs which correspond to an id in the departments table! It will fail otherwise
   
3. Show INSERT fails if id doesn't match department table
 -  ` INSERT INTO employees(name, department_id) VALUES('Bea Arthur', 7);  `   // "violates key constraint" error
 [At 31 minutes](https://youtu.be/gCIblrIR-II?t=1894) 
 
