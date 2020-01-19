1. Define JDBC

2. JDBC (Java Database Connectivity) is a java API which allow java application to interact with database. 
   
   1. Define Database
   +Database is an organized collection of data, which is stored and accessed by a computer system.
      Database consist of table.
   
3. What is RDBMS? When do we prefer RDBMS over NoSQL?

   1. What is RDBMS?
      +RDBMS (Relational Database Management System) is DBMS that is designed to maintain relational database.
      +Relational Database is a database that stores data in a structured format using rows and columns. Table can also be related to other tables.
   2. When do we prefer RDBMS over NoSQL?
      +Accounting and payment management system (ACID is always needed in this situation)
      +Complex query and analysis (query-intensive workloads)

4. Research and write any 4 positives and 2 negatives of MySQL, Oracle, SQL Server and PostgreSQL

   1. 

   * MySQL
     Advantages
     -easy to use
     -open-source (free->10000$)
     -widely used which mean there are many documentation and help from different community.
     -can run on many platform
     Disadvantages
     -The performance drop very fast as the data grow
     -hard to scale
   * Oracle
     Advantages
     -grouping transaction
     -support many OS which mean we can install this in Linux server or Unix server (more secure)
     -backup and recovery
     -version changes (oracle is good at reporting of what will be updating ahead which is important for large-project)
     -great performance (multiple server can work on one database)
     Disadvantages
     -very expensive (even the salary of database administrator)
     -hard to use
   * SQL Server
     Advantages
     -ability to process large amount of data
     -can backup, recovery and rollback data easily
     -can create a database mirroring and clustering
     -easy installation
     Disadvantages
     -Limited Compatibility(supported windows only)
     -Costly
   * PostgreSQL
     Advantages
     -support many operation system
     -easy to use
     -open-source and community support
     -reliability and stability (some companies reported that this DB has never crashed after running for so many year)
     Disadvantages
     -the performance is not that great
     -Memory performance(every new connection, new process will be created and around 10mb of ram will be consumed)

   2. Which one do you prefer if you develop a web application and why?
      +I think I like both the MySQL and PostgreSQL, but in this situation I think MySQL is the best choice because MySQL's features is good enough and since web-application need high read speed. complex queries and heavy loads isn't the case for a small web application. In addition, transaction is also work well in MySQL. so i think MySQL is enough and it's also open-source which mean it's free for commercial-use.

5. Write SQL Query
    a. create database and table
  + create database DBName;
  + create table TableName (column1 datatype,column 2 datatype,....);
    b. altering a table
  + Add Column
    	-alter table TableName add ColumnName datatype;
  + Drop Column
    	-alter table TableName drop column ColumnName;
  + Change Column's datatype
    	-alter table TableName alter column ColumnName datatype;
    c. write query to insert, update, delete a record from a table
  + insert into TableName (column1 datatype,column 2 datatype,....) values (value1,value2,...);
  + update TableName set column1 = value1,column2 = value2,... where condition;
  + delete from TableName where condition;
    d. what is the difference between deleting and dropping a table and write query to do both.
  + delete table TableName; (delete only the data but the table structure is remained)
  + drop table TableName; (delete the data and also the table structure)
