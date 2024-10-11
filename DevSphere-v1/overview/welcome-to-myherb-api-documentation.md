---
description: Welcome to the official documentation for the myHerb API.
icon: unity
cover: ../.gitbook/assets/Designer (22).jpeg
coverY: 0
layout:
  cover:
    visible: true
    size: full
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Welcome to myHerb API Documentation

## This documentation provides all the information you need to integrate with our API and make the most out of our services.

### Getting Started

To get started with the myHerb API, follow these steps:

1. **Sign Up**: Create an account on our website.
2. **API Key**: Obtain your API key from the dashboard.
3. **Read the Docs**: Familiarize yourself with the API endpoints and usage.

### API Endpoints

#### Authentication

* **POST /auth/login**
  * Description: Authenticate a user and obtain a token.
  * Parameters:
    * `username` (string): The user's username.
    * `password` (string): The user's password.
  * Response:
    * `token` (string): The authentication token.

#### Herbs

* **GET /herbs**
  * Description: Retrieve a list of all herbs.
  * Parameters: None
  * Response:
    * `herbs` (array): A list of herbs.
* **POST /herbs**
  * Description: Add a new herb.
  * Parameters:
    * `name` (string): The name of the herb.
    * `description` (string): A description of the herb.
  * Response:
    * `herb` (object): The newly created herb.

### Error Handling

Our API uses standard HTTP status codes to indicate the success or failure of an API request. Here are some standard status codes:

* **200 OK**: The request was successful.
* **400 Bad Request**: The request was invalid or cannot be served.
* **401 Unauthorized:** Authentication failed, or the user does not have permissions.
* **404 Not Found**: The requested resource could not be found.
* **500 Internal Server Error**: An error occurred on the server.

### Support

If you have any questions or need further assistance, please get in touch with our support team at dev@myherb.co.il

### Additional Resources

* API Reference
* Developer Guide
* Community Forum

Thank you for using myHerb API!
