---
layout: default
title: Enhancement One
description: Software design and engineering enhancement of the IT-145 zoo monitoring program for Kenneth Borges's CS499 ePortfolio.
---

## Enhancement One: Software Design and Engineering

For this enhancement I chose to improve the design and functionality of my zoo monitoring project from IT-145. The original purpose of this code was a simple tracking program written in Java to monitor zoo animals and habitats. My original plan was to add several uses, including a login feature and employee records. I was also planning a transfer section as a log of incoming and outgoing animals, but that section was dropped because of time constraints and because I was learning methods I had not used before.

I chose this artifact for my portfolio, and for all three sections, because it shows that I can refactor Java code and improve it on several layers. It was also a challenge to manage my time across multiple enhancements to the same program, and to order the work so each enhancement still fit with the others. Overall, this enhancement shows that I can add better logic and design solutions, and improve existing code. This project was originally completed in late 2018, so it also showed that I could return to code I had not touched in a few years, relearn how it worked, and improve it.

For this enhancement, the first major rework was the main menu.

<details markdown="1">
<summary>Original main method</summary>

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

I went from a long main menu, which complicated the main program more than it needed to, to a shorter main program. In the new version I streamlined the main program and reorganized the main menu so it fits the new format.

<details markdown="1">
<summary>New main method</summary>

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

I made the main system sleeker and simpler. It handles login, trims the value returned from login, and then calls the main menu. Login verification was added, and the program reads the user's access level from the username. The new main menu uses that level to show only the options available to the user.

<details markdown="1">
<summary>Switch within the new main menu</summary>

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

This switch uses the access level from the new login so each person sees only what that level allows. It guards personal information and limits access to what the job level needs. I also added an Employees class that lets HR and admin personnel retrieve employee records such as name, hire date, position, and salary, and, for zookeepers, the habitats they maintain.

Many of the other major reworks change the program from reading lines in a text file with a scanner to connecting to SQL Server and reading the query output. I am including only the login here, because it is the shortest example of the SQL method from start to finish and it does not need extra decision logic. It takes the login information from the main method and checks whether that login is in the SQL database. The project files are in the download links below. This part took me a while to get right the first time, because I had not connected to SQL Server outside of SQL Server Management Studio. It was a useful way to connect what I was learning in school with my experience at work.

<details markdown="1">
<summary>SQL logic in the Login class using JDBC</summary>

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

Additional enhancements on top of this could include a GUI. You could also add different levels of zookeeper and HR personnel, so people with more responsibility have higher access. That would make it possible to add functions in a modular way, including adding records, updating records, and deleting them when needed to keep the records accurate. Instead of deleting a row, I would probably use a deleted flag so the row is shown only when someone is looking for deleted records, which would be limited to higher level personnel.

**Links to project file downloads**

- [Application](ZooMonitoringSystem.zip)
- [SQL Files and Bak](Codes_and_Backups_for_CS499_SQL.zip)

{% include portfolio-nav.html %}
