# Flaws in Web Applications

**SQL Injection:** Obtaining Active Directory usernames and performing a password spraying attack against a VPN or email portal.

**File Inclusion:** Reading source code to find a hidden page or directory which exposes additional functionality that can be used to gain remote code execution.

**Unrestricted File Upload:** A web application that allows a user to upload a profile picture that allows any file type to be uploaded (not just images). This can be leveraged to gain full control of the web application server by uploading malicious code.

**Insecure Direct Object Referencing (IDOR):** When combined with a flaw such as broken access control, this can often be used to access another user's files or functionality. An example would be editing your user profile browsing to a page such as /user/701/edit-profile. If we can change the `701` to `702`, we may edit another user's profile!

**Broken Access Control:** Another example is an application that allows a user to register a new account. If the account registration functionality is designed poorly, a user may perform privilege escalation when registering. Consider the `POST` request when registering a new user, which submits the data `username=bjones&password=Welcome1&email=bjones@inlanefreight.local&roleid=3`. What if we can manipulate the `roleid` parameter and change it to `0` or `1`. We have seen real-world applications where this was the case, and it was possible to quickly register an admin user and access many unintended features of the web application.

**Password Spraying Attack:** A **password spraying attack** is a type of brute-force attack where an attacker attempts to gain unauthorized access to user accounts by trying a **small number of common or weak passwords** (like "123456", "password", or "welcome") across many different accounts, rather than focusing on one account with multiple password guesses.

### How It Works:

1. **Identify a List of Usernames**: The attacker gathers a list of usernames, often through open-source intelligence (OSINT), data breaches, or phishing.
2. **Attempt a Common Password**: Instead of guessing many different passwords for a single user (which would quickly lock out the account due to security mechanisms like account lockout policies), the attacker tries the same common password (e.g., "password123") across multiple accounts.
3. **Repeat with Other Passwords**: Once the attacker has tried one password across all accounts, they move on to the next password in their list and repeat the process. 

# Layers in Web Applications Layout

|   |   |
|---|---|
|`Web Application Infrastructure`|Describes the structure of required components, such as the database, needed for the web application to function as intended. Since the web application can be set up to run on a separate server, it is essential to know which database server it needs to access.|
|`Web Application Components`|The components that make up a web application represent all the components that the web application interacts with. These are divided into the following three areas: `UI/UX`, `Client`, and `Server` components.|
|`Web Application Architecture`|Architecture comprises all the relationships between the various web application components.|
# Web Application Infrastructure

The different setups in web applications are called models. They are divided into four categories:

- `Client-Server`
- `One Server`
- `Many Servers - One Database`
- `Many Servers - Many Databases`

### Client Server

A server hosts the web application and distributes across any clients trying to access it. In this model, the web applications have two components, Front-End (interpreted and executed on the client-side i.e. browser) and Back-End (compiled, interpreted and executed by the hosting server).

![Client-Server Architecture](https://academy.hackthebox.com/storage/modules/75/client-server-model.jpg)

### One Server

The entire or even several web applications and their components, including the database are hosted on a single hosted server. It is the riskiest design. If the webserver goes down for any reason, all hosted web applications become entirely inaccessible until the issue is resolved.

One Server design represents an "`all eggs in one basket`" approach since if any of the hosted web applications are vulnerable, the entire webserver becomes vulnerable.

![One Server](https://academy.hackthebox.com/storage/modules/75/one-server-arch.jpg)

### Many Servers - One Database

The database is separated onto its own database server and allows the web applications' hosting server to access the database server to store and retrieve data. This model allows several web applications to access a single database to have access to the same data without syncing the data between them. 

This model's main advantage (`from a security point of view`) is segmentation, where each of the main components of a web application is located and hosted separately. In case one webserver is compromised, other webservers are not directly affected. Similarly, if the database is compromised (i.e., through a SQL injection vulnerability), the web application itself is not directly affected.

![Many Servers-One Database](https://academy.hackthebox.com/storage/modules/75/many-server-one-db-arch.jpg)

### Many Servers - Many Databases

 Within the database server, each web application's data is hosted in a separate database. The web application can only access private data and only common data that is shared across web applications. It is also possible to host each web application's database on its separate database server.

This design is also widely used for redundancy purposes, so if any web server or database goes offline, a backup will run in its place to reduce downtime as much as possible. 

![Many Servers-Many Databases](https://academy.hackthebox.com/storage/modules/75/many-server-many-db-arch.jpg)

# Web Application Components

All of the components can be broken down to:

1. Clients
2. Server
	1. Web Server
	2. Web Application Logic
	3. Database
3. Services (Microservices)
	1. Third Party Integrations
	2. Web Applications Integrations
4. Functions (Serverless)

# Web Application Architecture

The components of a web application are divided into three different layers (AKA Three Tier Architecture).

|   |   |
|---|---|
|`Presentation Layer`|Consists of UI process components that enable communication with the application and the system. These can be accessed by the client via the web browser and are returned in the form of HTML, JavaScript, and CSS.|
|`Application Layer`|This layer ensures that all client requests (web requests) are correctly processed. Various criteria are checked, such as authorization, privileges, and data passed on to the client.|
|`Data Layer`|The data layer works closely with the application layer to determine exactly where the required data is stored and can be accessed.|
