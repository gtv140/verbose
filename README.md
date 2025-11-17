<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Login Test</title>
<style>
body{font-family:sans-serif;background:#000;color:#0ff;text-align:center;padding:50px;}
input{padding:10px;margin:5px;}
button{padding:10px 20px;margin:5px;cursor:pointer;background:#0ff;color:#000;border:none;border-radius:5px;}
.dashboard{display:none;margin-top:20px;}
</style>
</head>
<body>

<div id="auth">
<h2>Login / Signup</h2>
<input type="text" id="username" placeholder="Username">
<input type="password" id="password" placeholder="Password">
<br>
<button id="loginBtn">Login</button>
<button id="signupBtn">Signup</button>
</div>

<div class="dashboard" id="dashboard">
<h2>Welcome <span id="userDisplay"></span></h2>
<div>Balance: <span id="userBalance">0</span></div>
<button id="logoutBtn">Logout</button>
</div>

<script>
// --- User system ---
let users = JSON.parse(localStorage.getItem('users')) || [];
let currentUser = JSON.parse(localStorage.getItem('currentUser')) || null;

// --- Functions ---
function signup(){
  const u=document.getElementById('username').value.trim();
  const p=document.getElementById('password').value.trim();
  if(!u||!p){ alert('Enter username & password'); return; }
  if(users.find(user=>user.username===u)){ alert('Username exists'); return; }
  users.push({username:u,password:p,balance:0});
  localStorage.setItem('users',JSON.stringify(users));
  alert('Signup success. Please login.');
}

function login(){
  const u=document.getElementById('username').value.trim();
  const p=document.getElementById('password').value.trim();
  const user = users.find(user=>user.username===u && user.password===p);
  if(!user){ alert('Invalid username or password'); return; }
  currentUser = user;
  localStorage.setItem('currentUser',JSON.stringify(currentUser));
  showDashboard();
}

function logout(){
  currentUser = null;
  localStorage.removeItem('currentUser');
  document.getElementById('dashboard').style.display='none';
  document.getElementById('auth').style.display='block';
}

function showDashboard(){
  document.getElementById('auth').style.display='none';
  document.getElementById('dashboard').style.display='block';
  document.getElementById('userDisplay').innerText=currentUser.username;
  document.getElementById('userBalance').innerText=currentUser.balance;
}

// --- Event Listeners ---
document.getElementById('loginBtn').addEventListener('click', login);
document.getElementById('signupBtn').addEventListener('click', signup);
document.getElementById('logoutBtn').addEventListener('click', logout);

// --- Persistent login ---
window.addEventListener('load', ()=>{
  if(currentUser){ showDashboard(); }
});
</script>
</body>
</html>
