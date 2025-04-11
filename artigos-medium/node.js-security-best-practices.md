---
icon: key
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Node.js Security Best Practices

Node.js applications are often exposed to security risks such as **injection attacks, insecure dependencies, and misconfigurations**. To protect your applications, follow these **best security practices**:

<figure><img src="https://miro.medium.com/v2/resize:fit:700/1*fwSUroKlOsPr1oYvwn8btA.png" alt="" height="558" width="700"><figcaption></figcaption></figure>

## ✅ Final Security Checklist <a href="#id-92be" id="id-92be"></a>

> ✔ Keep dependencies updated (`npm audit`)\
> ✔ Use environment variables (`dotenv`)\
> ✔ Sanitize user input (`express-validator`, `joi`)\
> ✔ Implement authentication securely (`bcrypt`, JWT, OAuth)\
> ✔ Prevent XSS (`helmet`)\
> ✔ Use rate limiting (`express-rate-limit`)\
> ✔ Secure API access (`cors`, RBAC)\
> ✔ Prevent directory traversal (`path.join()`)\
> ✔ Always use HTTPS and secure cookies

## 1️⃣ Keep Dependencies Up to Date <a href="#id-2f19" id="id-2f19"></a>

* Use `npm audit` to check for vulnerabilities:

```
npm audit
```

* Use `npm outdated` to check for dependency updates.
* Regularly update packages with:

```
npm update
```

✅ **Best Practice**: Use **npm audit**, **Snyk**, or **Dependabot** to detect vulnerabilities.

## 2️⃣ Avoid Using `eval()` and Other Dangerous Functions <a href="#df18" id="df18"></a>

Functions like `eval()`, `setTimeout()`, and `setInterval()` with user input can lead to **code injection attacks**.

🚨 **Bad Practice**:

```
let userInput = "console.log('Hacked!')";
eval(userInput); // Executes arbitrary code!
```

✅ **Best Practice**: Avoid `eval()` and use `JSON.parse()` or other safe alternatives.

## 3️⃣ Use Environment Variables for Sensitive Data <a href="#id-0879" id="id-0879"></a>

Never hardcode credentials, API keys, or database passwords in your source code.

🚨 **Bad Practice**:

```
const dbPassword = "supersecret123"; // ❌ Hardcoded password
```

✅ **Best Practice**: Use `.env` files and load them with `dotenv`:

```
# .env file
DB_PASSWORD=supersecret123
```

```
require("dotenv").config();
const dbPassword = process.env.DB_PASSWORD;
```

## 4️⃣ Sanitize User Input to Prevent Injection Attacks <a href="#id-473d" id="id-473d"></a>

User input should always be validated and sanitized to prevent **SQL Injection, NoSQL Injection, and XSS attacks**.

✅ **Best Practice**: Use libraries like `express-validator` or `joi`:

```
const { body, validationResult } = require("express-validator");

app.post("/register", [
  body("email").isEmail(),
  body("password").isLength({ min: 8 }),
], (req, res) => {
  const errors = validationResult(req);
  if (!errors.isEmpty()) {
    return res.status(400).json({ errors: errors.array() });
  }
});
```

## 5️⃣ Implement Proper Authentication and Authorization <a href="#id-9bcd" id="id-9bcd"></a>

* Use **OAuth, JWT, or session-based authentication** instead of rolling your own authentication.
* Hash passwords securely using `bcrypt`:

```
const bcrypt = require("bcrypt");
const hashedPassword = await bcrypt.hash(password, 10);
```

✅ **Best Practice**: Use **role-based access control (RBAC)** to limit user permissions.

## 6️⃣ Protect Against Cross-Site Scripting (XSS) <a href="#id-2c81" id="id-2c81"></a>

XSS occurs when an attacker injects malicious scripts into a webpage.

✅ **Best Practice**: Escape user input using libraries like `helmet`:

```
const helmet = require("helmet");
app.use(helmet());
```

Helmet sets secure HTTP headers to prevent attacks.

## 7️⃣ Use Rate Limiting to Prevent DDoS Attacks <a href="#id-73f6" id="id-73f6"></a>

Attackers can flood your API with requests, causing it to crash.

✅ **Best Practice**: Use `express-rate-limit`:

```
const rateLimit = require("express-rate-limit");
```

```
const rateLimit = require("express-rate-limit");

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100, // limit each IP to 100 requests per windowMs
});

app.use(limiter);
```

## 8️⃣ Enable CORS Properly <a href="#f813" id="f813"></a>

CORS (Cross-Origin Resource Sharing) should be properly configured to allow requests only from trusted domains.

✅ **Best Practice**: Use the `cors` package:

```
const cors = require("cors");

app.use(cors({
  origin: ["https://your-website.com"], // Allow only trusted domains
  methods: ["GET", "POST"],
}));
```

## 9️⃣ Prevent Directory Traversal Attacks <a href="#id-5617" id="id-5617"></a>

