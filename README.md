Here is a simple, minimal portfolio website with separate `HTML`, `CSS`, `script.js`, `server.js`, and `.env` files. The contact form sends messages to your Gmail using your App Password.

## 1. Folder structure

text

```
portfolio/
├── public/
│   ├── index.html
│   ├── style.css
│   └── script.js
├── server.js
└── .env
```

## 2. Install packages

bash

```
npm init -y
npm i express nodemailer dotenv
```

## 3. `public/index.html`

Replace the placeholder text with your personal information.

HTMLWrap linesCopy codePreview

```

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <title>Your Name | Portfolio</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <nav><b>Your Name</b><a href="#contact">Contact</a></nav>

  <section>
    <p>Hello, I'm</p>
    <h1>Your Full Name</h1>
    <h3>Web Developer | Student</h3>
    <p>I create simple and responsive websites.</p>
    <a href="#projects">View Projects ↓</a>
  </section>

  <section>
    <h2>About Me</h2>
    <p>Write about yourself, your education and career goals.</p>
    <p><b>Skills:</b> HTML, CSS, JavaScript, Node.js</p>
  </section>

  <section id="projects">
    <h2>My Projects</h2>
    <article><h3>Project One</h3>
      <p>Brief description of your project.</p>
      <a href="YOUR_PROJECT_URL">View Project</a>
    </article>
    <article><h3>Project Two</h3>
      <p>Brief description of another project.</p>
      <a href="YOUR_GITHUB_URL">View Code</a>
    </article>
  </section>

  <section>
    <h2>Education</h2>
    <p>Your Degree — Your College — Graduation Year</p>
  </section>

  <section id="contact">
    <h2>Contact Me</h2>
    <form id="form">
      <input name="name" placeholder="Your Name" required>
      <input name="email" type="email" placeholder="Your Email" required>
      <textarea name="message" placeholder="Your Message" required></textarea>
      <button>Send Message</button>
      <p id="status"></p>
    </form>
  </section>

  <footer>© 2026 Your Name</footer>
  <script src="script.js"></script>
</body>
</html>
```

## 4. `public/style.css`

css

```

*{box-sizing:border-box}
body{margin:0;background:#fafafa;color:#222;font:16px Arial}
nav{display:flex;justify-content:space-between;padding:25px 8%}
section{max-width:650px;margin:65px auto;padding:0 20px}
h1{font-size:44px;margin:10px 0}
p{color:#666;line-height:1.6}
a{color:#222}
article{padding:15px;border:1px solid #ddd;margin:15px 0}
input,textarea,button{display:block;width:100%;padding:12px;
margin:10px 0;border:1px solid #ddd;border-radius:5px}
button{background:#222;color:white;cursor:pointer}
button:disabled{opacity:.6}
footer{text-align:center;padding:25px;color:#777}
@media(max-width:700px){section{margin:40px auto}h1{font-size:36px}}
```

## 5. `public/script.js`

Sends the form to the backend without refreshing the page.

javascript

```

document.getElementById("form").onsubmit = async e => {
  e.preventDefault();
  const form = e.target, btn = form.querySelector("button");
  const status = document.getElementById("status");
  btn.disabled = true;
  status.textContent = "Sending...";

  try {
    const res = await fetch("/contact", {
      method: "POST",
      headers: {"Content-Type": "application/json"},
      body: JSON.stringify(Object.fromEntries(new FormData(form)))
    });
    status.textContent = await res.text();
    if (res.ok) form.reset();
  } catch {
    status.textContent = "Failed. Please try again.";
  }
  btn.disabled = false;
};
```

## 6. `server.js`

Receives the form and sends the message to your Gmail.

javascript

```

require("dotenv").config();
const express = require("express");
const nodemailer = require("nodemailer");
const app = express();

app.use(express.static("public"));
app.use(express.json());

const mailer = nodemailer.createTransport({
  service: "gmail",
  auth: {
    user: process.env.MAIL_USER,
    pass: process.env.MAIL_PASS
  }
});

app.post("/contact", async (req, res) => {
  const {name, email, message} = req.body;
  if (![name, email, message].every(v =>
      typeof v === "string" && v.trim()))
    return res.status(400).send("Please fill all fields.");

  try {
    await mailer.sendMail({
      from: process.env.MAIL_USER,
      to: process.env.MAIL_USER,
      replyTo: email,
      subject: `Portfolio message from ${name.trim()}`,
      text: message.trim()
    });
    res.send("Message sent successfully!");
  } catch (err) {
    console.error(err);
    res.status(500).send("Email failed. Please try again.");
  }
});

app.listen(process.env.PORT || 3000,
  () => console.log("http://localhost:3000"));
```

## 7. `.env`

env

```
MAIL_USER=yourgmail@gmail.com
MAIL_PASS=your_16_character_app_password
PORT=3000
```

for record print put the .env file without your password (MAIL_PASS=your_16_character_app_password)

- Replace `yourgmail@gmail.com` with the email address where you want to receive messages.
- Replace the password with a Google App Password, not your regular Gmail password.
- Generate one at [Google App Passwords](<https://myaccount.google.com/apppasswords>). Two-Step Verification is generally required.

## 8. Run the project

bash

```
node server.js
```

Open `http://localhost:3000`, fill out the contact form and submit it. The message will be emailed to the configured Gmail account if sending succeeds.

Important: Keep `.env` private and never upload it to GitHub. Before publishing your portfolio online, add rate limiting and spam protection to prevent contact-form abuse.



## Step 1: Initialize your project

Run:

```
npm init -y
```

This creates `package.json`.

## Step 2: Install the required packages

powershell

```
npm install express nodemailer dotenv
```

Wait until installation finishes successfully.

## Step 3: Check your file structure

Make sure your project looks like this:

text

```
portfolio_ex10_rec/
├── public/
│   ├── index.html
│   ├── style.css
│   └── script.js
├── server.js
├── .env
├── package.json
└── node_modules/
```

Important: Your `.env` file and `server.js` must be in the same main folder.

## Step 4: Start the server

powershell

```
node server.js
```

You should see:

text

```
http://localhost:3000
```

Open that address in your browser.

## Step 5: Test the contact form

Submit a message using the portfolio form. If your `.env` contains a valid Gmail address and Google App Password, the message should be sent to that Gmail account.

Example `.env`:

env

```
MAIL_USER=yourgmail@gmail.com
MAIL_PASS=your16characterapppassword
PORT=3000
```

Do not include quotes around the values or share your app password.

<img width="1240" height="1428" alt="image" src="https://github.com/user-attachments/assets/e529dc63-890f-43a1-81ff-0a8cbedea80c" />
<img width="1804" height="872" alt="image" src="https://github.com/user-attachments/assets/f7d4930e-49b0-4470-9084-620f0bbba9f5" />
<img width="1617" height="973" alt="image" src="https://github.com/user-attachments/assets/d12461e1-d66e-47ef-b037-9644c95c100e" />




