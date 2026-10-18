---
title: Enhancement One
description: Enhancement one of the zoo monitoring project, covering software design, login, role-based menus, and reading records from SQL Server.
---

## Enhancement One: Software Design and Engineering

For this enhancement I chose to improve the design and functionality of my zoo monitoring project from IT-145. The original purpose of this code was a simple tracking program, written in Java, to monitor zoo animals and habitats. My original plan was to add several uses, including a login feature and employee records. I was also planning a transfer section, as a log of incoming and outgoing animals, but that section was dropped because of time constraints and because I was learning methods I had not used before.

I chose to include this artifact in all three sections of the portfolio because it shows how I refactored Java code and improved it in more than one layer. It was also a challenge to schedule several modular enhancements to the same program, and to order them while keeping in mind the changes the other enhancements would need. This enhancement shows how I added clearer logic, designed a solution, and improved existing code. The original project was completed in late 2018, so this also meant returning to code I had not touched in a few years, relearning what it did, and improving it.

The code samples below are excerpts. Wording inside the samples, including prompts printed by the program, is unchanged.

The first major rework was the main menu.

<details markdown="1">
<summary>Original main method (excerpt)</summary>

```java
public class ZooKeeperMonitoringSystem {


/**
 * @param args the command line arguments
 * @throws java.io.FileNotFoundException
 */
public static void main(String[] args) throws FileNotFoundException {

    //Class initialization
    Scanner scan = new Scanner(System.in);
    Animals animal = new Animals();
    Habitats habitat = new Habitats();


    //Variables  initialization

    String userSelection;


    //Asking user which system to monitor
    System.out.println("Would you an animal, a habitat, an employee, a transfer, or exit?");
    userSelection = scan.nextLine();

    //Verification for selection
    if(userSelection.toUpperCase().contains("ANIMAL") == false && userSelection.toUpperCase().contains("HABITAT") == false && userSelection.toUpperCase().contains("EXIT") == false ) {
        while( userSelection.toUpperCase().contains("ANIMAL") == false && userSelection.toUpperCase().contains("HABITAT") == false && userSelection.toUpperCase().contains("EXIT") == false ){
            System.out.println("Invalid choice\n");
            System.out.println("Would you an animal, a habitat, an employee, a transfer, or exit?");
            userSelection = scan.nextLine();
        }
    }

    //entering proper choice loop
    while(userSelection.toUpperCase().contains("EXIT") == false) {

        //path if animal
        if(userSelection.toUpperCase().contains("ANIMAL")) {

            animal.montiorAnimals(userSelection);
        }

        //path if habitat
        if (userSelection.toUpperCase().contains("HABITAT")) {

            habitat.monitorHabitats(userSelection);
        }


         //to re-enter loop and check something else or exit
         System.out.println("Would you an animal, a habitat, an employee, a transfer, or exit?");
         userSelection = scan.nextLine();

         //inner loop re-verification before restarting loop
         //Verification for selection
        if(userSelection.toUpperCase().contains("ANIMAL") == false && userSelection.toUpperCase().contains("HABITAT") == false && userSelection.toUpperCase().contains("EMPLOYEE") == false && userSelection.toUpperCase().contains("NEW ARRIVAL") == false && userSelection.toUpperCase().contains("Transfer") == false && userSelection.toUpperCase().contains("EXIT") == false ) {
        while( userSelection.toUpperCase().contains("ANIMAL") == false && userSelection.toUpperCase().contains("HABITAT") == false && userSelection.toUpperCase().contains("EMPLOYEE") == false && userSelection.toUpperCase().contains("NEW ARRIVAL") == false && userSelection.toUpperCase().contains("Transfer") == false && userSelection.toUpperCase().contains("EXIT") == false ){
            System.out.println("Invalid choice\n");
            System.out.println("Would you an animal, a habitat, an employee, a transfer, or exit?");
            userSelection = scan.nextLine();
```

</details>

The long main menu made the main program more complicated than it needed to be. In the new version I reduced the code in the main program so it is more streamlined, and I reorganized the main menu into a structure that fits the new format.

<details markdown="1">
<summary>New main method (excerpt)</summary>

```java
 public class ZooMonitoringSystem {

/**
 * @param args the command line arguments
 */
public static void main(String[] args) {
    // TODO code application logic here
    //Class initialization
    Scanner scan = new Scanner(System.in);
    Login login = new Login();

    //Variable initialization
    String userSelection;
    String username;
    String password;
    String userRole = "Invalid";

    //Initializing SQL connection


    while(userRole.equals("Invalid")){
        System.out.println("Enter your Username: ");
        username = scan.nextLine();
        System.out.println("Enter your password: ");
        password = scan.nextLine();

        //calls the method to try and verify log in using the Users DB
        userRole = login.publicLogin(username, password);
        //validation for invalid and valid logins
        if(userRole.equals("Invalid")){
            System.out.println("Login was invalid.");
        }
        else {
            System.out.println("Login Successful\n\n");
        }

    }

    //fixing the whitespace given by the login()
    userRole = userRole.trim();

    //test outputs
    //System.out.println("Out of while loop");
    //System.out.println(userRole);

    // main menu program call
    MainMenu.mainMenu(userRole);



    //exiting main to terminate
    System.exit(0);
```

