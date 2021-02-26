# Kenneth Borges CS499 ePortfolio

## Enhancement One: Software Design and Engineering

For this enhancement I chose to enhance the design and functionality for my Zoo Monitoring Project from IT-145. The original purpose of this code was a simple tracking program in using Java to monitor Zoo animals and habitats. My original plan for this enhancement was to add multiple additional uses for this program including a log in feature, Employee Records. I was also planning on a Transfer section originally for new incoming and outgoing animals as a log but due to time constraints with the other sections and learning new methods of performing operations I have never attempted to before that section was dropped.

 I chose to include this artifact in my portfolio and all three sections as it demonstrated my ability to refactor my code in Java to improve it on multiple layers. It also provided a challenge in managing my time to meet multiple modular enhancements to the same program and organize the order and best way to handle each enhancement while keeping in mind changes the other enhancements would require. Overall, I think this enhancement specifically shows my ability to add better logic, design solution and work towards accomplishing improvement on existing code. This project was originally completed in late 2018. So, it also showed my ability to look at code that I have not looked at or touched in a few years and relearn all the functions of the code and improve on them.
 
 For this enhancement, the first major rework was the main menu.

<details><summary>Original Main method</summary>
'''
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
</details>

I went from the long main menu which complicated the main program more than it needed to and in the new addition I reduced the code in the main program to be more steam lined and regarded the main menu into a structure that fits the new format.

<details><summary>New Main method</summary>
~~~
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
 </details>

I made the main system sleeker and simpler where it was just log in and calling the main menu after trimming the return from login. With the addition of login verification code was added as well as it pulled based on username the users access level. this is important for the new main menu program which uses that to show only the options available to the user.

<details><summary>Switch within new main menu</summary>
~~~
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
  </details>
  
  This shows the switch used based on the access level retrieved from the new login for a more direct this is what your access allowed method. To guard personal information and limit user access to a job level need basis only. In addition to the limited access and new login method. I have added an Employees class which allows HR and admin personnel to retrieve employee records with data such as Name, hired, position, salary and for Zookeepers the habitats they maintain.
  
  Many of the other major reworks involve the change from scanner reading lines in a text document to connecting and reading SQL Server output. I will only include the login for this purpose of showing the code used. The entire project can be found in the GitHub link below. This part took me a while to get right the first time as I've never connected to SQL Server outside of SQL Server Management Studio. It was a very good learning process though to link my schooling and current expierencing in my job. I'm using the Login class and method because it is the shortest code that shows the start and finish of the sql method with no decision logic needed. This just takes the log in information provided from the main method and attempts to see if it is in the SQL DB for log ins.
  
  

<details><summary>SQL Logic in Login class using JDBC</summary> 
~~~
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
</details>

Additional enhancements that could be added on top would be a GUI. In addition to that, you could add in the ability to have different levels of ZK and HR personnel, so they have higher access in to respect their higher level of responsibilities. This would allow additional functions and features to be modularly added in. Including but not limited to adding in records, updating records, and deleting them as necessary to maintain accurate records. Instead of deleting though for posterity I would probably program a deleted flag to indicate that it should only be shown when looking for deleted records which would be limited to higher level personnel.


**Links to project File Downloads**
- [Application](ZooMonitoringSystem.zip)
- [SQL Files and Bak](Codes_and_Backups_for_CS499_SQL.zip)


**Portfolio Links**
- [Professional Self-Assessment](index.html)
- [Refinement Plan & Code Review](CodeReview.html)
- [Enhancement One](Enhancement1.html)
- [Enhancement Two](Enhancement2.html)
- [Enhancement Three](Enhancement3.html)

