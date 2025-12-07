<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE</title>
<style>
body{margin:0;font-family:Arial;background:#0a0a0a;color:#fff;}
header{background:#001f3f;color:#0ff;padding:15px;text-align:center;font-size:26px;font-weight:bold;font-family:Verdana, sans-serif;text-shadow:0 0 5px #0ff,0 0 10px #0ff;}
.login-box,.page{max-width:400px;margin:20px auto;background:#111;padding:20px;border-radius:10px;box-shadow:0 0 20px #00f;}
input,button,select{width:100%;padding:10px;margin-top:10px;border-radius:5px;border:1px solid #0ff;background:#222;color:#0ff;}
button{background:#0ff;color:#000;font-weight:bold;border:none;cursor:pointer;}
button:hover{background:#00f;color:#fff;}
.nav{position:fixed;bottom:0;left:0;right:0;background:#111;display:flex;justify-content:space-around;padding:10px 0;border-top:1px solid #0ff;}
.nav div{text-align:center;font-size:12px;cursor:pointer;color:#0ff;}
.hidden{display:none;}
.user-box{background:#001f3f;padding:10px;border-radius:8px;margin-bottom:10px;font-weight:bold;color:#0ff;}
.alert-box{background:#330000;padding:8px;border-radius:5px;margin-bottom:10px;color:#f00;font-weight:bold;}
.logout-btn{position:fixed;bottom:60px;right:15px;background:red;color:#fff;padding:8px 12px;border-radius:5px;cursor:pointer;}
.plan-box{border:1px solid #0ff;padding:10px;margin:10px 0;border-radius:8px;background:#111;}
.offer{color:#ff0;font-weight:bold;}
#backgroundCanvas{position:fixed;top:0;left:0;width:100%;height:100%;z-index:-1;}
</style>
</head>
<body>

<canvas id="backgroundCanvas"></canvas>

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
<div class="alert-box">For any deposit, withdrawal, or account issues, contact our support team immediately.</div>
<div class="user-box">Username: <span id="dashUser"></span> | Balance: Rs <span id="dashBalance">0</span></div>
<h2>Dashboard</h2>
<p>Welcome to VERBOSE! Trusted platform, millions of users, daily profits, secure & reliable investment services.</p>
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
<p>VERBOSE is a professional investment platform offering secure, reliable, and transparent investment services. Our team is available 24/7 to assist with account, deposit, and withdrawal inquiries.</p>
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
// BACKGROUND ANIMATION
const canvas=document.getElementById('backgroundCanvas');
const ctx=canvas.getContext('2d');
canvas.width=window.innerWidth;
canvas.height=window.innerHeight;
let stars=[];
for(let i=0;i<150;i++){
    stars.push({x:Math.random()*canvas.width,y:Math.random()*canvas.height,r:Math.random()*1.5,speed:Math.random()*0.5});
}
function animate(){
    ctx.fillStyle='rgba(0,0,0,0.3)';
    ctx.fillRect(0,0,canvas.width,canvas.height);
    ctx.fillStyle='#0ff';
    stars.forEach(s=>{
        s.y+=s.speed;
        if(s.y>canvas.height){s.y=0;s.x=Math.random()*canvas.width;}
        ctx.beginPath();
        ctx.arc(s.x,s.y,s.r,0,2*Math.PI);
        ctx.fill();
    });
    requestAnimationFrame(animate);
}
animate();
window.onresize=function(){canvas.width=window.innerWidth;canvas.height=window.innerHeight;};

// USERS & LOCAL STORAGE
let currentUser = localStorage.getItem('verbose_user') || null;
let balance = parseFloat(localStorage.getItem('verbose_balance')) || 0;
let plansData = [];
let userPlans = JSON.parse(localStorage.getItem('verbose_userPlans')||'[]');
let countdownIntervals = {};

// CREATE 25 PLANS 200-30000, days 20-70, 7 special 24h offer
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
    if(!localStorage.getItem('verbose_balance')) localStorage.setItem('verbose_balance','5000');
    balance=parseFloat(localStorage.getItem('verbose_balance'));
    document.getElementById("dashUser").innerText=currentUser;
    document.getElementById("dashBalance").innerText=balance;
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

// PLANS
function renderPlans(){
    let list=document.getElementById("plansList");
    list.innerHTML='';
    plansData.forEach(p=>{
        let div=document.createElement('div');
        div.className='plan-box';
        div.innerHTML=`<b>${p.name}</b> ${p.offer?'<span class="offer">🔥 <span id="countdown${p.id}"></span></span>':''}<br>
        Invest: Rs ${p.invest}<br>
        Days: ${p.days}<br>
        Total Profit: Rs ${p.total}<br>
        Daily Profit: Rs ${Math.round(p.total/p.days)}<br>
        <button onclick="buyPlan(${p.id})">Buy Now</button>`;
        list.appendChild(div);
        if(p.offer) startCountdown(p.id,24*60*60);
    });
}

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
    document.getElementById('depositNumber').select();
    document.execCommand('copy');
}
function submitDeposit(){
    let tx=document.getElementById('depositTxId').value.trim();
    let proof=document.getElementById('depositProof').files[0];
    let amount=parseFloat(document.getElementById('depositAmount').value);
    if(!tx||!proof){alert("Fill TX ID & upload proof");return;}
    balance+=amount;
    localStorage.setItem('verbose_balance',balance);
    document.getElementById('dashBalance').innerText=balance;
    alert("Deposit submitted! Our team will verify and process it shortly.");
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
    alert(`Withdrawal request of Rs ${amt} received. Our support team will process it.`);
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
            balance+=p.dailyProfit*daysPassed;
            p.lastUpdate=now;
        }
    });
    localStorage.setItem('verbose_balance',balance);
    document.getElementById('dashBalance').innerText=balance;
    localStorage.setItem('verbose_userPlans',JSON.stringify(userPlans));
}

// COUNTDOWN TIMER (REFRESH-SAFE)
function startCountdown(id,seconds){
    let display=document.getElementById(`countdown${id}`);
    let endTime = localStorage.getItem('offerEndTime'+id);
    if(!endTime){
        endTime = Date.now() + seconds*1000;
        localStorage.setItem('offerEndTime'+id, endTime);
    } else {
        endTime = parseInt(endTime);
    }

    clearInterval(countdownIntervals[id]);
    countdownIntervals[id] = setInterval(()=>{
        let diff=Math.floor((endTime-Date.now())/1000);
        if(diff<=0){
            display.innerText="Offer Expired";
            clearInterval(countdownIntervals[id]);
            localStorage.removeItem('offerEndTime'+id);
            return;
        }
        let h=Math.floor(diff/3600);
        let m=Math.floor((diff%3600)/60);
        let s=diff%60;
        display.innerText=`${h}h ${m}m ${s}s`;
    },1000);
}

// ONLOAD
window.onload=function(){
    if(currentUser){
        document.getElementById("dashUser").innerText=currentUser;
        balance=parseFloat(localStorage.getItem('verbose_balance'));
        document.getElementById("dashBalance").innerText=balance;
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
