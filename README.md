<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE</title>
<style>
/* BASE STYLES */
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
    padding:20px;
    font-size:28px;
    font-weight:bold;
    letter-spacing:2px;
    background:linear-gradient(90deg,#00ffff,#ff00ff);
    -webkit-background-clip:text;
    -webkit-text-fill-color:transparent;
}
.login-box, .page {
    max-width:400px;
    margin:20px auto;
    background:#111;
    padding:20px;
    border-radius:10px;
    box-shadow:0 0 20px #0ff;
}
input,button,select {
    width:100%;
    padding:10px;
    margin-top:10px;
    border-radius:5px;
    border:1px solid #0ff;
    background:#000;
    color:#fff;
}
button {
    background:#0ff;
    border:none;
    cursor:pointer;
    transition:0.3s;
}
button:hover {
    background:#00f;
    color:#fff;
}
.nav {
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
.user-box {
    background:#001f3f;
    padding:10px;
    border-radius:8px;
    margin-bottom:10px;
    font-weight:bold;
}
.alert-box {
    background:#330000;
    padding:8px;
    border-radius:5px;
    margin-bottom:10px;
    color:#f00;
    font-weight:bold;
}
.logout-btn {
    position:fixed;
    bottom:60px;
    left:50%;
    transform:translateX(-50%);
    background:red;
    color:#fff;
    padding:10px 15px;
    border-radius:8px;
    cursor:pointer;
}
.plan-box {
    border:1px solid #0ff;
    padding:10px;
    margin:10px 0;
    border-radius:8px;
    background:#111;
    transition:0.3s;
}
.plan-box:hover{
    box-shadow:0 0 15px #0ff;
}
.offer{color:#0ff;font-weight:bold;}
.countdown{font-weight:bold;color:#0ff;margin-top:5px;}
.referral-box{background:#111;padding:10px;border-radius:8px;margin:10px 0;text-align:center;}

/* NEON BACKGROUND ANIMATION */
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
<select id="userOption">
<option value="login">Login</option>
<option value="signup">New User Signup</option>
</select>
<input id="user" placeholder="Username">
<input id="pass" placeholder="Password" type="password">
<button onclick="login()">Submit</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
<div class="alert-box">For deposits, withdrawals, or account issues, contact support below.</div>
<div class="user-box">
Username: <span id="dashUser"></span> | Balance: Rs <span id="dashBalance">0</span> | Daily Profit: Rs <span id="dashDaily">0</span>
</div>
<h2>Dashboard</h2>
<p>Welcome to VERBOSE! Trusted platform, secure & reliable investment services.</p>
<div class="referral-box">
Referral Link: <input id="refLink" readonly>
<button onclick="copyReferral()">Copy Link</button>
</div>
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
<p>Need help? Reach our support team immediately:</p>
<p>WhatsApp: <a href="https://chat.whatsapp.com/Kmaiv3VdSo09rio4qcRTRM" target="_blank">Join WhatsApp Group</a></p>
<p>Email: <a href="mailto:rock.earn92@gmail.com">rock.earn92@gmail.com</a></p>
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
// USERS & LOCAL STORAGE
let currentUser = localStorage.getItem('verbose_user') || null;
let balance = parseFloat(localStorage.getItem('verbose_balance')) || 0;
let dailyProfit = parseFloat(localStorage.getItem('verbose_daily')) || 0;
let plansData = [];
let userPlans = JSON.parse(localStorage.getItem('verbose_userPlans')||'[]');
let referralCode = localStorage.getItem('verbose_referral') || '';

// CREATE PLANS 200-30000, 7 special offers, 5 coming soon
for(let i=1;i<=25;i++){
    let invest = Math.round(200 + (i-1)*(30000-200)/24);
    let days = 20 + Math.floor((i-1)*(70-20)/24);
    let multiplier = i<=7 ? 3 : 2.5;
    let comingSoon = i>25-5; 
    plansData.push({id:i,name:`Plan ${i}`,invest:invest,days:days,total:Math.round(invest*multiplier),multiplier:multiplier,offer:i<=7,coming:comingSoon});
}

// LOGIN / SIGNUP
function login(){
    let option=document.getElementById('userOption').value;
    let u=document.getElementById('user').value.trim();
    let p=document.getElementById('pass').value.trim();
    if(!u||!p){alert("Enter username & password");return;}
    currentUser=u;
    localStorage.setItem('verbose_user',currentUser);
    referralCode = referralCode || Math.random().toString(36).substring(2,10);
    localStorage.setItem('verbose_referral', referralCode);
    if(option==='signup'){
        localStorage.setItem('verbose_balance','0');
        localStorage.setItem('verbose_daily','0');
        userPlans=[];
        localStorage.setItem('verbose_userPlans',JSON.stringify(userPlans));
    }
    balance=parseFloat(localStorage.getItem('verbose_balance'));
    dailyProfit=parseFloat(localStorage.getItem('verbose_daily'));
    document.getElementById("dashUser").innerText=currentUser;
    document.getElementById("dashBalance").innerText=balance;
    document.getElementById("dashDaily").innerText=dailyProfit;
    document.getElementById("refLink").value=`https://gtv140.github.io/verbose/?ref=${referralCode}`;
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

// COPY REFERRAL LINK
function copyReferral(){
    let ref=document.getElementById('refLink');
    ref.select();
    ref.setSelectionRange(0,99999);
    document.execCommand('copy');
    alert("Referral link copied!");
}

// RENDER PLANS
function renderPlans(){
    let list=document.getElementById("plansList");
    list.innerHTML='';
    plansData.forEach(p=>{
        let div=document.createElement('div');
        div.className='plan-box';
        div.innerHTML=`<b>${p.name}</b> ${p.offer?'<span class="offer">🔥 24h Offer</span>':''} ${p.coming?'<span class="offer">Coming Soon</span>':''}<br>
        Invest: Rs ${p.invest}<br>
        Days: ${p.days}<br>
        Total Profit: Rs ${p.total}<br>
        Daily Profit: Rs ${Math.round(p.total/p.days)}<br>
        <span class="countdown" id="countdown${p.id}"></span><br>
        ${p.coming?'':'<button onclick="buyPlan('+p.id+')">Buy Now</button>'}`;
        list.appendChild(div);
        if(p.offer && !p.coming) startCountdown(p.id,24*3600);
    });
}

// COUNTDOWN TIMER
let countdownIntervals={};
function startCountdown(id,seconds){
    let display=document.getElementById(`countdown${id}`);
    let stored=localStorage.getItem(`countdown_${id}`);
    let endTime=stored ? parseInt(stored) : Date.now()+seconds*1000;
    localStorage.setItem(`countdown_${id}`,endTime);
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
    if(!userPlans.find(p=>p.planId===id)){
        userPlans.push({planId:id,lastUpdate:Date.now(),dailyProfit:Math.round(plan.total/plan.days)});
        localStorage.setItem('verbose_userPlans',JSON.stringify(userPlans));
    }
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
    let amount=parseFloat(document.getElementById('depositAmount').value);
    if(!tx||!proof){alert("Fill TX ID & upload proof");return;}
    balance+=amount;
    localStorage.setItem('verbose_balance',balance);
    document.getElementById('dashBalance').innerText=balance;
    alert("Deposit submitted! Admin will verify.");
    document.getElementById('depositTxId').value='';
    document.getElementById('depositProof').value='';
    showPage('dashboard');
}

// WITHDRAWAL
function updateWithdrawUsername(){document.getElementById('withdrawUsername').value=currentUser;}
function submitWithdraw(){
    let amt=parseFloat(document.getElementById('withdrawAmount').value);
    let acc=document.getElementById('withdrawAccount').value.trim();
    if(!amt||!acc){alert("Enter amount & account"); return;}
    if(amt>balance){alert("Insufficient balance"); return;}
    balance-=amt;
    localStorage.setItem('verbose_balance',balance);
    document.getElementById('dashBalance').innerText=balance;
    alert(`Withdrawal request of Rs ${amt} received. Admin will process it.`);
    document.getElementById('withdrawAmount').value='';
    document.getElementById('withdrawAccount').value='';
    showPage('dashboard');
}

// DAILY PROFIT AUTO ADD
function addDailyProfit(){
    let now=Date.now();
    userPlans.forEach(p=>{
        let last=p.lastUpdate||now;
        let daysPassed=Math.floor((now-last)/(1000*60*60*24));
        if(daysPassed>0){
            balance+=p.dailyProfit*daysPassed;
            dailyProfit+=p.dailyProfit*daysPassed;
            p.lastUpdate=now;
        }
    });
    localStorage.setItem('verbose_balance',balance);
    localStorage.setItem('verbose_daily',dailyProfit);
    document.getElementById('dashBalance').innerText=balance;
    document.getElementById('dashDaily').innerText=dailyProfit;
    localStorage.setItem('verbose_userPlans',JSON.stringify(userPlans));
}

// ONLOAD
window.onload=function(){
    if(currentUser){
        document.getElementById("dashUser").innerText=currentUser;
        balance=parseFloat(localStorage.getItem('verbose_balance'));
        dailyProfit=parseFloat(localStorage.getItem('verbose_daily'));
        document.getElementById("dashBalance").innerText=balance;
        document.getElementById("dashDaily").innerText=dailyProfit;
        document.getElementById("refLink").value=`https://gtv140.github.io/verbose/?ref=${referralCode}`;
        document.getElementById("loginPage").classList.add("hidden");
        document.getElementById("dashboard").classList.remove("hidden");
        document.getElementById("bottomNav").classList.remove("hidden");
        renderPlans();
        addDailyProfit();
    }
    setInterval(addDailyProfit,60000); // update daily profit every minute
};
</script>

</body>
</html>
