# SQL injections
#whatis basically its messing with SQL 
``$sql="SELECT * FROM users WHERE username='$username' AND password='$password'";
`# Query for user/pass retrieval from the DB.`

since the above logic is to check  the user name with the respective password 

simple SQL injection 
this logic is very nessesary 
```
admin '; #  & password 
```

`'` this symbol simply make the sql to end user name input 
`#` this symbol make the rest of the sql line a comment 
`;` this symbol make the existing sql line to end 

# if mariadb or or any sql is open when scan

use
`mysql -h host name -r user name `
