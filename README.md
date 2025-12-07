<html lang="en">
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
.logout-btn{position:fixed;bottom:60px;right:15px;background:red;color:#fff;padding:8px 12px;border-radius:5px;cursor:pointer;}
.plan-box{border:1px solid #ccc;padding:10px;margin:10px 0;border-radius:8px;background:#f9f9f9;}
.offer{color:red;font-weight:bold;}
</style>
</head>
<body><header>VERBOSE</header><!-- LOGIN --><div id="loginPage" class="login-box">
<h2>Login / Signup</h2>
<input id="user" placeholder="Username">
<input id="pass" placeholder="Password" type="password">
<button onclick="login()">Login</button>
</div><!-- DASHBOARD --><div id="dashboard" class="page hidden">
<div class="alert-box">For any deposit, withdrawal, or account issues, contact our support team immediately for assistance.</div>
<div class="user-box">Username: <span id="dashUser"></span> | Balance: Rs <span id="dashBalance">0</span></div>
<h2>Dashboard</h2>
<p>Welcome to VERBOSE! Trusted platform, millions of users, daily profits, secure & reliable investment services. Our professional team is committed to providing a safe and efficient investment experience.</p>
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
<div style="display:flex; gap:10px;">
  <input id="depositNumber" readonly style="flex:1;">
  <button onclick="copyDepositNumber()" style="flex:0 0 80px;">Copy</button>
</div>
<label>Amount</label>
<input id="depositAmount" readonly>
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
<h2>Contact Administration</h2>
<p>For any deposit, withdrawal, or account issues, contact our professional support team immediately.</p>
<p>WhatsApp: <a href="https://chat.whatsapp.com/Kmaiv3VdSo09rio4qcRTRM" target="_blank">Join WhatsApp Group</a></p>
<p>Email: <a href="mailto:rock.earn92@gmail.com">rock.earn92@gmail.com</a></p>
<p>VERBOSE is a professional investment platform offering secure, reliable, and transparent investment services to millions of users. Our team is available 24/7 to assist with account, deposit, and withdrawal inquiries. Your investment experience and security are our top priorities.</p>
</div><!-- NAVIGATION --><div id="bottomNav" class="nav hidden">
<div onclick="showPage('dashboard')">🏠<br>Home</div>
<div onclick="showPage('plans')">📦<br>Plans</div>
<div onclick="showPage('deposit')">💰<br>Deposit</div>
<div onclick="showPage('withdrawal')">💵<br>Withdraw</div>
<div onclick="showPage('support')">📞<br>Support</div>
</div><script>
// USERS & LOCAL STORAGE
let currentUser = localStorage.getItem('verbose_user') || null;
let balance = parseFloat(localStorage.getItem('verbose_balance')) || 0;
let plansData = [];
let userPlans = JSON.parse(localStorage.getItem('verbose_userPlans')||'[]');

// CREATE 25 PLANS 200-30000, days 20-70, 7 special 24h offer
for(let i=1;i<=25;i++){
    let invest = Math.round(200 + (i-1)*(30000-200)/24);
    let days = 20 + Math.floor((i-1)*(70-20)/24);
    let multiplier = i<=7 ? 3 : 2.5;
    plansData.push({id:i,name:`Plan ${i}`,invest:invest,days:days,total:Math.round(invest*multiplier),multiplier:multiplier,offer:i<=7,offerEnd:Date.now()+(24*60*60*1000)});
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
    let now = Date.now();
    plansData.forEach(p=>{
        let offerText = '';
        if(p.offer && now<p.offerEnd){
            let hours = Math.floor((p.offerEnd-now)/(1000*60*60));
            let minutes = Math.floor(((p.offerEnd-now)%(1000*60*60))/(1000*60));
            offerText = `<span class="offer">🔥 24h Offer (${hours}h ${minutes}m left)</span>`;
        }
        div=document.createElement('div');
        div.className='plan-box';
        div.innerHTML=`<b>${p.name}</b> ${offerText}<br>
        Invest: Rs ${p.invest}<br>
        Days: ${p.days}<br>
        Total Profit: Rs ${p.total}<br>
        Daily Profit: Rs ${Math.round(p.total/p.days)}<br>
        <button onclick="buyPlan(${p.id})">Buy Now</button>`;
        list.appendChild(div);
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

// COPY DEPOSIT NUMBER
function copyDepositNumber(){
    let num = document.getElementById('depositNumber');
    num.select();
    num.setSelectionRange(0, 99999);
    navigator.clipboard.writeText(num.value).then(()=>{
        alert('Deposit number copied!');
    });
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
</script></body>
</html>
