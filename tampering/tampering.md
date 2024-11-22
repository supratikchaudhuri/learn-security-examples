# Tampering

This example demonstrates tampering through script injection.

## Steps to reproduce

1. Install all dependencies

   `npm install`

2. Start the **insecure.ts** server

   `npx ts-node insecure.ts`

3. In the browser, type a potentially malicious script in the name field of the form

   ```
       <script> document.body.innerHTML = "<a href='https://google.com'> Gotcha </a>"</script>
   ```

4. Do you see the potentially malicious hyperlink being injected into the form?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**

   The insecure.ts application is vulnerable due to improper input handling and session management. It directly incorporates user input (e.g., the name field) into the response without sanitization, enabling Cross-Site Scripting (XSS) attacks. Additionally, inadequate input validation increases the risk of session hijacking or manipulation by malicious scripts.

2. Briefly explain how a malicious attacker can exploit them.

- By injecting malicious scripts into the name field, an attacker can execute the script when the page is rendered, stealing sensitive information like session cookies or performing unauthorized actions on behalf of the user.
- By injecting scripts to access or modify session data, an attacker could impersonate other users, escalate privileges, or disrupt the application's session flow.

3. Briefly explain why **secure.ts** does not have the same vulnerabilties?

In **secure.ts**, user input from the `name` field is sanitized using the `escapeHTML` function before being included in the response. This function escapes special HTML characters, ensuring that any malicious scripts, such as `<script>alert('Hacked!');</script>`, are rendered as plain text instead of being executed in the browser. Additionally, **secure.ts** adheres to best security practices by using the `httpOnly` and `sameSite` cookie attributes, effectively mitigating Cross-Site Request Forgery (CSRF) and other session-related attacks.
