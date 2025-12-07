<VERBOSE><html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE</title>
<style>
body{margin:0;font-family:Arial;background:#f2f2f2;}
header{background:#007bff;color:#fff;padding:15px;text-align:center;font-size:22px;font-weight:bold;}
.login-box,.page{max-width:400px;margin:20px auto;background:#fff;padding:20px;border-radius:10px;box-shadow:0 0 10px #ccc;}
input,button,select{width:100%;padding:10px;margin-top:10px;border-radius:5px;border:1px solid #aaa;}
button{background:#007bff;color:#fff;border:none;cursor:pointer;}
button:hover{background:#0056b3;}
.nav{position:fixed;bottom:0;left:0;right:0;background:#fff;display:flex;justify-content:space-around;padding:10px 0;border-top:1px solid #ccc;}
.nav div{text-align:center;font-size:12px;cursor:pointer;}
.hidden{display:none;}
.user-box{background:#e0f7ff;padding:10px;border-radius:8px;margin-bottom:10px;font-weight:bold;}
.alert-box{background:#ffe0e0;padding:8px;border-radius:5px;margin-bottom:10px;color:#900;font-weight:bold;}
.logout-btn{position:fixed;bottom:60px;left:15px;background:red;color:#fff;padding:8px 12px;border-radius:5px;cursor:pointer;}
</style>
</head>
<body><header>VERBOSE</header><!-- LOGIN --><div id="loginPage" class="login-box">
<h2>Login</h2>
<input id="user" placeholder="Username">
<input id="pass" placeholder="Password" type="password">
<button onclick="login()">Login</button>
</div><!-- DASHBOARD --><div id="dashboard" class="page hidden">
<div class="alert-box">Deposit/Withdrawal ke issues ya queries → Admin WhatsApp: 03705519562 / Email: rock.earn92@gmail.com</div>
<div class="user-box">Username: <span id="dashUser"></span> | Balance: Rs <span id="dashBalance">0</span></div>
<h2>Dashboard</h2>
<p>Welcome to VERBOSE! Trusted platform, millions of users, daily profits.</p>
<button class="logout-btn" onclick="logout()">Logout</button>
</div><!-- PLANS --><div id="plans" class="page hidden">
<h2>Plans</h2>
<div id="plansList"></div>
</div><!-- DEPOSIT --><div id="deposit" class="page hidden">
<h2>Deposit</h2>
<label>Method</label>
<select id="depositMethod" onchange="updateDepositNumber()">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<input id="depositNumber" readonly>
<label>Transaction ID</label>
<input id="depositTxId" placeholder="TX ID">
<label>Upload Proof</label>
<input type="file" id="depositProof">
<button onclick="submitDeposit()">Submit Deposit</button>
</div><!-- WITHDRAWAL --><div id="withdrawal" class="page hidden">
<h2>Withdrawal</h2>
<label>Method</label>
<select id="withdrawMethod">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
<option value="bank">Bank</option>
</select>
<input id="withdrawUsername" readonly>
<input id="withdrawAccount" placeholder="Account Number (manual)">
<input id="withdrawAmount" placeholder="Amount">
<button onclick="submitWithdraw()">Request Withdrawal</button>
</div><!-- SUPPORT --><div id="support" class="page hidden">
<h2>Support</h2>
<p>WhatsApp: 03705519562</p>
<p>Email: rock.earn92@gmail.com</p>
</div><!-- NAVIGATION --><div id="bottomNav" class="nav hidden">
<div onclick="showPage('dashboard')">🏠<br>Home</div>
<div onclick="showPage('plans')">📦<br>Plans</div>
<div onclick="showPage('deposit')">💰<br>Deposit</div>
<div onclick="showPage('withdrawal')">💵<br>Withdraw</div>
<div onclick="showPage('support')">📞<br>Support</div>
</div><script>
// USERS & BALANCE
let currentUser = null;
let balance = 0;
let plansData = [];
for(let i=1;i<=25;i++){
    let invest = i<=3 ? i*1000 : i*1200;
    let profitMultiplier = i<=3 ? 3 : 2.5;
    plansData.push({id:i,name:`Plan ${i}`,invest:invest,days:i*3,profit:invest*profitMultiplier});
}

// LOGIN
function login(){
let u=document.getElementById("user").value;
let p=document.getElementById("pass").value;
if(!u||!p){alert("Enter username & password");return;}
currentUser=u;
balance=5000; // starting balance
document.getElementById("dashUser").innerText=currentUser;
document.getElementById("dashBalance").innerText=balance;
document.getElementById("loginPage").classList.add("hidden");
document.getElementById("dashboard").classList.remove("hidden");
document.getElementById("bottomNav").classList.remove("hidden");
renderPlans();
updateWithdrawUsername();
}

// LOGOUT
function logout(){
currentUser=null;
balance=0;
document.getElementById("loginPage").classList.remove("hidden");
document.getElementById("dashboard").classList.add("hidden");
document.getElementById("bottomNav").classList.add("hidden");
document.getElementById("user").value='';
document.getElementById("pass").value='';
}

// SHOW PAGE
function showPage(id){
let pages=document.querySelectorAll(".page");
pages.forEach(p=>p.classList.add("hidden"));
document.getElementById(id).classList.remove("hidden");
}

// PLANS
function renderPlans(){
let list=document.getElementById("plansList");
list.innerHTML='';
plansData.forEach(p=>{
let div=document.createElement('div');
div.style.border='1px solid #ccc';
div.style.padding='10px';
div.style.margin='10px 0';
div.innerHTML=`<b>${p.name}</b><br>Invest: Rs ${p.invest}<br>Days: ${p.days}<br>Total Profit: Rs ${p.profit.toFixed(0)}<br><button onclick="buyPlan(${p.id})">Buy Now</button>`;
list.appendChild(div);
});
}

function buyPlan(id){
let plan=plansData.find(p=>p.id===id);
alert(`You selected ${plan.name}. Deposit Rs ${plan.invest} now.`);
showPage('deposit');
document.getElementById('depositMethod').value='jazzcash';
updateDepositNumber();
}

// DEPOSIT
const depositNumbers={jazzcash:'03705519562',easypaisa:'03379827882'};
function updateDepositNumber(){
let method=document.getElementById('depositMethod').value;
document.getElementById('depositNumber').value=depositNumbers[method];
}
function submitDeposit(){
let tx=document.getElementById('depositTxId').value.trim();
let proof=document.getElementById('depositProof').files[0];
if(!tx||!proof){alert("Fill TX ID & upload proof");return;}
balance+=1000; // simulate deposit
document.getElementById('dashBalance').innerText=balance;
alert("Deposit submitted! Admin will check.");
document.getElementById('depositTxId').value='';
document.getElementById('depositProof').value='';
showPage('dashboard');
}

// WITHDRAWAL
function updateWithdrawUsername(){document.getElementById('withdrawUsername').value=currentUser;}
function submitWithdraw(){
let amt=parseInt(document.getElementById('withdrawAmount').value);
let acc=document.getElementById('withdrawAccount').value.trim();
if(!amt||!acc){alert("Enter amount & account"); return;}
if(amt>balance){alert("Insufficient balance"); return;}
balance-=amt;
document.getElementById('dashBalance').innerText=balance;
alert(`Withdraw Rs ${amt} requested. Contact admin if needed.`);
document.getElementById('withdrawAmount').value='';
document.getElementById('withdrawAccount').value='';
showPage('dashboard');
}
</script></body>
</html>
