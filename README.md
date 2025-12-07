<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>VERBOSE — Premium Neon App</title>

<style>
*{
 margin:0; padding:0; font-family:Arial;
 box-sizing:border-box;
}

body{
 background:#000;
 color:#fff;
 padding:10px;
}

/* HEADER */
header{
 text-align:center;
 font-size:24px;
 font-weight:bold;
 padding:12px;
 margin-bottom:15px;
 color:#0ff;
 text-shadow:0 0 15px #0ff;
}

/* CARD STYLE */
.card{
 background:#111;
 border:1px solid #0ff;
 border-radius:12px;
 padding:15px;
 margin-bottom:15px;
 box-shadow:0 0 12px #0ff;
}

/* BUTTONS */
.btn{
 width:100%;
 padding:12px;
 border:none;
 background:#0ff;
 color:#000;
 font-weight:bold;
 margin-top:10px;
 border-radius:8px;
 box-shadow:0 0 15px #0ff;
}

input, select{
 width:100%;
 padding:10px;
 background:#000;
 border:1px solid #0ff;
 border-radius:8px;
 color:#0ff;
 margin-top:8px;
}

.nav{
 position:fixed; bottom:0; left:0; width:100%;
 background:#000;
 border-top:2px solid #0ff;
 display:flex;
 justify-content:space-around;
 padding:10px 0;
}

.nav div{
 color:#0ff;
 text-align:center;
 font-size:12px;
}

section{ display:none; }

/* NEON TITLES */
h3{
 color:#0ff;
 text-shadow:0 0 10px #0ff;
 margin-bottom:10px;
}

/* PLAN CARDS */
.planBox{
 background:#000;
 border:1px solid #0ff;
 border-radius:10px;
 padding:10px;
 margin-bottom:10px;
 color:#0ff;
}

.buyBtn{
 background:#0ff;
 color:#000;
 padding:10px;
 width:100%;
 font-weight:bold;
 border:none;
 border-radius:6px;
 margin-top:8px;
 box-shadow:0 0 12px #0ff;
}
</style>
</head>

<body>

<header>⚡ VERBOSE Premium Neon</header>

<!-- LOGIN PAGE -->
<section id="loginPage" style="display:block">
<div class="card">
<h3>👤 Login Account</h3>

<label>Username</label>
<input id="loginUser">

<label>Password</label>
<input id="loginPass" type="password">

<button class="btn" onclick="login()">Login</button>

<p style="margin-top:10px;color:#0ff;">Not Registered? Create Account Below</p>
</div>

<div class="card">
<h3>✍️ Create Account</h3>

<label>Username</label>
<input id="regUser">

<label>Password</label>
<input id="regPass" type="password">

<button class="btn" onclick="register()">Create Account</button>
</div>
</section>

<!-- DASHBOARD -->
<section id="dashboard">
<div class="card">
<h3>🏠 Dashboard</h3>
<p id="welcomeName" style="font-size:18px;font-weight:bold;color:#0ff;"></p>
<p><strong>Balance: </strong> <span id="balance" style="color:#0ff;">0</span> PKR</p>
<p><strong>Daily Profit:</strong> <span id="dailyProfit" style="color:#0ff;">0</span> PKR</p>
<br>
<p style="color:#0ff;">
⚠ Deposit / Withdrawal issue?  
Administration se rabta karein:  
📞 03705519562  
✉ rock.earn92@gmail.com
</p>
</div>
</section>

<!-- PLANS PAGE -->
<section id="plansPage">
<div class="card">
<h3>📦 Special Offers (3x — 24 Hours)</h3>
<div id="specialPlans"></div>
</div>

<div class="card">
<h3>💼 Normal Plans (2.5x)</h3>
<div id="normalPlans"></div>
</div>
</section>

<!-- DEPOSIT -->
<section id="depositPage">
<div class="card">
<h3>📥 Deposit</h3>

<label>Amount</label>
<input id="depAmount" readonly>

<label>Method</label>
<select id="depMethod">
<option>JazzCash — 03705519562</option>
<option>Easypaisa — 03379827882</option>
</select>

<label>Transaction ID</label>
<input id="trxID">

<label>Upload Proof</label>
<input type="file" id="proof">

<button class="btn" onclick="makeDeposit()">Add Deposit</button>

</div>
</section>

