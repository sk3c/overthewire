# Natas 13 - 14

## Details
**Username:** natas14
**Password:** z3UYcr4v4uBpeX8f7EZbMHlzK4UR2XtQ
**URL:** http://natas14.natas.labs.overthewire.org
**Flag/Next Password:** SdqIqBsFcz3yotlNYErZSZwblkm0lrvx

## TL;DR
This machine contains a login page that can be bypassed using SQL injection. The source code reveals an SQL query that is executed during login.
By manipulating this query, we can bypass authentication and obtain the flag for the next machine.


## Solution

### **Step 1: Understanding the Login Process**

The login page requires a username and password. The backend login logic is visible in the source code. The process works as follows:

- A MySQL connection is established using the natas14 user.

- The submitted username and password are directly used in an SQL query.

- If the query returns a valid user, the application displays the flag.

- Otherwise, an Access denied message is returned.

In short, authentication depends entirely on the result of this SQL query.

### **Step 2: Identifying the Vulnerability**

From the source code, we can see that the SQL query uses the username and password without proper filtering or sanitization. This makes the application vulnerable to SQL injection.

To test this, we try a basic SQL injection payload:

```
' OR 1 = 1 --
```

This payload returns an error message, confirming that user input is being interpreted as part of the SQL query and that the application is vulnerable to SQL injection.


### **Step 3: Exploiting the Vulnerability**

By carefully analyzing the SQL query, we can craft a payload that fits its structure. After a few attempts, the following payload successfully bypasses authentication:

```
" OR 1 = "1
```

This condition always evaluates to true, allowing us to bypass the login check and retrieve the flag/password for the next level.
