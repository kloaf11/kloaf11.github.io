---
title: Software Design and Engineering
description: "Java zoo monitoring application with login, role-based menus, and a clearer program structure. Stack: Java, JDBC, and SQL Server."
---

## Zoo monitoring: software design and engineering

This is a Java console application that tracks zoo animals and habitats. The first version, finished in late 2018, read simple text files. I later reworked the design so the program is easier to follow, and so each person sees only the records their role needs.

The application starts with a login. After a successful login it loads that person's access level and opens a menu for that level. HR and admin can look up employee records such as name, hire date, position, and salary. Zookeepers can look up animals and habitats, including the habitats they maintain. Admin can use both. I had planned a transfer log for animals moving in and out, and I dropped that part because the login, menus, and data access took the time I had.

The samples below are excerpts. User-facing prompts are corrected for grammar. Method names and the decision logic are unchanged.

### Original main menu

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
    System.out.println("Would you like an animal, a habitat, an employee, a transfer, or exit?");
    userSelection = scan.nextLine();

    //Verification for selection
    if(userSelection.toUpperCase().contains("ANIMAL") == false && userSelection.toUpperCase().contains("HABITAT") == false && userSelection.toUpperCase().contains("EXIT") == false ) {
        while( userSelection.toUpperCase().contains("ANIMAL") == false && userSelection.toUpperCase().contains("HABITAT") == false && userSelection.toUpperCase().contains("EXIT") == false ){
            System.out.println("Invalid choice\n");
            System.out.println("Would you like an animal, a habitat, an employee, a transfer, or exit?");
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
         System.out.println("Would you like an animal, a habitat, an employee, a transfer, or exit?");
         userSelection = scan.nextLine();

         //inner loop re-verification before restarting loop
         //Verification for selection
        if(userSelection.toUpperCase().contains("ANIMAL") == false && userSelection.toUpperCase().contains("HABITAT") == false && userSelection.toUpperCase().contains("EMPLOYEE") == false && userSelection.toUpperCase().contains("NEW ARRIVAL") == false && userSelection.toUpperCase().contains("Transfer") == false && userSelection.toUpperCase().contains("EXIT") == false ) {
        while( userSelection.toUpperCase().contains("ANIMAL") == false && userSelection.toUpperCase().contains("HABITAT") == false && userSelection.toUpperCase().contains("EMPLOYEE") == false && userSelection.toUpperCase().contains("NEW ARRIVAL") == false && userSelection.toUpperCase().contains("Transfer") == false && userSelection.toUpperCase().contains("EXIT") == false ){
            System.out.println("Invalid choice\n");
            System.out.println("Would you like an animal, a habitat, an employee, a transfer, or exit?");
            userSelection = scan.nextLine();
```

</details>

The original menu lived in the main method and became hard to follow. I moved the menu into its own flow and left main responsible for login and for starting that menu.

### Login and main method

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

Login checks the username and password, trims the access level that comes back, and passes that level to the menu.

### Role-based menu

<details markdown="1">
<summary>Switch in the new main menu (excerpt)</summary>

```java
while(!selection.toLowerCase().equals("exit")){
        switch (level) {
            case "HR":
                System.out.println("Would you like to view employee information or exit?");
                selection = scan.nextLine();

                //Verification for selection
                if(selection.toUpperCase().contains("EMPLOYEE") == false && selection.toUpperCase().contains("EXIT") == false ) {
                    while(selection.toUpperCase().contains("EMPLOYEE") == false && selection.toUpperCase().contains("EXIT") == false ){
                        System.out.println("Invalid choice\n");
                        System.out.println("Would you like to view employee or exit?");
                        selection = scan.nextLine();
                    }
                }
                if(selection.toUpperCase().contains("EMPLOYEE")){
                    Employees.empHR();
                }

                break;

            case "ZK":

                System.out.println("Would you like to view animal or habitat information or exit? \n" + "Please enter Animal, Habitat or Exit. \n");
                selection = scan.nextLine();

                //Verification for selection
                if(selection.toUpperCase().contains("ANIMAL") == false && selection.toUpperCase().contains("HABITAT") == false && selection.toUpperCase().contains("EXIT") == false ) {
                    while(selection.toUpperCase().contains("ANIMAL") == false && selection.toUpperCase().contains("HABITAT") == false && selection.toUpperCase().contains("EXIT") == false){
                        System.out.println("Invalid choice\n");
                        System.out.println("Would you like an animal, a habitat, or exit?");
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
                System.out.println("Would you like to view employee, animal, or habitat information or exit? \n" + "Please enter Employee, Animal, Habitat or Exit. \n");
                selection = scan.nextLine();

                //Verification for selection
                if(selection.toUpperCase().contains("ANIMAL") == false && selection.toUpperCase().contains("HABITAT") == false && selection.toUpperCase().contains("EMPLOYEE") == false && selection.toUpperCase().contains("EXIT") == false ) {
                    while(selection.toUpperCase().contains("ANIMAL") == false && selection.toUpperCase().contains("HABITAT") == false && selection.toUpperCase().contains("EMPLOYEE") == false && selection.toUpperCase().contains("EXIT") == false ){
                        System.out.println("Invalid choice\n");
                        System.out.println("Would you like to view employee, animal, habitat information, or exit?");
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

The menu switches on the access level, so HR, zookeeper, and admin each get a short list of choices. Employee information stays with the roles that need it. An Employees class reads those records.

The rest of the design work replaced line-by-line text-file reads with SQL Server queries through JDBC. The login excerpt is the shortest sample of that path. It takes the username and password from main and checks them against the Users table. I had not connected Java to SQL Server outside of SQL Server Management Studio before this project. The password in the connection string is a placeholder.

<details markdown="1">
<summary>SQL logic in the Login class, using JDBC (excerpt)</summary>

```java
String access = null;
      String connectionUrl = "jdbc:sqlserver://localhost:56219;databaseName=ZooInformationSystem;user=ZooAppUser;password=YOUR_PASSWORD;";

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

A graphical interface would be a natural next step. So would finer-grained zookeeper and HR roles, and the ability to add, update, and retire records. I would retire a record with a deleted flag, shown only to higher-level roles, instead of removing the row.

A recorded walkthrough is on the [code review](CodeReview.html) page.

{% include downloads.html %}

{% include portfolio-nav.html current="one" %}
