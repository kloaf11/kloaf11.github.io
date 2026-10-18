---
layout: default
title: Enhancement Three
description: Database enhancement of the IT-145 zoo monitoring program for Kenneth Borges's CS499 ePortfolio.
---

## Enhancement Three: Databases

The artifact I chose for this was to augment the zoo monitoring program with a new SQL Server database in place of the text files.

I chose to build this artifact from scratch because it let me show a few things. The first is that I can create usable, realistic databases from nothing. The second is that it gave me a chance to learn JDBC drivers for Java. I learned how to install and use the drivers, and the result was a usable product. The application could be modified to use a company virtual server and be usable there with a small code change. If I wanted it to be used more widely, I could also move the server name and the SQL login out of the code and into a config file that the SQL methods look for. That would make the SQL setup easier to change for a client's server.

### SQL creation scripts

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

Using these scripts, I set up basic tables that the program connects to with JDBC. I also created data that is included in the project files. The data is not too simple. Most of the tables, except employees, include rows that test the program, including several types of animals.

The JDBC Java syntax I used is below.

<details markdown="1">
<summary>SQL example</summary>

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

These are my improvements to the database side of the program, which I built from scratch.

**Links to project file downloads**

- [Application](ZooMonitoringSystem.zip)
- [SQL Files and Bak](Codes_and_Backups_for_CS499_SQL.zip)

{% include portfolio-nav.html %}