Attackers may try to access system files using `../` paths.

✅ **Best Practice**: Use `path.join()` to resolve file paths securely:

```
const path = require("path");
app.get("/download", (req, res) => {
  const safePath = path.join(__dirname, "files", req.query.file);
  res.sendFile(safePath);
});
```

## 🔟 Use HTTPS and Secure Cookies <a href="#fd06" id="fd06"></a>

* Always serve your Node.js application over **HTTPS**.
* Set **secure and HttpOnly** flags for cookies to prevent attacks.

✅ **Best Practice**: Use `cookie-session` with security options:

```
app.use(require("cookie-session")({
  name: "session",
  keys: ["secretKey"],
  secure: true, // Only allow over HTTPS
  httpOnly: true, // Prevent JavaScript access
}));
```

## 1️⃣1️⃣ Limiting Request Body Payload Size in Node.js (Smart & Secure Way) <a href="#id-44bc" id="id-44bc"></a>

Large payloads can be a **security risk** (e.g., DoS attacks) or cause **performance issues**. Limiting the request body size is a **must-do** for any secure Node.js app!

**✅ How to Limit Request Body Size with `express.json()`**

If you’re using Express:

```
const express = require("express");
const app = express();

// Limit JSON payload to 10KB
app.use(express.json({ limit: '10kb' }));

app.post("/data", (req, res) => {
  res.send("Payload received!");
});

app.listen(3000, () => console.log("🚀 Server running on port 3000"));
```

🧠 `limit: '10kb'` ensures the body parser rejects requests with payloads larger than 10KB.

## 1️⃣2️⃣ **Leaking Server Information in Node.js** <a href="#id-7f5b" id="id-7f5b"></a>

Leaking server info can unintentionally expose **internal details** like:

* Frameworks or platform versions (e.g., Express, Node.js version)
* Server type (`X-Powered-By: Express`)
* Stack traces or error details
* Folder structures and internal paths

These leaks give attackers **clues** to exploit vulnerabilities.

🚨 Examples of Leaks

```
HTTP/1.1 200 OK  
X-Powered-By: Express
```

Or worse, detailed stack traces in error responses:

```
{
  "error": "TypeError: Cannot read property 'name' of undefined",
  "stack": "at app.js:45:17..."
}
```

### ✅ How to Prevent Server Info Leaks in Node.js <a href="#id-2dbf" id="id-2dbf"></a>

### 1️⃣ Disable `X-Powered-By` Header <a href="#id-5988" id="id-5988"></a>

```
const express = require("express");
const app = express();

app.disable("x-powered-by");
```

## 1️⃣3️⃣ **Authentication Limits** <a href="#id-6cbb" id="id-6cbb"></a>

To protect your application from **brute-force attacks**, **credential stuffing**, and **login abuse**, you should implement strict **authentication limits**.

🔐 1️⃣ **Rate Limit Login Attempts**

Prevent bots from hammering your login endpoint.

```
const rateLimit = require("express-rate-limit");

const loginLimiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 5, // Allow only 5 login attempts per IP
  message: "Too many login attempts. Try again later.",
});

app.use("/login", loginLimiter);
```

🔐 2️⃣ **Lock Account Temporarily After Failed Attempts**

Temporarily disable login after repeated failures for a specific user.

✅ Use tools like `express-brute` or implement in DB:

```
// Pseudo-logic: Track login attempts per user/IP
if (user.failedAttempts >= 5) {
  lockAccountFor(30); // Lock for 30 minutes
}
```

🔐 3️⃣ **Use CAPTCHA or reCAPTCHA**

Add CAPTCHA on login after a few failed attempts to stop bots.

🔐 4️⃣ **2FA (Two-Factor Authentication)**

Add an extra layer of protection via OTP, email code, or authenticator apps like Google Authenticator

## 🔒 NODE.JS SECURITY: CODE SAFE, SLEEP EASY <a href="#id-0c66" id="id-0c66"></a>

🚀 **UPDATE OR DIE** — Patch dependencies (`npm audit`) before hackers do!\
🔑 **SECRETS ARE SECRET** – Store keys in `.env`, not your code. (`dotenv`)\
🛑**TRUST NO INPU**T — Sanitize all inputs, Validate everything `(express-validato`r, `jo`i).\
🔐 **HASH IT, LOCK I**T – Secure passwords `(bcryp`t, JWT, OAut`h) 🚫 **XSS? NOT TODAY!** – Protect with` helme`t like a knight in shining code. 🐢 **THROTTLE THE BOTS** – Rate limit requests` (express-rate-limi`t). 🌍 **TRUST BUT VERIFY** – Control API access` (cor`s, RBA`C).\
📁 **NO SNEAKY PATH**S – Lock down file access `(path.join(`)).\
🌐 **HTTPS OR GTF**O – Encrypt everything, always.
