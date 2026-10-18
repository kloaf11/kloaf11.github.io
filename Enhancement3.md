---
title: Enhancement Three
description: Enhancement three of the zoo monitoring project, covering SQL Server tables and JDBC access in place of text files.
---

## Enhancement Three: Databases

The artifact for this enhancement augments the zoo monitoring program with a new SQL Server database in place of the text files.

I built this part from scratch so I could show two things. The first is creating a usable, realistic database from nothing. The second is learning JDBC drivers for Java: how to install them and how to use them. The result is a usable product. The application could be pointed at a virtual server and, with a small code change, be ready to use there. If it needed to be set up in more than one place, the server and login entries could move into a config file that the SQL methods read. That would let each setup use its own server without changing the Java methods.

The scripts below create the tables.

<details markdown="1">
<summary>Security script</summary>

```sql
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
```

</details>

<details markdown="1">
<summary>Habitat script</summary>

```sql
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
```

</details>

<details markdown="1">
<summary>Animal script</summary>

```sql
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
```

</details>

<details markdown="1">
<summary>Employees script</summary>

```sql
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
```

</details>

With these scripts I set up basic tables that JDBC can connect to. I also created data that is included with the project files. The data is not trivial. Most tables, other than employees, include rows that exercise the program, including more than one animal type.

The JDBC example below is an excerpt.

<details markdown="1">
<summary>JDBC example (excerpt)</summary>

```java
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
```

</details>

These are my improvements to the database side of the program, built from scratch.

{% include downloads.html %}

{% include portfolio-nav.html current="three" %}
