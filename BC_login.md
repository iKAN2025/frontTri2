---
layout: base
title: Login
permalink: /login/
---
<html>
<head>
    <title>Login</title>
</head>
<body>
    <h1>Login</h1>
    <form id="loginForm">
        <label for="uid">Username:</label>
        <input type="text" id="uid" name="uid" required><br><br>        
        <label for="password">Password:</label>
        <input type="password" id="password" name="password" required><br><br>       
        <input type="submit" value="Login">
    </form>
    <div id="userDisplayName"></div>
    <script>
        document.getElementById('loginForm').addEventListener('submit', function(event) {
            event.preventDefault(); // Prevent form submission
            const uid = document.getElementById('uid').value;
            const password = document.getElementById('password').value;
            const loginData = {
                uid: uid,
                password: password
            };
            fetch('http://127.0.0.1:8240/api/users/login', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify(loginData)
            })
            .then(response => {
                if (response.ok) {
                    return response.json();
                } else {
                    if (response.status === 401) {
                        throw new Error('Wrong username or password.Please retype.');
                    } else if (response.status === 404) {
                        throw new Error('Username or password not found. Please register first.');
                    } else {
                        throw new Error('Login failed');
                    }
                }
            })
            .then(data => {
                const loggedInUserName = data.user.name;
                const loggedInUserId = data.user.id;
                localStorage.setItem('loggedInUserName', loggedInUserName);
                localStorage.setItem('loggedInUserId', loggedInUserId);
                document.getElementById('userDisplayName').textContent = `Welcome, ${loggedInUserName}!`;
                document.getElementById('loginForm').style.display = 'none';
                const userIDFromLocalStorage = localStorage.getItem('loggedInUserId');
                console.log(userIDFromLocalStorage)
                
                
            })
            .catch(error => {
                console.error('Error:', error.message);
                alert(error.message);
            });
        });
    </script>
</body>
</html>

