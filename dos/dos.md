# Denial-of-Service (DoS)

This example demonstrates DoS vulnerabilities and how they can be exploited.

## Steps to reproduce

1. Install all dependencies

   `$ npm install`

2. Ignore if you have already done this once. Insert test data in the MongoDB database. Make sure the mongod is up and running by typing the `mongosh` command in the termainal. If mongod process is up then you will see that the connection was successful. Command to insert test data:

   `$ npx ts-node insert-test-users.ts`

This will create a database in MongoDB called **infodisclosure**. Verify its presence by connecting with mongosh and running the command `show dbs;`.

2. Start the **insecure.ts** server

   `$ npx ts-node insecure.ts`

3. In the browser, pretend to be a hacker and type a malicious request

   ```
       http://localhost:3000/userinfo?id[$ne]=
   ```

4. Do you see the server crashing?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts** that can lead to a DoS attack.

- Lack of Input Validation: The id query parameter is used directly in the database query without validation. An attacker may send malformed or very large values for id to overload the server's resources or cause database errors.
- No rate limiting is in place: an attacker could send a high number of requests to the /userinfo endpoint, overwhelming the server and resulting in a DoS.
- NoSQL Injection Risk: Since the id parameter is not being sanitized, an attacker may craft a malicious payload that generates complex or resource-intensive queries, hence leading to a great deal of load on the database as well as the server.

2. Briefly explain how a malicious attacker can exploit them.

- Malformed or Large Inputs: Malformed or large values for the id query parameter can make the database queries fail or become very resource-intensive, slowing down or crashing the application.
- Flooding Requests: It could also be used to flood the /userinfo endpoint with a high volume of requests by an attacker in the absence of rate limiting, rendering the server unavailable for legitimate users.
- NoSQL Injection: Since there is no sanitization being performed, an attacker could inject malicious query objects into the id parameter with the intention of performing resource-intensive or unauthorized queries. This may overload the database in such a manner that it becomes unresponsive.

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the DoS vulnerability?

- The Express Rate Limit middleware is applied to limit the number of requests from a single IP address to the /userinfo endpoint within a given time frame. In this instance, it will only allow 1 request every 5 seconds. This prevents flooding of the server with too many requests but still allows access to the service by legitimate users.
- The try-catch block surrounds the database query; hence, if any error occurs during the execution of the query, it will be caught and handled properly. Instead of crashing the server, the application logs the error and responds with a 500 Internal Server Error status, keeping the application stable.
