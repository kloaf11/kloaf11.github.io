# Kenneth Borges CS499 ePortfolio

## Enhancement Two: Data Structure and Algorithms

The artifact I chose for this was to augment the Zoo monitoring program with a completely new SQL Server Database over the text files.

I chose to do this artifact from scratch because it allowed me to showcase a few things. The first is my ability to create usable and realistic databases from nothing. The second is it gave me a chance to learn something new in the form of JDBC drivers for Java. I learned how to install and use the drivers. And it came out with an extremely usable product. My application could easily be modified to use a company virtual server and be almost immediately usable to them with a small code change. If I wanted it to be a mass used product, I could also replace the entries for a server and login/password for the sql server to be in a config file that the creation SQL methods look for. That would allow the SQL to be highly customizable for the client and their server setup.


<details><summary>SQL creation scripts</summary>
<details><summary>Security Script</summary>
  ``` SQL
  
      BEGIN TRANSACTION
      SET QUOTED_IDENTIFIER ON
      SET ARITHABORT ON
      SET NUMERIC_ROUNDABORT OFF
      SET CONCAT_NULL_YIELDS_NULL ON
      SET ANSI_NULLS ON
      SET ANSI_PADDING ON
      SET ANSI_WARNINGS ON
       COMMIT
      BEGIN TRANSACTION
      GO
      CREATE TABLE dbo.Security
	    (
	    Id int NOT NULL,
	    username nchar(10) NULL,
	    password nchar(10) NULL,
	    userLevel nchar(10) NULL
	    )  ON [PRIMARY]
      GO
      ALTER TABLE dbo.Security ADD CONSTRAINT
	    PK_Table_1_1 PRIMARY KEY CLUSTERED 
	    (
	    Id
	    ) WITH( STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]

    GO
    ALTER TABLE dbo.Security SET (LOCK_ESCALATION = TABLE)
    GO
    COMMIT
</details>

<details><summary>Habitat Script</summary>
``` SQL

      BEGIN TRANSACTION
      SET QUOTED_IDENTIFIER ON
      SET ARITHABORT ON
      SET NUMERIC_ROUNDABORT OFF
      SET CONCAT_NULL_YIELDS_NULL ON
      SET ANSI_NULLS ON
      SET ANSI_PADDING ON
      SET ANSI_WARNINGS ON
      COMMIT
      BEGIN TRANSACTION
      GO
      CREATE TABLE dbo.Habitats
      	(
      	ID int NOT NULL IDENTITY (1, 1),
      	Name nchar(20) NOT NULL,
      	DateCleaned date NULL,
      	FoodSource nchar(25) NOT NULL,
      	LastFeed date NULL,
      	Temperature nchar(10) NULL
      	)  ON [PRIMARY]
      GO
      ALTER TABLE dbo.Habitats ADD CONSTRAINT
      	PK_Habitats PRIMARY KEY CLUSTERED 
      	(
      	Id
      	) WITH( STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
      
      GO
      ALTER TABLE dbo.Habitats SET (LOCK_ESCALATION = TABLE)
      GO
      COMMIT
      
      
</details>

<details><summary>Animal Script</summary>
``` SQL
  
        BEGIN TRANSACTION
      SET QUOTED_IDENTIFIER ON
      SET ARITHABORT ON
      SET NUMERIC_ROUNDABORT OFF
      SET CONCAT_NULL_YIELDS_NULL ON
      SET ANSI_NULLS ON
      SET ANSI_PADDING ON
      SET ANSI_WARNINGS ON
      COMMIT
      BEGIN TRANSACTION
      GO
      CREATE TABLE dbo.Animals
      	(
      	AnimalId int NOT NULL IDENTITY (1, 1),
      	Name nchar(10) NOT NULL,
      	Age int NOT NULL,
      	BirthDate date NULL,
      	FeedingsToday int NOT NULL,
      	FeedingsperDay int NOT NULL,
      	AnimalType nchar(10) NOT NULL,
      	HeathConcerns nchar(50) NULL,
      	Habitat nchar(15) NOT NULL
      	)  ON [PRIMARY]
      GO
      ALTER TABLE dbo.Animals ADD CONSTRAINT
      	PK_Animals PRIMARY KEY CLUSTERED 
      	(
      	AnimalId
      	) WITH( STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
      
      GO
      ALTER TABLE dbo.Animals SET (LOCK_ESCALATION = TABLE)
      GO
      COMMIT

</details>

<details><summary>Switch within new main menu</summary> 
``` SQL
  
      BEGIN TRANSACTION
    SET QUOTED_IDENTIFIER ON
    SET ARITHABORT ON
    SET NUMERIC_ROUNDABORT OFF
    SET CONCAT_NULL_YIELDS_NULL ON
    SET ANSI_NULLS ON
    SET ANSI_PADDING ON
    SET ANSI_WARNINGS ON
    COMMIT
    BEGIN TRANSACTION
    GO
    CREATE TABLE dbo.Employees
    	(
    	Id int NOT NULL IDENTITY (1, 1),
    	FirstName nchar(20) NOT NULL,
    	LastName nchar(20) NOT NULL,
    	DateofHire date NOT NULL,
    	Age int NOT NULL,
    	Position nchar(20) NOT NULL,
    	HabitatId1 int NULL,
    	HabitatId2 int NULL,
    	HabitatId3 int NULL,
    	Salary nchar(10) NOT NULL
    	)  ON [PRIMARY]
    GO
    ALTER TABLE dbo.Employees ADD CONSTRAINT
    	PK_Table_1 PRIMARY KEY CLUSTERED 
    	(
    	Id
    	) WITH( STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
    
    GO
    ALTER TABLE dbo.Employees SET (LOCK_ESCALATION = TABLE)
    GO
    COMMIT
    
</details>
</details>

Using these scripts, I set up some basic tables that could be connected to using JDBC. In addition, I created data that will be in the files for the project. The data is not too simple and most except employees included ways to test the program including data involving multiple types of animals. 

the JDBC java syntax I used can be found below.



<details><summary>SQL example</summary> 
``` SQL
  
        String connectionUrl = "jdbc:sqlserver://localhost:56219;databaseName=ZooInformationSystem;user=ZooAppUser;password=123;";
  
        System.out.println("Please enter the ID of the employee. \n");
            
            rs2 = statement.executeQuery("SELECT MAX(id) FROM EMPLOYEES");
            
            rs2.next();
            
            countId = rs2.getInt(1);
            
            System.out.println("Max ID currently is " + countId + ". \n");
            
            while (id == 0) {
                System.out.print("Enter an integer: ");
                try {
                    id = scan.nextInt();
                    if(id < 1 || id > countId){
                        System.out.println("Invalid ID number. Please enter a valid employee ID number between 1 and " + countId + ".\n");
                        id = scan.nextInt();
                    }
                }
                catch (InputMismatchException e) {
                    System.out.println("\tInvalid input must be a valid Employee ID. \n Max ID currently is " + countId + ". \n");
                    scan.nextLine();  // Clear invalid input from scanner buffer.
                }
            }
            
            select = idSearch(id);
            
        }
        
            
            // Execute a SELECT SQL statement.
            
            rs = statement.executeQuery(select);
            
            if(!rs.isBeforeFirst()) {
                System.out.println("No Results Found");
            }
            
            while(rs.next()){
             System.out.println("\n\nID: " + rs.getInt(1) + " | First Name: " + rs.getString(2).trim() + " | Last Name: " + rs.getString(3).trim() + " | Position: " +      rs.getString(4).trim() + " | Salary: "  + rs.getString(10).trim() + "\n\n");
                         
            }
            
            
            //closing values for connections
            rs.close();
            connection.close();
            statement.close();
</details>




**Portfolio Links**
- [Professional Self-Assessment](kloaf11.github.io/index.html)
- [Refinement Plan & Code Review](kloaf11.github.io/CodeReview.html)
- [Enhancement One](kloaf11.github.io/Enhancement1.html)
- [Enhancement Two](kloaf11.github.io/Enhancement2.html)
- [Enhancement Three](kloaf11.github.io/Enhancement3.html)
