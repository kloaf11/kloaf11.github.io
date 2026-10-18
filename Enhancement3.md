---
title: Databases
description: "SQL Server tables and JDBC access for the zoo monitoring application, replacing the original text files. Stack: Java, JDBC, and SQL Server."
---

## Zoo monitoring: databases

I replaced the text files with a SQL Server database that the Java application reads through JDBC. The tables are small and realistic enough for the program to query, and the connection settings are the only piece that has to change to point the application at another server.

The tables are Security (login and access level), Habitats, Animals, and Employees. The scripts below create them. Sample data is in the project download. Most tables, other than employees, include rows that exercise the program, including more than one animal of a type.

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

The Java excerpt below is the JDBC connection and an employee lookup by id. The password in the connection string is a placeholder. Replace it before using the code.

<details markdown="1">
<summary>JDBC example (excerpt)</summary>

```java
String connectionUrl = "jdbc:sqlserver://localhost:56219;databaseName=ZooInformationSystem;user=ZooAppUser;password=YOUR_PASSWORD;";

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

That is the database side of the application: tables created for this project, and Java reading them with JDBC.

A recorded walkthrough is on the [code review](CodeReview.html) page.

{% include downloads.html %}

{% include portfolio-nav.html current="three" %}
