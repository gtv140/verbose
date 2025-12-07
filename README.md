<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Verbose App</title>

<style>
    body {
        margin:0;
        padding:0;
        font-family: Arial, sans-serif;
        background:#111;
        color:white;
    }
    .container {
        width:90%;
        margin:auto;
        margin-top:40px;
    }
    .box {
        background:#1a1a1a;
        padding:20px;
        border-radius:10px;
        margin-bottom:20px;
    }
    input, button {
        width:100%;
        padding:12px;
        margin-top:10px;
        border:none;
        border-radius:5px;
        font-size:16px;
    }
    input { background:#333; color:white; }
    button { background:#0f62fe; color:white; cursor:pointer; }
    button:hover { opacity:0.8; }
    .hidden { display:none; }
    .link-btn {
        display:block;
        margin-top:10px;
        padding:12px;
        background:#008000;
        text-align:center;
        color:white;
        border-radius:5px;
        text-decoration:none;
    }
</style>
</head>

<body>

<div class="container">

    <!-- LOGIN PAGE -->
    <div id="loginPage" class="box">
        <h2>Login</h2>
        <input type="text" id="username" placeholder="Username">
        <input type="password" id="password" placeholder="Password">
        <button onclick="login()">Login</button>
    </div>

    <!-- HOME PAGE -->
    <div id="homePage" class="box hidden">
        <h2>Welcome <span id="userNameText"></span></h2>
        
        <a class="link-btn" href="https://wa.me/923705519562" target="_blank">WhatsApp Contact</a>
        <a class="link-btn" style="background:#d44638;" href="mailto:test@example.com">Email Contact</a>

        <button style="background:#444;" onclick="refreshPage()">Refresh</button>
        <button style="background:#b30000;" onclick="logout()">Logout</button>
    </div>

</div>

<script>
    // Auto Login Check
    window.onload = function() {
        const savedUser = localStorage.getItem("verboseUser");
        if (savedUser) {
            document.getElementById("loginPage").classList.add("hidden");
            document.getElementById("homePage").classList.remove("hidden");
            document.getElementById("userNameText").innerText = savedUser;
        }
    }

    function login() {
        let user = document.getElementById("username").value.trim();
        let pass = document.getElementById("password").value.trim();

        if (user === "" || pass === "") {
            alert("Username ya password missing hai!");
            return;
        }

        // Auto accept login (no backend needed)
        localStorage.setItem("verboseUser", user);

        document.getElementById("userNameText").innerText = user;
        document.getElementById("loginPage").classList.add("hidden");
        document.getElementById("homePage").classList.remove("hidden");
    }

    function refreshPage() {
        location.reload();
    }

    function logout() {
        localStorage.removeItem("verboseUser");
        location.reload();
    }
</script>

</body>
</html>
