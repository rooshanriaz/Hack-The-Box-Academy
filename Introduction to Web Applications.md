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