2. Write step by step procedure to insert a row, update row and delete row from table
    +Load driver into the project using code 
    	Download JDBC driver
    	Import the driver package(.jar) into project 
    	Load driver to project using the following code below 
    	    Class.forName("com.mySQL.jdbc.Driver";)
    +Establish connection with java & DB 
    	Find the location of the database and store into a string 
    	    String DBURL = "jdbc:mysql://localhost:8080/B6B"; 
    	Store the username and password of the DB into a string 
    	    String user = "root"; 
    	    String pass = "123"; 
    	Connection con = DriverManager.getConnection(DBURL,user,pass); 
    +Creating statement 
    	Call the create Statement() using the connection object 
    	Store the object returned by createStatement into a Statement object reference 
    	    Statement st = con.createStatement(); 
    +Execute SQL commands 
    	Call the method execute() (insert,delete) or executeQuery() (read/selection) or executeUpdate() (update) 
    	    st.execute("insert into Student (id,name) values ("123","Reak")); 
    	    st.executeUpdate("update Student set name="Arun" where id = "123"); 
    +Handle result if any 
    	ResultSet rs = st.executeQuery("select * from Student"); 
    	while(rs.next()) { 
    		int id = rs.getInt("id"); 
    		String n = rs.getString("name"); 
    		System.out.prinln(id+":"+n); 
    	} 
3. Write a simple code to read and display a single, all and specific record from a table

```java
public class example {
	public static void main (String args[]){
		Class.forName("com.mySQL.jdbc.Driver");
		String url = "----url----";
		String user = "----user----";
		String password = "----password----";
		connection = DriverManager.getConnection(url, user, password);
		statement = connection.createStatement();
		Scanner scanner = new Scanner(System.in);
		int id = scanner.nextInt();

		//show the single record
		ResultSet result = statement.executeQuery("select * from learndb where id="+id);
		result.next()
		int id = result.getInt(1);
		String name = result.getString(2);
		System.out.println("Row -----> " + id + " : " + name);
	
		//show the whole table
		ResultSet result = statement.executeQuery("select * from learndb");
		while (result.next()) {
			int id = result.getInt(1);
			String name = result.getString(2);
			System.out.println("Row -----> " + id + " : " + name);
		}


		connection.close();
	
	}
}
```
8. Compare PreparedStatement with statement and explain PreparedStatement with the help of program
	+Statement is used when we want to create statement and execute
	Example:
	Statement s = connection.createStatement();
	
```java
	     ps.execute("insert into learndb (id,name) values (1,"ah kmav")");			
```

	+PreparedStatement is used when we want to create statement with parameters, which mean every time we want to interact with the DBMS we will need to fill in the parameters and execute it
	Example:
```java
   PreparedStatement ps = connection.prepareStatement("insert into learndb (id,name) values (?,?)");
        ps.setInt(1,1);
        ps.setString(2,"Ah kmav");
        ps.execute();

```
9. Explain Batch Processing with the help of suitable example program.
	+Batch processing allow us to add many related statement together as batch and submit them all with one call to database.
	Example:
```java
	LinkedList<Student> list_of_students = new LinkedList<>()
	PreparedStatement ps = connection.prepareStatement("insert into learndb (id,name) values (?,?)");
	    for (Student student:listOfStudent) {
	        ps.setInt(1,student.getId());
	        ps.setString(2,student.getName());
	        ps.addBatch();
	    }
       ps.executeBatch();
``
10. Explain the use of CallableStatement and explain with an example program
	In postgresql:
   ```java
	create or replace procedure insertStudent(int,varchar(20))
	    language plpgsql
	    as $$
	    begin
	     insert into learndb(id,name) values ($1,$2);
	     commit;
	    end;
	    $$;
	
	String url = "----url----";
	String user = "----user----";
	String password = "----password----";
	connection = DriverManager.getConnection(url, user, password);
	CallableStatement callableStatement = connection.prepareCall("call insertStudent(?,?)");
	    callableStatement.setInt(1, id);
	    callableStatement.setString(2, name);
	    callableStatement.execute();

```

11. What is ACID property in transaction?
	+ACID refer to Atomicity(all success or none), Consistency(guarantees the committed state), Isolation(the transaction is isolated from others) and Durability(the transaction will remain the same even the failure happen)
12. Write a code to explain the transaction process in java
   ```java
        int debId=1;
        int credId=2;
        int amount=150;
        try{
            Class.forName("com.mysql.jdbc.Driver");
            Connection con = DriverManager.getConnection("url", "user", "password");
            con.setAutoCommit(false);
            try{
                Statement stat = con.createStatement();
            
                stat.execute("UPDATE bank_accid SET balance = balance - '"+amount+"' WHERE id = '"+debId+"'");
                stat.execute("UPDATE bank_acci	d SET balance = balance + '"+amount+"' WHERE id = '"+credId+"'");
                //STEP 2 : IF THE QUERY STATEMENT WORK PROPERLY COMMIT CHANGE
                con.commit();
    
                System.out.println("Thanks for using our fucking service");
            }
            catch(Exception e){
                System.out.println("Fail! Try agazin!");
                con.rollback();
            }
        }
        catch(Exception e){
            System.out.println(e);
        }
   ```
13.Define RowsetFactory and explain with a program
	Rowset allow us to interact with database in a disconnected architecture while ResultSet use a connected architecture. which mean that rowset can be serialized. we can get all data store in our primary memory and update bunch of manipulated data in one go to the database. Another special feature: when we want to send the data outside of our app as xml, we can also use WebRowSet.
	Example:

```java
	Class.forName("oracle.jdbc.driver.MySQL");  
	
	JdbcRowSet rowSet = RowSetProvider.newFactory().createJdbcRowSet();  
	rowSet.setUrl("url");  
	rowSet.setUsername("user");  
	rowSet.setPassword("password");
	           
	rowSet.setCommand("select * from emp400");  
	rowSet.execute();  	
	           
	while (rowSet.next()) {  
		System.out.println("Id: " + rowSet.getString(1));  
		System.out.println("Name: " + rowSet.getString(2));  
		System.out.println("Salary: " + rowSet.getString(3));  
	}
```
14. Driver types
	a. JDBC-ODBC bridge driver : the driver convert JDBC method calls into ODBC function calls. the client need to install libraries of database.
	b. Native-API driver : the driver convert JDBC method calls into native calls of the database API. the client still need to install libraries of database.
	c. JDBC-Net pure java : middleware has a role to convert JDBC calls into vendor-specific database protocol which mean the clients don't need to install the libraries of database
	d. Thin driver : this driver can convert JDBC calls directly into vendor-specific database protocol. no need special software. we use it by just download the driver which is provided by the vendor and import it into our project.
