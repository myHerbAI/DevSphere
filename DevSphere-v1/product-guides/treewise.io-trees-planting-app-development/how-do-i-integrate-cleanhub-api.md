---
icon: star-christmas
description: >-
  Integrating the CleanHub API into your application involves several steps,
  including setting up your project, making API requests, and handling
  responses.
cover: >-
  ../../.gitbook/assets/screenshot cleanhub Explain what is myHerb and provide
  recommendations for beginner.png
coverY: -66
---

# How do I integrate CleanHub API?

{% hint style="info" %}
[Read the Impact Report](https://www.cleanhub.com/impact-report/myherb) to learn about the environmental activities and initiatives undertaken by myHerb in collaboration with CleanHub.
{% endhint %}

## Here’s a detailed guide to help you get started:

### **1. Get API Key**

First, you need to obtain an API key from CleanHub. You can do this by signing up on their website and requesting access to their API.

### **2. Install Dependencies**

Ensure you have Node.js and npm installed. Then, create a new Node.js project and install the necessary dependencies.

#### **Command:**

```bash
npm init -y
npm install express body-parser node-fetch dotenv
```

### **3. Set Up Project Structure**

Create the following files and directories:

```
cleanhub-app/
├── index.html
├── styles.css
├── script.js
├── server.js
├── .env
└── README.md
```

### **4. Configure Environment Variables**

Create a `.env` file to store your API key securely.

#### **.env:**

```plaintext
CLEANHUB_API_KEY=your_api_key_here
```

### **5. Create Server**

Set up an Express server to handle API requests.

#### **server.js:**

```javascript
const express = require('express');
const bodyParser = require('body-parser');
const fetch = require('node-fetch');
require('dotenv').config();

const app = express();
app.use(bodyParser.json());

app.post('/donate', async (req, res) => {
    const { project, amount } = req.body;
    try {
        const response = await fetch('https://api.cleanhub.com/donate', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': `Bearer ${process.env.CLEANHUB_API_KEY}`
            },
            body: JSON.stringify({ project, amount })
        });
        const data = await response.json();
        res.json({ message: 'Donation successful', data });
    } catch (error) {
        console.error('Error:', error);
        res.status(500).json({ message: 'Donation failed' });
    }
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
    console.log(`Server is running on port ${PORT}`);
});
```

### **6. Create Frontend**

Set up the HTML, CSS, and JavaScript files for the frontend.

#### **index.html:**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CleanHub Integration</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <header>
        <h1>CleanHub Integration</h1>
    </header>
    <main>
        <section>
            <h2>Donate to Plastic Cleanup Projects</h2>
            <form id="donation-form">
                <label for="project">Choose a project:</label>
                <select id="project" name="project">
                    <option value="plastic-cleanup">Plastic Cleanup</option>
                </select>
                <label for="amount">Donation Amount:</label>
                <input type="number" id="amount" name="amount" required>
                <button type="submit">Donate</button>
            </form>
        </section>
        <section id="confirmation">
            <h2>Confirmation</h2>
            <p id="confirmation-message"></p>
        </section>
    </main>
    <footer>
        <p>&copy; 2024 CleanHub Integration</p>
    </footer>
    <script src="script.js"></script>
</body>
</html>
```

#### **styles.css:**

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

#### **script.js:**

```javascript
document.addEventListener('DOMContentLoaded', function() {
    const donationForm = document.getElementById('donation-form');
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
        })
        .catch(error => console.error('Error:', error));
    });
});
```

### **7. Run the Application**

Start your server and open `index.html` in your web browser.

#### **Command:**

```bash
node server.js
```

Following these steps, you can integrate the CleanHub API into your application, allowing users to donate to plastic cleanup projects. This setup ensures secure handling of API keys and provides a user-friendly interface for making donations.

Would you like more detailed guidance on any specific part of this process or additional features for your application?
