<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE Premium</title>
<style>
/* BACKGROUND ANIMATION */
body{
  margin:0;
  font-family:Arial;
  overflow-x:hidden;
  background: linear-gradient(270deg, #ff00cc, #3333ff, #00ffcc, #ffcc00);
  background-size: 800% 800%;
  animation: gradientBG 20s ease infinite;
  color:#fff;
}
@keyframes gradientBG{
  0%{background-position:0% 50%;}
  50%{background-position:100% 50%;}
  100%{background-position:0% 50%;}
}

/* FLOATING PARTICLES */
.particles{
  position:fixed;
  top:0; left:0;
  width:100%; height:100%;
  pointer-events:none;
  z-index:0;
}
.particle{
  position:absolute;
  border-radius:50%;
  opacity:0.7;
  animation: floatUp linear infinite;
}
@keyframes floatUp{
  0%{transform:translateY(100vh);}
  100%{transform:translateY(-10vh);}
}

/* HEADER */
header{
  background:rgba(0,0,0,0.6);
  color:#0ff;
  padding:15px;
  text-align:center;
  font-size:26px;
  font-weight:bold;
  text-shadow:0 0 10px #0ff, 0 0 20px #0ff;
  position:relative;
  z-index:1;
}

/* PAGE BOXES */
.login-box,.page{
  max-width:400px;
  margin:20px auto;
  background:rgba(0,0,0,0.6);
  padding:20px;
  border-radius:15px;
  box-shadow:0 0 20px #0ff;
  position:relative;
  z-index:1;
  transition:all 0.5s ease;
}

/* INPUTS & BUTTONS */
input,select,button{
  width:100%;
  padding:12px;
  margin-top:10px;
  border-radius:8px;
  border:1px solid #0ff;
  background:rgba(0,0,0,0.4);
  color:#0ff;
  font-weight:bold;
}
button{
  cursor:pointer;
  transition:0.3s;
  box-shadow:0 0 10px #0ff,0 0 20px #0ff;
}
button:hover{
  transform:scale(1.05);
  box-shadow:0 0 20px #0ff,0 0 40px #0ff;
}

/* NAVIGATION */
.nav{
  position:fixed;
  bottom:0; left:0; right:0;
  background:rgba(0,0,0,0.6);
  display:flex;
  justify-content:space-around;
  padding:12px 0;
  border-top:1px solid #0ff;
  z-index:1;
}
.nav div{text-align:center;font-size:12px;cursor:pointer;transition:0.3s;}
.nav div:hover{color:#ff0; transform:scale(1.2);}

/* SPECIAL STYLES */
.hidden{display:none;}
.user-box{background:rgba(0,255,255,0.1);padding:10px;border-radius:8px;margin-bottom:10px;font-weight:bold; text-align:center;}
.alert-box{background:rgba(255,0,0,0.2);padding:8px;border-radius:5px;margin-bottom:10px;color:#f00;font-weight:bold; text-align:center;}
.logout-btn{position:fixed;bottom:60px;right:15px;background:red;color:#fff;padding:8px 12px;border-radius:5px;cursor:pointer;}
.plan-box{border:1px solid #0ff;padding:12px;margin:10px 0;border-radius:10px;background:rgba(0,0,0,0.4); text-align:center; transition:0.3s;}
.plan-box:hover{transform:scale(1.05); box-shadow:0 0 15px #0ff,0 0 30px #0ff;}
.offer{color:#ff0; font-weight:bold; text-shadow:0 0 5px #ff0;}

/* COUNTDOWN */
#offerTimer{font-size:18px; font-weight:bold; color:#ff0; text-align:center; margin-top:10px; text-shadow:0 0 10px #ff0;}
</style>
</head>
<body>

<!-- PARTICLES -->
<div class="particles" id="particles"></div>

<header>VERBOSE Premium</header>

<!-- LOGIN -->
<div id="loginPage" class="login-box">
<h2>Login / Signup</h2>
<input id="user" placeholder="Username">
<input id="pass" placeholder="Password" type="password">
<button onclick="login()">Login</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
<div class="alert-box">For any deposit, withdrawal, or account issues, contact support immediately.</div>
<div class="user-box">Username: <span id="dashUser"></span> | Balance: Rs <span id="dashBalance">0</span></div>
<h2>Dashboard</h2>
<p>Welcome to VERBOSE Premium! Secure, reliable, professional investment platform. Our team ensures smooth and safe transactions for millions of users daily.</p>
<button class="logout-btn" onclick="logout()">Logout</button>
</div>

<!-- PLANS -->
<div id="plans" class="page hidden">
<h2>Plans</h2>
<div id="plansList"></div>
<div id="offerTimer"></div>
</div>

<!-- DEPOSIT -->
<div id="deposit" class="page hidden">
<h2>Deposit</h2>
<label>Method</label>
<select id="depositMethod" onchange="updateDepositNumber()">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<input id="depositNumber" readonly>
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
</div>

<!-- SUPPORT -->
<div id="support" class="page hidden">
<h2>Contact Administration</h2>
<p>For any deposit, withdrawal, or account issues, contact our professional support team immediately.</p>
<p>WhatsApp: <a href="https://chat.whatsapp.com/Kmaiv3VdSo09rio4qcRTRM" target="_blank">Join WhatsApp Group</a></p>
<p>Email: <a href="mailto:rock.earn92@gmail.com">rock.earn92@gmail.com</a></p>
<p>VERBOSE Premium is a professional investment platform offering secure, reliable, and transparent investment services. Our team is available 24/7 to assist with all inquiries.</p>
</div>

<!-- NAVIGATION -->
<div id="bottomNav" class="nav hidden">
<div onclick="showPage('dashboard')">🏠<br>Home</div>
<div onclick="showPage('plans')">📦<br>Plans</div>
<div onclick="showPage('deposit')">💰<br>Deposit</div>
<div onclick="showPage('withdrawal')">💵<br>Withdraw</div>
<div onclick="showPage('support')">📞<br>Support</div>
</div>

<script>
// PARTICLES
const particlesContainer=document.getElementById('particles');
for(let i=0;i<50;i++){
  let p=document.createElement('div');
  p.className='particle';
  let size=Math.random()*6+4;
  p.style.width=size+'px';
  p.style.height=size+'px';
  p.style.background='rgba(255,255,255,'+Math.random()+')';
  p.style.left=Math.random()*100+'%';
  p.style.animationDuration=(5+Math.random()*10)+'s';
  p.style.animationDelay=Math.random()*10+'s';
  particlesContainer.appendChild(p);
}

// USERS & PLANS
let currentUser=localStorage.getItem('verbose_user')||null;
let balance=parseFloat(localStorage.getItem('verbose_balance'))||0;
let plansData=[],userPlans=JSON.parse(localStorage.getItem('verbose_userPlans')||'[]');

// CREATE 25 PLANS, 7 special
for(let i=1;i<=25;i++){
  let invest=Math.round(200+(i-1)*(30000-200)/24);
  let days=20+Math.floor((i-1)*(70-20)/24);
  let multiplier=i<=7?3:2.5;
  plansData.push({id:i,name:`Plan ${i}`,invest:invest,days:days,total:Math.round(invest*multiplier),multiplier:multiplier,offer:i<=7});
}

// LOGIN
function login(){
  let u=document.getElementById("user").value;
  let p=document.getElementById("pass").value;
  if(!u||!p){alert("Enter username & password"); return;}
  currentUser=u;
  localStorage.setItem('verbose_user',currentUser);
  if(!localStorage.getItem('verbose_balance')) localStorage.setItem('verbose_balance','5000');
  balance=parseFloat(localStorage.getItem('verbose_balance'));
  document.getElementById("dashUser").innerText=currentUser;
  document.getElementById("dashBalance").innerText=balance;
  document.getElementById("loginPage").classList.add("hidden");
  document.getElementById("dashboard").classList.remove("hidden");
  document.getElementById("bottomNav").classList.remove("hidden");
  renderPlans(); updateWithdrawUsername(); addDailyProfit();
}

// LOGOUT
function logout(){
  currentUser=null; localStorage.removeItem('verbose_user');
  document.getElementById("loginPage").classList.remove("hidden");
  document.getElementById("dashboard").classList.add("hidden");
  document.getElementById("bottomNav").classList.add("hidden");
  document.getElementById("user").value=''; document.getElementById("pass").value='';
}

// SHOW PAGE
function showPage(id){
  document.querySelectorAll(".page").forEach(p=>p.classList.add("hidden"));
  document.getElementById(id).classList.remove("hidden");
}

// RENDER PLANS
function renderPlans(){
  let list=document.getElementById("plansList");
  list.innerHTML='';
  plansData.forEach(p=>{
    let div=document.createElement('div');
    div.className='plan-box';
    div.innerHTML=`<b>${p.name}</b> ${p.offer?'<span class="offer">🔥 24h Offer</span>':''}<br>
    Invest: Rs ${p.invest}<br>
    Days: ${p.days}<br>
    Total Profit: Rs ${p.total}<br>
    Daily Profit: Rs ${Math.round(p.total/p.days)}<br>
    <button onclick="buyPlan(${p.id})">Buy Now</button>`;
    list.appendChild(div);
  });
}

// BUY PLAN
function buyPlan(id){
  let plan=plansData.find(p=>p.id===id);
  document.getElementById('depositAmount').value=plan.invest;
  document.getElementById('depositMethod').value='jazzcash';
  updateDepositNumber();
  showPage('deposit');
  if(!userPlans.find(p=>p.planId===id)){
    userPlans.push({planId:id,lastUpdate:Date.now(),dailyProfit:Math.round(plan.total/plan.days)});
    localStorage.setItem('verbose_userPlans',JSON.stringify(userPlans));
  }
}

// DEPOSIT
const depositNumbers={jazzcash:'03705519562',easypaisa:'03379827882'};
function updateDepositNumber(){
  document.getElementById('depositNumber').value=depositNumbers[document.getElementById('depositMethod').value];
}
function submitDeposit(){
  let tx=document.getElementById('depositTxId').value.trim();
  let proof=document.getElementById('depositProof').files[0];
  let amount=parseFloat(document.getElementById('depositAmount').value);
  if(!tx||!proof){alert("Fill TX ID & upload proof");return;}
  balance+=amount; localStorage.setItem('verbose_balance',balance);
  document.getElementById('dashBalance').innerText=balance;
  alert("Deposit submitted! Our team will verify it.");
  document.getElementById('depositTxId').value=''; document.getElementById('depositProof').value='';
  showPage('dashboard');
}

// WITHDRAWAL
function updateWithdrawUsername(){document.getElementById('withdrawUsername').value=currentUser;}
function submitWithdraw(){
  let amt=parseFloat(document.getElementById('withdrawAmount').value);
  let acc=document.getElementById('withdrawAccount').value.trim();
  if(!amt||!acc){alert("Enter amount & account"); return;}
  if(amt>balance){alert("Insufficient balance"); return;}
  balance-=amt; localStorage.setItem('verbose_balance',balance);
  document.getElementById('dashBalance').innerText=balance;
  alert(`Withdrawal request of Rs ${amt} received. Support will process it.`);
  document.getElementById('withdrawAmount').value=''; document.getElementById('withdrawAccount').value='';
  showPage('dashboard');
}

// DAILY PROFIT
function addDailyProfit(){
  let now=Date.now();
  userPlans.forEach(p=>{
    let last=p.lastUpdate||now;
    let daysPassed=Math.floor((now-last)/(1000*60*60*24));
    if(daysPassed>0){balance+=p.dailyProfit*daysPassed; p.lastUpdate=now;}
  });
  localStorage.setItem('verbose_balance',balance);
  document.getElementById('dashBalance').innerText=balance;
  localStorage.setItem('verbose_userPlans',JSON.stringify(userPlans));
}

// COUNTDOWN OFFER 24H
let offerEnd=Date.now()+24*60*60*1000;
function updateOfferTimer(){
  let now=Date.now();
  let diff=offerEnd-now;
  if(diff<0){document.getElementById('offerTimer').innerText='Offer expired'; return;}
  let h=Math.floor(diff/1000/60/60);
  let m=Math.floor(diff/1000/60)%60;
  let s=Math.floor(diff/1000)%60;
  document.getElementById('offerTimer').innerText=`🔥 24h Special Offer ends in ${h}h ${m}m ${s}s`;
}
setInterval(updateOfferTimer,1000);

// ONLOAD
window.onload=function(){
  if(currentUser){
    document.getElementById("dashUser").innerText=currentUser;
    balance=parseFloat(localStorage.getItem('verbose_balance'));
    document.getElementById("dashBalance").innerText=balance;
    document.getElementById("loginPage").classList.add("hidden");
    document.getElementById("dashboard").classList.remove("hidden");
    document.getElementById("bottomNav").classList.remove("hidden");
    renderPlans(); updateWithdrawUsername(); addDailyProfit();
  }
};
</script>
</body>
</html>
