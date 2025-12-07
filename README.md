<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE</title>
<style>
body {
    margin:0;
    font-family:Arial, sans-serif;
    background:#000;
    color:#fff;
    overflow-x:hidden;
    position:relative;
    background: linear-gradient(45deg,#000,#001f3f,#000,#001f3f);
    background-size: 400% 400%;
    animation: neonbg 20s linear infinite;
}
@keyframes neonbg{
    0%{background-position:0 0;}
    50%{background-position:100% 100%;}
    100%{background-position:0 0;}
}
header{
    text-align:center;
    padding:20px;
    font-size:28px;
    font-weight:bold;
    letter-spacing:2px;
    background:linear-gradient(90deg,#0ff,#00f,#0ff);
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
    background:#00f;
    border:none;
    cursor:pointer;
    transition:0.3s;
}
button:hover{
    background:#0ff;
    color:#000;
}
.nav{
    position:fixed;
    bottom:0; left:0; right:0;
    background:#111;
    display:flex;
    justify-content:space-around;
    padding:10px 0;
    border-top:1px solid #0ff;
}
.nav div{
    text-align:center;
    font-size:16px;
    cursor:pointer;
}
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
    bottom:60px;
    left:50%;
    transform:translateX(-50%);
    background:red;
    color:#fff;
    padding:10px 15px;
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

<!-- LOGIN/SIGNUP -->
<div id="loginPage" class="login-box">
<h2>Welcome</h2>
<button onclick="showNewUser()">New User Signup</button>
<button onclick="showLogin()">Already Have Account</button>
<div id="loginForm" class="hidden">
<input id="user" placeholder="Username">
<input id="pass" placeholder="Password" type="password">
<button onclick="login()">Login</button>
</div>
<div id="signupForm" class="hidden">
<input id="newUser" placeholder="Choose Username">
<input id="newPass" placeholder="Choose Password" type="password">
<button onclick="signup()">Signup</button>
</div>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
<div class="alert-box">For any deposit, withdrawal, or account issues, contact support immediately.</div>
<div class="user-box">Username: <span id="dashUser"></span> | Balance: Rs <span id="dashBalance">0</span> | Daily Profit: Rs <span id="dailyProfit">0</span></div>
<h2>Dashboard</h2>
<p>Welcome to VERBOSE! Trusted platform, millions of users, secure & reliable. Buy plans to earn daily profit automatically.</p>
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
<p>Our professional support team is available 24/7 for deposit, withdrawal, or account issues.</p>
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
<div onclick="shareReferral()">🔗<br>Referral</div>
</div>

<script>
// STORAGE & DATA
let users = JSON.parse(localStorage.getItem('verbose_users')||'{}');
let currentUser = localStorage.getItem('verbose_currentUser') || null;
let balance = 0;
let dailyProfit = 0;
let plansData=[];
let userPlans = JSON.parse(localStorage.getItem('verbose_userPlans')||'[]');

// CREATE PLANS
for(let i=1;i<=25;i++){
    let invest = Math.round(200 + (i-1)*(30000-200)/24);
    let days = 20 + Math.floor((i-1)*(70-20)/24);
    let multiplier = i<=7 ? 3 : 2.5;
    plansData.push({id:i,name:`Plan ${i}`,invest:invest,days:days,total:Math.round(invest*multiplier),multiplier:multiplier,offer:i<=7});
}

// LOGIN/SIGNUP UI
function showNewUser(){
    document.getElementById('signupForm').classList.remove('hidden');
    document.getElementById('loginForm').classList.add('hidden');
}
function showLogin(){
    document.getElementById('loginForm').classList.remove('hidden');
    document.getElementById('signupForm').classList.add('hidden');
}

// SIGNUP
function signup(){
    let u=document.getElementById('newUser').value.trim();
    let p=document.getElementById('newPass').value.trim();
    if(!u||!p){alert("Fill username & password"); return;}
    if(users[u]){alert("Username exists!"); return;}
    users[u]={password:p,balance:0,profit:0,plans:[],referrals:0};
    localStorage.setItem('verbose_users',JSON.stringify(users));
    alert("Signup successful! Login now.");
    showLogin();
}

// LOGIN
function login(){
    let u=document.getElementById('user').value.trim();
    let p=document.getElementById('pass').value.trim();
    if(!u||!p){alert("Fill username & password"); return;}
    if(!users[u] || users[u].password!==p){alert("Invalid credentials"); return;}
    currentUser = u;
    localStorage.setItem('verbose_currentUser',currentUser);
    balance = users[u].balance;
    dailyProfit = users[u].profit;
    document.getElementById('dashUser').innerText = currentUser;
    document.getElementById('dashBalance').innerText = balance;
    document.getElementById('dailyProfit').innerText = dailyProfit;
    document.getElementById('loginPage').classList.add('hidden');
    document.getElementById('dashboard').classList.remove('hidden');
    document.getElementById('bottomNav').classList.remove('hidden');
    renderPlans();
}

// LOGOUT
function logout(){
    currentUser=null;
    localStorage.removeItem('verbose_currentUser');
    document.getElementById("loginPage").classList.remove("hidden");
    document.getElementById("dashboard").classList.add("hidden");
    document.getElementById("bottomNav").classList.add("hidden");
}

// NAVIGATION
function showPage(id){
    document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));
    document.getElementById(id).classList.remove('hidden');
}