</details>

The main system is simpler. It handles login, trims the value returned from login, and then calls the main menu. Login verification was added, and the program loads the user's access level from the username. The new main menu uses that level to show only the options available to that user.

<details markdown="1">
<summary>Switch in the new main menu (excerpt)</summary>

```java
while(!selection.toLowerCase().equals("exit")){
        switch (level) {
            case "HR":
                System.out.println("Would you to view employee information or exit?");
                selection = scan.nextLine();

                //Verification for selection
                if(selection.toUpperCase().contains("EMPLOYEE") == false && selection.toUpperCase().contains("EXIT") == false ) {
                    while(selection.toUpperCase().contains("EMPLOYEE") == false && selection.toUpperCase().contains("EXIT") == false ){
                        System.out.println("Invalid choice\n");
                        System.out.println("Would you to view employee or exit?");
                        selection = scan.nextLine();
                    }
                }
                if(selection.toUpperCase().contains("EMPLOYEE")){
                    Employees.empHR();
                }

                break;

            case "ZK":

                System.out.println("Would you to view animal or habitat information or exit? \n" + "Please enter Animal, Habitat or Exit. \n");
                selection = scan.nextLine();

                //Verification for selection
                if(selection.toUpperCase().contains("ANIMAL") == false && selection.toUpperCase().contains("HABITAT") == false && selection.toUpperCase().contains("EXIT") == false ) {
                    while(selection.toUpperCase().contains("ANIMAL") == false && selection.toUpperCase().contains("HABITAT") == false && selection.toUpperCase().contains("EXIT") == false){
                        System.out.println("Invalid choice\n");
                        System.out.println("Would you an animal, a habitat, or exit?");
                        selection = scan.nextLine();
                    }
                }
                if(selection.toUpperCase().contains("ANIMAL")){
                    Animals.animalSearch();
                }

                if(selection.toUpperCase().contains("HABITAT")){
                    Habitats.habitatSearch();
                }

                break;

            case "AD":
                System.out.println("Would you to view employee, animal, or habitat information or exit? \n" + "Please enter Employee, Animal, Habitat or Exit. \n");
                selection = scan.nextLine();

                //Verification for selection
                if(selection.toUpperCase().contains("ANIMAL") == false && selection.toUpperCase().contains("HABITAT") == false && selection.toUpperCase().contains("EMPLOYEE") == false && selection.toUpperCase().contains("EXIT") == false ) {
                    while(selection.toUpperCase().contains("ANIMAL") == false && selection.toUpperCase().contains("HABITAT") == false && selection.toUpperCase().contains("EMPLOYEE") == false && selection.toUpperCase().contains("EXIT") == false ){
                        System.out.println("Invalid choice\n");
                        System.out.println("Would you to view employee, animal, habitat information, or exit?");
                        selection = scan.nextLine();
                    }
                }
                if(selection.toUpperCase().contains("ANIMAL")){
                    Animals.animalSearch();
                }

                if(selection.toUpperCase().contains("HABITAT")){
                    Habitats.habitatSearch();
                }
                if(selection.toUpperCase().contains("EMPLOYEE")){
                    Employees.empHR();
                }

                break;
```

</details>

The switch uses the access level from login so the menu can show what that access allows. The goal is to guard personal information and limit access to what that role needs. I also added an Employees class so HR and admin personnel can retrieve employee records such as name, hire date, position, and salary, and so zookeepers can see the habitats they maintain.

Many of the other reworks change the program from a scanner reading lines in a text file to a connection that reads SQL Server output. The login excerpt below is the shortest sample that shows the start and finish of that SQL path, with no extra decision logic. The project files are in the downloads below. This part took a while the first time, because I had not connected to SQL Server outside of SQL Server Management Studio. It was a useful way to connect what I was learning in school with experience from my job. The method takes the login information from the main method and checks whether it is in the SQL database of logins.

<details markdown="1">
<summary>SQL logic in the Login class, using JDBC (excerpt)</summary>

```java
String access = null;
      String connectionUrl = "jdbc:sqlserver://localhost:56219;databaseName=ZooInformationSystem;user=ZooAppUser;password=123;";

   ResultSet rs;

   try (Connection connection = DriverManager.getConnection(connectionUrl);
           Statement statement = connection.createStatement();) {

       // Create and execute a SELECT SQL statement.
       //String selectSql = String.format(select, user, pass);
       String select = "SELECT userLevel From Users WHERE username = '%s' AND password = '%s' ";
       String stm = String.format(select, user, pass);

       rs = statement.executeQuery(stm);

       if(!rs.isBeforeFirst()) {
           access = "Invalid";
           return access;
       }
       rs.next();
       access = rs.getString(1);

       //access = "valid";

       //closing values for connections
       rs.close();
       connection.close();
       statement.close();
   }
```

</details>

Further enhancements could include a GUI. Another possibility is different levels of zookeeper and HR personnel, so people with more responsibility have matching access. That would make room for modular features such as adding, updating, and deleting records. Instead of deleting, I would probably use a deleted flag so a record is shown only when someone is looking for deleted records, and only higher level personnel could do that.

{% include downloads.html %}

{% include portfolio-nav.html current="one" %}
