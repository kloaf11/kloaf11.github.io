# Kenneth Borges CS499 ePortfolio

## Enhancement Two: Data Structure and Algorithms

The artifact I chose for Data Structure and alhorithms was the same as the first enhancement. My original plan for this structure was to add more robust data structure to the elements and classes that this code covers.


<details><summary>text block of original Text Files</summary> 
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
</details>

and I transfered over and added new variables for the animals. As well as making the users and Employees data for logging in and employee records.

<details><summary>SQL Data types and columns</summary> 
 <p>
 1. Users:<br/>
    	Id int NOT NULL,<br/>
  	    username nchar(10) NULL,  <br/>
	      password nchar(10) NULL,  <br/>
	     userLevel nchar(10) NULL  <br/>
 2. Animals:  <br/>
        AnimalId int NOT NULL IDENTITY (1, 1),  <br/>
	      Name nchar(10) NOT NULL,  <br/>
	      Age int NOT NULL,  <br/>
	    BirthDate date NULL,  <br/>
	    FeedingsToday int NOT NULL,  <br/>
	    FeedingsperDay int NOT NULL,  <br/>
	    AnimalType nchar(10) NOT NULL,  <br/>
	    HeathConcerns nchar(50) NULL,  <br/>
	    Habitat nchar(15) NOT NULL  <br/>
 3. Habitat:  <br/>
      ID int NOT NULL IDENTITY (1, 1),  <br/>
	    Name nchar(20) NOT NULL,  <br/>
	    DateCleaned date NULL,  <br/>
	    FoodSource nchar(25) NOT NULL,  <br/>
	    LastFeed date NULL,  <br/>
	    Temperature nchar(10) NULL  <br/>
 3. Employees:  <br/>
      Id int NOT NULL IDENTITY (1, 1),  <br/>
	    FirstName nchar(20) NOT NULL,  <br/>
	    LastName nchar(20) NOT NULL,  <br/>
	    DateofHire date NOT NULL,  <br/>
	    Age int NOT NULL,  <br/>
	    Position nchar(20) NOT NULL,  <br/>
	    HabitatId1 int NULL,  <br/>
	    HabitatId2 int NULL,  <br/>
	    HabitatId3 int NULL,  <br/>
	    Salary nchar(10) NOT NULL  <br/>
    
</p>
</details>

I have added a lot of information to most of the fields that seemed relavent without overdoing it. In addition to additional fields I increased the amount of situations it can handle. So previously it only handled looking for animal by type in a single list of 1 of each animal type. But for Animal I added a search for Name, Type and SQL ID.
<details><summary>New animal search types</summary>
  <p>
  ```
  
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
        
        
</p>
</details>

In addition to the new search typd for animals there is more for employees and Habitats. Habitats is just name and ID so similar to animal. Employees can do open searches on ID and Name as well. But Name then splits into a search for First name only, Last Name only or both.

And just like animal any result that matches more than one row of results in SQL will be iterated through and show all applicable results which was not capable with the original program.

<details><summary>New animal search types</summary>
  <p>
  ```
    
    Looking at the Name or ID choice.
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
        then the logic for Name choices
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
        And theres verification for the ID that it's a valid ID
        
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
</p>
</details>

All the choices call a sql statement constructor method and returns that string and the actual query is don by a single statement in the main of each class. It's possible that I could even make a sql statment running query. and pass each statement to it. To minimize the amount of places connections are opened or close.

**Portfolio Links**
- [Professional Self-Assessment](kloaf11.github.io/index.html)
- [Refinement Plan & Code Review](kloaf11.github.io/CodeReview.html)
- [Enhancement One](kloaf11.github.io/Enhancement1.html)
- [Enhancement Two](kloaf11.github.io/Enhancement2.html)
- [Enhancement Three](kloaf11.github.io/Enhancement3.html)
