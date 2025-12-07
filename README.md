<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE</title>
<style>
body {margin:0; font-family:sans-serif; background:#070707; color:#fff; text-align:center;}
header {padding:20px; font-size:24px; font-weight:bold; color:#0ff;}
.login-box, .dashboard {max-width:400px; margin:20px auto; padding:20px; background:rgba(0,255,255,0.1); border-radius:10px;}
input, select, button {width:100%; margin:8px 0; padding:10px; border-radius:6px; border:none; font-size:14px;}
button {background:#0ff; color:#000; font-weight:bold; cursor:pointer;}
.nav {position:fixed; bottom:0; width:100%; display:flex; justify-content:space-around; background:#111; padding:10px 0;}
.nav div {cursor:pointer; font-size:14px;}
.hidden {display:none;}
</style>
</head>
<body>

<header>VERBOSE</header>

<div id="loginBox" class="login-box">
  <h3>Login / Signup</h3>
  <select id="userOption">
    <option value="login">Login</option>
    <option value="signup">Signup</option>
  </select>
  <input id="username" placeholder="Username">
  <input id="password" placeholder="Password" type="password">
  <button onclick="login()">Submit</button>
</div>

<div id="dashboard" class="dashboard hidden">
  <h3>Welcome, <span id="userDisplay">—</span></h3>
  <p>Balance: Rs <span id="balance">0</span></p>
  <p>Daily Profit: Rs <span id="daily">0</span></p>
  <button onclick="logout()">Logout</button>
</div>

<div class="nav">
  <div onclick="showPage('dashboard')">Home</div>
  <div onclick="alert('Plans Page')">Plans</div>
  <div onclick="alert('Deposit Page')">Deposit</div>
  <div onclick="alert('Withdraw Page')">Withdraw</div>
  <div onclick="alert('Support Page')">Support</div>
</div>

<script>
let currentUser = localStorage.getItem('verboseUser') || '';
let balance = parseFloat(localStorage.getItem('verboseBalance')) || 0;
let daily = parseFloat(localStorage.getItem('verboseDaily')) || 0;

function login(){
  const u = document.getElementById('username').value.trim();
  const p = document.getElementById('password').value.trim();
  if(!u||!p){ alert('Enter username & password'); return; }
  currentUser = u;
  localStorage.setItem('verboseUser', currentUser);
  localStorage.setItem('verboseBalance', balance);
  localStorage.setItem('verboseDaily', daily);
  document.getElementById('userDisplay').innerText = currentUser;
  document.getElementById('balance').innerText = balance;
  document.getElementById('daily').innerText = daily;
  document.getElementById('loginBox').classList.add('hidden');
  document.getElementById('dashboard').classList.remove('hidden');
}

function logout(){
  currentUser='';
  localStorage.removeItem('verboseUser');
  document.getElementById('loginBox').classList.remove('hidden');
  document.getElementById('dashboard').classList.add('hidden');
}
</script>

</body>
</html>
