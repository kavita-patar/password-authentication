Comprehensive example that demonstrates how to use various password authentication and hashing libraries in Node.js. We'll cover `bcrypt`, `argon2`, `pbkdf2`, `scrypt`, and integrate them with `passport.js` and `jsonwebtoken` for a complete authentication flow.

### 1. **Setup and Installation**

First, set up a new Node.js project and install the required dependencies:

```bash
mkdir auth-example
cd auth-example
npm init -y
npm install express bcrypt argon2 passport passport-local jsonwebtoken crypto
```

### 2. **Basic Setup**

Create the following files and folders:

- `index.js` (Main server file)
- `auth.js` (Authentication logic)
- `passport-config.js` (Passport configuration)
- `db.js` (Simulated database)

### 3. **`db.js`**

Simulate a simple database:

```javascript
// db.js
const users = [];

module.exports = {
  getUserByUsername: (username) =>
    users.find((user) => user.username === username),
  addUser: (user) => users.push(user),
};
```

### 4. **`passport-config.js`**

Configure Passport.js for local strategy:

```javascript
// passport-config.js
const passport = require("passport");
const LocalStrategy = require("passport-local").Strategy;
const db = require("./db");
const bcrypt = require("bcrypt");

passport.use(
  new LocalStrategy((username, password, done) => {
    const user = db.getUserByUsername(username);
    if (!user) {
      return done(null, false, { message: "Incorrect username." });
    }

    bcrypt.compare(password, user.passwordHash, (err, result) => {
      if (err) return done(err);
      if (result) return done(null, user);
      return done(null, false, { message: "Incorrect password." });
    });
  })
);

passport.serializeUser((user, done) => {
  done(null, user.username);
});

passport.deserializeUser((username, done) => {
  const user = db.getUserByUsername(username);
  done(null, user);
});
```

### 5. **`auth.js`**

Add user registration and JWT generation:

```javascript
// auth.js
const express = require("express");
const bcrypt = require("bcrypt");
const argon2 = require("argon2");
const jwt = require("jsonwebtoken");
const crypto = require("crypto");
const passport = require("passport");
const db = require("./db");
const passportConfig = require("./passport-config");

const router = express.Router();

// Register route with bcrypt
router.post("/register-bcrypt", async (req, res) => {
  const { username, password } = req.body;
  const passwordHash = await bcrypt.hash(password, 10);
  db.addUser({ username, passwordHash });
  res.send("User registered with bcrypt");
});

// Register route with argon2
router.post("/register-argon2", async (req, res) => {
  const { username, password } = req.body;
  const passwordHash = await argon2.hash(password);
  db.addUser({ username, passwordHash });
  res.send("User registered with argon2");
});

// Register route with pbkdf2
router.post("/register-pbkdf2", (req, res) => {
  const { username, password } = req.body;
  crypto.pbkdf2(password, "salt", 100000, 64, "sha512", (err, derivedKey) => {
    if (err) throw err;
    db.addUser({ username, passwordHash: derivedKey.toString("hex") });
    res.send("User registered with pbkdf2");
  });
});

// Register route with scrypt
router.post("/register-scrypt", (req, res) => {
  const { username, password } = req.body;
  crypto.scrypt(password, "salt", 64, (err, derivedKey) => {
    if (err) throw err;
    db.addUser({ username, passwordHash: derivedKey.toString("hex") });
    res.send("User registered with scrypt");
  });
});

// Login route
router.post(
  "/login",
  passport.authenticate("local", { session: false }),
  (req, res) => {
    const user = req.user;
    const token = jwt.sign(
      { username: user.username },
      "your-very-secure-secret",
      { expiresIn: "1h" }
    );
    res.json({ token });
  }
);

module.exports = router;
```

### 6. **`index.js`**

Set up the Express server and integrate routes:

```javascript
// index.js
const express = require("express");
const passport = require("passport");
const session = require("express-session");
const bodyParser = require("body-parser");
const authRoutes = require("./auth");

const app = express();

app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));
app.use(
  session({
    secret: "your-session-secret",
    resave: false,
    saveUninitialized: false,
  })
);
app.use(passport.initialize());
app.use(passport.session());

app.use("/auth", authRoutes);

app.listen(3000, () => {
  console.log("Server started on http://localhost:3000");
});
```

### 7. **Testing**

You can now test the API with tools like Postman or cURL:

1. **Register User** (with bcrypt, argon2, pbkdf2, or scrypt):

   ```bash
   POST http://localhost:3000/auth/register-bcrypt
   Content-Type: application/json
   Body: {"username": "user", "password": "password"}
   ```

2. **Login User**:

   ```bash
   POST http://localhost:3000/auth/login
   Content-Type: application/json
   Body: {"username": "user", "password": "password"}
   ```

   You should receive a JWT token upon successful login.

### Notes

- **Security**: Ensure you handle sensitive data (like passwords and secrets) securely and follow best practices.
- **Environment Variables**: In a real application, store secrets and configurations in environment variables rather than hard-coding them.
- **Error Handling**: Add proper error handling for production-level applications.

This example integrates various password hashing methods and demonstrates a simple authentication flow using Passport.js and JWT.
