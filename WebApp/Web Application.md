## Web Application

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

    - Pros : easy
    - Cons: bad performance 

    ![](/home/tsuyoshi/Documents/WebApp/driver1.jpeg)
    b. Native-API driver : the driver convert JDBC method calls into native calls of the database API. the client still need to install libraries of database.

    ![](/home/tsuyoshi/Documents/WebApp/driver2.jpeg)
    c. JDBC-Net pure java : middleware has a role to convert JDBC calls into vendor-specific database protocol which mean the clients don't need to install the libraries of database
    ![](/home/tsuyoshi/Documents/WebApp/driver3.jpeg)
    d. Thin driver : this driver can convert JDBC calls directly into vendor-specific database protocol. no need special software. we use it by just download the driver which is provided by the vendor and import it into our project.
    ![](/home/tsuyoshi/Documents/WebApp/driver4.jpeg)

    

15. Exceptions in JDBC

    **Exceptions** refer to anything that try to interrupt the normal flow of our program.

    **SQLException** could occur in the driver or from the table.

16. All the design patterns we have learnt from class and student presentations

    - Design pattern are well proved solution for solving specific problem/task.

    ![img](https://www.evernote.com/shard/s610/sh/40abca35-35fc-4424-8974-7d2af658f42b/e951e24747203151298d7190487c274a/res/eeefc991-cca3-46fa-ac35-db29848bcd9d/Screen%20Shot%202020-04-12%20at%209.30.21%20AM.png)

    - Creational pattern

    - - Factory Pattern: define an interface or abstract class for creating an object but let the subclass decide which class to instantiated.

    ```java
    public class Factory_Design_Pattern {
    
    public static void main(String[] args) {
        String car;
        Scanner s = new Scanner(System.in);
        System.out.println("Enter the car you want:");
        car = s.nextLine();
        Cars c = CarFactory.getCar(car);
        	c.printInfo();
        }
    }
    
    class CarFactory{
        public static Cars getCar(String car){
            if(car.equals("audi")){
            	return new Audi();
            }
            else if(car.equals("tesla")){
            	return new Tesla();
            }
            else if(car.equals("cybertruck")){
            	return new CyberTruck();
            }
            else{
            	return null;
            }
        }
    }
    
    class Audi implements Cars{
        public void printInfo(){
            System.out.println("Design_Pattern.Audi");
        }
    }
    class Tesla implements Cars{
        public void printInfo(){
        	System.out.println("Design_Pattern.Tesla...");
        }
    }
    
    class CyberTruck implements Cars{
        public void printInfo(){
        	System.out.println("Cyber Truck...");
        }
    }
    interface Cars{
    	void printInfo();
    }
    ```

    

    - - Singleton Pattern: provide class that has one instance and provide a global point of access to it.

      - - Early instantiate: class will be created at the time of class loading

          ```java
          public class SingletonExample {
          
              private int id;
          
              private String name;
          
              private static SingletonExample sd = new SingletonExample();
          
              private SingletonExample() {}
          
              public void printInfo() {
          
                  System.out.println(name, id)
          
              }
          
              public static SingletonExample getInstance() {
          
                  return sd;
          
              }
          }
          
          public class DesignPatternsExample {
          
              public static void main(String[] args) {
          
                  SingletonExample se = SingletonExample.getInstance();
          
                  Se.printInfo();
          
              }
          }
          ```

    - - - Lazy instantiate class will be create when required

          **MainClass.java**

          ```java
          public class MainClass {
          
              static Singleton s = null;
          
              public static void main(String[] args) {
          
                  Thread t1 = new Thread(new Runnable() {
          
                      @Override
          
                      public void run() {
          
                          s = Singleton.getObj();
          
                          System.out.println(s);
          
                      }
          
                  });
          
                  Thread t2 = new Thread(new Runnable() {
          
                  });
              }
          
          }
          ```

          **Singleton.java:**

          ```java
          private static Object lock = new Object();
          
          @Override
          
          public void run() {
          
              s = Singleton.getObj();
          
              System.out.println(s);
          
          }
          
          t1.start();
          
          t2.start();
          
          public class Singleton {
          
              private int id;
          
              private String name;
          
              private static Singleton s = null;
          
              private static Object lock2 = new Object();
          
              private Singleton() {}
          
              public static Singleton getObj() {
          
                  synchronized(lock) {
          
                      if (s == null) {
          
                          s = new Singleton();
          
                      }
          
                  }
          
                  synchronized(lock2) {
                      System.out.println("jsdfjs");
                      System.out.println("lfaslf");
          
                  }
          
                  return s;
              }
          
          }
          ```

          

    - - - **Thread safe instantiate**: A thread safe singleton in created so that singleton property is maintained even in multithreaded environment. To make a singleton class thread-safe, getInstance() method is made synchronized so that multiple threads can’t access it simultaneously.

          ```java
          public class GFG {
              private static GFG instance;
              private GFG() {
                  private constructor
              }
              synchronized public static GFG getInstance() {
                  if (instance == null) {
                      instance = new GFG();
                  }
                  return instance;
              }
          }
          ```

        - **Builder Pattern**: construct a complex object from simple object using step by step approach.

          ```java
          package Design_Pattern;
          
          class Car {
          
              String brand, color, engineType;
          
              Car(String brand, String color, String engineType) {
          
                  this.color = color;
          
                  this.brand = brand;
          
                  this.engineType = engineType;
          
              }
          
              public String toString() {
          
                  return color + " : " + brand + " : " + engineType;
          
              }
          
              public static class CarBuilder {
          
                  String brand, color, engineType;
          
                  public CarBuilder setColor(String color) {
          
                      this.color = color;
          
                      return this;
          
                  }
          
                  public CarBuilder setBrand(String brand) {
          
                      this.brand = brand;
          
                      return this;
          
                  }
          
                  public CarBuilder setEngineType(String engineType) {
          
                      this.engineType = engineType;
          
                      return this;
          
                  }
          
                  public Car build() {
          
                      Car c = new Car(brand, color, engineType);
          
                      return c;
          
                  }
          
              }
          
          }
          
          class Building_Pattern {
          
              public static void main(String[] args) {
          
                  Car c = new Car.CarBuilder().setColor("Black").setBrand("Tesla").setEngineType("V8").build();
          
                  System.out.println(c);
          
              }
          
          }
          ```

          

    - Structural pattern

    - - Adaptor pattern : convert interface of a class into another interface that client want

        ```java
        package Design_Pattern;
        
        interface Earphone {
        
            void listen();
        
        }
        
        interface Airpod {
        
            void listen();
        
        }
        
        class Android implements Earphone {
        
            @Override
        
            public void listen() {
        
                System.out.println("Listening on Design_Pattern.Android device");
        
            }
        
        }
        
        class Earphone_connector implements Earphone
        
        {
        
            private Iphone iphone;
        
            public Earphone_connector(Iphone iphone) {
        
                this.iphone = iphone;
        
            }
        
            @Override
        
            public void listen() {
        
                iphone.listen();
        
            }
        
        }
        
        class Iphone implements Airpod {
        
            @Override
        
            public void listen() {
        
                System.out.println("Listening on Design_Pattern.Iphone device");
        
            }
        
        }
        
        public class Adapter_Pattern {
        
            public static void main(String[] args) {
        
                Android S10 = new Android();
        
                S10.listen();
        
                Iphone iphone7 = new Iphone();
        
                iphone7.listen();
        
                Earphone iphoneX = new Earphone_connector(new Iphone());
        
                iphoneX.listen();
        
            }
        
        }
        ```

        

    - - Composite pattern: allow client to operate in generic manner on object that may or may not represent a hierarchy of object.

        ```java
        package Design_Pattern;
        
        import java.util.ArrayList;
        
        import java.util.List;
        
        public class Composite_Pattern {
        
            public static void main(String[] args) {
        
                Developer dev = new Developer(100, "John");
        
                Manager manager = new Manager(101, "Whisky");
        
                CompanyDirectory company = new CompanyDirectory();
        
                company.addEmployee(dev);
        
                company.addEmployee(manager);
        
                company.showEmployeeDetails();
        
            }
        
        }
        
        interface Employee {
        
            public void showEmployeeDetails();
        
        }
        
        class Developer implements Employee {
        
            private String name;
        
            private long empId;
        
            public Developer(long empId, String name) {
        
                this.name = name;
        
                this.empId = empId;
        
            }
        
            @Override
        
            public void showEmployeeDetails() {
        
                System.out.println("Name: " + name + ", ID: " + empId);
        
            }
        
        }
        
        class Manager implements Employee {
        
            private String name;
        
            private long empId;
        
            public Manager(long empId, String name) {
        
                this.empId = empId;
        
                this.name = name;
        
            }
        
            @Override
        
            public void showEmployeeDetails() {
        
                System.out.println("Name: " + name + ", ID: " + empId);
        
            }
        
        }
        
        class CompanyDirectory implements Employee {
        
            private List < Employee > employeeList = new ArrayList < Employee > ();
        
            @Override
        
            public void showEmployeeDetails() {
        
                for (Employee employee: employeeList) {
        
                    employee.showEmployeeDetails();
        
                }
        
            }
        
            public void addEmployee(Employee employee) {
        
                employeeList.add(employee);
        
            }
        
            public void removeEmployee(Employee employee) {
        
                employeeList.remove(employee);
        
            }
        
        }
        ```

        

    - - Decorator pattern: attach a flexible addition responsibility to an object dynamically.

        ```java
        package Design_Pattern;
        
        public class Decorative_Pattern {
        
            public static void main(String[] args) {
        
                Car_Decorative tesla = new TeslaAutoPilot(new TeslaBlue(new SimpleTesla()));
        
                System.out.println(tesla.getCost());
        
                System.out.println(tesla.getDescription());
        
            }
        
        }
        
        interface Car_Decorative {
        
            int getCost();
        
            String getDescription();
        
        }
        
        class SimpleTesla implements Car_Decorative {
        
            @Override
        
            public int getCost() {
        
                return 3000;
        
            }
        
            @Override
        
            public String getDescription() {
        
                return "Simple Design_Pattern.Car";
        
            }
        
        }
        
        abstract class TeslaDecorator implements Car_Decorative {
        
            private Car_Decorative car;
        
            public TeslaDecorator(Car_Decorative car) {
        
                this.car = car;
        
            }
        
            @Override
        
            public int getCost() {
        
                return car.getCost();
        
            }
        
            @Override
        
            public String getDescription() {
        
                return car.getDescription();
        
            }
        
        }
        
        class TeslaBlue extends TeslaDecorator {
        
            TeslaBlue(Car_Decorative car) {
        
                super(car);
        
            }
        
            @Override
        
            public int getCost() {
        
                return super.getCost() + 2000;
        
            }
        
            @Override
        
            public String getDescription() {
        
                return super.getDescription() + ", add Color Blue";
        
            }
        
        }
        
        class TeslaAutoPilot extends TeslaDecorator {
        
            TeslaAutoPilot(Car_Decorative car) {
        
                super(car);
        
            }
        
            @Override
        
            public int getCost() {
        
                return super.getCost() + 8000;
        
            }
        
            @Override
        
            public String getDescription() {
        
                return super.getDescription() + ", add AutoPilot";
        
            }
        
        }
        ```

        

    - - Proxy pattern: Provide the control for accessing the original object

        ```java
        package Design_Pattern;
        
        public class Proxy_Pattern {
        
            public static void main(String[] args) {
        
                Image image = new ProxyImage("Photo.jpg");
        
                image.DisplayImage();
        
            }
        
        }
        
        interface Image {
        
            void DisplayImage();
        
        }
        
        class RealImage implements Image {
        
            private String fileName;
        
            public RealImage(String fileName) {
        
                this.fileName = fileName;
        
                loadFromDisk(fileName);
        
            }
        
            private void loadFromDisk(String fileName) {
        
                System.out.println("load" + fileName);
        
            }
        
            public void DisplayImage() {
        
                System.out.println("Display: " + fileName);
        
            }
        
        }
        
        class ProxyImage implements Image {
        
            private String fileName;
        
            private RealImage realImage;
        
            public ProxyImage(String fileName) {
        
                this.fileName = fileName;
        
            }
        
            @Override
        
            public void DisplayImage() {
        
                if (realImage == null) {
        
                    realImage = new RealImage(fileName);
        
                }
        
                realImage.DisplayImage();
        
            }
        
        }
        ```

        

    - Behavioral pattern

    - - Command pattern: one class can command and invoke other object.

        ```java
        package Design_Pattern;
        
        interface Command {
        
            public void execute();
        
        }
        
        class Light {
        
            public void on() {
        
                System.out.println("The light is on");
        
            }
        
            public void off() {
        
                System.out.println("The light is off");
        
            }
        
        }
        
        class commandLightOn implements Command {
        
            Light light;
        
            public commandLightOn(Light light) {
        
                this.light = light;
        
            }
        
            public void execute() {
        
                light.on();
        
            }
        
        }
        
        class commandLightOff implements Command {
        
            Light light;
        
            public commandLightOff(Light light) {
        
                this.light = light;
        
            }
        
            public void execute() {
        
                light.off();
        
            }
        
        }
        
        class SimpleRemoteControl {
        
            Command command;
        
            public void setCommand(Command command) {
        
                this.command = command;
        
            }
        
            public void buttonWasPressed() {
        
                command.execute();
        
            }
        
        }
        
        class Command_Pattern {
        
            public static void main(String[] args) {
        
                SimpleRemoteControl remote = new SimpleRemoteControl();
        
                Light light = new Light();
        
                remote.setCommand(new commandLightOff(light));
        
                remote.buttonWasPressed();
        
                remote.setCommand(new commandLightOn(light));
        
                remote.buttonWasPressed();
        
            }
        
        }
        ```

        

    - - Mediator pattern: When a few class need to interact with each other for producing the output. So what if that class more functionality or we have to add more class for interact with each other this really matter so need mediator pattern for handle this things.

        ```java
        package Design_Pattern;
        
        public class Mediator_Pattern {
        
            public static void main(String[] args) {
        
                IATCMediator atcMediator = new ATCMediator();
        
                Flight sparrow101 = new Flight(atcMediator);
        
                Runway mainRunway = new Runway(atcMediator);
        
                atcMediator.registerFlight(sparrow101);
        
                atcMediator.registerRunway(mainRunway);
        
                sparrow101.getReady();
        
                mainRunway.land();
        
                sparrow101.land();
        
            }
        
        }
        
        class ATCMediator implements IATCMediator {
        
            private Flight flight;
        
            private Runway runway;
        
            public boolean land;
        
            public void registerRunway(Runway runway) {
        
                this.runway = runway;
        
            }
        
            public void registerFlight(Flight flight) {
        
                this.flight = flight;
        
            }
        
            public boolean isLandingOK() {
        
                return land;
        
            }
        
            public void setLandingStatus(boolean status) {
        
                land = status;
        
            }
        
        }
        
        interface Command_Mediator
        
        {
        
            void land();
        
        }
        
        interface IATCMediator
        
        {
        
            void registerRunway(Runway runway);
        
            void registerFlight(Flight flight);
        
            boolean isLandingOK();
        
            void setLandingStatus(boolean status);
        
        }
        
        class Runway implements Command_Mediator
        
        {
        
            private IATCMediator atcMediator;
        
            public Runway(IATCMediator atcMediator) {
        
                this.atcMediator = atcMediator;
        
                atcMediator.setLandingStatus(true);
        
            }
        
            @Override
        
            public void land() {
        
                System.out.println("Landing permission granted");
        
                atcMediator.setLandingStatus(true);
        
            }
        
        }
        
        class Flight implements Command_Mediator
        
        {
        
            IATCMediator atcMediator;
        
            @Override
        
            public void land() {
        
                if (atcMediator.isLandingOK()) {
        
                    System.out.println("Successfully landed.");
        
                    atcMediator.setLandingStatus(true);
        
                } else {
        
                    System.out.println("Waiting for landing");
        
                }
        
            }
        
            public void getReady() {
        
                System.out.println("Ready for landing");
        
            }
        
            public Flight(IATCMediator atcMediator) {
        
                this.atcMediator = atcMediator;
        
            }
        
        }
        ```

        

    - - Strategy pattern: define family functionality, encapsulate each one, make them interchangeable.

        ```java
        package Design_Pattern;
        
        interface strategy {
        
            public int doOperation(int a, int b);
        
        }
        
        class doOperationAdd implements strategy {
        
            @Override
        
            public int doOperation(int a, int b) {
        
                return a + b;
        
            }
        
        }
        
        class doOperationSubstract implements strategy {
        
            @Override
        
            public int doOperation(int a, int b) {
        
                return a - b;
        
            }
        
        }
        
        class doOperationMultiply implements strategy {
        
            @Override
        
            public int doOperation(int a, int b) {
        
                return a * b;
        
            }
        
        }
        
        class context {
        
            strategy strategy;
        
            public context(strategy strategy) {
        
                this.strategy = strategy;
        
            }
        
            public int executeOperation(int a, int b) {
        
                return strategy.doOperation(a, b);
        
            }
        
        }
        
        public class Strategy_Pattern {
        
            public static void main(String[] args) {
        
                context context = new context(new doOperationAdd());
        
                System.out.println(String.format("10 + 20 = %s", context.executeOperation(10, 20)));
        
                context = new context(new doOperationSubstract());
        
                System.out.println(String.format("20 - 10 = %s", context.executeOperation(20, 10)));
        
            }
        
        }
        ```

        

    - - Template pattern: just define a skeleton of a function in an operation, deferring some step to its subclass.

        ```java
        package Design_Pattern;
        
        public class Template_Pattern {
        
            public static void main(String[] args) {
        
                House house = new WoodenHouse();
        
                house.templateMethod();
        
            }
        
        }
        
        abstract class House
        
        {
        
            abstract void header();
        
            abstract void body();
        
            //hook
        
            void footer()
        
            {
        
                System.out.println("Build with sand and steel");
        
            }
        
            //template method
        
            public final void templateMethod()
        
            {
        
                footer();
        
                body();
        
                header();
        
            }
        
        }
        
        class WoodenHouse extends House
        
        {
        
            @Override
        
            protected void header() {
        
                System.out.println("Wooden header");
        
            }
        
            @Override
        
            protected void body() {
        
                System.out.println("Wooden Body");
        
            }
        
        }
        
        class ConcreteHouse extends House
        
        {
        
            @Override
        
            void header() {
        
                System.out.println("Concrete roof");
        
            }
        
            @Override
        
            void body() {
        
                System.out.println("Concrete body");
        
            }
        
        }
        ```

        

17. What is the difference between website and web application?

    1. Website is mainly used for showing information mostly, because it has low user interaction. Example: We want to announce an event that is going to happen next week. In this case, we might need a countdown page until the event starts, which is our homepage. An about page to show our sponsor. So just a website is all we need.
    2. Web applications can also be used to show information and allow users to interact with it. Example: We would like to create E-Commerce by using web technology. In this case, we would need to build a webapp that users can sign up, login, set their addresses, search the products, buy any product that the user would like to do so ... ,etc. Which means in this situation we would need a Web application.

18. What is a web container?

    1. Web container is a java application that controls servlets. Client sends a request to a web server that contains the servlet. Web server receives the request then sends the request to the web container. Web container will find the requested servlet and pass the HTTP request and HTTP response to the requested servlet.

19. Write any two differences between HTTP GET and POST Requests.

    1. GET is used for reading data from the web server while POST is used for inserting data into the web server. Another key difference, is that GET carries information by using request parameters appended to the URL. while POST carries information using its body.

20. What is the other name for web.xml?

    1. Deployment Descriptor

21. Define Servlet.

    1. Servlet is a web component that is deployed on the server (inside a web container) and could generate  responses to the client back based on the client’s request.

22. What are filters?
    1. Filter is an object that performs a filtering task even the request to resources and the response from the resources or even both. Example of task that filter should do:
    2. Logging
    3. Data compressing
    4. Input validation
    5. Conversion
    6. … etc

23. Define Cookie.

    1. Cookie is a small piece of data that is saved on the user’s computer by the user’s browser.

24. Mention the Life Cycle methods of Servlet.
    1. init() is called when the servlet is created
    2. service() the server receives a request for a servlet, Service is like a thread that check the HTTP request type (GET, POST, PUT, DELETE, etc.) and after checking it will call the right method doGet or doPost or doPut and all you need to do is to override doGet doPost...
    3. destroy() this method is called to do the cleanup such as closing database connection, stop the background threads

25. Mention the Life Cycle methods of filter
    1. init() is called when the web container instantiate a filter
    2. doFilter() is called if filter should be applied to the request
    3. destroy() is called when the web container remove the filter instance, or the web container stopped

26. When would you prefer JSP over Servlet?

    JSP provide us an easy way to design dynamic html front-end, it’s better to use JSP if you prefer to have freedom and faster way to design your front-end, but in some cases you have a lot of business logic code to write, i think it’s better to use servlet.

27. What is the alternative for web.xml in a web project?

    Annotations

28. Who converts a JSP file into a Servlet?

    Jsp container

29. What is the name of &lt;%= %&gt; in JSP and what is the use of it?

    Expression tag

30. Why do you need Expression Language?

    Use to display output or do calculation Display result of an expression Basically what the expression returns

31. List down the JSP directives.

    <%@page%>,<%@include%>,<%@taglib%>

32. ServletConfig

    1. **ServletConfig** is created by web container for each servlet. it could get configuration information from web.xml

    2. web.xml

       ```xml
       <web-app>  
         <servlet>
             blah blah
           <init-param>  
             <param-name>parametername</param-name>  
             <param-value>parametervalue</param-value>  
           </init-param>
         </servlet>  
       </web-app>  
       ```

       ```java
       public void doGet(HttpServletRequest request, HttpServletResponse response)  
           throws ServletException, IOException {  
         
           response.setContentType("text/html");  
           PrintWriter out = response.getWriter();  
             
           ServletConfig config=getServletConfig();  
           String secret=config.getInitParameter("secret");  
           out.print("My secret is: "+secret);  
                 
           out.close();  
           }  
         
       }  
       ```

       

33. ServletContext 

    1. **ServletContext** is created by the web container at the time of deploying the project. It also get configuration information from web.xml file

       ```xml
       <web-app>  
         
             
         <context-param>  
           <param-name>testingvalue</param-name>  
           <param-value>my value</param-value>  
         </context-param>  
       
       </web-app>  
       ```

       ```java
       public class DemoServlet extends HttpServlet{  
       public void doGet(HttpServletRequest req,HttpServletResponse res)  
       throws ServletException,IOException  
       {  
       res.setContentType("text/html");  
       PrintWriter pw=res.getWriter();  
         
       //creating ServletContext object  
       ServletContext context=getServletContext();  
         
       //Getting the value of the initialization parameter and printing it  
       String testingvalue= context.getInitParameter("testingvalue");  
       pw.println(testingvalue);  
         
       pw.close();  
         
       }}  
       ```

       

34. Servlet Chaining

    1. **Servlet Chaining** means the output of one **servlet** act as a input to another **servlet**. 
    2. it could achieved by using *RequestDispatcher* class

35. RequestDispatcher class 

    1. **RequestDispatcher** allow us to dispatch the request to another resource
    2. That another resource could be html, servlet and jsp.

36. The forward() and include() and it's differences 

    1. **Forward** : forward a request to another resource and that resource will repond to the client.

       ![](/home/tsuyoshi/Documents/WebApp/forward.jpeg)

    2. **Include** : include the content of a resource in the response. basically it forward the request to another resource too but it just put all the stuff that it need to put then it return everything back and the resource that forward the request will send the response back to the client.

       ![](/home/tsuyoshi/Documents/WebApp/include.jpeg)

37. Session Management and Tracking Introduction 

    1. **HTTP** is stateless so which mean that everytime user request to the server and that server will treat that request as a new request as always, so we must come with a way to maintain state or data of the user or to recognize the user.
    2. There are 4 Session tracking techniques
       1. Cookies
       2. Hidden Form Field
       3. URL rewriting
       4. HttpSession 

38. Sessions in Servlet and HttpSession interface

    1. Web container will create a unique session id for each user and that session id will be used to identify each user.

    2. **HttpSession** can be used to bind object , view and manipulate information about a session.

       ```html
       <form action="servlet1">  
           Name: <input type="text" name="userName"/><br/>  
           <input type="submit" value="go"/>  
       </form>  
       ```

       ```java
       @WebServlet(urlPatterns="/firstServlet",name="firstServlet")
       public class FirstServlet extends HttpServlet {  
         
       public void doGet(HttpServletRequest request, HttpServletResponse response){  
               try{  
         
               response.setContentType("text/html");  
               PrintWriter out = response.getWriter();  
                 
               String n=request.getParameter("userName");  
               out.print("Welcome "+n);  
                 
               HttpSession session=request.getSession();  
               session.setAttribute("uname",n);  
         
               out.print("<a href='servlet2'>visit</a>");  
                         
               out.close();  
         
                       }catch(Exception e){System.out.println(e);}  
           }  
         
       }  
       ```

       ```java
       @WebServlet(urlPatterns="/secondServlet",name="secondServlet")
       public class SecondServlet extends HttpServlet {  
         
           public void doGet(HttpServletRequest request, HttpServletResponse response)  
               try {  
       
                   response.setContentType("text/html");  
                   PrintWriter out = response.getWriter();  
       
                   HttpSession session=request.getSession(false);  
                   String n=(String)session.getAttribute("uname");  
                   out.print("Hello "+n);  
       
                   out.close();  
       
               }catch(Exception e) {
                   System.out.println(e);
               }  
       	}  
       }  
       ```

       

39. Cookies in a web application 

    1. **Cookie** is a small piece of information that is save as key/value pair. which is include in the http header when we make request to the server.

    2. There are 2 types of cookies

       1. Persistent Cookie : it is valid for multiple session
       2. Non-persistent cookie: it is valid for a single session only

    3. Example

       1. Creating Cookie

          ```java
          Cookie ck=new Cookie("user","sonoo jaiswal");//creating cookie object  
          response.addCookie(ck);//adding cookie in the response  
          ```

       2. Deleting Cookie

          ```java
          Cookie ck=new Cookie("user","");//deleting value of cookie  
          ck.setMaxAge(0);//changing the maximum age to 0 seconds  
          response.addCookie(ck);//adding cookie in the response  
          ```

       3. Getting all cookie

          ```java
          Cookie ck[]=request.getCookies();  
          for(int i=0;i<ck.length;i++){  
           out.print("<br>"+ck[i].getName()+" "+ck[i].getValue());
          }  
          
          ```

          

40. URL Rewriting in Servlets 

    1. **URL Rewriting** is done by appending token or identifier to the URL.

       Example : haha.com/profile?name=vikran&id=1

    2. And we get the parameter by using getParameter() method

    3. Example

       ```java
       @WebServlet(urlPatterns="/firstServlet",name="firstServlet")
       public class FirstServlet extends HttpServlet {  
         
       public void doGet(HttpServletRequest request, HttpServletResponse response){  
               try{  
               out.print("<a href='secondServlet?uname=Vikran>visit</a>");      
               out.close();  
         
                       }catch(Exception e){System.out.println(e);}  
           }  
         
       }  
       ```

       ```java
       @WebServlet(urlPatterns="/secondServlet",name="secondServlet")
       public class SecondServlet extends HttpServlet {  
         
       public void doGet(HttpServletRequest request, HttpServletResponse response {
               try{  
         
               response.setContentType("text/html");  
               PrintWriter out = response.getWriter();  
                 
               String n=request.getParameter("uname");  
               out.print("Welcome "+n); 
                         
               out.close();  
         
                       }catch(Exception e){System.out.println(e);}  
           }  
         
       }  
       ```

       

41. ServletRequestListener

    1. **ServletRequestListener** is used to listen to any request from the client so that we can log it.

    2. Example

       ```java
       @WebServlet(name="HelloServlet",urlPatterns="/HelloServlet")
       public class HelloServlet extends HttpServlet {
       	private static final long serialVersionUID = 1L;
        
       	protected void doGet(HttpServletRequest request,
       			HttpServletResponse response) throws ServletException, IOException {
        
       	}
        
       }
       ```

       ```java
       public class RequestListener implements ServletRequestListener {
        
       	public void requestDestroyed(ServletRequestEvent event) {
       		System.out.println("request being sent to "
       				+ event.getServletRequest().getRemoteAddr());
       	}
        
       	public void requestInitialized(ServletRequestEvent event) {
       		System.out.println("now initializing request"
       				+ event.getServletRequest().getRemoteAddr());
        
       	}
        
       }
       ```

       ```xml
       <web-app>
           <listener>
               <listener-class>
                   RequestListener
               </listener-class>
           </listener>
       </web-app>
       ```

       

42. ServletRequestAttributeListener

    1. **ServletRequestAttributeListener** give notifications about changes to the attributes of ServletRequest.

    2. Example

       ```html
       <!DOCTYPE html>
       <html>
           <head>
               <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
               <title>JSP Page</title>
           </head>
           <body>
               <form action="newServlet" method="post">
                   Username:
                   <input type="text" name="user">
                   <input type="submit" value="Login">
               </form>
           </body>
       </html>
       ```

       ```java
       @WebServlet(name="NewServlet",urlPatterns="/newServlet")
       public class NewServlet extends HttpServlet {
       
        @Override
        protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException
       
        {
         req.setAttribute("color", "Red");
       
         req.setAttribute("color", "Green");
       
         req.removeAttribute("color");
        }
       }
       ```

       ```java
       @WebListener
       public class ServletRequestAttributeLogger implements ServletRequestAttributeListener {
       
        @Override
        public void attributeAdded(ServletRequestAttributeEvent event) {
         System.out.println("attribute: " + event.getName() + " was added with value: " + event.getValue());
        }
       
        @Override
        public void attributeRemoved(ServletRequestAttributeEvent event) {
         System.out.println("attribute: " + event.getName() + " was added with value: " + event.getValue());
        }
       
        @Override
        public void attributeReplaced(ServletRequestAttributeEvent event) {
          System.out.println("attribute: " + event.getName() + " was added with value: " + event.getValue());
       ```

       

43. ServletContextListener

    1. **ServletContextListener** is used when we have some code to run before a web application starts.

    2. Example

       ```java
       import javax.servlet.ServletContextEvent;
       import javax.servlet.ServletContextListener;
        
       public class MyServletContextListener implements ServletContextListener {
        
       	public void contextInitialized(ServletContextEvent event) {
       		System.out.println("context initialized");
       	}
        
       	public void contextDestroyed(ServletContextEvent event) {
       	}
       }
       ```

       ```xml
       </web-app>
         <listener>
           <listener-class>com.thejavageek.MyServletContextListener</listener-class>
         </listener>
       </web-app>
       ```

       

44. ServletContextAttributeListener

    ```java
    @WebListener
    public class ApplicationContextAttributeListener implements ServletContextAttributeListener {
    
        @Override
        public void attributeAdded(ServletContextAttributeEvent event) {
            System.out.println("attribute: " + event.getName() + " was added with value: " + event.getValue());
        }
    
        @Override
        public void attributeRemoved(ServletContextAttributeEvent event) {
            System.out.println("attribute: " + event.getName() + " was removed with value: " + event.getValue());
        }
    
        @Override
        public void attributeReplaced(ServletContextAttributeEvent event) {
            System.out.println("attribute: " + event.getName() + " was replaced with value: " + event.getValue());
        }
    }
    ```

    ```java
    @WebServlet(name="myServlet",urlPatterns="/myServlet")
    public class myServlet extends HttpServlet {
        @Override
        public void doGet(HttpServletRequest req, HttpServletReponse resp) throws ServletException, IOException {
            ServletContext ctx = request.getServletContext();
            ctx.setAttribute("User", "Pankaj");
            String user = (String) ctx.getAttribute("User");
            ctx.removeAttribute("User");
        }
    }
    ```

    

45. HttpSessionListener

    1. **HttpSessionListener** is used to listen to session creation and destruction events.

    2. Example

       ```java
       public class MySessionListener implements HttpSessionListener {
        
       	public void sessionCreated(HttpSessionEvent event) {
       		System.out.println("A new session is created");
       	}
        
       	public void sessionDestroyed(HttpSessionEvent event) {
       		System.out.println("session is destroyed");
       	}
        
       ```

       ```xml
       <listener>
           <listener-class>com.thejavageek.MySessionListener</listener-class>
       </listener>
       ```

       

46. HttpSessionAttributeListener

    1. **HttpSessionAttributeListener** is used to keep track session's attribute in a web application.

    2. Example:

       ```java
       @WebServlet
       public class MyServlet extends HttpServlet {
           @Override
           public void doGet (HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
             session.setAttribute("url", "hahah.com"); //attributeAdded() is executed
       session.setAttribute("url", "hahahsdfjl.com"); //attributeReplaced() is executed
       session.removeAttribute("url"); //attributeRemoved() is executed
           }
       }
       ```

       

       ```java
       @WebListener
       public class MyAttributeListener implements HttpSessionAttributeListener {
       
       	@Override
       	public void attributeAdded(HttpSessionBindingEvent event) {
       		String attributeName = event.getName();
       		Object attributeValue = event.getValue();
       		System.out.println("Attribute added : " + attributeName + " : " + attributeValue);
       	}
       
       	@Override
       	public void attributeRemoved(HttpSessionBindingEvent event) {
       		String attributeName = event.getName();
       		Object attributeValue = event.getValue();
       		System.out.println("Attribute removed : " + attributeName + " : " + attributeValue);
       	}
       hu
       	@Override
       	public void attributeReplaced(HttpSessionBindingEvent event) {
       		String attributeName = event.getName();
       		Object attributeValue = event.getValue();
       		System.out.println("Attribute replaced : " + attributeName + " : " + attributeValue);	
       	} 
       }
       ```

        

47. Filters creation and configuration 

    ```java
    @WebFilter("/myServlet")
    public class MyFilter implements Filter {
        private ServletContext context;
        public void init (FilterConfig config) throws ServletException {
            this.context  = config.getServletContext();
            
        }
        public void doFilter (ServletRequest request, ServletResponse resp,FilterChain chain) throws IOException, ServletException {
            chain.doFilter(req,resp);
        }
        public void destroy () {
            
        }
    }
    ```
    
48. JSP 

    1. JSP introduction

       1. allow us to create web application easily by inserting java code in html pages using jsp tags that most of its start with <% and end with %>

    2. Life cycle

       **Translation of JSP page to Servlet :**

        This is the first step of JSP life cycle. This translation phase deals  with Syntactic correctness of JSP. Here test.jsp file is transllated to  test.java.

        **Compilation of JSP page :**

        Here the generated java servlet file (test.java) is compiled to a class file (test.class).

        **Classloading :**

        Servlet class which has been loaded from JSP source is now loaded into container.

        **Instantiation :**

        Here instance of the class is generated. The container manages one or more instance by providing response to requests.

        **Initialization :**

        jspInit() method is called only once during the life cycle immediately after the  generation of Servlet instance from JSP.

        **Request processing :**

        _jspService() method is used to serve the raised requests by JSP.It  takes request and response object as parameters.This method cannot be  overridden.

        **JSP Cleanup :**

        In order to remove the JSP from use by the container or to destroy  method for servlets jspDestroy()method is used. This method is called  once, if you need to perform any cleanup task like closing open files,  releasing database connections jspDestroy() can be overridden.

       

       Ref: https://www.geeksforgeeks.org/life-cycle-of-jsp/

       ![](/home/tsuyoshi/Github/semester3/WebApp/jsplifecycle.png)

    4. **Scriptlet tag** can contain any number of JAVA language statements, variable or method declarations, or expressions that are valid in the page  scripting language.

       ```jsp
       <html>
          <head><title>Hello World</title></head>
          
          <body>
             Hello World!<br/>
             <%
                out.println("Your IP address is " + request.getRemoteAddr());
             %>
          </body>
       </html>
       ```

    5. **Declaration tag** declares one or more variables or methods that you can use in Java code later in the JSP file. You must declare the variable or  method before you use it in the JSP file.

       ```jsp
       <%! int i = 0; %> 
       <%! int a, b, c; %> 
       <%! Circle a = new Circle(2.0); %> 
       ```

    6. **Expression tag** element contains a scripting language expression that  is evaluated, converted to a String, and inserted where the expression  appears in the JSP file.

       ```jsp
       <html> 
          <head><title>A Comment Test</title></head> 
          
          <body>
             <p>Today's date: <%= (new java.util.Date()).toLocaleString()%></p>
          </body> 
       </html> 
       ```

    6. **Comment tag** marks text or statements that the JSP container should  ignore. A JSP comment is useful when you want to hide or "comment out", a part of your JSP page.

    7. **JSP implicit object** are the Java objects that the JSP Container makes  available to the developers in each page and the developer can call them directly without being explicitly declared.

       1. request : HttpServletRequest
       2. response : HttpServletResponse
       3. out : PrintWriter
       4. session : HttpSession
       5. application : ServletContext
       6. config : ServletConfig
       7. pageContext  : JspWriters
       8. page : this
       9. exception : Exception

       Ref : https://www.tutorialspoint.com/jsp/jsp_implicit_objects.htm

    8. JSP directive

       1. The **page** directive is used to provide instructions to the  container. These instructions pertain to the current JSP page. You may  code page directives anywhere in your JSP page. By convention, page  directives are coded at the top of the JSP page

          ![](/home/tsuyoshi/Github/semester3/WebApp/pageattribute.png)

       2. The **include** directive is used to include a file during the  translation phase. This directive tells the container to merge the  content of other external files with the current JSP during the  translation phase. You may code the ***include\*** directives anywhere in your JSP page.

          1. The filename in the include directive is actually a relative URL. If you just specify a filename with no associated path, the JSP compiler  assumes that the file is in the same directory as your JSP.

             ```jsp
             <jsp:directive.include file = "relative url" />
             ```

       3. The **taglib** directive declares that your JSP page uses a set of  custom tags, identifies the location of the library, and provides means  for identifying the custom tags in your JSP page.

          ```jsp
          <%@ taglib uri="uri" prefix = "prefixOfTag" >
          ```

    9. JSP Action Tag:

       1. jsp:forward forward the request and response to another servlet or jsp page and let that servlet or page response

          1. index.jsp

             ```jsp
             <html>
                <body>
                   <h1>I’m index</h1>
                   <jsp:forward page=”printname.jsp”/>
                </body>
             </html>
             ```

          2. printname.jsp

             ```jsp
             <html>
                <body>
                   <h1>I’m printname</h1>
                   <% out.print("My name is Hun Vikran”); %> 
                </body>
             </html>
             ```

          3. Output:

             ```
             I’m print name
             My name is Hun Vikran
             ```

             

       2. jsp:include pass the request and response to another servlet or jsp page after doing some logic that page or servlet will pass back the request and response.

          1. index.jsp

             ```jsp
             <html>
                <body>
                   <h1>I’m index</h1>
                   <jsp:include page=”printname.jsp”/>
                </body>
             </html>
             ```

             

          2. printname.jsp

             ```jsp
             <html>
                <body>
                   <h1>I’m printname</h1>
                   <% out.print("My name is Hun Vikran”); %> 
                </body>
             </html>
             ```

             Output: 

             ```
             I’m index
             I’m printname
             My name is Hun Vikran
             ```

       3. jsp:useBean

          1. Java Bean is a java class it should have a no-arg constructor, it should be Serializable, it should provide methods to set and get the values of the properties, known as getter and setter methods.They are used to  encapsulate many objects into a single object (the bean), so that they  can be passed around as a single bean object instead of as multiple  individual objects.

          2. index.jsp

             ```jsp
             <%-- 
                 Document   : index
                 Created on : Apr 16, 2020, 9:47:17 PM
                 Author     : tsuyoshi
             --%>
             
             <%@page contentType="text/html" pageEncoding="UTF-8"%>
             <!DOCTYPE html>
             <html>
                <head>
                   <title>Using JavaBeans in JSP</title>
                </head>
                <body>
                   <center>
                      <h2>Using JavaBeans in JSP</h2>
                      <jsp:useBean id = "test" class = "mybeans.Bean" />
                      <jsp:setProperty name = "test" property = "msg" value = "Hello JSP..." />
                      <p>Got message....</p>
                      <jsp:getProperty name = "test" property = "msg" />
                   </center>
                </body>
             </html>
             
             ```

          3. mybeans.Bean

             ```java
             package mybeans;
             
             /*
              * To change this license header, choose License Headers in Project Properties.
              * To change this template file, choose Tools | Templates
              * and open the template in the editor.
              */
             
             /**
              *
              * @author tsuyoshi
              */
             public class Bean {
                 private String msg="ahah";
             
                 public String getMsg() {
                     return msg;
                 }
             
                 public void setMsg(String msg) {
                     this.msg = msg;
                 }
             }
             ```
       
    10. JSTL

        1. The JSP Standard Tag Library (JSTL) represents a set of tags to simplify the JSP development.

        2. Why we use JSTL:

              1. **Fast Development** JSTL provides many tags that simplify the JSP.

           2. **Code Reusability** We can use the JSTL tags on various pages.

           3. **No need to use scriptlet tag** It avoids the use of scriptlet tag.
        3. The <c:out> tag is similar to JSP expression tag, but it can only  be used with expression. It will display the result of an expression,  similar to the way < %=...% > work.

           ```jsp
           <%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>  
           <%@page contentType="text/html" pageEncoding="UTF-8"%>
           <!DOCTYPE html>
           <html>
              <head>
                 <title>Using JavaBeans in JSP</title>
              </head>
              <body>
                 <center>
            <c:out value="i'm vikran"/>  
                 </center>
              </body>
           </html>
           ```

        4. The <c:import> is similar to jsp 'include', with an additional  feature of including the content of any resource either within server or outside the server.

           ```jsp
           <%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>  
           <%@page contentType="text/html" pageEncoding="UTF-8"%>
           <!DOCTYPE html>
           <html>
              <head>
                 <title>Using JavaBeans in JSP</title>
              </head>
              <body>
                 <center>
           <c:import var="data" url="http://www.google.com"/>  
           <c:out value="${data}"/> 
                 </center>
              </body>
           </html>
           ```

        5. <c:set> is used to set the result of an expression evaluated in a 'scope'.  The <c:set> tag is helpful because it evaluates the expression and use the result to set a value of java.util.Map or JavaBean.

           ```jsp
           <c:set var="Income" scope="session" value="${4000*4}"/>  
           <c:out value="${Income}"/>  
           ```

           output:

           ```
           16000
           ```

        6. c:remove is used for removing the specified variable from a particular scope.  This action is not particularly helpful, but it can be used for ensuring that a JSP can also clean up any scope resources.

           ```jsp
           <c:set var="income" scope="session" value="${4000*4}"/>  
           <p>Before Remove Value is: <c:out value="${income}"/></p>  
           <c:remove var="income"/>  
           <p>After Remove Value is: <c:out value="${income}"/></p> 
           ```

           output:

           ```
           Before Remove Value is: 16000  
           After Remove Value is:  
           ```

           

        7. c:catch is used for Catches any Throwable exceptions that occurs in the body  and optionally exposes it. In general it is used for error handling and  to deal more easily with the problem occur in program

           ```jsp
           <%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>  
           <html>
              <head>
                 <title>Core Tag Example</title>
              </head>
              <body>
                 <c:catch var ="catchtheException">  
                    <% int x = 2/0;%>  
                 </c:catch>
                 <c:if test = "${catchtheException != null}">
                    <p>The type of exception is : ${catchtheException} <br />  
                       There is an exception: ${catchtheException.message}
                    </p>
                 </c:if>
              </body>
           </html>
           ```

           

        8. The < c:if > tag is used for testing the condition and it display the body content, if the expression evaluated is true.

           ```jsp
           <%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>  
           <html>
              <head>
                 <title>Core Tag Example</title>
              </head>
              <body>
                 <c:set var="income" scope="session" value="${4000*4}"/>
                 <c:if test="${income > 8000}">
                    <p>
                       My income is: 
                       <c:out value="${income}"/>
                    <p>  
                 </c:if>
              </body>
           </html>
           
           ```

        9. The < c:choose > tag is a conditional tag that establish a context for mutually exclusive conditional operations. It works like a Java  **switch**  statement in which we choose between a numbers of alternatives. 

           ```jsp
           <c:set var="income" scope="session" value="${4000*4}"/>  
           <p>Your income is : <c:out value="${income}"/></p>  
           <c:choose>  
               <c:when test="${income <= 1000}">  
                  Income is not good.  
               </c:when>  
               <c:when test="${income > 10000}">  
                   Income is very good.  
               </c:when>  
               <c:otherwise>  
                  Income is undetermined...  
               </c:otherwise>  
           </c:choose>  
           ```

           ```
            Your income is : 16000  
            Income is very good.  
           ```

        10. The < c:choose > tag is a conditional tag that establish a context for mutually exclusive conditional operations. It works like a Java switch statement in which we choose between a numbers of alternatives. 

           ```jsp
           <c:forEach var="j" begin="1" end="3">  
              Item <c:out value="${j}"/><p>  
           </c:forEach>  
           ```

           ```
             Item 1  
             Item 2  
             Item 3  
           ```

        11. The < c:forTokens > tag iterates over tokens which is separated by the supplied delimeters. It is used for break a string into tokens and  iterate through each of the tokens to generate output.

            ```jsp
            <c:forTokens items="haha-lol-jaja" delims="-" var="name">  
               <c:out value="${name}"/><p>  
            </c:forTokens> 
            ```

            ```
            haha
            lol
            jaja
            ```

        12. The < c:param > tag add the parameter in a containing 'import'  tag's URL. It allow the proper URL request parameter to be specified  within URL and it automatically perform any necessary URL encoding.

            ```jsp
            <c:url value="/index1.jsp" var="completeURL"/>  
             <c:param name="id" value="1"/>  
             <c:param name="user" value="kran"/>  
            </c:url>  
            ${completeURL}  
            ```

            ```
            /LearningWebApp/index.jsp?id=1&user=kran 
            ```

        13. The < c:param > tag add the parameter in a containing 'import' tag's URL. It allow the proper URL request parameter to be specified within URL and it automatically perform any necessary URL encoding.

            ```jsp
              <c:set var="url" value="0" scope="request"/>  
              <c:if test="${url<1}">  
                 <c:redirect url="http://google.com"/>  
              </c:if>  
              <c:if test="${url>1}">  
                 <c:redirect url="http://facebook.com"/>  
              </c:if>  
            ```

            ```
            Since the value of the variable 'url' is 0 the page gets directed to the http://google.com .
            ```

        14. The < c:url > tag creates a URL with optional query parameter. It  is used for url encoding or url formatting. This tag automatically  performs the URL rewriting operation.

            ```jsp
            <c:url value="/haha.jsp"/>
            ```

            ```
            /LearningWebApp/haha.jsp
            ```

        15. **SQL tag** : <%@ taglib uri="http://java.sun.com/jsp/jstl/functions" prefix="fn" %>  

            ```jsp
            <%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>  
            <%@ taglib uri="http://java.sun.com/jsp/jstl/sql" prefix="sql"%>  
            <html>  
                <head>  
                	<title>sql:setDataSource Tag</title>  
                </head>  
                <body>  
            
                <sql:setDataSource var="db" driver="com.mysql.jdbc.Driver"  
                     url="jdbc:mysql://localhost/test"  
                     user="root"  password="1234"/>  
                </body>  
            </html>  
            ```

            ```jsp
            update & query
            
            <%@ page import="java.io.*,java.util.*,java.sql.*"%>  
            <%@ page import="javax.servlet.http.*,javax.servlet.*" %>  
            <%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>  
            <%@ taglib uri="http://java.sun.com/jsp/jstl/sql" prefix="sql"%>  
            <html>
               <head>
                  <title>sql:update Tag</title>
               </head>
               <body>
                  <sql:setDataSource var="db" driver="com.mysql.jdbc.Driver"  
                     url="jdbc:mysql://localhost/test"  
                     user="root"  password="1234"/>
                  <sql:update dataSource="${db}" var="count">  
                     INSERT INTO Students VALUES (154,'Nasreen', 'jaha', 25);  
                  </sql:update>
                  <sql:query dataSource="${db}" var="rs">  
                     SELECT * from Students;  
                  </sql:query>
                  <table border="2" width="100%">
                     <tr>
                        <th>Student ID</th>
                        <th>First Name</th>
                        <th>Last Name</th>
                        <th>Age</th>
                     </tr>
                     <c:forEach var="table" items="${rs.rows}">
                        <tr>
                           <td>
                              <c:out value="${table.id}"/>
                           </td>
                           <td>
                              <c:out value="${table.First_Name}"/>
                           </td>
                           <td>
                              <c:out value="${table.Last_Name}"/>
                           </td>
                           <td>
                              <c:out value="${table.Age}"/>
                           </td>
                        </tr>
                     </c:forEach>
                  </table>
               </body>
            </html>
            ```

             

            ```jsp
            param
            
            <%@ page import="java.io.*,java.util.*,java.sql.*"%>  
            <%@ page import="javax.servlet.http.*,javax.servlet.*" %>  
            <%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>  
            <%@ taglib uri="http://java.sun.com/jsp/jstl/sql" prefix="sql"%>  
            <html>
               <head>
                  <title>sql:update Tag</title>
               </head>
               <body>
                  <sql:setDataSource var="db" driver="com.mysql.jdbc.Driver"  
                     url="jdbc:mysql://localhost/test"  
                     user="root"  password="1234"/>
                  <c:set var="StudentId" value="152"/>
                  <sql:update dataSource="${db}" var="count">
                     DELETE FROM Students WHERE Id = ?  
                     <sql:param value="${StudentId}" />
                  </sql:update>
                  <sql:query dataSource="${db}" var="rs">  
                     SELECT * from Students;  
                  </sql:query>
                  <table border="2" width="100%">
                     <tr>
                        <th>Student ID</th>
                        <th>First Name</th>
                        <th>Last Name</th>
                        <th>Age</th>
                     </tr>
                     <c:forEach var="table" items="${rs.rows}">
                        <tr>
                           <td>
                              <c:out value="${table.id}"/>
                           </td>
                           <td>
                              <c:out value="${table.First_Name}"/>
                           </td>
                           <td>
                              <c:out value="${table.Last_Name}"/>
                           </td>
                           <td>
                              <c:out value="${table.Age}"/>
                           </td>
                        </tr>
                     </c:forEach>
                  </table>
               </body>
            </html>
            ```

            ```jsp
            dateParam
            
            <%@ page import="java.io.*,java.util.*,java.sql.*"%>  
            <%@ page import="javax.servlet.http.*,javax.servlet.*" %>  
            <%@ page import="java.util.Date,java.text.*" %>  
            <%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>  
            <%@ taglib uri="http://java.sun.com/jsp/jstl/sql" prefix="sql"%>  
            <html>
               <head>
                  <title>sql:dateParam Tag</title>
               </head>
               <body>
                  <sql:setDataSource var="db" driver="com.mysql.jdbc.Driver"  
                     url="jdbc:mysql://localhost/test"  
                     user="root"  password="1234"/>
                  <%  
                     Date DoB = new Date("2000/10/16");  
                     int studentId = 151;  
                     %>  
                  <sql:update dataSource="${db}" var="count">
                     UPDATE Student SET dob = ? WHERE Id = ?  
                     <sql:dateParam value="<%=DoB%>" type="DATE" />
                     <sql:param value="<%=studentId%>" />
                  </sql:update>
                  <sql:query dataSource="${db}" var="rs">  
                     SELECT * from Student;  
                  </sql:query>
                  <table border="2" width="100%">
                     <tr>
                        <th>Emp ID</th>
                        <th>First Name</th>
                        <th>Last Name</th>
                        <th>DoB</th>
                     </tr>
                     <c:forEach var="table" items="${rs.rows}">
                        <tr>
                           <td>
                              <c:out value="${table.id}"/>
                           </td>
                           <td>
                              <c:out value="${table.First_Name}"/>
                           </td>
                           <td>
                              <c:out value="${table.Last_Name}"/>
                           </td>
                           <td>
                              <c:out value="${table.dob}"/>
                           </td>
                        </tr>
                     </c:forEach>
                  </table>
               </body>
            </html>
            ```

            ```jsp
            Transaction
            
            <%@ page import="java.io.*,java.util.*,java.sql.*"%>  
            <%@ page import="javax.servlet.http.*,javax.servlet.*" %>  
            <%@ page import="java.util.Date,java.text.*" %>  
            <%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>  
            <%@ taglib uri="http://java.sun.com/jsp/jstl/sql" prefix="sql"%>  
               
            <html>  
            <head>  
            <title>sql:transaction Tag</title>  
            </head>  
            <body>  
               
            <sql:setDataSource var="db" driver="com.mysql.jdbc.Driver"  
                 url="jdbc:mysql://localhost/test"  
                 user="root"  password="1234"/>  
              
            <%  
            Date DoB = new Date("2000/10/16");  
            int studentId = 151;  
            %>  
               
            <sql:transaction dataSource="${db}">  
               <sql:update var="count">  
                  UPDATE Student SET First_Name = 'Suraj' WHERE Id = 150  
               </sql:update>  
               <sql:update var="count">  
                  UPDATE Student SET Last_Name= 'Saifi' WHERE Id = 153  
               </sql:update>  
               <sql:update var="count">  
                 INSERT INTO Student   
                 VALUES (154,'Supriya', 'Jaiswal', '1995/10/6');  
               </sql:update>  
            </sql:transaction>  
               
            <sql:query dataSource="${db}" var="rs">  
               SELECT * from Student;  
            </sql:query>  
               
            <table border="2" width="100%">  
            <tr>  
               <th>Emp ID</th>  
               <th>First Name</th>  
               <th>Last Name</th>  
               <th>DoB</th>  
            </tr>  
            <c:forEach var="table" items="${rs.rows}">  
            <tr>  
               <td><c:out value="${table.id}"/></td>  
               <td><c:out value="${table.First_Name}"/></td>  
               <td><c:out value="${table.Last_Name}"/></td>  
               <td><c:out value="${table.dob}"/></td>  
            </tr>  
            </c:forEach>  
            </table>  
               
            </body>  
            </html>  
            ```

        16. XML Tag

            ```jsp
            parse & out
            <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>  
            <%@ taglib prefix="x" uri="http://java.sun.com/jsp/jstl/xml" %>  
            <html>
               <head>
                  <title>XML Tags</title>
               </head>
               <body>
                  <h2>Vegetable Information:</h2>
                  <c:set var="vegetable">
                     <vegetables>
                        <vegetable>
                           <name>onion</name>
                           <price>40/kg</price>
                        </vegetable>
                        <vegetable>
                           <name>Potato</name>
                           <price>30/kg</price>
                        </vegetable>
                        <vegetable>
                           <name>Tomato</name>
                           <price>90/kg</price>
                        </vegetable>
                     </vegetables>
                  </c:set>
                  <x:parse xml="${vegetable}" var="output"/>
                  <b>Name of the vegetable is</b>:  
                  <x:out select="$output/vegetables/vegetable[1]/name" />
                  <br>  
                  <b>Price of the Potato is</b>:  
                  <x:out select="$output/vegetables/vegetable[2]/price" />
               </body>
            </html>
            
            output:
            Vegetable Information:
            Name of the vegetable is: onion
            Price of the Potato is: 30/kg
            ```
    ```
        
    ​```jsp
            choose
    <c:set var="xmltext">  
            <books>  
            <book>  
              <name>Three mistakes of my life</name>  
      <author>Chetan Bhagat</author>  
              <price>200</price>  
    </book>  
            <book>  
      <name>Tomorrow land</name>  
              <author>Brad Bird</author>  
              <price>2000</price>  
            </book>  
    </books>  
            </c:set>  
      
            <x:parse xml="${xmltext}" var="output"/>  
    <x:choose>  
               <x:when select="$output//book/author = 'Chetan bhagat'">  
                  Book is written by Chetan bhagat  
               </x:when>  
       <x:when select="$output//book/author = 'Brad Bird'">  
                  Book is written by Brad Bird  
               </x:when>  
               <x:otherwise>  
                  The author is unknown...  
               </x:otherwise>  
            </x:choose> 
            
            
            Books Information:
            
            Book is written by Brad Bird
            
    ```

        ```jsp
        if
        <c:set var="vegetables">  
        <vegetables>  
            <vegetable>  
              <name>onion</name>  
              <price>40</price>  
            </vegetable>  
         <vegetable>  
              <name>Potato</name>  
              <price>30</price>  
            </vegetable>  
         <vegetable>  
              <name>Tomato</name>  
              <price>90</price>  
            </vegetable>  
        </vegetables>  
        </c:set>  
        <x:parse xml="${vegetables}" var="output"/>  
        <x:if select="$output/vegetables/vegetable/price < 100">  
           Vegetables prices are very low.  
        </x:if>
        
        ```
    
        ```jsp
        transform & param
        <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>  
        <%@ taglib prefix="x" uri="http://java.sun.com/jsp/jstl/xml" %>  
        <html>
           <head>
              <title>x:transform Tag</title>
           </head>
           <body>
              <h3>Novels Information:</h3>
              <c:set var="xmltext">
                 <books>
                    <book>
                       <name>Three mistakes of my life</name>
                       <author>Chetan Bhagat</author>
                       <price>200</price>
                    </book>
                    <book>
                       <name>Tomorrow land</name>
                       <author>Brad Bird</author>
                       <price>1000</price>
                    </book>
                    <book>
                       <name>Wings of fire</name>
                       <author>Dr. APJ Abdul Kalam</author>
                       <price>500</price>
                    </book>
                 </books>
              </c:set>
              <c:import url="transfer.xsl" var="xslt"/>
              <x:transform xml="${xmltext}" xslt="${xslt}">
                 <x:param name="bgColor" value="yellow"/>
              </x:transform>
           </body>
        </html>
        output: table with backgroud yellow following xsl to render xml
        ```

    17. Function Tag function  provides a number of standard functions, most of these functions are common string manipulation functions. 

        ```jsp
        boolean contains(java.lang.String, java.lang.String)
        <%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>  
        <%@ taglib uri="http://java.sun.com/jsp/jstl/functions" prefix="fn" %>  
        <html>
           <head>
              <title>Using JSTL Functions</title>
           </head>
           <body>
              <c:set var="String" value="Welcome to javatpoint"/>
              <c:if test="${fn:contains(String, 'javatpoint')}">
                 <p>Found javatpoint string
                 <p>
              </c:if>
              <c:if test="${fn:contains(String, 'JAVATPOINT')}">  
              <p>Found JAVATPOINT string<p>  
              </c:if>  
           </body>
        </html>
        output: 
        Found javatpoint string  
        ```

        ```jsp
        containsIgnoreCase
        <c:set var="String" value="Welcome to javatpoint"/>  
          
        <c:if test="${fn:containsIgnoreCase(String, 'javatpoint')}">  
           <p>Found javatpoint string<p>  
        </c:if>  
          
        <c:if test="${fn:containsIgnoreCase(String, 'JAVATPOINT')}">  
           <p>Found JAVATPOINT string<p>  
        </c:if>
        output:
        Found javatpoint string
        Found JAVATPOINT string
        ```

        ```jsp
        java.lang.String escapeXml(java.lang.String)  
        <c:set var="String" value="Welcome to JSP programming"/>  
          
        <c:if test="${fn:endsWith(String, 'programming')}">  
           <p>String ends with programming<p>  
        </c:if>  
          
        <c:if test="${fn:endsWith(String, 'JSP')}">  
           <p>String ends with JSP<p>  
        </c:if>  
        ```

        ```jsp
        <c:set var="string1" value="It is first String."/>  
        <c:set var="string2" value="It is <xyz>second String.</xyz>"/>  
          
        <p>With escapeXml() Function:</p>  
        <p>string-1 : ${fn:escapeXml(string1)}</p>  
        <p>string-2 : ${fn:escapeXml(string2)}</p>  
          
        <p>Without escapeXml() Function:</p>  
        <p>string-1 : ${string1}</p>  
        <p>string-2 : ${string2}</p>  
        
        output
        With escapeXml() Function:  
        string-1 : It is first String.  
        string-2 : It is <xyz>second String.</xyz>  
        Without escapeXml() Function:  
        string-1 : It is first String.  
        string-2 : It is second String.  
        ```

        ```jsp
        indexOf
        <c:set var="string1" value="It is first String."/>  
        <c:set var="string2" value="It is <xyz>second String.</xyz>"/>  
          
        <p>Index-1 : ${fn:indexOf(string1, "first")}</p>  
        <p>Index-2 : ${fn:indexOf(string2, "second")}</p>  
        
        output:
        Index-1 : 6  
        Index-2 : 11  
        ```

        ```jsp
        trim & length
        <c:set var="str1" value="Welcome to JSP        programming         "/>  
        <p>String-1 Length is : ${fn:length(str1)}</p>  
          
        <c:set var="str2" value="${fn:trim(str1)}" />  
        <p>String-2 Length is : ${fn:length(str2)}</p>  
        <p>Final value of string is : ${str2}</p>  
        output
        String-1 Length is : 42  
        String-2 Length is : 33  
        Final value of string is : Welcome to JSP programming  
        ```

        ```jsp
        startsWith
        <c:set var="msg" value="The Example of JSTL fn:startsWith() Function"/>  
        The string starts with "The": ${fn:startsWith(msg, 'The')}  
        <br>The string starts with "Example": ${fn:startsWith(msg, 'Example')}  
        output
        The string starts with "The": true  
        The string starts with "Example": false
        ```

        ```jsp
        split & join
        <c:set var="str1" value="Welcome-to-JSP-Programming."/>  
        <c:set var="str2" value="${fn:split(str1, '-')}" />  
        <c:set var="str3" value="${fn:join(str2, ' ')}" />  
          
        <p>String-3 : ${str3}</p>  
        <c:set var="str4" value="${fn:split(str3, ' ')}" />  
        <c:set var="str5" value="${fn:join(str4, '-')}" />  
          
        <p>String-5 : ${str5}</p>  
        ```

        + quite simple no need  toLowerCase toUpperCase

        ```jsp
        replace
        <c:set var="author" value="Ramesh Kumar"/>  
        <c:set var="string" value="pqr xyz abc PQR"/>  
        ${fn:replace(author, "Ramesh", "Suresh")}  
        ${fn:replace(string, "pqr", "hello")}  
        output:
        Suresh Kumar hello xyz abc PQR  
        ```

        ```jsp
        substring
        <c:set var="string" value="This is the first string."/>  
        ${fn:substring(string, 5, 17)} 
        output:
        is the first
        ```

        ```jsp
        substringBefore
        <c:set var="string" value="Hi, This is JAVATPOINT.COM developed by SONOO JAISWAL."/>  
        ${fn:substringBefore(string, "developed")}  
        output:
        Hi, This is JAVATPOINT.COM  
        ```

        ```jsp
        substringAfter
        <c:set var="string" value="Nakul Jain"/>  
        ${fn:substringAfter(string, "Nakul")}  
        output:
        Jain
        ```


​    
18. Format Tag provide support for message formatting, number and date formatting etc. The url for the formatting tags is  **http://java.sun.com/jsp/jstl/fmt**  and prefix is  **fmt.**

    ref: https://www.javatpoint.com/jstl-formatting-tags        

​    **Internationalization**

    1. Using Java

    ```java
    package com.javatpoint;
    import java.util.ListResourceBundle;  
    public class Simple extends ListResourceBundle {  
        public Object[][] getContents() {  
            return contents;  
        }
        static final Object[][] contents = { { "colour.Violet", "Violet" },  
                { "colour.Indigo", "Indigo" }, { "colour.Blue", "Blue" }, };  
    } 
    ```

    ```jsp
        <%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt" %>
        <html>
            <head>
            	<title>fmt:bundle Tag</title>
            </head>
            <body>
                <fmt:bundle basename="com.javatpoint.Simple" prefix="colour.">
                    <fmt:message key="Violet"/><br/>
                    <fmt:message key="Indigo"/><br/>
                    <fmt:message key="Blue"/><br/>
                </fmt:bundle>
            </body>
        </html>
    ```

    ```
        Violet  
        Indigo  
        Blue  
    ```

    2. Using properties file

       messages.properties

    ```properties
    label.welcome = Welcome
    ```

​    ​       messages_fr.properties

    ```
    label.welcome = Bienvenue
    ```

    ```jsp
    <%@ page contentType="text/html;charset=UTF-8" language="java" %>
    <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
    <%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
    <%@ page isELIgnored="false" %>
     
    <fmt:setLocale value="${param.lang}"/>
            <fmt:setBundle basename="resources.messages"/>
     
    <html>
    <head>
        <title>PhraseApp - i18n</title>
    </head>
    <body>
        <h2>
            <fmt:message key="label.welcome" />
        </h2>
    </body>
    </html>
    
    url : blah.com/?lang=en
    output : Welcome
    
    url : blah.com/?lang=fr
    output : Bienvenue
    ```

19. Custom Tag

    1. **Eliminates the need of scriptlet tag** The custom tags eliminates the need of scriptlet tag which is considered bad programming approach in JSP.
    2. **Separation of business logic from JSP**  The custom tags separate the the business logic from the JSP page so that it may be easy to maintain.
    3. **Re-usability** The custom tags makes the possibility to reuse the same business logic again and again.
    4. Example
    
    ```jsp
    <?xml version="1.0" encoding="UTF-8"?>
    <taglib version="2.1" xmlns="http://java.sun.com/xml/ns/javaee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-jsptaglibrary_2_1.xsd">
      <tlib-version>1.0</tlib-version>
      <short-name>mytag</short-name>
      <uri>/WEB-INF/tlds/mytag</uri>
      <tag>
          <name>today</name>
          <tag-class>customTag.mytag</tag-class>
      </tag>
    </taglib>
    ```
    
    ```java
    package customTag;
    import java.util.Calendar;  
    import javax.servlet.jsp.JspException;  
    import javax.servlet.jsp.JspWriter;  
    import javax.servlet.jsp.tagext.TagSupport;  
    public class mytag extends TagSupport{  
      
        public int doStartTag() throws JspException {  
            JspWriter out=pageContext.getOut();//returns the instance of JspWriter  
            try{  
             out.print(Calendar.getInstance().getTime());//printing date and time using JspWriter  
            }catch(Exception e){System.out.println(e);}  
            return SKIP_BODY;//will not evaluate the body content of the tag  
        }  
    }  
    ```
    
    ```jsp
    <%@ page contentType="text/html;charset=UTF-8" language="java" %>
    <%@ taglib uri="/WEB-INF/tlds/mytag" prefix="m" %>  
    <html>
        <head>
    
        </head>
        <body>
            Current Date and Time is: <m:today/> 
        </body>
    </html>
    ​```
    
    ​```
    output: 
    Current Date and Time is: Sat Apr 18 21:02:54 ICT 2020
    ​```
    ```