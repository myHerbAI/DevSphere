---
description: >-
  Follow these steps to integrate CleanHub’s plastic reduction API and
  Tree-Nation’s planting certification API into your application.
icon: seedling
cover: >-
  ../../.gitbook/assets/myHerb office futuristic atmosphere with high-tech
  electronic trees.png
coverY: 118
---

# TreeWise.io: Trees Planting App development

{% hint style="info" %}
_This guide will help you set up the APIs, handle responses, and email planting certificates to users._
{% endhint %}

## **Project Structure**

```
treewise-app/
├── index.html
├── styles.css
├── script.js
├── README.md
├── server.js
├── config/
│   └── config.js
├── public/
│   └── images/
└── routes/
    └── api.js
```

### **1. index.html**

This file defines the structure of your web application.

#### **Code:**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TreeWise.io</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <header>
        <h1>TreeWise.io</h1>
    </header>
    <main>
        <section>
            <h2>Donate to Environmental Projects</h2>
            <form id="donation-form">
                <label for="project">Choose a project:</label>
                <select id="project" name="project">
                    <option value="tree-planting">Tree Planting</option>
                    <option value="plastic-cleanup">Plastic Cleanup</option>
                </select>
                <label for="amount">Donation Amount:</label>
                <input type="number" id="amount" name="amount" required>
                <button type="submit">Donate</button>
            </form>
        </section>
        <section>
            <h2>Plant a Tree</h2>
            <form id="planting-form">
                <label for="location">Choose a location:</label>
                <input type="text" id="location" name="location" required>
                <label for="tree-type">Choose a tree type:</label>
                <input type="text" id="tree-type" name="tree-type" required>
                <label for="email">Your Email (optional):</label>
                <input type="email" id="email" name="email">
                <button type="submit">Plant</button>
            </form>
        </section>
        <section id="confirmation">
            <h2>Confirmation</h2>
            <p id="confirmation-message"></p>
        </section>
    </main>
    <footer>
        <p>&copy; 2024 TreeWise.io</p>
    </footer>
    <script src="script.js"></script>
</body>
</html>
```

### **2. styles.css**

This file contains the CSS styles for your web application.

#### **Code:**

```css
body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
    background-color: #f4f4f4;
}

header {
    background-color: #333;
    color: #fff;
    padding: 1rem 0;
    text-align: center;
}

main {
    padding: 2rem;
}

section {
    margin-bottom: 2rem;
}

footer {
    background-color: #333;
    color: #fff;
    text-align: center;
    padding: 1rem 0;
    position: fixed;
    width: 100%;
    bottom: 0;
}
```

### **3. script.js**

This file contains the JavaScript code to handle form submissions, integrate with the APIs, and save data to various storage options.

#### **Code:**

```javascript
document.addEventListener('DOMContentLoaded', function() {
    const donationForm = document.getElementById('donation-form');
    const plantingForm = document.getElementById('planting-form');
    const confirmationMessage = document.getElementById('confirmation-message');

    donationForm.addEventListener('submit', function(event) {
        event.preventDefault();
        const project = document.getElementById('project').value;
        const amount = document.getElementById('amount').value;

        fetch('/donate', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({ project, amount })
        })
        .then(response => response.json())
        .then(data => {
            confirmationMessage.textContent = `Thank you for your donation of $${amount} to ${project}!`;
            saveAction('donation', { project, amount });
        })
        .catch(error => console.error('Error:', error));
    });

    plantingForm.addEventListener('submit', function(event) {
        event.preventDefault();
        const location = document.getElementById('location').value;
        const treeType = document.getElementById('tree-type').value;
        const email = document.getElementById('email').value;

        fetch('/plant', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({ location, treeType, email })
        })
        .then(response => response.json())
        .then(data => {
            confirmationMessage.textContent = `A ${treeType} tree has been planted at ${location}!`;
            saveAction('planting', { location, treeType, email });
        })
        .catch(error => console.error('Error:', error));
    });

    function saveAction(type, data) {
        // Save action to Google Drive, OneDrive, or local storage
        // Example: Save to local storage
        localStorage.setItem(`${type}-${Date.now()}`, JSON.stringify(data));
    }
});
```

### **4. server.js**

This file contains the Node.js server code to handle API requests, integrate with the One Tree Planted and CleanHub APIs, and send emails.

#### **Code:**

```javascript
const express = require('express');
const bodyParser = require('body-parser');
const nodemailer = require('nodemailer');
const { google } = require('googleapis');
const { OneDrive } = require('onedrive-api');
const fetch = require('node-fetch');

const app = express();
app.use(bodyParser.json());

app.post('/donate', (req, res) => {
    const { project, amount } = req.body;
    // Integrate with CleanHub API for plastic cleanup donations
    fetch('https://api.cleanhub.com/donate', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json',
            'Authorization': 'Bearer YOUR_CLEANHUB_API_KEY'
        },
        body: JSON.stringify({ project, amount })
    })
    .then(response => response.json())
    .then(data => {
        console.log(`Donation received: $${amount} for ${project}`);
        res.json({ message: 'Donation successful' });
    })
    .catch(error => {
        console.error('Error:', error);
        res.status(500).json({ message: 'Donation failed' });
    });
});

app.post('/plant', (req, res) => {
    const { location, treeType, email } = req.body;
    // Integrate with Tree-Nation API for tree planting
    fetch('https://api.tree-nation.com/plant', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json',
            'Authorization': 'Bearer YOUR_TREE_NATION_API_KEY'
        },
        body: JSON.stringify({ location, treeType })
    })
    .then(response => response.json())
    .then(data => {
        console.log(`Tree planted: ${treeType} at ${location}`);
        res.json({ message: 'Tree planting successful' });

        if (email) {
            // Send planting certificate via email
            const transporter = nodemailer.createTransport({
                service: 'gmail',
                auth: {
                    user: 'your-email@gmail.com',
                    pass: 'your-email-password'
                }
            });

            const mailOptions = {
                from: 'your-email@gmail.com',
                to: email,
                subject: 'Tree Planting Certificate',
                text: `A ${treeType} tree has been planted at ${location}. Thank you for your contribution!`
            };

            transporter.sendMail(mailOptions, (error, info) => {
                if (error) {
                    console.log('Error sending email:', error);
                } else {
                    console.log('Email sent:', info.response);
                }
            });
        }
    })
    .catch(error => {
        console.error('Error:', error);
        res.status(500).json({ message: 'Tree planting failed' });
    });
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
    console.log(`Server is running on port ${PORT}`);
});
```

### **5. README.md**

This file provides an overview of the project and instructions for setting it up.

#### **Code:**

```markdown
# TreeWise.io

TreeWise.io is a web application that allows users to donate to environmental projects and plant trees. Users can choose the type of project, location, and tree type. The application also sends a planting certificate via email and saves each action to Google Drive, OneDrive, or local storage.

## Project Structure
```

treewise-app/ ├── index.html ├── styles.css ├── script.js ├── README.md └── server.js

````

## How to Run
1. Clone the repository:
   ```bash
   git clone https://github.com/your-username/treewise-app.git
````

2.  Install dependencies:

    ```bash
    cd treewise-app
    npm install express body-parser nod
    ```
