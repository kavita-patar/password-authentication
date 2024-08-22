When dealing with password authentication and hashing in Node.js, there are several methods and libraries you can use to ensure secure handling of passwords. Here's a list of popular ones:

### **Libraries for Password Hashing and Authentication**

1. **bcrypt**
   - **Description**: A widely-used library for hashing passwords. It provides a strong and secure hashing algorithm.
   - **Usage**:
     ```bash
     npm install bcrypt
     ```
     ```javascript
     const bcrypt = require('bcrypt');
     
     // Hash a password
     bcrypt.hash('password', 10, (err, hash) => {
       // Store hash in your password DB.
     });

     // Compare a password with a hash
     bcrypt.compare('password', hash, (err, result) => {
       if (result) {
         // Passwords match
       } else {
         // Passwords do not match
       }
     });
     ```

2. **argon2**
   - **Description**: A modern hashing algorithm that is considered very secure and is recommended by experts.
   - **Usage**:
     ```bash
     npm install argon2
     ```
     ```javascript
     const argon2 = require('argon2');

     // Hash a password
     argon2.hash('password').then(hash => {
       // Store hash in your password DB.
     }).catch(err => {
       // Handle error
     });

     // Verify a password
     argon2.verify(hash, 'password').then(match => {
       if (match) {
         // Passwords match
       } else {
         // Passwords do not match
       }
     }).catch(err => {
       // Handle error
     });
     ```

3. **pbkdf2**
   - **Description**: A built-in Node.js module for password hashing based on the PBKDF2 algorithm.
   - **Usage**:
     ```javascript
     const crypto = require('crypto');

     // Hash a password
     crypto.pbkdf2('password', 'salt', 100000, 64, 'sha512', (err, derivedKey) => {
       if (err) throw err;
       // Store derivedKey in your password DB.
     });

     // Compare a password with a stored hash
     // You need to perform the same hashing process with the provided password and compare it with the stored hash.
     ```

4. **scrypt**
   - **Description**: Another built-in Node.js module for hashing passwords using the scrypt algorithm.
   - **Usage**:
     ```javascript
     const crypto = require('crypto');

     // Hash a password
     crypto.scrypt('password', 'salt', 64, (err, derivedKey) => {
       if (err) throw err;
       // Store derivedKey in your password DB.
     });

     // Compare a password with a stored hash
     // Similarly, you need to hash the provided password and compare it with the stored hash.
     ```

### **Additional Libraries for Authentication**

1. **Passport.js**
   - **Description**: Middleware for Node.js that can be used to handle authentication strategies, including local username/password authentication.
   - **Usage**:
     ```bash
     npm install passport passport-local
     ```
     ```javascript
     const passport = require('passport');
     const LocalStrategy = require('passport-local').Strategy;

     passport.use(new LocalStrategy(
       (username, password, done) => {
         // Fetch user from database and compare password
       }
     ));
     ```

2. **jsonwebtoken (JWT)**
   - **Description**: A library for generating and verifying JSON Web Tokens (JWTs) which can be used for authentication.
   - **Usage**:
     ```bash
     npm install jsonwebtoken
     ```
     ```javascript
     const jwt = require('jsonwebtoken');

     // Sign a token
     const token = jwt.sign({ userId: 123 }, 'your-very-secure-secret', { expiresIn: '1h' });

     // Verify a token
     jwt.verify(token, 'your-very-secure-secret', (err, decoded) => {
       if (err) {
         // Token is invalid or expired
       } else {
         // Token is valid
       }
     });
     ```

3. **OAuth2 Libraries (e.g., `oauth2-server`, `simple-oauth2`)**
   - **Description**: Libraries to implement OAuth2 authentication.
   - **Usage**:
     ```bash
     npm install oauth2-server
     ```
     ```javascript
     const OAuth2Server = require('oauth2-server');
     const server = new OAuth2Server({
       model: {}, // Implement your model for handling tokens, etc.
     });

     // Authenticate a request
     server.authenticate(request, response)
       .then(token => {
         // Token is valid
       })
       .catch(err => {
         // Token is invalid
       });
     ```

These libraries and methods can be combined or used separately depending on the security requirements and architecture of your application. Always ensure you keep your dependencies up-to-date and follow best practices for security.