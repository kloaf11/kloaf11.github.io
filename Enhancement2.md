---
title: Algorithms and Data Structures
description: "How the zoo monitoring application stores records and searches them by name, type, and ID. Stack: Java, JDBC, and SQL Server."
---

## Zoo monitoring: algorithms and data structures

This is the same Java application. The original records were text files, one block of text per animal or habitat. I replaced that layout with tables and added searches that can return more than one match.

<details markdown="1">
<summary>Original text files</summary>

```text
animals.txt
    Details on lions
  Details on tigers
  Details on bears
  Details on giraffes

  Animal - Lion
  Name: Leo
  Age: 5
  *****Health concerns: Cut on left front paw
  Feeding schedule: Twice daily

  Animal - Tiger
  Name: Maj
  Age: 15
  Health concerns: None
  Feeding schedule: 3x daily

  Animal - Bear
  Name: Baloo
  Age: 1
  Health concerns: None
  *****Feeding schedule: None on record

  Animal - Giraffe
  Name: Spots
  Age: 12
  Health concerns: None
  Feeding schedule: Grazing


habitat.txt
    Details on penguin habitat
  Details on bird house
  Details on aquarium

  Habitat - Penguin
  Temperature: Freezing
  *****Food source: Fish in water running low
  Cleanliness: Passed

  Habitat - Bird
  Temperature: Moderate
  Food source: Natural from environment
  Cleanliness: Passed

  Habitat - Aquarium
  Temperature: Varies with output temperature
  Food source: Added daily
  *****Cleanliness: Needs cleaning from algae
```

</details>

The text files held one animal of each type and a few habitats. Notes were marked in the text itself. There was no id, and a search could not return several rows.

### Tables and columns

I moved animals, habitats, users, and employees into SQL Server tables and added fields the text files did not have, including birth date, feedings, hire date, and habitat ids. Column names below are the names in the database. `HeathConcerns` is spelled that way in the table.

<details markdown="1">
<summary>SQL data types and columns</summary>

```text
1. Users:
   Id int NOT NULL,
   username nchar(10) NULL,
   password nchar(10) NULL,
   userLevel nchar(10) NULL

2. Animals:
   AnimalId int NOT NULL IDENTITY (1, 1),
   Name nchar(10) NOT NULL,
   Age int NOT NULL,
   BirthDate date NULL,
   FeedingsToday int NOT NULL,
   FeedingsperDay int NOT NULL,
   AnimalType nchar(10) NOT NULL,
   HeathConcerns nchar(50) NULL,
   Habitat nchar(15) NOT NULL

3. Habitat:
   ID int NOT NULL IDENTITY (1, 1),
   Name nchar(20) NOT NULL,
   DateCleaned date NULL,
   FoodSource nchar(25) NOT NULL,
   LastFeed date NULL,
   Temperature nchar(10) NULL

4. Employees:
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
```

</details>

### Searches

Animals can be looked up by name, type, or id. The text-file version could only look up an animal by type, and it held one animal of each type.

<details markdown="1">
<summary>New animal search types (excerpt)</summary>

```java
// choice on what to search by
System.out.println("Welcome to the animal record section. \n Would you like to look someone up by the animal ID, Type, or Name? Please Enter 'ID', 'Type', or 'Name' \n");
lookupChoice = scan.nextLine();

//Verification for selection
if(lookupChoice.toUpperCase().contains("NAME") == false && lookupChoice.toUpperCase().contains("ID") == false && lookupChoice.toUpperCase().contains("TYPE") == false){
    while( lookupChoice.toUpperCase().contains("NAME") == false && lookupChoice.toUpperCase().contains("ID") && lookupChoice.toUpperCase().contains("TYPE") == false){
        System.out.println("Invalid choice\n");
        System.out.println("Invalid choice. Please enter 'ID', 'Type', or 'Name' \n");
        lookupChoice = scan.nextLine();
    }
}
```

</details>

Habitats can be looked up by name or id. Employees can be looked up by id or by name, and a name search can use the first name, the last name, or both. When more than one row matches, the program walks the result set and shows each match. The text-file version could not do that.

<details markdown="1">
<summary>Employee search choices (excerpt)</summary>

Looking at the name or ID choice.

```java
System.out.println("Welcome to the employee record section. \n Would you like to look someone up by the employee ID or Name? Please Enter 'Name' or 'ID' \n");
lookupChoice = scan.nextLine();

//Verification for selection
if(lookupChoice.toUpperCase().contains("NAME") == false && lookupChoice.toUpperCase().contains("ID") == false){
    while( lookupChoice.toUpperCase().contains("NAME") == false && lookupChoice.toUpperCase().contains("ID")){
        System.out.println("Invalid choice\n");
        System.out.println("Would you like to look someone up by the employee ID or Name? Please Enter 'Name' or 'ID'? \n");
        lookupChoice = scan.nextLine();
    }
}
```

Then the logic for the name choices.

```java
if(lookupChoice.toUpperCase().contains("NAME")) {
    System.out.println("Would you like to look by First name, last name or both? Please enter 'First', 'Last', or 'Both' \n");
    lookup2 = scan.nextLine();
     //Verification for selection
    if(lookup2.toUpperCase().contains("FIRST") == false && lookup2.toUpperCase().contains("LAST") == false && lookup2.toUpperCase().contains("BOTH") == false ){
        while(lookup2.toUpperCase().contains("FIRST") == false && lookup2.toUpperCase().contains("LAST") == false && lookup2.toUpperCase().contains("BOTH") == false){
            System.out.println("Invalid choice\n");
            System.out.println("Would you like to look by First name, last name or both? Please enter 'First', 'Last', or 'Both' \n");
            lookup2 = scan.nextLine();
        }
    }
    if(lookup2.toUpperCase().contains("FIRST")){
        System.out.println("Please enter the first name.");
        first = scan.nextLine();
        select = nameSearch(first, last);
    }
    else if(lookup2.toUpperCase().contains("LAST")){
        System.out.println("Please enter the last name.");
        last = scan.nextLine();
        select = nameSearch(first, last);
    }
    else{
        System.out.println("Please enter the first name.");
        first = scan.nextLine();
        System.out.println("Please enter the last name.");
        last = scan.nextLine();
        select = nameSearch(first, last);
    }
}
```

There is also a check that the ID is a valid ID.

```java
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
```

</details>

Each choice builds a SQL string in a method, and one statement in the class runs that string. A later improvement would be a single method that runs any of those statements, so the connection is opened and closed in one place.

A recorded walkthrough is on the [code review](CodeReview.html) page.

{% include downloads.html %}

{% include portfolio-nav.html current="two" %}
