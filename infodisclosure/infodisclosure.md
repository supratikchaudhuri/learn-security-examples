# Tampering

This example demonstrates information disclosure by injecting malicious query objects to a NoSQL database.

## Steps to reproduce

1. Install all dependencies

   `$ npm install`

2. Insert test data in the MongoDB database. Make sure the mongod is up and running by typing the `mongosh` command in the termainal. If mongod process is up then you will see that the connection was successful. Command to insert test data:

   `$ npx ts-node insert-test-users.ts`

This will create a database in MongoDB called **infodisclosure**. Verify its presence by connecting with mongosh and running the command `show dbs;`.

2. Start the **insecure.ts** server

   `$ npx ts-node insecure.ts`

3. In the browser, pretend to be a hacker and type a malicious request

   ```
       http://localhost:3000/userinfo?username[$ne]=
   ```

4. Do you see user information being displayed despite the malicious request not having a valid username in the request?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**

The `insecure.ts` file has a NoSQL injection vulnerability. The problem arises because of how the code handles user input in the `/userinfo` route. The code directly uses the `id` query parameter in a MongoDB query without any kind of validation or sanitization. This would, therefore, allow an attacker to exploit this bug by injecting malicious query objects into the database query.

2. Briefly explain how a malicious attacker can exploit them.

- Query Injection: The id query parameter is directly used to query the MongoDB database, with no validation or sanitization, which might allow an attacker to inject malicious query objects and manipulate the database query.
- ObjectId Validation: The id parameter is not validated as a valid MongoDB ObjectId, which allows attackers to send crafted requests to manipulate the intended query, possibly allowing unauthorized access to sensitive data.

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the information disclosure vulnerability?

1. **Input Type Validation**: This validates that the query parameter `username` is of type string. If this condition does not hold true, then the server will return a status of `400 Bad Request` to ensure that only valid inputs are processed.
1. Input Sanitization: The username input gets sanitized through a regular expression (`username.replace(/[^\\\\w\\s]/gi, '')`), which removes any non-alphanumeric characters and, therefore, prevents malicious inputs from being executed in database queries to mitigate NoSQL injection risks.
1. **Error Handling**: The database query is wrapped in a `try-catch` block to catch and log any errors, ensuring that no sensitive information gets exposed while returning a generic `500 Internal Server Error` response to the client.
1. **Context-Appropriate Querying**: It does not query using a generic identifier like `_id` but rather the `username` field, which is more applicable to this authentication and less likely to accidentally expose data.
