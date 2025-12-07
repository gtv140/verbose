<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE</title>
<style>
/* BASE */
body{
    margin:0;
    font-family:Arial, sans-serif;
    background:#000;
    color:#fff;
    overflow-x:hidden;
    position:relative;
}

/* NEON ANIMATED BACKGROUND */
@keyframes neonbg{
    0%{background-position:0 0;}
    50%{background-position:100% 100%;}
    100%{background-position:0 0;}
}
body{
    background:linear-gradient(45deg,#000,#001f3f,#000,#001f3f);
    background-size:400% 400%;
    animation:neonbg 30s linear infinite;
}

/* HEADER */
header{
    text-align:center;
    padding:25px;
    font-size:32px;
    font-weight:bold;
    letter-spacing:2px;
    background:linear-gradient(90deg,#0ff,#00f);
    -webkit-background-clip:text;
    -webkit-text-fill-color:transparent;
    text-shadow:0 0 5px #0ff, 0 0 10px #0ff;
}

/* PAGES */
.login-box,.page{
    max-width:400px;
    margin:20px auto;
    background:#111;
    padding:20px;
    border-radius:12px;
    box-shadow:0 0 20px #0ff;
}
input,button,select{
    width:100%;
    padding:10px;
    margin-top:10px;
    border-radius:5px;
    border:1px solid #0ff;
    background:#000;
    color:#0ff;
    font-weight:bold;
}
button{
    background:#0ff;
    border:none;
    cursor:pointer;
    transition:0.3s;
    font-weight:bold;
}
button:hover{
    background:#00f;
    color:#fff;
}

/* NAV */
.nav{
    position:fixed;
    bottom:0; left:0; right:0;
    background:#111;
    display:flex;
    justify-content:space-around;
    padding:12px 0;
    border-top:2px solid #0ff;
}
.nav div{text-align:center;font-size:18px;cursor:pointer;}

/* USER & ALERT */
.user-box{
    background:#001f3f;
    padding:12px;
    border-radius:10px;
    margin-bottom:12px;
    font-weight:bold;
    text-align:center;
    box-shadow:0 0 10px #0ff;
}
.alert-box{
    background:#330000;
    padding:10px;
    border-radius:5px;
    margin-bottom:12px;
    color:#f00;
    font-weight:bold;
    text-align:center;
}

/* LOGOUT */
.logout-btn{
    position:fixed;
    bottom:70px;
    left:50%;
    transform:translateX(-50%);
    background:red;
    color:#fff;
    padding:12px 22px;
    border-radius:6px;
    cursor:pointer;
    font-weight:bold;
    box-shadow:0 0 10px #f00;
}

/* PLAN */
.plan-box{
    border:1px solid #0ff;
    padding:12px;
    margin:12px 0;
    border-radius:10px;
    background:#111;
    transition:0.3s;
    text-align:center;
}
.plan-box:hover{
    box-shadow:0 0 15px #0ff;
}
.offer{color:#0ff;font-weight:bold;}
.countdown{
    font-weight:bold;
    color:#0ff;
    margin-top:5px;
}
</style>
</head>
<body>

<header>VERBOSE</header>

<!-- LOGIN -->
<div id="loginPage" class="login-box">
<h2>Login / Signup</h2>
<button onclick="newUser()">New User Signup</button>
<button onclick="existingUser()">Existing User Login</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
<div class="alert-box">For deposits, withdrawals, or issues, contact our professional support immediately.</div>
<div class="user-box">Username: <span id="dashUser"></span> | Balance: Rs <span id="dashBalance">0</span> | Daily Profit: Rs <span id="dashProfit">0</span></div>
<h2>Dashboard</h2>
<p>Welcome to VERBOSE – a premium, secure platform for investments and daily profits.</p>
<p>Referral Link: <input id="referralLink" readonly><button onclick="copyReferral()">Copy</button></p>
<button class="logout-btn" onclick="logout()">Logout</button>
</div>

<!-- PLANS -->
<div id="plans" class="page hidden">
<h2>Plans</h2>
<div id="plansList"></div>
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
<p>For deposits, withdrawals, or account issues, contact our professional team anytime.</p>
<p>WhatsApp: <a href="https://chat.whatsapp.com/Kmaiv3VdSo09rio4qcRTRM" target="_blank">Join WhatsApp Group</a></p>
<p>Email: <a href="mailto:rock.earn92@gmail.com">rock.earn92@gmail.com</a></p>
<p>VERBOSE ensures transparency, security, and automatic daily profit addition based on purchased plans.</p>
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
// USERS
let currentUser = null;
let balance = 0;
let dailyProfit = 0;
let plansData = [];
let countdownIntervals={};
let userData = JSON.parse(localStorage.getItem('verbose_allUsers')||'{}');

// PLANS
for(let i=1;i<=25;i++){
    let invest = Math.round(200 + (i-1)*(30000-200)/24);
    let days = 20 + Math.floor((i-1)*(70-20)/24);
    let multiplier = i<=7 ? 3 : 2.5;
    plansData.push({id:i,name:`Plan ${i}`,invest:invest,days:days,total:Math.round(invest*multiplier),multiplier:multiplier,offer:i<=7});
}

// LOGIN / SIGNUP
function newUser(){
    let u=prompt("Enter new username:");
    if(!u) return;
    if(userData[u]){alert("Username exists"); return;}
    userData[u]={balance:0,dailyProfit:0,plans:[],referrals:[]};
    localStorage.setItem('verbose_allUsers',JSON.stringify(userData));
    currentUser=u;
    loadDashboard();
}
function existingUser(){
    let u=prompt("Enter username:");
    if(!u || !userData[u]){alert("User not found"); return;}
    currentUser=u;
    loadDashboard();
}

// DASHBOARD
function loadDashboard(){
    let user=userData[currentUser];
    balance=user.balance;
    dailyProfit=user.dailyProfit;
    document.getElementById('dashUser').innerText=currentUser;
    document.getElementById('dashBalance').innerText=balance;
    document.getElementById('dashProfit').innerText=dailyProfit;
    document.getElementById('referralLink').value=`https://example.com/?ref=${currentUser}`;
    document.getElementById('loginPage').classList.add('hidden');
    document.getElementById('dashboard').classList.remove('hidden');
    document.getElementById('bottomNav').classList.remove('hidden');
    renderPlans();
}

// LOGOUT
function logout(){
    currentUser=null;
    document.getElementById('loginPage').classList.remove('hidden');
    document.getElementById('dashboard').classList.add('hidden');
    document.getElementById('bottomNav').classList.add('hidden');
}

// SHOW PAGE
function showPage(id){
    document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));
    document.getElementById(id).classList.remove('hidden');
}

// PLANS DISPLAY
function renderPlans(){
    let list=document.getElementById('plansList');
    list.innerHTML='';
    plansData.forEach(p=>{
        let div=document.createElement('div');
        div.className='plan-box';
        div.innerHTML=`<b>${p.name}</b> ${p.offer?'<span class="offer">🔥 24h Offer</span>':''}<br>
        Invest: Rs ${p.invest}<br>
        Days: ${p.days}<br>
        Total Profit: Rs ${p.total}<br>
        Daily Profit: Rs ${Math.round(p.total/p.days)}<br>
        <span class="countdown" id="countdown${p.id}"></span><br>
        <button onclick="buyPlan(${p.id})">Buy Now</button>`;
        list.appendChild(div);
        if(p.offer) startCountdown(p.id,24*60*60);
    });
}

// COUNTDOWN
function startCountdown(id,seconds){
    let display=document.getElementById(`countdown${id}`);
    let endTime=Date.now()+seconds*1000;
    clearInterval(countdownIntervals[id]);
    countdownIntervals[id]=setInterval(()=>{
        let diff=Math.floor((endTime-Date.now())/1000);
        if(diff<=0){display.innerText="Offer Expired"; clearInterval(countdownIntervals[id]); return;}
        let h=Math.floor(diff/3600);
        let m=Math.floor((diff%3600)/60);
        let s=diff%60;
        display.innerText=`Offer ends in ${h}h ${m}m ${s}s`;
    },1000);
}

// BUY PLAN
function buyPlan(id){
    let plan=plansData.find(p=>p.id===id);
    document.getElementById('depositAmount').value=plan.invest;
    document.getElementById('depositMethod').value='jazzcash';
    updateDepositNumber();
    showPage('deposit');
    let user=userData[currentUser];
    if(!user.plans.find(x=>x.planId===id)){
        user.plans.push({planId:id,lastUpdate:Date.now(),dailyProfit:Math.round(plan.total/plan.days)});
        userData[currentUser]=user;
        localStorage.setItem('verbose_allUsers',JSON.stringify(userData));
        dailyProfit=Math.round(plan.total/plan.days);
        document.getElementById('dashProfit').innerText=dailyProfit;
    }
}

// DEPOSIT
const depositNumbers={jazzcash:'03705519562',easypaisa:'03379827882'};
function updateDepositNumber(){
    document.getElementById('depositNumber').value=depositNumbers[document.getElementById('depositMethod').value];
}
function copyDepositNumber(){
    let num=document.getElementById('depositNumber');
    num.select(); num.setSelectionRange(0,99999);
    document.execCommand('copy'); alert("Deposit number copied!");
}
function submitDeposit(){
    let tx=document.getElementById('depositTxId').value.trim();
    let proof=document.getElementById('depositProof').files[0];
    let amt=parseFloat(document.getElementById('depositAmount').value);
    if(!tx||!proof){alert("Fill TX ID & upload proof"); return;}
    let user=userData[currentUser];
    user.balance+=amt;
    userData[currentUser]=user;
    localStorage.setItem('verbose_allUsers',JSON.stringify(userData));
    document.getElementById('dashBalance').innerText=user.balance;
    alert("Deposit submitted! Admin will verify.");
    document.getElementById('depositTxId').value='';
    document.getElementById('depositProof').value='';
    showPage('dashboard');
}

// WITHDRAWAL
function submitWithdraw(){
    let amt=parseFloat(document.getElementById('withdrawAmount').value);
    let acc=document.getElementById('withdrawAccount').value.trim();
    if(!amt||!acc){alert("Enter amount & account"); return;}
    let user=userData[currentUser];
    if(amt>user.balance){alert("Insufficient balance"); return;}
    user.balance-=amt;
    userData[currentUser]=user;
    localStorage.setItem('verbose_allUsers',JSON.stringify(userData));
    document.getElementById('dashBalance').innerText=user.balance;
    alert(`Withdrawal request of Rs ${amt} received. Admin will process it.`);
    document.getElementById('withdrawAmount').value='';
    document.getElementById('withdrawAccount').value='';
    showPage('dashboard');
}

// REFERRAL
function copyReferral(){
    let ref=document.getElementById('referralLink');
    ref.select(); ref.setSelectionRange(0,99999);
    document.execCommand('copy'); alert("Referral link copied!");
}

// DAILY PROFIT AUTOMATIC
setInterval(()=>{
    if(!currentUser) return;
    let user=userData[currentUser];
    if(user.plans.length===0) return;
    let now=Date.now();
    user.plans.forEach(p=>{
        let last=p.lastUpdate||now;
        let daysPassed=Math.floor((now-last)/(1000*60*60*24));
        if(daysPassed>0){
            p.lastUpdate=now;
            user.balance+=p.dailyProfit*daysPassed;
            user.dailyProfit=p.dailyProfit;
            document.getElementById('dashBalance').innerText=user.balance;
            document.getElementById('dashProfit').innerText=user.dailyProfit;
        }
    });
    userData[currentUser]=user;
    localStorage.setItem('verbose_allUsers',JSON.stringify(userData));
},1000);

// ONLOAD
window.onload=function(){
    if(currentUser) loadDashboard();
};
</script>
</body>
</html>
