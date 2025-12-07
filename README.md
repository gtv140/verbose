<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE</title>
<style>
/* BASE STYLES */
body{
    margin:0;
    font-family:Arial, sans-serif;
    background:#000;
    color:#fff;
    overflow-x:hidden;
    position:relative;
}
header{
    text-align:center;
    padding:20px;
    font-size:28px;
    font-weight:bold;
    letter-spacing:2px;
    background:linear-gradient(90deg,#0ff,#00f);
    -webkit-background-clip:text;
    -webkit-text-fill-color:transparent;
}
.login-box,.page{
    max-width:400px;
    margin:20px auto;
    background:#111;
    padding:20px;
    border-radius:10px;
    box-shadow:0 0 15px #0ff;
}
input,button,select{
    width:100%;
    padding:10px;
    margin-top:10px;
    border-radius:5px;
    border:1px solid #0ff;
    background:#000;
    color:#fff;
}
button{
    background:#0ff;
    border:none;
    cursor:pointer;
    transition:0.3s;
}
button:hover{
    background:#00f;
    color:#fff;
}
.nav{
    position:fixed;
    bottom:0; left:0; right:0;
    background:#111;
    display:flex;
    justify-content:space-around;
    padding:10px 0;
    border-top:1px solid #0ff;
    font-size:14px;
}
.nav div{text-align:center;cursor:pointer;}
.hidden{display:none;}
.user-box{
    background:#001f3f;
    padding:10px;
    border-radius:8px;
    margin-bottom:10px;
    font-weight:bold;
}
.alert-box{
    background:#330000;
    padding:8px;
    border-radius:5px;
    margin-bottom:10px;
    color:#f00;
    font-weight:bold;
}
.logout-btn{
    position:fixed;
    bottom:70px;
    left:50%;
    transform:translateX(-50%);
    background:red;
    color:#fff;
    padding:8px 15px;
    border-radius:5px;
    cursor:pointer;
}
.plan-box{
    border:1px solid #0ff;
    padding:10px;
    margin:10px 0;
    border-radius:8px;
    background:#111;
    transition:0.3s;
}
.plan-box:hover{
    box-shadow:0 0 10px #0ff;
}
.offer{color:#0ff;font-weight:bold;}
.countdown{
    font-weight:bold;
    color:#0ff;
    margin-top:5px;
}

/* NEON BACKGROUND */
@keyframes neonbg{
    0%{background-position:0 0;}
    50%{background-position:100% 100%;}
    100%{background-position:0 0;}
}
body{
    background:linear-gradient(45deg,#000,#001f3f,#000,#001f3f);
    background-size:400% 400%;
    animation:neonbg 20s linear infinite;
}
</style>
</head>
<body>

<header>VERBOSE</header>

<!-- LOGIN / SIGNUP -->
<div id="loginPage" class="login-box">
<h2>Login / Signup</h2>
<button onclick="showSignup()">New User Signup</button>
<button onclick="showLogin()">Existing Account Login</button>
<div id="loginForm" class="">
<input id="user" placeholder="Username">
<input id="pass" placeholder="Password" type="password">
<button onclick="login()">Login</button>
</div>
<div id="signupForm" class="hidden">
<input id="newUser" placeholder="New Username">
<input id="newPass" placeholder="Password" type="password">
<button onclick="signup()">Signup</button>
</div>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
<div class="alert-box">For any deposit, withdrawal, or account issues, contact admin immediately.</div>
<div class="user-box">Username: <span id="dashUser"></span> | Balance: Rs <span id="dashBalance">0</span> | Daily Profit: Rs <span id="dashDaily">0</span></div>
<h2>Dashboard</h2>
<p>Welcome to VERBOSE! Trusted platform, secure & reliable, millions of users daily profits.</p>
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

<!-- REFERRAL -->
<div id="referral" class="page hidden">
<h2>Referral</h2>
<p>Share your referral link to earn Rs 30 bonus when someone deposits using your link.</p>
<input id="refLink" readonly>
<button onclick="copyReferral()">Copy Link</button>
</div>

<!-- SUPPORT -->
<div id="support" class="page hidden">
<h2>Administration</h2>
<p>Need help? Our professional admin team is ready 24/7 to assist with deposits, withdrawals, and account inquiries.</p>
<p>WhatsApp: <a href="https://chat.whatsapp.com/Kmaiv3VdSo09rio4qcRTRM" target="_blank">Join WhatsApp Group</a></p>
<p>Email: <a href="mailto:rock.earn92@gmail.com">rock.earn92@gmail.com</a></p>
<p>VERBOSE is a secure, transparent investment platform with real-time profit tracking and smooth user experience.</p>
</div>

<!-- NAVIGATION -->
<div id="bottomNav" class="nav hidden">
<div onclick="showPage('dashboard')">🏠<br>Home</div>
<div onclick="showPage('plans')">📦<br>Plans</div>
<div onclick="showPage('deposit')">💰<br>Deposit</div>
<div onclick="showPage('withdrawal')">💵<br>Withdraw</div>
<div onclick="showPage('referral')">🔗<br>Referral</div>
<div onclick="showPage('support')">📞<br>Support</div>
</div>

<script>
// USERS & LOCAL STORAGE
let currentUser = localStorage.getItem('verbose_user') || null;
let balance = parseFloat(localStorage.getItem('verbose_balance')) || 0;
let dailyProfit = parseFloat(localStorage.getItem('verbose_daily')) || 0;
let plansData = [];
let userPlans = JSON.parse(localStorage.getItem('verbose_userPlans')||'[]');
let referrals = JSON.parse(localStorage.getItem('verbose_ref')||'{}');

// CREATE PLANS
for(let i=1;i<=25;i++){
    let invest = Math.round(200 + (i-1)*(30000-200)/24);
    let days = 20 + Math.floor((i-1)*(70-20)/24);
    let multiplier = i<=7 ? 3 : 2.5;
    plansData.push({id:i,name:`Plan ${i}`,invest:invest,days:days,total:Math.round(invest*multiplier),multiplier:multiplier,offer:i<=7});
}

// LOGIN / SIGNUP FUNCTIONS
function showSignup(){document.getElementById('signupForm').classList.remove('hidden');document.getElementById('loginForm').classList.add('hidden');}
function showLogin(){document.getElementById('signupForm').classList.add('hidden');document.getElementById('loginForm').classList.remove('hidden');}

function login(){
    let u=document.getElementById("user").value;
    let p=document.getElementById("pass").value;
    if(!u||!p){alert("Enter username & password");return;}
    currentUser=u;
    localStorage.setItem('verbose_user',currentUser);
    if(!localStorage.getItem('verbose_balance')) localStorage.setItem('verbose_balance','0');
    if(!localStorage.getItem('verbose_daily')) localStorage.setItem('verbose_daily','0');
    balance=parseFloat(localStorage.getItem('verbose_balance'));
    dailyProfit=parseFloat(localStorage.getItem('verbose_daily'));
    document.getElementById("dashUser").innerText=currentUser;
    document.getElementById("dashBalance").innerText=balance;
    document.getElementById("dashDaily").innerText=dailyProfit;
    document.getElementById("loginPage").classList.add("hidden");
    document.getElementById("dashboard").classList.remove("hidden");
    document.getElementById("bottomNav").classList.remove("hidden");
    renderPlans();
}

function signup(){
    let u=document.getElementById("newUser").value;
    let p=document.getElementById("newPass").value;
    if(!u||!p){alert("Enter username & password");return;}
    currentUser=u;
    localStorage.setItem('verbose_user',currentUser);
    localStorage.setItem('verbose_balance','0');
    localStorage.setItem('verbose_daily','0');
    balance=0;
    dailyProfit=0;
    document.getElementById("dashUser").innerText=currentUser;
    document.getElementById("dashBalance").innerText=balance;
    document.getElementById("dashDaily").innerText=dailyProfit;
    document.getElementById("loginPage").classList.add("hidden");
    document.getElementById("dashboard").classList.remove("hidden");
    document.getElementById("bottomNav").classList.remove("hidden");
    renderPlans();
}

// LOGOUT
function logout(){
    currentUser=null;
    localStorage.removeItem('verbose_user');
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

// COPY DEPOSIT NUMBER
function copyDepositNumber(){
    let num=document.getElementById('depositNumber');
    num.select();
    num.setSelectionRange(0,99999);
    document.execCommand('copy');
    alert("Deposit number copied!");
}

// COPY REFERRAL
function copyReferral(){
    let link=document.getElementById('refLink');
    link.select();
    link.setSelectionRange(0,99999);
    document.execCommand('copy');
    alert("Referral link copied!");
}

// PLANS
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
        <span class="countdown" id="countdown${p.id}"></span><br>
        <button onclick="buyPlan(${p.id})">Buy Now</button>`;
        list.appendChild(div);
        if(p.offer) startCountdown(p.id,24*60*60);
    });
}

// COUNTDOWN
let countdownIntervals={};
function startCountdown(id,seconds){
    let display=document.getElementById(`countdown${id}`);
    let endTime = Date.now() + seconds*1000;
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
}

// DEPOSIT
const depositNumbers={jazzcash:'03705519562',easypaisa:'03379827882'};
function updateDepositNumber(){
    let method=document.getElementById('depositMethod').value;
    document.getElementById('depositNumber').value=depositNumbers[method];
}
function submitDeposit(){
    let amount=parseFloat(document.getElementById('depositAmount').value);
    balance+=amount;
    dailyProfit+=Math.round(amount*0.01); // example daily profit 1% of deposit
    localStorage.setItem('verbose_balance',balance);
    localStorage.setItem('verbose_daily',dailyProfit);
    document.getElementById('dashBalance').innerText=balance;
    document.getElementById('dashDaily').innerText=dailyProfit;
    alert("Deposit submitted! Admin will verify.");
    showPage('dashboard');
}

// WITHDRAWAL
function submitWithdraw(){
    let amt=parseFloat(document.getElementById('withdrawAmount').value);
    if(!amt || amt>balance){alert("Invalid amount"); return;}
    balance-=amt;
    localStorage.setItem('verbose_balance',balance);
    document.getElementById('dashBalance').innerText=balance;
    alert(`Withdrawal request of Rs ${amt} received. Admin will process it.`);
    showPage('dashboard');
}

// ONLOAD
window.onload=function(){
    if(currentUser){
        balance=parseFloat(localStorage.getItem('verbose_balance')||0);
        dailyProfit=parseFloat(localStorage.getItem('verbose_daily')||0);
        document.getElementById("dashUser").innerText=currentUser;
        document.getElementById("dashBalance").innerText=balance;
        document.getElementById("dashDaily").innerText=dailyProfit;
        document.getElementById("loginPage").classList.add("hidden");
        document.getElementById("dashboard").classList.remove("hidden");
        document.getElementById("bottomNav").classList.remove("hidden");
        renderPlans();
    }
};
</script>
</body>
</html>
