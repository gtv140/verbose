<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE - Fresh Version</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.2/css/all.min.css">
<style>
:root{
  --bg:#0b0f1a;
  --primary:#4da6ff;
  --text:#e6ebf5;
  --muted:#a0aec0;
  --alert:#ff4d4d;
}
body{margin:0;font-family:Arial,sans-serif;background:var(--bg);color:var(--text);}
header{padding:20px;text-align:center;font-size:28px;font-weight:bold;color:var(--primary);}
.wrap{max-width:480px;margin:12px auto;padding:12px;}
.card{background:#111827;padding:15px;border-radius:12px;margin-bottom:12px;border:1px solid var(--primary);}
input,select,button{width:100%;padding:10px;margin-top:6px;border-radius:8px;border:none;background:#1f2937;color:var(--text);}
.btn{background:var(--primary);color:#fff;font-weight:600;cursor:pointer;}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;padding:10px;background:#1f2937;}
.nav div{color:var(--primary);text-align:center;font-size:13px;cursor:pointer;}
.hidden{display:none;}
.badge{background:#1f2937;padding:4px 8px;border-radius:999px;color:var(--primary);}
</style>
</head>
<body>

<header><i class="fas fa-coins"></i> VERBOSE</header>
<div class="wrap">

<!-- LOGIN -->
<div id="loginCard" class="card">
<h3 style="color:var(--primary)"><i class="fas fa-user"></i> Login / Signup</h3>
<input id="username" placeholder="Username"/>
<input id="password" type="password" placeholder="Password"/>
<button class="btn" onclick="login()">Login</button>
</div>

<!-- DASHBOARD -->
<div id="dashboardCard" class="card hidden">
<h3 style="color:var(--primary)"><i class="fas fa-home"></i> Dashboard</h3>
<div><b>Welcome:</b> <span id="userDisplay"></span></div>
<div class="badge">Balance: Rs <span id="balance">0</span></div>
<button class="btn" onclick="logout()">Logout</button>

<!-- ACTION ICONS -->
<div style="display:flex;justify-content:space-around;margin-top:15px;">
<div onclick="alert('Deposit clicked')" style="text-align:center;cursor:pointer;">
<i class="fas fa-wallet" style="font-size:24px;color:var(--primary)"></i><br>Deposit
</div>
<div onclick="alert('Withdraw clicked')" style="text-align:center;cursor:pointer;">
<i class="fas fa-hand-holding-usd" style="font-size:24px;color:var(--primary)"></i><br>Withdraw
</div>
<div onclick="alert('Plans clicked')" style="text-align:center;cursor:pointer;">
<i class="fas fa-gift" style="font-size:24px;color:var(--primary)"></i><br>Plans
</div>
<div onclick="alert('Support clicked')" style="text-align:center;cursor:pointer;">
<i class="fas fa-headset" style="font-size:24px;color:var(--primary)"></i><br>Support
</div>
</div>
</div>

</div>

<script>
let currentUser = null;
let balance = 0;

function login(){
  const user = document.getElementById('username').value.trim();
  const pass = document.getElementById('password').value.trim();
  if(!user||!pass){alert('Enter username and password'); return;}
  currentUser = user;
  balance = 1000; // default balance
  document.getElementById('userDisplay').innerText = currentUser;
  document.getElementById('balance').innerText = balance;
  document.getElementById('loginCard').classList.add('hidden');
  document.getElementById('dashboardCard').classList.remove('hidden');
}

function logout(){
  currentUser = null;
  document.getElementById('loginCard').classList.remove('hidden');
  document.getElementById('dashboardCard').classList.add('hidden');
}
</script>

</body>
</html>
