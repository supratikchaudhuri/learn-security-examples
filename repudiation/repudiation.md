# Repudiation

The example demonstrates a vulnerability that can lead to repudiation by malicious users attempting to access the services provided by a server.

## Steps to reproduce

1. Install all dependencies

   `$ npm install`

2. Run the server **insecure.ts**.

3. Pretend to be a malicous user and interact with the services by sending requests from the browser.

4. Do you think your actions can be repudiated?

## For you to do

1. Briefly explain the vulnerability.

The vulnerability in **insecure.ts** stems from the absence of authentication and logging mechanisms. This allows malicious users to send and retrieve messages without verifying their identity. Consequently, users can deny responsibility for their actions, as there is no record linking messages to specific users. This lack of accountability makes the service susceptible to misuse and potential abuse, as anyone can freely interact with the endpoints without oversight.

2. Briefly explain why the vulnerability is addressed in **secure.ts**.

The **secure.ts** file addresses the vulnerability by introducing user authentication and logging mechanisms. It ensures that users must be authenticated before sending or retrieving messages, restricting these actions to authorized individuals. Furthermore, it logs all user requests and actions, creating a comprehensive audit trail. This logging makes it possible to track user interactions with the server, preventing users from denying their actions and enhancing accountability.

3. Which design pattern is used in the secure version to address the vulnerability? Briefly explain how it works?

Security-related issues in the **secure.ts** file are handled through the use of the **Middleware design pattern**. Middleware functions process requests before they reach the route handlers. In **secure.ts**, middleware is used to log requests, handle errors, and check for authentication. This helps in separation of concerns; hence, different functionalities like logging, authentication, and error handling can be handled independently yet applied consistently over all routes. By using middleware, the application can keep its structure clean but still ensure that security measures are applied properly throughout the application.
