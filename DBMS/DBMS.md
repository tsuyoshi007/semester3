1. **Useful keywords**:

   1. **Data** is a collection of information
   2. **Database** is a collection of interrelated data.
   3. **Database System** is computerized system that is created to maintain information and supplied information based on demand.
   4. **People who deal with database** 
      1. Actors on the scene: The people, whose jobs involve the day-to-day use of a database
      2. Database Administrators: responsible for authorizing access to the database,coordinating and monitoring its use, and acquiring software and hardware resources as needed

2. **DBMS** a system software for creating and managing databases.

3. **Purpose of DBMS**

   1. Data Independence: if data stay together with metadata(info of database that store data), it will be hard to fulfill the user's requirement now or later. example such as scaling, add remove, or edit table ....
   2. Efficient Data Access.
   3. Data Integrity and security.
   4. Data administration.
   5. Concurrent access and Crash recovery.
   6. Reduced Application Development Time.

4. **Physical Level** describes how a record is stored.

5. **Logical Level** describes data stored in database, and the relationships among the data.

6. **View Level** application programs hide details of data types and hide information for security purposes,such as bank account number.

   ![image-20200117220623985](./database-level)

7. **Schema** the logical structure of the database.

8. **Instance** the actual content of the database at a particular point in time.

9. **Physical Data Independence** the ability to modify the physical schema without changing the logical schema

10. **Data Models** concepts for presenting data in ways that are close to the way people perceive data.

11. **Types of Database**

    1. Hierarchical databases: child has one parent only parent can have multiple child
    2. Network databases: like hierarchical databases could have multiple parent
    3. Relational databases: data stored as tabular way (rows and columns) rows represent record and columns represents an attribute
    4. Object-oriented databases
    5. Graph databases
    6. ER model databases
    7. Document databases
    8. NoSQL databases

12. **Data Definition Language** a computer language used to create and modify the structure of database objects in a database. example: create alter drop truncate(remove all records) comment rename

13. **Data Manipulation Language** language used to modify the data. example: select, insert, update, delete, merge call ,explain plan, lock table.

    * Two classes of languages
      * *procedural* user tell the data required and how to get data
      * *non-procedural* user tell the data required without tell how to get those data

    * SQL is the most widely used query language

14. **Three tiers architecture of database**

    1. *One-tier architecture* Client server and database is in one machine
    2. *Two-tier architecture* provide API for user to call DBMS by using ODBC drivers usually it's more secure because DBMS is not exposed to client
    3. *Three-tier architecture* middleware access DBMS

15. **Relational Database Management System (RDBMS)**

    * Major Components

      * *Table* collection of data represented in rows and columns. each table has a name
      * *Record or Tuple* rows in table
      * *Field or Column name or Attribute* columns in table
      * *Domain* set of allowed values for an attribute in table. value must be in domain in order to insert into table. only null is a member of every domain.
      * *Instance* 
      * *Schema* structure of database described in a formal language supported by the DBMS.
      * *Keys*

    * rows may be stored in an arbitrary order

    * All operation

      * **Select Operation**

        <img src="select" alt="image-20200119004355580" style="zoom:67%;" />

      * **Project Operation**

        <img src="project" alt="image-20200119004444692" style="zoom:67%;" />

      * **Union of 2 relations**

        <img src="union" alt="image-20200119004603664" style="zoom:67%;" />

      * **Set difference of 2 relations**

        <img src="set-difference" alt="image-20200119004652267" style="zoom:67%;" />

      * **Set intersection of 2 relations**

        <img src="set-intersection" alt="image-20200119004721194" style="zoom:67%;" />

      * **Cartesian Product**

        <img src="cartesian" alt="image-20200119004807505" style="zoom:67%;" />

        * naming issue: we use table name to identify different attributes from which table.

          ![image-20200119004243313](naming-issue)

      * **Composite of operation** we can build expressions using multiple operations

      * **Natural Join** 

      <img src="natural-join" alt="image-20200119005727388" style="zoom:67%;" />

16. **Codd's Rules** 

    * *Rule 0* RDBMS --- must be able to manage data bases entirely through its relation capabilities

    1. *Rule 1 Information Rule* information must be stored 
    2. *Rule 2 Guaranteed Access Rule* --- Everything in a database must be stored in a table format.
    3. *Rule 3 Systematic Treatment of NULL Values*  --- NULL values in a database must be given by the system, meaning missing information and inapplicable information
    4. *Rule 4 Active Online Catalog* users must  be able to access the database's  structure (catalog) using the same  query language that they use to  access the database's data
    5. *Rule 5 The comprehensive data sub language rule* must support a 

