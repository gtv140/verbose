<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE</title>
<style>
body {
  margin:0;
  font-family:Arial,sans-serif;
  background:#000;
  color:#fff;
  overflow-x:hidden;
  position:relative;
}
header {
  text-align:center;
  padding:25px 0;
  font-size:30px;
  font-weight:bold;
  letter-spacing:2px;
  background:linear-gradient(90deg,#0ff,#00f);
  -webkit-background-clip:text;
  -webkit-text-fill-color:transparent;
}
.login-box, .page {
  max-width:420px;
  margin:20px auto;
  background:#111;
  padding:25px;
  border-radius:12px;
  box-shadow:0 0 20px #0ff;
}
input, button, select {
  width:100%;
  padding:12px;
  margin-top:12px;
  border-radius:6px;
  border:1px solid #0ff;
  background:#000;
  color:#0ff;
  font-size:16px;
}
button {
  background:#0ff;
  border:none;
  cursor:pointer;
  transition:0.3s;
  font-weight:bold;
}
button:hover {
  background:#00f;
  color:#fff;
}
.nav {
  position:fixed;
  bottom:0;
  left:0;
  right:0;
  background:#111;
  display:flex;
  justify-content:space-around;
  padding:12px 0;
  border-top:2px solid #0ff;
  font-size:16px;
}
.nav div {text-align:center; cursor:pointer;}
.hidden {display:none;}
.user-box, .alert-box {
  padding:12px;
  border-radius:10px;
  margin-bottom:12px;
  font-weight:bold;
}
.user-box {background:#001f3f;}
.alert-box {background:#330000; color:#f00;}
.logout-btn {
  position:fixed;
  bottom:65px;
  left:50%;
  transform:translateX(-50%);
  background:red;
  color:#fff;
  padding:12px 18px;
  border-radius:6px;
  cursor:pointer;
  font-weight:bold;
  font-size:16px;
}
.plan-box {
  border:1px solid #0ff;
  padding:12px;
  margin:12px 0;
  border-radius:10px;
  background:#111;
  transition:0.3s;
}
.plan-box:hover {
  box-shadow:0 0 20px #0ff;
}
.offer {color:#0ff;font-weight:bold;}
.countdown {font-weight:bold;color:#0ff;margin-top:5px; font-size:15px;}
@keyframes neonbg{
  0%{background-position:0 0;}
  50%{background-position:100% 100%;}
  100%{background-position:0 0;}
}
body {background:linear-gradient(45deg,#000,#001f3f,#000,#001f3f);background-size:400% 400%;animation:neonbg 25s linear infinite;}
</style>
</head>
<body>

<header>VERBOSE</header>

<!-- LOGIN / SIGNUP -->
<div id="loginPage" class="login-box">
<h2 style="text-align:center;color:#0ff;">Login / Signup</h2>
<input id="user" placeholder="Username">
<input id="pass" placeholder="Password" type="password">
<button onclick="login()">Login</button>
<button onclick="signup()">Signup</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
<div class="alert-box">For deposits, withdrawals or account issues, contact our admin immediately.</div>
<div class="user-box">Username: <span id="dashUser"></span> | Balance: Rs <span id="dashBalance">0</span> | Daily Profit: Rs <span id="dailyProfit">0</span></div>
<h2 style="color:#0ff;">Dashboard</h2>
<p style="color:#0ff;">Welcome to VERBOSE! Secure, reliable, and professional investment platform.</p>
<div id="referralDiv" style="margin-top:10px;">Referral Link: <input id="referralLink" readonly style="background:#000;color:#0ff"></div>
<button class="logout-btn" onclick="logout()">Logout</button>
</div>

<!-- PLANS -->
<div id="plans" class="page hidden">
<h2 style="color:#0ff;">Plans</h2>
<div id="plansList"></div>
</div>

<!-- DEPOSIT -->
<div id="deposit" class="page hidden">
<h2 style="color:#0ff;">Deposit</h2>
<label>Method</label>
<select id="depositMethod" onchange="updateDepositNumber()">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<input id="depositNumber" readonly>
<button onclick="copyDepositNumber()">Copy Number</button>
<label>Amount</label>
<input id="depositAmount" readonly>
<label>Transaction ID</label>
<input id="depositTxId" placeholder="TX ID">
<label>Upload Proof</label>
<input type="file" id="depositProof">
<button onclick="submitDeposit()">Submit Deposit</button>
</div>

<!-- WITHDRAWAL -->
<div id="withdrawal" class="page hidden">
<h2 style="color:#0ff;">Withdrawal</h2>
<label>Method</label>
<select id="withdrawMethod">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
<option value="bank">Bank</option>
</select>
<input id="withdrawUsername" readonly>
<input id="withdrawAccount" placeholder="Account Number">
<input id="withdrawAmount" placeholder="Amount">
<button onclick="submitWithdraw()">Request Withdrawal</button>
</div>

<!-- SUPPORT -->
<div id="support" class="page hidden">
<h2 style="color:#0ff;">Contact Administration</h2>
<p>Our professional support team is available 24/7 for any deposit, withdrawal, or account issues.</p>
<p>WhatsApp: <a href="https://chat.whatsapp.com/Kmaiv3VdSo09rio4qcRTRM" target="_blank" style="color:#0ff;">Join WhatsApp Group</a></p>
<p>Email: <a href="mailto:rock.earn92@gmail.com" style="color:#0ff;">rock.earn92@gmail.com</a></p>
<p>VERBOSE ensures safe and transparent operations for all users.</p>
</div>

<!-- NAV -->
<div id="bottomNav" class="nav hidden">
<div onclick="showPage('dashboard')">🏠<br>Home</div>
<div onclick="showPage('plans')">📦<br>Plans</div>
<div onclick="showPage('deposit')">💰<br>Deposit</div>
<div onclick="showPage('withdrawal')">💵<br>Withdraw</div>
<div onclick="showPage('support')">📞<br>Support</div>
</div>

<script>
// USERS & LOCAL STORAGE
let currentUser = localStorage.getItem('verbose_user') || null;
let balance = parseFloat(localStorage.getItem('verbose_balance')) || 0;
let dailyProfit = parseFloat(localStorage.getItem('verbose_dailyProfit')) || 0;
let plansData = [];
let userPlans = JSON.parse(localStorage.getItem('verbose_userPlans')||'[]');

// CREATE 25 PLANS
for(let i=1;i<=25;i++){
    let invest = Math.round(200 + (i-1)*(30000-200)/24);
    let days = 20 + Math.floor((i-1)*(70-20)/24);
    let multiplier = i<=7 ? 3 : 2.5;
    plansData.push({id:i,name:`Plan ${i}`,invest:invest,days:days,total:Math.round(invest*multiplier),multiplier:multiplier,offer:i<=7});
}

// LOGIN & SIGNUP
function login(){
    let u=document.getElementById("user").value.trim();
    let p=document.getElementById("pass").value.trim();
    if(!u||!p){alert("Enter username & password");return;}
    if(!localStorage.getItem("user_"+u)){alert("User not found. Please signup first."); return;}
    let data=JSON.parse(localStorage.getItem("user_"+u));
    if(data.password!==p){alert("Incorrect password"); return;}
    currentUser=u; balance=data.balance; dailyProfit=data.dailyProfit; userPlans=data.userPlans || [];
    localStorage.setItem('verbose_user',currentUser);
    localStorage.setItem('verbose_balance',balance);
    localStorage.setItem('verbose_dailyProfit',dailyProfit);
    localStorage.setItem('verbose_userPlans',JSON.stringify(userPlans));
    document.getElementById("dashUser").innerText=currentUser;
    document.getElementById("dashBalance").innerText=balance;
    document.getElementById("dailyProfit").innerText=dailyProfit;
    document.getElementById("loginPage").classList.add("hidden");
    document.getElementById("dashboard").classList.remove("hidden");
    document.getElementById("bottomNav").classList.remove("hidden");
    renderPlans(); updateWithdrawUsername(); updateReferral(); addDailyProfit();
}
function signup(){
    let u=document.getElementById("user").value.trim();
    let p=document.getElementById("pass").value.trim();
    if(!u||!p){alert("Enter username & password");return;}
    if(localStorage.getItem("user_"+u)){alert("Username already exists"); return;}
    let data={password:p,balance:0,dailyProfit:0,userPlans:[]};
    localStorage.setItem("user_"+u,JSON.stringify(data));
    alert("Signup successful. Please login.");
}

// LOGOUT
function logout(){if(!currentUser) return; saveUserData(); currentUser=null; document.getElementById("loginPage").classList.remove("hidden"); document.getElementById("dashboard").classList.add("hidden"); document.getElementById("bottomNav").classList.add("hidden"); document.getElementById("user").value=''; document.getElementById("pass").value='';}
function saveUserData(){if(!currentUser) return; let data={password:JSON.parse(localStorage.getItem("user_"+currentUser)).password,balance:balance,dailyProfit:dailyProfit,userPlans:userPlans}; localStorage.setItem("user_"+currentUser,JSON.stringify(data));}

// SHOW PAGE
function showPage(id){let pages=document.querySelectorAll(".page");pages.forEach(p=>p.classList.add("hidden"));document.getElementById(id).classList.remove("hidden");}

// DEPOSIT
const depositNumbers={jazzcash:'03705519562',easypaisa:'03379827882'};
function updateDepositNumber(){document.getElementById('depositNumber').value=depositNumbers[document.getElementById('depositMethod').value];}
function copyDepositNumber(){let num=document.getElementById('depositNumber');num.select(); num.setSelectionRange(0,99999); document.execCommand('copy'); alert("Deposit number copied!");}
function submitDeposit(){let tx=document.getElementById('depositTxId').value.trim(); let proof=document.getElementById('depositProof').files[0]; let amount=parseFloat(document.getElementById('depositAmount').value); if(!tx||!proof){alert("Fill TX ID & upload proof");return;} balance+=amount; dailyProfit+=Math.round(amount*0.006); localStorage.setItem('verbose_balance',balance); localStorage.setItem('verbose_dailyProfit',dailyProfit); document.getElementById('dashBalance').innerText=balance; document.getElementById('dailyProfit').innerText=dailyProfit; alert("Deposit submitted! Admin will verify."); document.getElementById('depositTxId').value=''; document.getElementById('depositProof').value=''; saveUserData(); showPage('dashboard');}

// WITHDRAWAL
function updateWithdrawUsername(){document.getElementById('withdrawUsername').value=currentUser;}
function submitWithdraw(){let amt=parseFloat(document.getElementById('withdrawAmount').value); let acc=document.getElementById('withdrawAccount').value.trim(); if(!amt||!acc){alert("Enter amount & account"); return;} if(amt>balance){alert("Insufficient balance"); return;} balance-=amt; document.getElementById('dashBalance').innerText=balance; alert(`Withdrawal request of Rs ${amt} received. Admin will process it.`); document.getElementById('withdrawAmount').value=''; document.getElementById('withdrawAccount').value=''; saveUserData(); showPage('dashboard');}

// PLANS
function renderPlans(){let list=document.getElementById("plansList");list.innerHTML=''; plansData.forEach(p=>{let div=document.createElement('div'); div.className='plan-box'; div.innerHTML=`<b>${p.name}</b> ${p.offer?'<span class="offer">🔥 24h Offer</span>':''}<br>Invest: Rs ${p.invest}<br>Days: ${p.days}<br>Total Profit: Rs ${p.total}<br>Daily Profit: Rs ${Math.round(p.total/p.days)}<br><span class="countdown" id="countdown${p.id}"></span><br><button onclick="buyPlan(${p.id})">Buy Now</button>`; list.appendChild(div); if(p.offer) startCountdown(p.id,24*60*60);});}

// COUNTDOWN
let countdownIntervals={};
function startCountdown(id,seconds){let display=document.getElementById(`countdown${id}`); let endTime = localStorage.getItem(`offer_end_${id}`); let end = endTime ? parseInt(endTime) : Date.now()+seconds*1000; localStorage.setItem(`offer_end_${id}`, end); clearInterval(countdownIntervals[id]); countdownIntervals[id]=setInterval(()=>{let diff=Math.floor((end-Date.now())/1000); if(diff<=0){display.innerText="Offer Expired"; clearInterval(countdownIntervals[id]); return;} let h=Math.floor(diff/3600); let m=Math.floor((diff%3600)/60); let s=diff%60; display.innerText=`Offer ends in ${h}h ${m}m ${s}s`;},1000);}

// BUY PLAN
function buyPlan(id){let plan=plansData.find(p=>p.id===id); document.getElementById('depositAmount').value=plan.invest; document.getElementById('depositMethod').value='jazzcash'; updateDepositNumber(); showPage('deposit'); if(!userPlans.find(p=>p.planId===id)){userPlans.push({planId:id,lastUpdate:Date.now(),dailyProfit:Math.round(plan.total/plan.days)}); saveUserData();}}

// DAILY PROFIT AUTO ADD
function addDailyProfit(){if(!userPlans.length) return; let now=Date.now(); userPlans.forEach(p=>{let last=p.lastUpdate||now; let daysPassed=Math.floor((now-last)/(1000*60*60*24)); if(daysPassed>0){balance+=p.dailyProfit*daysPassed; dailyProfit+=p.dailyProfit*daysPassed; p.lastUpdate=now;}}); document.getElementById('dashBalance').innerText=balance; document.getElementById('dailyProfit').innerText=dailyProfit; saveUserData();}

// REFERRAL
function updateReferral(){if(!currentUser) return; document.getElementById('referralLink').value=`https://gtv140.github.io/verbose/?ref=${currentUser}`;}

// ONLOAD
window.onload=function(){if(currentUser){let data=JSON.parse(localStorage.getItem("user_"+currentUser)); balance=data.balance; dailyProfit=data.dailyProfit; userPlans=data.userPlans || []; document.getElementById("dashUser").innerText=currentUser; document.getElementById("dashBalance").innerText=balance; document.getElementById("dailyProfit").innerText=dailyProfit; document.getElementById("loginPage").classList.add("hidden"); document.getElementById("dashboard").classList.remove("hidden"); document.getElementById("bottomNav").classList.remove("hidden"); renderPlans(); updateWithdrawUsername(); updateReferral(); addDailyProfit();}};
</script>
</body>
</html>
