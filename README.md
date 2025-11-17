<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE — Ultra Premium Earning Platform</title>
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700&display=swap" rel="stylesheet">
<style>
body {
  margin:0; font-family:'Orbitron',sans-serif;
  background:linear-gradient(270deg,#0f0c29,#302b63,#24243e);
  background-size:600% 600%;
  animation:gradientBG 15s ease infinite;
  color:#fff; overflow-x:hidden;
}
@keyframes gradientBG {
  0%{background-position:0% 50%}50%{background-position:100% 50%}100%{background-position:0% 50%}
}
h1,h2,h3{text-align:center;color:#0ff;text-shadow:0 0 5px #0ff,0 0 10px #0ff;}
.container{width:90%;margin:20px auto;}
button{
  cursor:pointer;padding:10px 20px;margin:5px;border:none;border-radius:10px;
  background:#0ff;color:#000;font-weight:bold;font-size:16px;transition:0.3s;
  box-shadow:0 0 5px #0ff,0 0 10px #0ff;
}
button:hover{
  background:#0cc; box-shadow:0 0 15px #0ff,0 0 30px #0ff; transform:scale(1.05);
}
input,select{padding:10px;margin:5px;border-radius:5px;border:none;font-size:16px;}
.plan-card{
  background:rgba(0,255,255,0.1);border:1px solid #0ff;padding:15px;margin:10px 0;
  border-radius:15px;animation:neonPulse 2s infinite alternate;position:relative;
}
.plan-card h3{color:#0ff;text-shadow:0 0 10px #0ff;}
.plan-card .badge{
  position:absolute;top:10px;right:10px;background:#ff0;color:#000;padding:5px 10px;
  border-radius:10px;font-weight:bold;animation:blink 1s infinite alternate;
}
@keyframes neonPulse{
  0%{box-shadow:0 0 5px #0ff}100%{box-shadow:0 0 25px #0ff;}
}
@keyframes blink{
  0%{opacity:1}100%{opacity:0.3}
}
.dashboard,.auth,.deposit,.withdrawal,.about{display:none;}
.active{display:block;}
.plan-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(250px,1fr));grid-gap:15px;}
.balance-display{font-weight:bold;color:#0ff;text-align:center;margin:10px 0;font-size:18px;}
.icon{margin-right:5px;}
</style>
</head>
<body>
<h1>VERBOSE Earning Platform</h1>
<h3>Owner: John Wilson | Launch Date: 17 Nov 2025</h3>
<div class="container">

<div class="auth" id="auth">
<h2>Login / Signup</h2>
<input type="text" id="username" placeholder="Enter Username">
<input type="password" id="password" placeholder="Enter Password">
<br>
<button onclick="login()">Login</button>
<button onclick="signup()">Signup</button>
<button onclick="googleLogin()">Login with Gmail</button>
</div>

<div class="dashboard" id="dashboard">
<h2>Welcome <span id="userDisplay"></span></h2>
<div class="balance-display">Balance: <span id="userBalance">0</span> PKR</div>
<button onclick="logout()">Logout</button>
<div class="plan-grid" id="planGrid"></div>
<button onclick="showDeposit()">💰 Deposit</button>
<button onclick="showWithdrawal()">💸 Withdrawal</button>
<button onclick="showAbout()">📊 About VERBOSE</button>
</div>

<div class="deposit" id="deposit">
<h2>Deposit Funds</h2>
<select id="planSelect" onchange="fillAmount()"></select>
<input type="text" id="amountInput" readonly>
<select id="paymentMethod" onchange="copyNumber()">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">Easypaisa</option>
</select>
<input type="text" id="paymentNumber" readonly>
<input type="text" id="transactionId" placeholder="Transaction ID">
<input type="file" id="uploadProof">
<button onclick="submitDeposit()">Submit Deposit</button>
<button onclick="backDashboard()">Back</button>
</div>

<div class="withdrawal" id="withdrawal">
<h2>Withdrawal</h2>
<select id="withdrawMethod">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">Easypaisa</option>
</select>
<input type="text" id="accountNumber" placeholder="Account Number">
<input type="number" id="withdrawAmount" placeholder="Amount">
<button onclick="submitWithdrawal()">Submit Withdrawal</button>
<button onclick="backDashboard()">Back</button>
</div>

<div class="about" id="about">
<h2>About VERBOSE</h2>
<p>VERBOSE ek cutting-edge digital earning platform hai jo users ko simple aur transparent ways me online income generate karne ka mauka deta hai. Humari mission hai har individual ko financial freedom aur flexible earning opportunities provide karna. Humari team advanced technology aur secure systems ka use kar ke seamless experience ensure karti hai. VERBOSE ke plans fully transparent, reliable aur user-friendly hain. Har user apni growth track kar sakta hai aur daily profits instantly check kar sakta hai. Humari special offers aur turbo-boost plans users ko fast-track earning ka option dete hain. Customer support always available hai, taake koi bhi query ya issue instantly solve ho jaye. VERBOSE har step me trustworthy aur innovative solutions provide karta hai, jahan har investor confident feel kare.</p>
<button onclick="backDashboard()">Back</button>
</div>

</div>

<script>
let currentUser = JSON.parse(localStorage.getItem('currentUser')) || null;
const users = JSON.parse(localStorage.getItem('users')) || [];

const plans=[
  {name:'Plan 1', invest:250, days:25, totalProfit:900},
  {name:'Plan 2', invest:500, days:30, totalProfit:2000},
  {name:'Plan 3', invest:1000, days:35, totalProfit:4500},
  {name:'Plan 4', invest:1500, days:40, totalProfit:7000},
  {name:'Plan 5', invest:2000, days:45, totalProfit:9500},
  {name:'Plan 6', invest:3000, days:50, totalProfit:15000},
  {name:'Plan 7', invest:4000, days:60, totalProfit:10000},
  {name:'Plan 8', invest:5000, days:70, totalProfit:12500},
  {name:'Plan 9', invest:6000, days:80, totalProfit:15000},
  {name:'Plan 10', invest:7000, days:90, totalProfit:17500},
  {name:'Plan 11', invest:8000, days:100, totalProfit:20000},
  {name:'Plan 12', invest:9000, days:110, totalProfit:22500},
  {name:'Plan 13', invest:10000, days:120, totalProfit:25000},
  {name:'Plan 14', invest:12000, days:130, totalProfit:30000},
  {name:'Plan 15', invest:14000, days:140, totalProfit:35000},
  {name:'Plan 16', invest:16000, days:150, totalProfit:40000},
  {name:'Plan 17', invest:18000, days:160, totalProfit:45000},
  {name:'Plan 18', invest:20000, days:170, totalProfit:50000},
  {name:'Plan 19', invest:25000, days:180, totalProfit:62500},
  {name:'Plan 20', invest:35000, days:200, totalProfit:87500},
  {name:'Plan 21', invest:40000, days:210, totalProfit:100000},
  {name:'Plan 22', invest:45000, days:220, totalProfit:112500},
  {name:'Plan 23', invest:50000, days:230, totalProfit:125000},
  {name:'Plan 24', invest:55000, days:240, totalProfit:137500},
  {name:'Plan 25', invest:60000, days:250, totalProfit:150000}
];

function signup(){
  const u=document.getElementById('username').value;
  const p=document.getElementById('password').value;
  if(!u||!p){alert('Enter username & password');return;}
  if(users.find(x=>x.username===u)){alert('User exists');return;}
  users.push({username:u,password:p,balance:0});
  localStorage.setItem('users',JSON.stringify(users));
  alert('Signup Success');
}

function login(){
  const u=document.getElementById('username').value;
  const p=document.getElementById('password').value;
  const user=users.find(x=>x.username===u && x.password===p);
  if(!user){alert('Invalid');return;}
  currentUser=user;
  localStorage.setItem('currentUser',JSON.stringify(currentUser));
  showDashboard();
}

function googleLogin(){
  alert('Google login simulated for demo.');
  currentUser={username:'GmailUser', balance:0};
  localStorage.setItem('currentUser',JSON.stringify(currentUser));
  showDashboard();
}

function showDashboard(){
  document.getElementById('auth').classList.remove('active');
  document.getElementById('deposit').classList.remove('active');
  document.getElementById('withdrawal').classList.remove('active');
  document.getElementById('about').classList.remove('active');
  document.getElementById('dashboard').classList.add('active');
  document.getElementById('userDisplay').innerText=currentUser.username;
  document.getElementById('userBalance').innerText=currentUser.balance || 0;
  loadPlans();
  fillPlanSelect();
}

function logout(){
  currentUser=null;
  localStorage.removeItem('currentUser');
  document.getElementById('dashboard').classList.remove('active');
  document.getElementById('auth').classList.add('active');
}

function loadPlans(){
  const grid=document.getElementById('planGrid');
  grid.innerHTML='';
  plans.forEach(p=>{
    const daily=Math.round(p.totalProfit/p.days);
    const card=document.createElement('div');
    card.className='plan-card';
    const badge = p.totalProfit>10000 ? `<div class="badge">🔥Turbo🔥</div>` : '';
    card.innerHTML=`${badge}<h3>${p.name}</h3>
      <p>Invest: ${p.invest} PKR</p>
      <p>Days: ${p.days}</p>
      <p>Daily Profit: ${daily} PKR</p>
      <p>Total Profit: ${p.totalProfit} PKR</p>
      <p>Total Return: ${p.invest+p.totalProfit} PKR</p>`;
    grid.appendChild(card);
  });
}

function fillPlanSelect(){
  const sel=document.getElementById('planSelect');
  sel.innerHTML='';
  plans.forEach((p,i)=>{
    const opt=document.createElement('option');
    opt.value=i;
    opt.innerText=`${p.name} — ${p.invest} PKR`;
    sel.appendChild(opt);
  });
  fillAmount();
}

function fillAmount(){
  const idx=document.getElementById('planSelect').value;
  document.getElementById('amountInput').value=plans[idx].invest;
}

function copyNumber(){
  const method=document.getElementById('paymentMethod').value;
  document.getElementById('paymentNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}

function submitDeposit(){
  const amount=parseInt(document.getElementById('amountInput').value);
  currentUser.balance = (currentUser.balance||0)+amount;
  document.getElementById('userBalance').innerText=currentUser.balance;
  saveCurrentUser();
  alert('Deposit submitted!');
}

function submitWithdrawal(){
  const amount=parseInt(document.getElementById('withdrawAmount').value);
  if(amount>currentUser.balance){alert('Insufficient balance');return;}
  currentUser.balance -= amount;
  document.getElementById('userBalance').innerText=currentUser.balance;
  saveCurrentUser();
  alert('Withdrawal submitted!');
}

function saveCurrentUser(){
  const idx=users.findIndex(u=>u.username===currentUser.username);
  if(idx>=0){users[idx]=currentUser; localStorage.setItem('users',JSON.stringify(users));}
}

function showDeposit(){document.getElementById('dashboard').classList.remove('active');document.getElementById('deposit').classList.add('active');fillPlanSelect();copyNumber();}
function showWithdrawal(){document.getElementById('dashboard').classList.remove('active');document.getElementById('withdrawal').classList.add('active');}
function showAbout(){document.getElementById('dashboard').classList.remove('active');document.getElementById('about').classList.add('active');}
function backDashboard(){document.querySelectorAll('.dashboard,.deposit,.withdrawal,.about').forEach(el=>el.classList.remove('active'));document.getElementById('dashboard').classList.add('active');}

window.onload = function(){
  if(currentUser){showDashboard();}
}
</script>
</body>
</html>
