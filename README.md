<welcome to verbose>
<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE</title>

<style>
    body { margin:0; font-family:Arial; background:#f2f2f2; }
    header { background:#007bff; padding:15px; text-align:center; color:#fff; font-size:22px; font-weight:bold; }
    .login-box, .page { max-width:400px; margin:40px auto; background:#fff; padding:20px; border-radius:10px; box-shadow:0 0 10px #ccc; }
    input, button { width:100%; padding:12px; margin-top:10px; border-radius:5px; border:1px solid #aaa; }
    button { background:#007bff; color:#fff; border:none; cursor:pointer; }
    button:hover { background:#0056b3; }

    .nav { position:fixed; bottom:0; left:0; right:0; background:#fff; display:flex; justify-content:space-around; padding:10px 0; border-top:1px solid #ccc; }
    .nav div { text-align:center; font-size:12px; cursor:pointer; }
    .icon { font-size:22px; }

    .hidden { display:none; }
</style>
</head>
<body>

<header>VERBOSE</header>

<!-- LOGIN PAGE -->
<div id="loginPage" class="login-box">
    <h2>Login</h2>
    <input id="user" placeholder="Username">
    <input id="pass" placeholder="Password" type="password">
    <button onclick="login()">Login</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
    <h2>Dashboard</h2>
    <p>Welcome to VERBOSE!</p>
</div>

<!-- PAGES -->
<div id="plans" class="page hidden"><h2>Plans</h2><p>All VERBOSE plans listed here.</p></div>
<div id="deposit" class="page hidden"><h2>Deposit</h2><p>Deposit page working.</p></div>
<div id="withdrawal" class="page hidden"><h2>Withdrawal</h2><p>Withdraw your balance here.</p></div>
<div id="support" class="page hidden"><h2>Support</h2><p>Contact VERBOSE support.</p></div>
<div id="activity" class="page hidden"><h2>Activity</h2><p>All activity logs.</p></div>
<div id="history" class="page hidden"><h2>History</h2><p>Your complete transaction history.</p></div>

<!-- NAVIGATION -->
<div id="bottomNav" class="nav hidden">
    <div onclick="showPage('dashboard')">🏠<br>Home</div>
    <div onclick="showPage('plans')">📦<br>Plans</div>
    <div onclick="showPage('deposit')">💰<br>Deposit</div>
    <div onclick="showPage('withdrawal')">💵<br>Withdraw</div>
    <div onclick="showPage('support')">📞<br>Support</div>
    <div onclick="showPage('activity')">📊<br>Activity</div>
    <div onclick="showPage('history')">📚<br>History</div>
</div>

<script>
function login() {
    let u = document.getElementById("user").value;
    let p = document.getElementById("pass").value;

    if(u === "" || p === ""){
        alert("Username aur password likho sweetie");
        return;
    }

    document.getElementById("loginPage").classList.add("hidden");
    document.getElementById("dashboard").classList.remove("hidden");
    document.getElementById("bottomNav").classList.remove("hidden");
}

function showPage(id){
    let pages = document.querySelectorAll(".page");
    pages.forEach(p => p.classList.add("hidden"));
    document.getElementById(id).classList.remove("hidden");
}
</script>

</body>
</html>
