Ref: 
https://www.descope.com/learn/post/password-authentication


## What is Password-Based Authentication?
Password-based authentication is a method that requires the user to enter their credentials — username and password — in order to confirm their identity. Once credentials are entered, they are compared against the stored credentials in the system's database, and the user is only granted access if the credentials match. 

Passwords are a knowledge factor i.e. something only the user knows.


### What is a password?
Passwords are a sequence of characters (letters, numerals, special characters) that are used to authenticate a user's identity and grant access to a system, application, or device. They are typically created by the user and kept confidential.


### How password authentication works
Password-based authentication is intuitive for users: they enter the right credentials and they’re granted access to a page or service. On the back end, however, there are a few more technical steps to authentication than users see on the login page. 

Most password-based authentication systems follow a process in which:

The user creates an account by providing a unique identifier such as email, username, or phone number.

The user is prompted to create a password, which usually must meet certain complexity requirements.

The set of credentials is stored in the system’s database, usually in an encrypted form to protect against data breaches.

When a user tries to log in, the authentication system checks their submitted credentials against those stored in its database.

If they match, the user is granted access.

If they don’t match, the user will be denied entry and may be prompted to reenter their information or reset their password in case they forgot it. 



##### In essence, passwords are fast and familiar, but that familiarity comes at the price of security and user experience. If you’re considering implementing password-based authentication in your app, it should be done as part of a multi-factor authentication (MFA) process.


### How to implement password-based authentication
Password-based authentication’s simplicity makes it a popular choice for website and app developers. Implementing password-based login credentials on your platform can be done in a few steps.

#### Determine password requirements
Setting the guidelines for password structure dictates how easy it might be for cybercriminals to compromise your users’ accounts later. As such, it’s smart to mandate a handful of rules for secure passwords, such as:

- 12 or more characters

- Uppercase letters

- Lowercase letters

- Special characters

- Numbers

- Words that can’t be found in the dictionary 

#### Secure credential storage
Storing passwords securely is an essential part of the implementation process of password-based authentication. When a user creates a password, it is important to store it in a way that protects it from unauthorized access or theft.

A common way to securely store passwords is with hashing. Hashing is a process of converting a password into a unique string of characters, known as a "hash", that is not reversible. This means that even if an attacker gains access to the database of hashed passwords, they cannot retrieve the original passwords.

Another technique that can be used in conjunction with hashing is salting. Salting involves adding a random string of characters to each password before it is hashed. This makes it even more difficult for attackers to crack passwords through brute-force attacks.

#### Establish your password reset process
A failsafe for forgotten passwords and compromised accounts is essential to keep users connected to your application. Hence, it is important to ensure that the password reset process is secure and that it cannot be used by attackers to enact account takeover. 

This can be done by implementing security measures such as sending password reset links or codes only to the user's registered email or mobile phone number, using CAPTCHAs to prevent automated attacks, and limiting the number of password reset attempts. 

#### Add extra layers of protection
Passwords are convenient, but they shouldn’t be the only means of authentication. Implementing at least one more authentication step, such as biometrics or OTPs, immensely increases login security. 

It has been found that implementing MFA on your app:

Blocks 99.9% of automated cyber attacks

Prevents 76% of targeted hacking attempts

Gives an added layer of security to users whose credentials have been affected by data breaches and compromised accounts