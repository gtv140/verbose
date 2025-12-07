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
    letter-spacing:3px;
    background:linear-gradient(90deg,#0ff,#00f,#0ff,#00f);
    -webkit-background-clip:text;
    -webkit-text-fill-color:transparent;
    text-shadow:0 0 10px #0ff,0 0 20px #00f;
}
.login-box,.page{
    max-width:400px;
    margin:20px auto;
    background:#111;
    padding:20px;
    border-radius:10px;
    box-shadow:0 0 20px #0ff;
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
    font-weight:bold;
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
    padding:12px 0;
    border-top:1px solid #0ff;
}
.nav div{text-align:center;font-size:14px;cursor:pointer;}
.nav div span{display:block;font-size:24px;} /* icons size bigger */
.hidden{display:none;}
.user-box{
    background:#001f3f;
    padding:10px;
    border-radius:8px;
    margin-bottom:10px;
    font-weight:bold;
    text-align:center;
    box-shadow:0 0 10px #0ff;
}
.alert-box{
    background:#330033;
    padding:10px;
    border-radius:5px;
    margin-bottom:10px;
    color:#0ff;
    font-weight:bold;
    text-align:center;
    box-shadow:0 0 10px #00f;
}
.logout-btn{
    position:fixed;
    bottom:70px;
    right:15px;
    background:red;
    color:#fff;
    padding:8px 12px;
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
    box-shadow:0 0 20px #0ff;
}
.offer{color:#0ff;font-weight:bold;}
.countdown{
    font-weight:bold;
    color:#0ff;
    margin-top:5px;
    text-align:center;
}

/* NEON ANIMATION BACKGROUND */
@keyframes neonbg{
    0%{background-position:0 0;}
    50%{background-position:100% 100%;}
    100%{background-position:0 0;}
}
body{
    background:linear-gradient(45deg,#000,#001f3f,#000,#001f3f);
    background-size:400% 400%;
    animation:neonbg 25s linear infinite;
}
</style>
</head>
<body>

<header>VERBOSE</header>

<!-- LOGIN -->
<div id="loginPage" class="login-box">
<h2>Login / Signup</h2>
<input id="user" placeholder="Username">
<input id="pass" placeholder="Password" type="password">
<button onclick="login()">Login</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
<div class="alert-box">Deposit, withdrawal, or account issues? Our professional admin team is always available to assist you.</div>
<div class="user-box">Username: <span id="dashUser"></span> | Balance: Rs <span id="dashBalance">0</span> | Daily Profit: Rs <span id="dashProfit">0</span></div>
<h2>Dashboard</h2>
<p style="text-align:center;">Welcome to VERBOSE! Secure, trusted platform delivering daily profits with complete transparency. Your funds and growth are our priority.</p>
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
<p>Our dedicated administration team is available 24/7 for deposits, withdrawals, or account inquiries. Reach out anytime for professional support.</p>
<p>WhatsApp: <a href="https://chat.whatsapp.com/Kmaiv3VdSo09rio4qcRTRM" target="_blank">Join WhatsApp Group</a></p>
<p>Email: <a href="mailto:rock.earn92@gmail.com">rock.earn92@gmail.com</a></p>
</div>

<!-- REFERRAL -->
<div id="referral" class="page hidden">
<h2>Get Referral Bonus</h2>
<p>Share your link and earn Rs 30 when the new user deposits!</p>
<input id="refLink" readonly>
<button onclick="copyReferral()">Copy Referral Link</button>
</div>

<!-- NAVIGATION -->
<div id="bottomNav" class="nav hidden">
<div onclick="showPage('dashboard')"><span>🏠</span>Home</div>
<div onclick="showPage('plans')"><span>📦</span>Plans</div>
<div onclick="showPage('deposit')"><span>💰</span>Deposit</div>
<div onclick="showPage('withdrawal')"><span>💵</span>Withdraw</div>
<div onclick="showPage('support')"><span>📞</span>Support</div>
<div onclick="showPage('referral')"><span>🎁</span>Bonus</div>
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

// LOGIN
function login(){
    let u=document.getElementById("user").value;
    let p=document.getElementById("pass").value;
    if(!u||!p){alert("Enter username & password");return;}
    currentUser=u;
    localStorage.setItem('verbose_user',currentUser);
    if(!localStorage.getItem('verbose_balance')) localStorage.setItem('verbose_balance','0');
    if(!localStorage.getItem('verbose_dailyProfit')) localStorage.setItem('verbose_dailyProfit','0');
    balance=parseFloat(localStorage.getItem('verbose_balance'));
    dailyProfit=parseFloat(localStorage.getItem('verbose_dailyProfit'));
    document.getElementById("dashUser").innerText=currentUser;
    document.getElementById("dashBalance").innerText=balance;
    document.getElementById("dashProfit").innerText=dailyProfit;

    // Referral link
    let refLink=document.getElementById('refLink');
    refLink.value = `${window.location.href}?ref=${currentUser}`;

    document.getElementById("loginPage").classList.add("hidden");
    document.getElementById("dashboard").classList.remove("hidden");
    document.getElementById("bottomNav").classList.remove("hidden");
    renderPlans();
    updateWithdrawUsername();
    addDailyProfit();
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
    let ref=document.getElementById('refLink');
    ref.select();
    ref.setSelectionRange(0,99999);
    document.execCommand('copy');
    alert("Referral link copied! Earn Rs 30 per deposit.");
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
    let endTime = localStorage.getItem(`offerEnd${id}`);
    let finalTime = endTime ? parseInt(endTime) : Date.now()+seconds*1000;
    localStorage.setItem(`offerEnd${id}`, finalTime);
    clearInterval(countdownIntervals[id]);
    countdownIntervals[id]=setInterval(()=>{
        let diff=Math.floor((finalTime-Date.now())/1000);
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
    dailyProfit += Math.round(amount*0.05);
    localStorage.setItem('verbose_balance',balance);
    localStorage.setItem('verbose_dailyProfit',dailyProfit);
    document.getElementById('dashBalance').innerText=balance;
    document.getElementById('dashProfit').innerText=dailyProfit;
    alert("Deposit submitted! Admin will verify.");

    // Referral bonus auto add
    let urlParams = new URLSearchParams(window.location.search);
    let refUser = urlParams.get('ref');
    if(refUser && refUser !== currentUser){
        let refData = JSON.parse(localStorage.getItem('verbose_referrals')||'{}');
        if(!refData[refUser]){
            refData[refUser]=0;
        }
        refData[refUser] += 30;
        localStorage.setItem('verbose_referrals',JSON.stringify(refData));
        if(refUser === currentUser){balance += 30; localStorage.setItem('verbose_balance',balance);}
        alert(`Referral bonus Rs 30 added to ${refUser}!`);
    }

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

// DAILY PROFIT
function addDailyProfit(){
    let now=Date.now();
    userPlans.forEach(p=>{
        let last=p.lastUpdate||now;
        let daysPassed=Math.floor((now-last)/(1000*60*60*24));
        if(daysPassed>0){
            balance += p.dailyProfit*daysPassed;
            dailyProfit += p.dailyProfit*daysPassed;
            p.lastUpdate = now;
        }
    });
    localStorage.setItem('verbose_balance',balance);
    localStorage.setItem('verbose_dailyProfit',dailyProfit);
    document.getElementById('dashBalance').innerText=balance;
    document.getElementById('dashProfit').innerText=dailyProfit;
    localStorage.setItem('verbose_userPlans',JSON.stringify(userPlans));
}

// ONLOAD
window.onload=function(){
    if(currentUser){
        document.getElementById("dashUser").innerText=currentUser;
        balance=parseFloat(localStorage.getItem('verbose_balance'));
        dailyProfit=parseFloat(localStorage.getItem('verbose_dailyProfit'));
        document.getElementById("dashBalance").innerText=balance;
        document.getElementById("dashProfit").innerText=dailyProfit;

        // Referral link
        let refLink=document.getElementById('refLink');
        refLink.value = `${window.location.href}?ref=${currentUser}`;

        document.getElementById("loginPage").classList.add("hidden");
        document.getElementById("dashboard").classList.remove("hidden");
        document.getElementById("bottomNav").classList.remove("hidden");
        renderPlans();
        updateWithdrawUsername();
        addDailyProfit();
    }
};
</script>
</body>
</html>
