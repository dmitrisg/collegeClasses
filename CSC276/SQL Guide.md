**How to Save Data From Your Game to the SQL Server.**

**Prerequisites:**
  1. Be on a campus desktop (or connect to one remotely).

**1. Connect to the server in MySQLWorkbench.**

    a. Open MySqlWorkbench, using AJ Apps.  
    b. Click the little plus sign next to MySQL Connections. This opens the Setup New Connection window.  
    c. For Connection Name, enter whatever you want to call it.  
    d. For hostname, enter cscmysql.lemoyne.edu.  
    e. For username, enter your username from the 2022_MySQL_Instructions.pdf on Canvas. This should be your last name.  
    f. Click "Store In Vault..." next to Password. This opens the Store Password For Connection window. Enter your password. You can find the password next to your username on the pdf. This should be your first name.  
    g. Do not modify any other fields.   
    h. Click OK at the bottom.  
    i. Now you will see your connection under MySQL Connections. Double click it.
  
**2. Set up your database.**
I'm sure this can all be done through Java, telekinesis, x86 assembly, etc. but using MySQLWorkbench is much easier. 

    a. After step 1i, you should see a workspace with panels including an Output bar on the bottom and a Navigator on the left side. In the center should be a space to enter text, labeled Query 1. If you do not see Query 1 or have turned it off by accident, click the symbol directly below File in the top left. This will open a new tab.
    b. Creating your database. Enter the code below. Note: do not run it yet.
  ```
  drop database if exists game276[username]; -- replace [username] with your username. This line deletes the DB by that name if one already exists.
  create database game276[username]; -- this creates a new DB.
  use game276[username]; -- makes sure the rest of commands will be executed in this DB.
  ```  
    c. Creating some tables. Enter this code and, afterwards, run the script. To do this, click the lightning bolt icon right above your texts. Note that if part of your script is selected, then only that part will execute.
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
    d. Adding some test records. Erase all your previous code from the query. (You just ran it and you don't want to run the same thing twice.) Write:
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
    If all runs correctly, some tabs will pop up with results. One will contain the Game records, and the other will have the Score records.
    
**3. Connect the library with the SQL related classes.** 

    a. Download mysql-connector-java-8.0.21.jar from Canvas.
    b. I don't know if this is necessary, but I put the JAR in my project src folder (in the same directory as my other classes) and extracted it. You probably don't have to do this.
    c. Connect the JAR to your project by adding it to the classpath. Look up how do this. This link gives instructions for Eclipse users: https://www.geeksforgeeks.org/how-to-add-jar-file-to-classpath-in-java/. 

**4. Set up a SQL connection in your Java project. (If you have made it this far without commiting suicide, consider doing it now.)**
    
    a. Download demoRdb.java from Canvas. This file will serve as a guideline. I suggest copy-and-pasting the code into a new class called something like "SqlConnector".
    b. Edit the code to be about your database. I have not fully figured this out. Unless you have previous experience with this process, trying to a send a command through to the server will probably take you a while. (I have spent several hours on it so far.) 
    c. By the end, you should be able to record the player names, turn scores, and total scores during the game and insert them into the DB using the SQLConnector. 
    
Suggestions for Step 4. 

    1. Test incrementally. For example,
        Compile,
        Establish a connection with the server,
        Send a simple insert command for the Game table, using hard-coded values, 
        Send an insert command for the Game table, but using variables,
        Insert into the Score table,
        Connect your SQL class to the model,
        Etc.        
        
    2. Take some time to understand the demoRdb.java code, it should save you a lot of debugging time in the long run. (Personally, I am too lazy to do this, but it would probably help.)
    
    3. Convert to a major world religion.
    4. Play Half-Life.
    5. Download a new IDE. 
    6. Switch to a major in Business Analytics.
    7. Learn about reversible computing (i think its cool).
    8. Watch Blade Runner 2049. 
    
*Note: I present one of many ways to set up and test your database. My code should give you an idea of what the process is like, but you should be able to write something similar on your own. Referencing the files in Rdb-create.zip on Canvas should help. I do not authorize the direct copying of my code.
