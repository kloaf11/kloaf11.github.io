---
layout: default
title: Enhancement Two
description: Data structures and algorithms enhancement of the IT-145 zoo monitoring program for Kenneth Borges's CS499 ePortfolio.
---

## Enhancement Two: Data Structure and Algorithms

The artifact I chose for data structures and algorithms was the same as the first enhancement. My original plan for this part was to add a more robust data structure to the elements and classes that this code covers.

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

I transferred the animal records and added new fields for the animals. I also added users and employees data for logging in and for employee records.

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

I added information to most of the fields that seemed relevant, without overdoing it. Along with the new fields, I increased the number of situations the program can handle. Previously it could only look up an animal by type, in a single list with one of each animal type. For animals, I added a search by name, type, and SQL id.

<details markdown="1">
<summary>New animal search types</summary>

```java
// choice on what to search by
System.out.println("Welcome to the animal record section. \n Would you look someone up by the animal ID, Type, or Name? Please Enter 'ID', 'Type', or 'Name' \n");
lookupChoice = scan.nextLine();

//Verification for selection
if(lookupChoice.toUpperCase().contains("NAME") == false && lookupChoice.toUpperCase().contains("ID") == false && lookupChoice.toUpperCase().contains("TYPE") == false){
    while( lookupChoice.toUpperCase().contains("NAME") == false && lookupChoice.toUpperCase().contains("ID") && lookupChoice.toUpperCase().contains("TYPE") == false){
        System.out.println("Invalid choice\n");
        System.out.println("Invalid animal ID, Type, or Name? Please Enter 'ID', 'Type', or 'Name' \n");
        lookupChoice = scan.nextLine();
    }
}
```

</details>

In addition to the new search types for animals, there are more for employees and habitats. Habitat search is by name and id, similar to animals. Employees can be searched by id and by name. Name then splits into a search for first name only, last name only, or both.

As with animals, any search that matches more than one SQL row is iterated so every matching result is shown. The original program could not do that.

<details markdown="1">
<summary>Employee search types</summary>

Looking at the name or id choice.

```java
System.out.println("Welcome to the employee record section. \n Would you look someone up by the employee ID or Name? Please Enter 'Name' or 'ID' \n");
lookupChoice = scan.nextLine();

//Verification for selection
if(lookupChoice.toUpperCase().contains("NAME") == false && lookupChoice.toUpperCase().contains("ID") == false){
    while( lookupChoice.toUpperCase().contains("NAME") == false && lookupChoice.toUpperCase().contains("ID")){
        System.out.println("Invalid choice\n");
        System.out.println("Would you look someone up by the employee ID or Name? Please Enter 'Name' or 'ID'? \n");
        lookupChoice = scan.nextLine();
    }
}
```

Then the logic for the name choices.

```java
if(lookupChoice.toUpperCase().contains("NAME")) {
    System.out.println("Would you look by First name, last name or both? Please enter 'First', 'Last', or 'Both' \n");
    lookup2 = scan.nextLine();
     //Verification for selection
    if(lookup2.toUpperCase().contains("FIRST") == false && lookup2.toUpperCase().contains("LAST") == false && lookup2.toUpperCase().contains("BOTH") == false ){
        while(lookup2.toUpperCase().contains("FIRST") == false && lookup2.toUpperCase().contains("LAST") == false && lookup2.toUpperCase().contains("BOTH") == false){
            System.out.println("Invalid choice\n");
            System.out.println("Would you look by First name, last name or both? Please enter 'First', 'Last', or 'Both' \n");
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

And there is verification that the id is valid.

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

All of the choices call a SQL statement constructor method and return that string. The actual query is done by a single statement in the main method of each class. I could even make one method that runs a SQL statement and pass each statement to it, to cut down the number of places where connections are opened or closed.

**Links to project file downloads**

- [Application](ZooMonitoringSystem.zip)
- [SQL Files and Bak](Codes_and_Backups_for_CS499_SQL.zip)

{% include portfolio-nav.html %}