<!-- WITHDRAW -->
<section id="withdrawPage">
<div class="card">
<h3>📤 Withdraw</h3>

<label>Amount</label>
<input id="wdAmount">

<label>Method</label>
<select id="wdMethod">
<option>JazzCash</option>
<option>Easypaisa</option>
<option>Bank Transfer</option>
</select>

<label>Your Number / Account</label>
<input id="wdAcc">

<button class="btn" onclick="withdraw()">Request Withdraw</button>
</div>
</section>

<!-- SUPPORT -->
<section id="supportPage">
<div class="card">
<h3>📞 Administration Support</h3>
<p>🟢 WhatsApp: <strong>03705519562</strong></p>
<p>✉ Email: <strong>rock.earn92@gmail.com</strong></p>
</div>
</section>

<!-- NAVIGATION -->
<div class="nav">
<div onclick="show('dashboard')">🏠 Home</div>
<div onclick="show('plansPage')">📦 Plans</div>
<div onclick="show('depositPage')">💰 Deposit</div>
<div onclick="show('withdrawPage')">💵 Withdraw</div>
<div onclick="show('supportPage')">📞 Support</div>
<div onclick="logout()">🚪 Logout</div>
</div>

<script>
// STORAGE SAFE LOGIN
function register(){
 let u = regUser.value;
 let p = regPass.value;
 if(!u || !p){ alert("Enter details"); return; }

 localStorage.setItem("acc_"+u, p);
 alert("Account Created!");
}

function login(){
 let u = loginUser.value;
 let p = loginPass.value;

 let chk = localStorage.getItem("acc_"+u);

 if(chk === p){
   localStorage.setItem("loggedUser", u);

   if(localStorage.getItem("bal_"+u)==null){
     localStorage.setItem("bal_"+u, 0);
     localStorage.setItem("profit_"+u,0);
   }

   loadDashboard();
   show('dashboard');
 } else {
   alert("Wrong username or password");
 }
}

// SAFE SHOW SECTIONS
function show(id){
 document.querySelectorAll("section").forEach(s=>s.style.display='none');
 document.getElementById(id).style.display='block';
 loadDashboard();
}

// DASHBOARD LOAD
function loadDashboard(){
 let u = localStorage.getItem("loggedUser");
 if(!u) return;

 welcomeName.innerHTML = "Welcome, " + u;

 balance.innerHTML = localStorage.getItem("bal_"+u);
 dailyProfit.innerHTML = localStorage.getItem("profit_"+u);
}

// LOGOUT SAFE
function logout(){
 localStorage.removeItem("loggedUser");
 show('loginPage');
}

// ======================
// PLANS
// ======================
let specials = [
200,300,500,700,1000,1500,3000
];

let normals = [
5000,7000,10000,12000,15000,18000,20000,
22000,24000,26000,28000,30000
];

// LOAD PLANS
function loadPlans(){
 let sp=""; let nm="";

 specials.forEach(a=>{
   sp+=`
   <div class='planBox'>
   <p>Plan Amount: ${a} PKR</p>
   <p>Profit: 3x</p>
   <p>Daily Profit: ${Math.round(a*3/30)} PKR</p>
   <button class='buyBtn' onclick="buyNow(${a})">Buy Now</button>
   </div>`;
 });

 normals.forEach(a=>{
   nm+=`
   <div class='planBox'>
   <p>Amount: ${a} PKR</p>
   <p>Profit: 2.5x</p>
   <p>Daily Profit: ${Math.round(a*2.5/30)} PKR</p>
   <button class='buyBtn' onclick="buyNow(${a})">Buy Now</button>
   </div>`;
 });

 specialPlans.innerHTML=sp;
 normalPlans.innerHTML=nm;
}

loadPlans();

// BUY NOW → Deposit Auto Fill
function buyNow(amount){
 depAmount.value = amount;
 show('depositPage');
}

// MAKE DEPOSIT
function makeDeposit(){
 let u = localStorage.getItem("loggedUser");
 if(!u) return;

 let amt = Number(depAmount.value);
 let bal = Number(localStorage.getItem("bal_"+u));

 localStorage.setItem("bal_"+u, bal+amt);

 alert("Deposit Added!");
 show('dashboard');
 loadDashboard();
}

// WITHDRAW
function withdraw(){
 alert("Withdrawal request submitted!");
 show('dashboard');
}
</script>

</body>
</html>
