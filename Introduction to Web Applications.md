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

| Category                         | Description                                                                                                                                                                                                                                                          |
| -------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Web Application Infrastructure` | Describes the structure of required components, such as the database, needed for the web application to function as intended. Since the web application can be set up to run on a separate server, it is essential to know which database server it needs to access. |
| `Web Application Components`     | The components that make up a web application represent all the components that the web application interacts with. These are divided into the following three areas: `UI/UX`, `Client`, and `Server` components.                                                    |
| `Web Application Architecture`   | Architecture comprises all the relationships between the various web application components.                                                                                                                                                                         |
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

| Layer                | Description                                                                                                                                                                                                         |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Presentation Layer` | Consists of UI process components that enable communication with the application and the system. These can be accessed by the client via the web browser and are returned in the form of HTML, JavaScript, and CSS. |
| `Application Layer`  | This layer ensures that all client requests (web requests) are correctly processed. Various criteria are checked, such as authorization, privileges, and data passed on to the client.                              |
| `Data Layer`         | The data layer works closely with the application layer to determine exactly where the required data is stored and can be accessed.                                                                                 |
### Common Web Application Architectures

#### ASP.NET Core Architecture

![.NET Core Architecture](https://docs.microsoft.com/en-us/dotnet/architecture/modern-web-apps-azure/media/image5-12.png)
[Common Web Application Architectures from Microsoft](https://learn.microsoft.com/en-us/dotnet/architecture/modern-web-apps-azure/common-web-application-architectures)

[Microservices-on-AWS](https://d1.awsstatic.com/whitepapers/microservices-on-aws.pdf)

### Microservices

We think of microservices as independent components of web application, which are mostly programmed for only one task. For example, for an online store, we can decompose core tasks into the following components:

- Registration
- Search
- Payments
- Ratings
- Reviews

These components communicate with the client and with each other. The communication between these microservices is `stateless`, which means that the request and response are independent. This is because the stored data is `stored separately` from the respective microservices. The use of microservices is considered [service-oriented architecture (SOA)](https://en.wikipedia.org/wiki/Service-oriented_architecture), built as a collection of different automated functions focused on a single business goal. Some benefits of microservices are: 

- Agility
- Resilience
- Flexible Scaling
- Easy Deployment
- Reusability

### Serverless

Cloud platforms such AWS, GCP, Azure, and others, offer serverless architecture. These platforms provide the applications frameworks to build web applications without having to worry about servers. These web applications are then run in stateless computing containers( such as Docker ). This type of architecture gives a company the flexibility to build and deploy applications and services without having to manage infrastructure; all server management is done by the cloud provider, which gets rid of the need to provision, scale, and maintain servers needed to run applications and databases.

More information about serverless computing and its various use cases can be found [here](https://aws.amazon.com/serverless).

### Architecture Security

An individual web application's vulnerability may not necessarily be caused by a programming error but by a design error in its architecture. For example, an individual web application may have all of its core functionality secure implemented. However, due to a lack of proper access control measures in its design, i.e., use of [Role-Based Access Control(RBAC)](https://en.wikipedia.org/wiki/Role-based_access_control), users may be able to access some admin features that are not intended to be directly accessible to them or even access other user's private information without having the privileges to do so.

Another example would be if we cannot find the database after exploiting a vulnerability and gaining control over the back-end server, which may mean that the database is hosted on a separate server. We may only find part of the database data, which may mean there are several databases in use.
# Front End vs Back End

The terms Front and Back End web development or Full Stack web development are becoming synonymous with web application development, as they comprise the majority of the web development cycle. 

## Front End

The front end of a web application contains the user's components directly through their web browser (client-side). These components make up the source code of the web page we view when visiting a web application and usually include `HTML`, `CSS`, and `JavaScript`, which is then interpreted in real-time by our browsers.

In modern web applications, front end components should adapt to any screen size and work within any browser on any device. This contrasts with back end components, which are usually written for a specific platform or operating system.

## Back End

The back end of a web application drives all of the core web application functionalities, all of which is executed at the back end server, which processes everything required for the web application to run correctly. There are four main back end components for web applications:

| **Component**            | **Description**                                                                                                                                                                                                              |
| ------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Back end Servers`       | The hardware and operating system that hosts all other components and are usually run on operating systems like `Linux`, `Windows`, or using `Containers`.                                                                   |
| `Web Servers`            | Web servers handle HTTP requests and connections. Some examples are `Apache`, `NGINX`, and `IIS`.                                                                                                                            |
| `Databases`              | Databases (`DBs`) store and retrieve the web application data. Some examples of relational databases are `MySQL`, `MSSQL`, `Oracle`, `PostgreSQL`, while examples of non-relational databases include `NoSQL` and `MongoDB`. |
##### Back End Architecture

![Back End Architecture](https://academy.hackthebox.com/storage/modules/75/backend-server.jpg)

It is also possible to host each component of the back end on its own isolated server, or in isolated containers, by utilizing services such as [Docker](https://www.docker.com/). To maintain logical separation and mitigate the impact of vulnerabilities, different components of the web application, such as the database, can be installed in one Docker container, while the main web application is installed in another, thereby isolating each part from potential vulnerabilities that may affect the other container(s).

Some of the main jobs performed by back end components include:

- Develop the main logic and services of the back end of the web application
- Develop the main code and functionalities of the web application
- Develop and maintain the back end database
- Develop and implement libraries to be used by the web application
- Implement technical/business needs for the web application
- Implement the main [APIs](https://en.wikipedia.org/wiki/API) for front end component communications
- Integrate remote servers and cloud services into the web application

## Securing Front/Back End

Suppose there is a search function in a web application that mistakenly does not process our search queries correctly. In that case, we could use specific techniques to manipulate the queries in such a way that we gain unauthorized access to specific database data [SQL injections](https://www.w3schools.com/sql/sql_injection.asp) or even execute operating system commands via the web application, also known as [Command Injections](https://owasp.org/www-community/attacks/Command_Injection). When we have full access to the source code of front end components, we can perform a code review to find vulnerabilities, which is part of what is referred to as [Whitebox Pentesting](https://en.wikipedia.org/wiki/White-box_testing). On the other hand, back end components' source code is stored on the back end server, so we do not have access to it by default, which forces us only to perform what is known as [Blackbox Pentesting](https://en.wikipedia.org/wiki/Black-box_testing)
[Local File Inclusion](https://owasp.org/www-project-web-security-testing-guide/v42/4-Web_Application_Security_Testing/07-Input_Validation_Testing/11.1-Testing_for_Local_File_Inclusion) could allow us to obtain the source code from the back end server. With this source code in hand, we can then perform a code review on back end components to further understand how the application works, potentially find sensitive data in the source code (such as passwords), and even find vulnerabilities that would be difficult or impossible to find without access to the source code.

The `top 20` most common mistakes web developers make that are essential for us as penetration testers are:

|**No.**|**Mistake**|
|---|---|
|`1.`|Permitting Invalid Data to Enter the Database|
|`2.`|Focusing on the System as a Whole|
|`3.`|Establishing Personally Developed Security Methods|
|`4.`|Treating Security to be Your Last Step|
|`5.`|Developing Plain Text Password Storage|
|`6.`|Creating Weak Passwords|
|`7.`|Storing Unencrypted Data in the Database|
|`8.`|Depending Excessively on the Client Side|
|`9.`|Being Too Optimistic|
|`10.`|Permitting Variables via the URL Path Name|
|`11.`|Trusting third-party code|
|`12.`|Hard-coding backdoor accounts|
|`13.`|Unverified SQL injections|
|`14.`|Remote file inclusions|
|`15.`|Insecure data handling|
|`16.`|Failing to encrypt data properly|
|`17.`|Not using a secure cryptographic system|
|`18.`|Ignoring layer 8|
|`19.`|Review user actions|
|`20.`|Web Application Firewall misconfigurations|

These mistakes lead to the [OWASP Top 10](https://owasp.org/www-project-top-ten/) vulnerabilities for web applications, which are as follows:

|**No.**|**Vulnerability**|
|---|---|
|`1.`|Broken Access Control|
|`2.`|Cryptographic Failures|
|`3.`|Injection|
|`4.`|Insecure Design|
|`5.`|Security Misconfiguration|
|`6.`|Vulnerable and Outdated Components|
|`7.`|Identification and Authentication Failures|
|`8.`|Software and Data Integrity Failures|
|`9.`|Security Logging and Monitoring Failures|
|`10.`|Server-Side Request Forgery (SSRF)|

# Front End Components

The Front End components of a web application consists of:
## HTML

HTML contains each page's basic elements, including titles, forms, images, and many other elements. The web browser, in turn, interprets these elements and displays them to the end-user.

### HTML Structure

```shell-session
document
 - html
   -- head
      --- title
   -- body
      --- h1
      --- p
```

The following is a very basic example of an HTML page:

#### Example

```html
<!DOCTYPE html>
<html>
    <head>
        <title>Page Title</title>
    </head>
    <body>
        <h1>A Heading</h1>
        <p>A Paragraph</p>
    </body>
</html>
```

Each HTML element is opened and closed with a tag that specifies the element's type 'e.g. `<p>` for paragraphs', where the content would be placed between these tags. Tags may also hold the element's id or class 'e.g. `<p id='para1'>` or `<p id='red-paragraphs'>`', which is needed for CSS to properly format the element. Both tags and the content comprise the entire element.

### URL Encoding

 For a browser to properly display a page's contents, it has to know the charset in use. In URLs, for example, browsers can only use [ASCII](https://en.wikipedia.org/wiki/ASCII) encoding, which only allows alphanumerical characters and certain special characters. Therefore, all other characters outside of the ASCII character-set have to be encoded within a URL. URL encoding replaces unsafe ASCII characters with a `%` symbol followed by two hexadecimal digits.

The single-quote character '`'`' is encoded to '`%27`', which can be understood by browsers as a single-quote. URLs cannot have spaces in them and will replace a space with either a `+` (plus sign) or `%20`. Some common character encodings are:

| Character | Encoding |
| --------- | -------- |
| space     | %20      |
| !         | %21      |
| "         | %22      |
| #         | %23      |
| $         | %24      |
| %         | %25      |
| &         | %26      |
| '         | %27      |
| (         | %28      |
| )         | %29      |

### Usage

The `<head>` element usually contains elements that are not directly printed on the page, like the page title, while all main page elements are located under `<body>`. Other important elements include the `<style>`, which holds the page's CSS code, and the `<script>`, which holds the JS code of the page. Each of these elements is called a [DOM (Document Object Model)](https://en.wikipedia.org/wiki/Document_Object_Model).

The DOM standard is separated into 3 parts:

- `Core DOM` - the standard model for all document types
- `XML DOM` - the standard model for XML documents
- `HTML DOM` - the standard model for HTML documents

Understanding the HTML DOM structure can help us understand where each element we view on the page is located, which enables us to view the source code of a specific element on the page and look for potential issues. We can locate HTML elements by their id, their tag name, or by their class name. This is also useful when we want to utilize front-end vulnerabilities (like `XSS`) to manipulate existing elements or create new elements to serve our needs.

## Cascading Style Sheets (CSS)

[CSS (Cascading Style Sheets)](https://www.w3.org/Style/CSS/Overview.en.html) is the stylesheet language used alongside HTML to format and set the style of HTML elements. Like HTML, there are several versions of CSS, and each subsequent version introduces a new set of capabilities that can be used for formatting HTML elements.

##### Example

At a fundamental level, CSS is used to define the style of each class or type of HTML elements (i.e., `body` or `h1`), such that any element within that page would be represented as defined in the CSS file. This could include the font family, font size, background color, text color and alignment, and more.


```css
body {
  background-color: black;
}

h1 {
  color: white;
  text-align: center;
}

p {
  font-family: helvetica;
  font-size: 10px;
}
```

### Syntax

CSS defines the style of each HTML element or class between curly brackets `{}`, within which the properties are defined with their values (i.e. `element { property : value; }`).

Each HTML element has many properties that can be set through CSS, such as `height`, `position`, `border`, `margin`, `padding`, `color`, `text-align`, `font-size`, and hundreds of other properties. All of these can be combined and used to design visually appealing web pages.

CSS can be used for advanced animations for a wide variety of uses, from moving items all the way to advanced 3D animations. Many CSS properties are available for animations, like `@keyframes`, `animation`, `animation-duration`, `animation-direction`, and many others. You can read about and try out many of these animation properties [here](https://www.w3schools.com/css/css3_animations.asp).

CSS can be used alongside other languages to implement their styles, like `XML` or within `SVG` items, and can also be used in modern mobile development platforms to design entire mobile application User Interfaces (UI).

### Frameworks

CSS frameworks have been introduced, which contain a collection of CSS style-sheets and designs, to make it much faster and easier to create beautiful HTML elements.  They are designed to be used with JavaScript and for wide use within a web application and contain elements usually required within modern web applications. Some of the most common CSS frameworks are:

- [Bootstrap](https://www.w3schools.com/bootstrap4/)
- [SASS](https://sass-lang.com/)
- [Foundation](https://en.wikipedia.org/wiki/Foundation_(framework))
- [Bulma](https://bulma.io/)
- [Pure](https://purecss.io/)

## JavaScript

`JavaScript` is usually used on the front end of an application to be executed within a browser. While `HTML` and `CSS` are mainly in charge of how a web page looks, `JavaScript` is usually used to control any functionality that the front end web page requires. Without `JavaScript`, a web page would be mostly static and would not have much functionality or interactive elements.

##### Example

Within the page source code, `JavaScript` code is loaded with the `<script>` tag, as follows:

```html
<script type="text/javascript">
..JavaScript code..
</script>
```

A web page can also load remote `JavaScript` code with `src` and the script's link, as follows:

```html
<script src="./script.js"></script>
```

An example of basic use of `JavaScript` within a web page is the following:

```javascript
document.getElementById("button1").innerHTML = "Changed Text!";
```

The above example changes the content of the `button1` HTML element. From here on, there are many more advanced uses of `JavaScript` on a web page.

### Usage

Most common web applications heavily rely on `JavaScript` to drive all needed functionality on the web page, like updating the web page view in real-time, dynamically updating content in real-time, accepting and processing user input, and many other potential functionalities. `JavaScript` is also used to automate complex processes and perform HTTP requests to interact with the back end components and send and retrieve data, through technologies like [Ajax](https://en.wikipedia.org/wiki/Ajax_(programming)).

All modern web browsers are equipped with `JavaScript` engines that can execute `JavaScript` code on the client-side without relying on the back end webserver to update the page. This makes using `JavaScript` a very fast way to achieve a large number of processes quickly.

### Frameworks

Some of the most common front end `JavaScript` frameworks are:

- [Angular](https://www.w3schools.com/angular/angular_intro.asp)
- [React](https://www.w3schools.com/react/react_intro.asp)
- [Vue](https://www.w3schools.com/whatis/whatis_vue.asp)
- [jQuery](https://www.w3schools.com/jquery/)

A listing and comparison of common JavaScript frameworks can be found [here](https://en.wikipedia.org/wiki/Comparison_of_JavaScript_frameworks).