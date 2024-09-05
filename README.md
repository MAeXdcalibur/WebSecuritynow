<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Secure Coding Awareness</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
            background-color: #f4f4f4;
            color: #333;
        }
        h1, h2 {
            color: #2d6187;
        }
        section {
            margin-bottom: 40px;
            padding: 20px;
            background-color: #fff;
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }
        input, button {
            padding: 10px;
            margin: 5px 0;
            border: 1px solid #ccc;
            border-radius: 5px;
        }
        button {
            background-color: #2d6187;
            color: white;
        }
        .highlight {
            background-color: #f9d342;
            padding: 5px;
        }
        .secure {
            color: green;
        }
        .vulnerable {
            color: red;
        }
    </style>
</head>
<body>

    <h1>Welcome to Secure Coding Awareness</h1>
    <p>This website demonstrates common security vulnerabilities and how to avoid them with secure coding practices.</p>

    <section id="sql-injection">
        <h2>SQL Injection Example</h2>
        <p>Simulate a login system vulnerable to SQL injection:</p>
        <form onsubmit="simulateSQLInjection(event)">
            <label for="username">Username: </label>
            <input type="text" id="username" required><br>
            <label for="password">Password: </label>
            <input type="password" id="password" required><br>
            <button type="submit">Login</button>
        </form>
        <p id="sql-result"></p>
        <p><strong>Tip:</strong> Always sanitize and validate user input on both client and server sides.</p>
    </section>

    <section id="xss">
        <h2>Cross-Site Scripting (XSS) Example</h2>
        <p>Enter a comment below to see how XSS can exploit a poorly secured website:</p>
        <form onsubmit="simulateXSS(event)">
            <label for="comment">Comment: </label>
            <input type="text" id="comment" required><br>
            <button type="submit">Post Comment</button>
        </form>
        <div id="comments"></div>
        <p><strong>Tip:</strong> Always encode and escape user input before displaying it on the page.</p>
    </section>

    <section id="csrf">
        <h2>Cross-Site Request Forgery (CSRF) Example</h2>
        <p>This is a basic form vulnerable to CSRF attacks. In a real-world scenario, another site could submit this form on your behalf:</p>
        <form action="submit" method="post">
            <label for="csrf-action">Perform Action: </label>
            <input type="text" id="csrf-action"><br>
            <button type="submit">Submit</button>
        </form>
        <p><strong>Tip:</strong> Use anti-CSRF tokens in forms to prevent unauthorized form submissions.</p>
    </section>

    <section id="secure-practices">
        <h2>Secure Coding Practices</h2>
        <p>Secure coding practices can prevent these vulnerabilities. Here are a few key tips:</p>
        <ul>
            <li><span class="highlight">Input Validation:</span> Always validate and sanitize user input.</li>
            <li><span class="highlight">Authentication:</span> Use secure password hashing techniques like bcrypt.</li>
            <li><span class="highlight">HTTPS:</span> Always use HTTPS to secure data transmitted between users and servers.</li>
        </ul>
    </section>

    <script>
        // Simulate SQL Injection
        function simulateSQLInjection(event) {
            event.preventDefault();
            let username = document.getElementById("username").value;
            let password = document.getElementById("password").value;
            
            // Simulate an SQL query
            let query = `SELECT * FROM users WHERE username = '${username}' AND password = '${password}'`;
            
            // Display the vulnerable query result
            document.getElementById("sql-result").innerHTML = `<p class="vulnerable">Vulnerable Query: ${query}</p>`;
        }

        // Simulate XSS (Cross-Site Scripting)
        function simulateXSS(event) {
            event.preventDefault();
            let comment = document.getElementById("comment").value;
            
            // Vulnerable output (no encoding)
            let commentSection = document.getElementById("comments");
            commentSection.innerHTML += `<p class="vulnerable">Vulnerable Comment: ${comment}</p>`;
        }
    </script>

</body>
</html>
