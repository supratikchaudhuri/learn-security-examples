# Spoofing

This example demonstrates spoofind through two ways -- Stealing cookies programmatically and cross site request forgery (CSRF).

## Steps to reproduce the vulnerability

1. Install dependencies

   `$ npx install`

2. Start the **insecure.ts** server

   `npx ts-node insecure.ts`

3. Start the malicious server **mal.ts**

   `npx ts-node mal.ts`

4. Open **http://localhost:8000** in a browser, type a name and Submit.

5. Open the **Application** tab in the Browser's inspect pane. Find the **Cookies** under **Storage**. You should see a **connect.sid** cookie being set.

6. Open the HTML file **mal-steal-cookie.html** file in the same browser (different tab). Open inspect and view the console.

7. Click the link in the HTML file. Do you see the cookie being stolen in the console?

8. Open the HTML file **mal-csrf.html** file in the same browser (different tab). What do you see if the user has not logged out of **insecure.ts**? What do you see if the user has logged out?

## For you to answer

1. Briefly explain the spoofing vulnerability in **insecure.ts**.

The spoofing vulnerability in insecure.ts arises from the insecure handling of session cookies, specifically setting the httpOnly property of the session cookie to false, which exposes it to client-side access and makes it susceptible to Cross-Site Scripting (XSS) attacks. An attacker could exploit this to steal session cookies by injecting malicious scripts. Furthermore, the lack of Cross-Site Request Forgery (CSRF) protection allows attackers to perform unauthorized actions on behalf of users, as illustrated in scenarios where malicious forms can trigger sensitive operations without user consent.

2. Briefly explain different ways in which vulnerability can be exploited.

- Attackers can use scripts to capture the session cookies for later use in other attacks.
- An attacker can inject harmful JavaScript into a webpage viewed by the victim. Since the session cookie is accessible due to the httpOnly flag being disabled, the attacker can steal the cookie and send it to their server, enabling session hijacking.
- An attacker can craft a malicious link that redirects the victim to a rogue server (e.g., mal.ts) designed to capture the session cookie. This is often achieved through phishing emails or deceptive ads.

3. Briefly explain why **secure.ts** does not have the spoofing vulnerability in **insecure.ts**.

- The httpOnly property of the session cookie is set to true, which prevents client-side scripts from accessing the session cookie, thus reducing the risk of Cross-Site Scripting (XSS) attacks that could steal the cookie.
- The sameSite attribute is set to true, which helps protect against Cross-Site Request Forgery (CSRF) attacks by restricting the sending of cookies with cross-origin requests, ensuring the session cookie is not included in requests made by third-party sites.
- The session secret is provided as a command-line argument, rather than being hardcoded in the source code. This practice improves security by preventing the exposure of sensitive information in version control systems.
