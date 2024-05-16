# FSEC_Web_Application_Hacking-SQL-INJECTION
SQL INJECTION
## Table of Contents
I. [What is SQL Injection?](#i-what-is-sql-injection)

II. [How Does a SQL Injection Attack Work?](#ii-how-does-a-sql-injection-attack-work)

III. [Impact and Actions of a Successful SQL Injection Attack](#iii-impact-and-actions-of-a-successful-sql-injection-attack)

IV. [How to Detect SQL Injection Vulnerabilities?](#iv-how-to-detect-sql-injection-vulnerabilities)

V. [Types of SQL Injection Attacks](#v-types-of-sql-injection-attacks)
  - [Union-based SQL Injection](#union-based-sql-injection)
  - [Error Based SQL Injection](#error-based-sql-injection)
  - [Blind SQL Injection](#blind-sql-injection)
     - [Boolean-based SQL Injection](#boolean-based-sql-injection)
     - [Time-based Blind SQL Injection](#time-based-blind-sql-injection)
   - [SQL injection attacks can also be classified by the method they use to inject data](#sql-injection-attacks-can-also-be-classified-by-the-method-they-use-to-inject-data)

VI. [SQL Injection Examples](#vi-sql-injection-examples)

VII. [How to Prevent SQL Injection?](#vii-how-to-prevent-sql-injection)


## I. What is SQL Injection?

`SQL Injection (SQLi)` is a form of injection attack enabling the execution of `harmful SQL commands`. These commands manipulate a database server connected to a web application. Exploiting SQL Injection vulnerabilities allows attackers to circumvent security protocols of an application. They can bypass authentication and authorization of a web page or web application, accessing the entirety of the SQL database. Additionally, SQL Injection can be utilized to `insert`, `alter`, or `delete data`, resulting in persistent modifications to the application's content or functionality. Such attacks may also lead to a `denial-of-service(DoS)` or compromise of the underlying server or other `backend systems`.
## II. How Does a SQL Injection Attack Work?

`SQL Injection` assault involves inserting or "injecting" a SQL inquiry through the client's input data into the application. A successful breach enables attackers to manipulate the SQL inquiries made by the application to its database. Typically, it involves the following attack:

1. **Identifying vulnerable inputs:** Attackers initially detect inputs within the web application susceptible to SQL injection. These inputs could be text fields within a form, URL parameters, or any other input mechanisms.

2. **Crafting the malevolent SQL inquiry:** Once a vulnerable input is recognized, attackers devise a SQL statement intended to be incorporated into the application's executed query. This statement aims to alter the original SQL inquiry to carry out actions unintended by the application developers.
    ```sql
    SELECT * FROM products WHERE name = 'product_name';
    ```

3. **Circumventing application security measures:** Attackers often need to evade security measures such as input validation or escaping special characters. They accomplish this through methods like string concatenation or utilizing SQL syntax to nullify portions of the initial query.
    ```sql
    SELECT * FROM products WHERE name = '' OR '1'='1';
    ```

4. **Executing the malevolent query:** As the application runs the SQL inquiry, it includes the attacker's malevolent input. This altered query can execute actions such as unauthorized data viewing, data deletion, or even modifications to the database schema.

5. **Extracting or altering data:** Depending on the assault, outcomes may include extracting sensitive data (like user credentials), altering existing data, adding new data, or even erasing substantial segments of the database.

6. **Exploiting database server vulnerabilities:** Advanced SQL injections may exploit vulnerabilities in the database server, extending the assault beyond the database to the server level. This can involve executing commands on the operating system or accessing other parts of the server's file system.

This procedure exploits the dynamic execution of SQL in applications where user inputs are directly incorporated into SQL statements lacking proper validation or escaping. It capitalizes on the construction of SQL inquiries, often in ways unanticipated by developers.
## III. Impact and Actions of a Successful SQL Injection Attack

A successful SQL injection attack can have severe consequences, including:

- **Unauthorized Access to Sensitive Data:** Attackers can gain access to sensitive information stored in the database, such as passwords, credit card details, and personal user information.

Actions a successful attacker may take on a compromised target include:

- **Bypassing Authentication:** Attackers can bypass authentication mechanisms, gaining unauthorized access to restricted areas of the application or system.

- **Exfiltrating/Stealing Data:** Attackers can steal valuable data from the database, which can be used for identity theft, financial fraud, or other malicious purposes.

- **Modifying or Corrupting Data:** Attackers can modify or corrupt existing data in the database, leading to data integrity issues and potentially disrupting the normal operation of the application or system.

- **Deleting Data:** Attackers can delete critical data from the database, causing data loss and potentially rendering the application or system unusable.

- **Executing Unauthorized Commands:** Attackers can execute arbitrary commands on the database server, leading to further compromise of the system or unauthorized actions.

- **Securing Full Control over Administrative Privileges:** In some cases, attackers can escalate their privileges to gain full control over the system's administrative functions, allowing them to manipulate the system at will.

Overall, the impact of a successful SQL injection attack can be devastating, resulting in financial losses, reputational damage, and legal consequences for affected organizations.
## IV. How to Detect SQL Injection Vulnerabilities?

Detecting SQL injection vulnerabilities can be done through various methods:

#### Manual Testing:

By systematically testing each entry point in the application, you can find SQL injection vulnerabilities. This involves submitting inputs such as:

- The single quote character `'` to check for errors or strange behaviors in the application's responses.
- SQL-specific syntax that gives back the original value of the entry point and a different value, watching for differences in how the application responds.
- Boolean conditions like `OR 1=1` and `OR 1=2`, and checking for differences in the application's responses.
- Payloads designed to trigger time delays in SQL queries, noting any changes in how long it takes the application to respond.
- Out-of-band (OAST) payloads intended to cause network interactions outside the application when used in SQL queries, and watching for any resulting interactions.

#### Automated Testing:

Tools like Burp Scanner can quickly and reliably find most SQL injection vulnerabilities. These tools automate the process, making it faster and easier compared to manual testing.

## V. Types of SQL Injection Attacks

There are several types of SQL injection attacks:

#### Union-based SQL Injection:

This is the most common type. It uses the UNION statement to combine two SELECT statements to fetch data from the database. For example:
```sql
SELECT name, email FROM users UNION SELECT username, password FROM admin;
```
This SQL query returns a single result set with two columns, containing values from columns `name` and `email` in `users` and columns `username` and `password` in `admin`.

#### Error Based SQL Injection:

Error-based SQL injection is a technique attackers use to exploit vulnerabilities in web applications. Unlike other methods that might directly steal data, error-based injection focuses on tricking the database server into revealing information. This information can then be used to launch further attacks. For instance:
```sql
SELECT * FROM products WHERE id = '1' AND 1=convert(int,(select @@version));
```
This SQL code executes a query from the "products" table to retrieve information about the product with an ID of '1'. The part "`AND 1=convert(int,(select @@version))`" is a component of an SQL injection technique called Error Based SQL Injection. If the query is successful, the condition "`1=convert(int,(select @@version))`" will be true, but it does not affect the result of the main SELECT statement.

#### Blind SQL Injection:

Blind SQL Injection is a type of SQL injection attack where the attacker does not directly receive any error messages or data responses from the database. Blind SQL injections can be divided into `boolean-based SQL Injection` and `time-based SQL Injection`.

- **Boolean-based SQL Injection:**
```sql
SELECT * FROM users WHERE username = 'admin' AND '1'='1';
```
The attacker is attempting to verify if the username 'admin' exists in the database. The injected condition '`1=1`' will always evaluate to true, but it doesn't affect the outcome of the query. Therefore, by observing the application's response, such as whether it returns data or not, the attacker can infer whether the username '`admin`' exists in the database or not, without directly seeing the database contents.

- **Time-based Blind SQL Injection:**
```sql
SELECT * FROM products WHERE category = 'Electronics' AND (SELECT CASE WHEN (1=1) THEN WAITFOR DELAY '0:0:5' ELSE WAITFOR DELAY '0:0:0' END);
```
The attacker is attempting to determine if the category 'Electronics' exists in the database. By injecting a time-delay function (e.g., `WAITFOR DELAY '0:0:5'`) into the query, the attacker can measure the response time of the application. If the response time is significantly longer, it indicates that the condition `(1=1)` is true, and the category 'Electronics' likely exists.

SQL injection attacks can also be classified by the method they use to inject data:

    - SQL injection based on user input
    - SQL injection based on cookies 
    - SQL injection based on HTTP headers 
## VI. SQL Injection Examples

There are numerous SQL injection vulnerabilities, attacks, and techniques that occur in different situations. Some common SQL injection examples include:

- **Retrieving Hidden Data**
- **Subverting Application Logic**
- **UNION Attacks**
- **Blind SQL Injection**

### Illustrative Example of Retrieving Hidden Data:

Consider a scenario where an e-commerce website has a section for premium users where they can access special deals and discounts on products under the category "Gifts". The website uses the following SQL query to retrieve products for premium users:
```sql
SELECT * FROM products WHERE category = 'Gifts' AND released = 1;
```
However, the attacker, who is a regular user, wants to access these special deals and discounts intended for premium users. They suspect that the website might be vulnerable to SQL injection. The attacker crafts a malicious input into the category parameter, injecting a comment symbol "`--`" to comment out the rest of the original query:
```sql
SELECT * FROM products WHERE category = 'Gifts'--' AND released = 1;
```
The injected comment symbol "`--`" effectively comments out the condition "`AND released = 1`", bypassing the requirement for the products to be released. As a result, the query retrieves all products under the category `Gifts` regardless of their release status, effectively exposing the hidden data of special deals and discounts intended only for premium users.
## VII. How to Prevent SQL Injection?

### Implement Input Validation and Sanitization:

Validate and sanitize user inputs to ensure they meet expected formats and types. Reject inputs containing suspicious characters or patterns.

**Example:** Ensure that the username input only contains alphanumeric characters:
```sql
SELECT * FROM users WHERE username REGEXP '^[a-zA-Z0-9]+$';
```

## Implement Input Validation and Sanitization:
Validate and sanitize user inputs to ensure they meet expected formats and types. Reject inputs containing suspicious characters or patterns.

1. **Implement Input Validation and Sanitization:** Validate and sanitize user inputs to ensure they meet expected formats and types. Reject inputs containing suspicious characters or patterns.
   - Example: Ensure that the username input only contains alphanumeric characters:
     ```sql
     SELECT * FROM users WHERE username REGEXP '^[a-zA-Z0-9]+$';
     ```

2. **Use Escaping for User Input:** Escape user inputs before incorporating them into SQL queries to prevent them from being interpreted as part of the SQL syntax.
   - Example: Escape single quotes in the username input:
     ```sql
     SELECT * FROM users WHERE username = 'O\'Stephen';
     ```

3. **Utilize Parameterized Statements (Prepared Statements):** Use parameterized queries or prepared statements with placeholders for user inputs, separating SQL code from data.

4. **Incorporate Stored Procedures:** Use stored procedures to encapsulate SQL logic on the database server, preventing direct manipulation of SQL queries.

5. **Conduct Continuous Scanning and Penetration Testing:** Regularly scan for vulnerabilities and conduct penetration testing to identify and address SQL injection vulnerabilities in the application code.

6. **Adopt the Least Privilege Principle:** Assign the least privilege necessary to database accounts, limiting access to specific resources and functionalities to reduce the potential impact of SQL injection attacks.

7. **Deploy Web Application Firewalls (WAF):** Implement WAFs to inspect incoming HTTP requests and block those that appear to be SQL injection attempts, providing an additional layer of defense against attacks. Database Firewall: Deploy a database firewall or intrusion detection system (IDS) to monitor and block suspicious SQL queries in real-time.

8. **Regular Security Audits:** Conduct regular security audits and vulnerability assessments to identify and address any SQL injection vulnerabilities in the application code.

9. **Error Handling:** Implement proper error handling mechanisms to provide minimal error messages to users. Avoid revealing sensitive information about the database structure or queries in error messages, which could aid attackers.

10. **Secure Configuration:** Ensure that the web application framework, database management system, and server configurations are securely configured. Disable unnecessary features and services that could potentially introduce security vulnerabilities.



