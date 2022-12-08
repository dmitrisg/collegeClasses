**How to Save Data From Your Game to the SQL Server.**

**Prerequisites:**
  1. Be on a campus desktop (or connect to one remotely).

**1. Connect to the server in MySQLWorkbench.**

    a. Open MySqlWorkbench, using AJ Apps.  
    b. Click the plus sign next to MySQL Connections. This opens the Setup New Connection window.  
    c. For Connection Name, enter whatever you want to call it.  
    d. For hostname, enter cscmysql.lemoyne.edu.  
    e. For username, enter your username from the 2022_MySQL_Instructions.pdf on Canvas. This should be your last name.  
    f. Click "Store In Vault..." next to Password. This opens the Store Password For Connection window. Enter your password. You can find the password next to your username on the pdf. This should be your first name.  
    g. Do not modify any other fields.   
    h. Click OK at the bottom.  
    i. Now you will see your connection under MySQL Connections. Double click it.
  
**2. Set up your database.**
The DB setup can probably be done through Java, telekinesis, x86 assembly, etc. but I think MySQLWorkbench is easier to use.

    a. After step 1i, you should see a workspace with panels including an Output bar on the bottom and a Navigator on the left side. In the center should be a space to enter text, labeled Query 1. If you do not see Query 1 or have turned it off by accident, click the symbol directly below File in the top left. This will open a new tab.
    b. Creating your database. Enter the code below. Note: do not run it yet.
  ```
  drop database if exists game276[username]; -- replace [username] with your username. This line deletes the DB by that name if one already exists.
  create database game276[username]; -- this creates a new DB.
  use game276[username]; -- makes sure the rest of commands will be executed in this DB.
  ```  
    c. Creating some tables. Enter this code and, afterwards, run the script. To do this, click the lightning bolt icon right above your text. Note that if part of your script is selected, then only that part will execute.
 ```
 create table Game
    (id INTEGER AUTO_INCREMENT PRIMARY KEY,
    computerName VARCHAR(30),
    humanName VARCHAR(30)
    );
    
 create table Score
    (turnId INTEGER AUTO_INCREMENT,
    player CHAR(1), -- this field stores either 'c' or 'h' to identify whether it is the computer's or human's turn
    gameId INTEGER, -- this references the id field from the Game table
    turnScore INTEGER,
    totalScore INTEGER,
    PRIMARY KEY (turnId, player, gameId), -- this defines the primary key (it is made up of multiple fields)
    FOREIGN KEY (gameId)  -- this defines the foreign key 
      REFERENCES Game(id)
      ON DELETE CASCADE
      ON UPDATE CASCADE);
 
``` 
    If it worked, you will be able to see it in the output tab under your script editor. You should see all the commands you just ran with a green checkmark next to each. You may see the message "Error loading schema content". I'm not sure what it means, but it does not seem to be important. 
    d. Adding some test records. Erase all your previous code from the query, so you don't run it twice. Write:
```
    use game276[username];
    insert into Game (computerName, humanName) VALUES ('x', 'y'); -- this will create a record in the Game table. Note that the id is auto generated, so you do not need to specify what it is.
    insert into Score 
        SET player = 'c', 
        turnScore = 7,
        totalScore = 10,
        gameId = (SELECT MAX(id) FROM Game); -- this sets the gameId to the whatever the highest id in Game is.
```
    Run the script.
    e. Retrieving your new records. If you successfully inserted the records, you should be able to fetch them. Erase your code again and run the script below.
```
    use game276[username];
    select * from Game; -- the * means "all".
    select * from Score;
``` 
    If the select worked, some tabs will pop up with results. One will contain the Game records, and the other will have the Score records. If the results show you the tables but no records under them, the insert mostly likely failed. Note: so far there should only be one record for each table.

    
**3. Connect the library with the SQL related classes.** 

    a. Download mysql-connector-java-8.0.21.jar from Canvas.
    b. I don't know if this is necessary, but I put the JAR in my project src folder (in the same directory as my other classes) and extracted it. You probably don't have to do this.
    c. Connect the JAR to your project by adding it to the classpath. There should be instructions for this online, but the details will depend on your IDE.

**4. Execute a SQL insert statement from you Java code. **
  1. Make sure you have the DB and the JAR set up, as described above.
  2. Create a SQLConnector class.
  3. Give it an instance variable of type Connection. (In this code, I call the variable dbConnection.) It can be null for now.
  4. The rest of your code should go into methods that can throw or catch an exception. 
  5. "Register the driver" (idk what that means tbh):
```Class.forName("com.mysql.cj.jdbc.Driver").getDeclaredConstructor().newInstance();```
  7. Plug your DB name, username, and password into the code below. 
```dbConnection = DriverManager.getConnection("jdbc:mysql://cscmysql.lemoyne.edu/game276[username]?user=[username]&password=[password]");```
  9. Make a String containing your SQL insert command, replacing the values with question marks. For example: "INSERT INTO GAME VALUES (?,?,?);"
  10. Create a PreparedStatement object using your String. 
```PreparedStatement stmt = dbConnection.prepareStatement([your sql string here]);```
  12. Replace the question marks with your values. For example:
```stmt.setInt(1, 34); //replaces the first question mark with the number 34
   stmt.setString(2, "X"); //replaces the second question mark with the String "X"
   stmt.setString(3, aRandomString); //replaces the third question mark with a variable
   ```
   13. Execute the statement. 
   ```stmt.executeUpdate();```
   14. There should be a new record in your database now. 
    
**Things to do instead of trying to connect to a SQL server:**

    1. Convert to a major world religion.
    2. Play Half-Life.
    3. Download a new IDE. 
    4. Switch to a major in Business Analytics.
    5. Learn about reversible computing (i think its cool).
    6. Watch Blade Runner 2049. 
    7. Have a life.
    8. Literally anything.
    
*Note: I do not authorize the direct copying of my code, for legal reasons. All the code in this document should be used as an illustration only. 
