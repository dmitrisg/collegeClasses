How to execute an insert statment from your Java game:
  1. Make sure you have the DB and the JAR set up, as described in the SQL Guide.
  2. Create a SQLConnector class.
  3. Make an instance variable of type Connection. (In this code, I call the variable dbConnection.) It can be null for now.
  4. Create a method with a try/catch block. 
  5. "Register the driver": 
```Class.forName("com.mysql.cj.jdbc.Driver").getDeclaredConstructor().newInstance();```
  7. Plug your DB name, username, and password into the code below. 
```dbConnection = DriverManager.getConnection("jdbc:mysql://cscmysql.lemoyne.edu/game276[username]?user=[username]&password=[password]");```
  9. Make a String with your SQL insert command, replacing the values with question marks. For example: "INSERT INTO GAME VALUES (?,?,?);"
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
