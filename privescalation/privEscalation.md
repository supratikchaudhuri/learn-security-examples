# Privilege Escalation

The example demonstrates a privilege escalation vulnerability and how to exploit it.

## Steps to reproduce

1. Install all dependencies

   `$ npm install`

2. Start the **insecure.ts** server

   `$ npx ts-node insecure.ts`

3. In the browser, send a GET request

   ```
       http://localhost:3000/send-form
   ```

4. Try different UserIds and see which one gives you authorized access to change the role of that user.

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**

1. **Insecure Authentication**: The app only simulates authentication; it just looks for a user in the `users` array by the provided `userId`, which makes it very easy to spoof any identity.
1. **Weak Authorization**: Authorization check is only done regarding the user's role being 'admin' before updating any role. A vulnerability like that could be leveraged by users manipulating the requests to elevate their privileges.
1. **Insufficient Input Validation**: The input `newRole` isn't validated; this allows an attacker to assign arbitrary roles, potentially further compromising the system.

1. Briefly explain how a malicious attacker can exploit them.

- An attacker can guess or provide a valid `userId` to impersonate another user because of the insecure authentication mechanism implemented.
- Privilege Escalation: An attacker holding a valid `userId` would be able to tamper with the request and update his role to have admin privileges or change roles of other users, bypassing proper authorization.
- Without input validation for the `newRole`, an attacker can assign unauthorized or even malicious roles, which might disrupt the system or provide unauthorized access.

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the privilege escalation vulnerability?

- Session-Based Authentication: Logs the user in by creating a session and allows only authorized users to access the update-role endpoint, then denies access if unauthorized.
- Validating inputs: The userId and newRole inputs are validated to allow only valid and expected data to be processed, which prevents malicious role assignments or unauthorized updates of a user. - Role-Based Authorization: Checking the role of the logged-in user against the session data to make sure they have the necessary 'admin' privileges before allowing such sensitive actions of updating roles.