// COPY DEPOSIT NUMBER
function copyDepositNumber(){
    let num=document.getElementById('depositNumber');
    num.select();
    num.setSelectionRange(0,99999);
    document.execCommand('copy');
    alert("Deposit number copied!");
}

// RENDER PLANS
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

// COUNTDOWN TIMER
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
    if(!currentUser){alert("Login first"); return;}
    let plan=plansData.find(p=>p.id===id);
    users[currentUser].plans.push({planId:id,dailyProfit:Math.round(plan.total/plan.days),lastUpdate:Date.now()});
    localStorage.setItem('verbose_users',JSON.stringify(users));
    alert("Plan bought! Daily profit will be added automatically.");
}

// DEPOSIT
const depositNumbers={jazzcash:'03705519562',easypaisa:'03379827882'};
function updateDepositNumber(){
    let method=document.getElementById('depositMethod').value;
    document.getElementById('depositNumber').value=depositNumbers[method];
}
function submitDeposit(){
    let amt= parseFloat(document.getElementById('depositAmount').value) || 0;
    if(!amt){alert("Enter amount"); return;}
    users[currentUser].balance += amt;
    localStorage.setItem('verbose_users',JSON.stringify(users));
    alert("Deposit added to account!");
    document.getElementById('dashBalance').innerText=users[currentUser].balance;
}

// WITHDRAWAL
function submitWithdraw(){
    let amt=parseFloat(document.getElementById('withdrawAmount').value);
    if(amt>users[currentUser].balance){alert("Insufficient"); return;}
    users[currentUser].balance -= amt;
    localStorage.setItem('verbose_users',JSON.stringify(users));
    alert("Withdrawal request submitted!");
}

// REFERRAL
function shareReferral(){
    let link = window.location.href+"?ref="+currentUser;
    alert("Share this link to get Rs 30 bonus: "+link);
}

// DAILY PROFIT ADDITION
setInterval(()=>{
    if(!currentUser) return;
    let user = users[currentUser];
    let totalProfit=0;
    if(user.plans){
        user.plans.forEach(p=>{
            let now = Date.now();
            let daysPassed = Math.floor((now-p.lastUpdate)/(1000*60*60*24));
            if(daysPassed>0){
                totalProfit += p.dailyProfit*daysPassed;
                p.lastUpdate = now;
            }
        });
        user.balance += totalProfit;
        user.profit += totalProfit;
        localStorage.setItem('verbose_users',JSON.stringify(users));
        document.getElementById('dashBalance').innerText = user.balance;
        document.getElementById('dailyProfit').innerText = user.profit;
    }
},5000);

// ONLOAD
window.onload=function(){
    if(currentUser && users[currentUser]){
        document.getElementById('dashUser').innerText = currentUser;
        document.getElementById('dashBalance').innerText = users[currentUser].balance;
        document.getElementById('dailyProfit').innerText = users[currentUser].profit;
        document.getElementById('loginPage').classList.add('hidden');
        document.getElementById('dashboard').classList.remove('hidden');
        document.getElementById('bottomNav').classList.remove('hidden');
        renderPlans();
    }
};
</script>
</body>
</html>
