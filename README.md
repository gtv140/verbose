<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE</title>
<style>
body { font-family:Arial; margin:0; background:#111; color:#fff; }
header { background:#007bff; padding:15px; text-align:center; font-size:22px; font-weight:bold; }
.login-box, .page { max-width:400px; margin:40px auto; background:#222; padding:20px; border-radius:10px; box-shadow:0 0 10px #000; }
input, button, select { width:100%; padding:12px; margin-top:10px; border-radius:5px; border:none; font-size:16px; }
button { background:#ffb200; font-weight:bold; color:#000; cursor:pointer; }
button:hover { background:#ffc800; }
.nav { position:fixed; bottom:0; left:0; right:0; background:#000; display:flex; justify-content:space-around; padding:10px 0; }
.nav div { text-align:center; font-size:12px; cursor:pointer; }
.plan { background:#1d1d1d; padding:12px; margin:10px 0; border-radius:10px; border:1px solid #333; }
.hidden { display:none; }
.copy-btn { padding:5px 8px; background:#007bff; color:#fff; border:none; border-radius:5px; cursor:pointer; margin-left:5px; }
.user-box { background:#1a1a1a; padding:12px; margin-bottom:15px; border-radius:10px; border:1px solid #333; text-align:center; }
</style>
</head>
<body>

<header>VERBOSE</header>

<!-- LOGIN PAGE -->
<div id="loginPage" class="login-box">
<h2>Login</h2>
<input id="user" placeholder="Username">
<input id="pass" placeholder="Password" type="password">
<button onclick="login()">Login</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
<div class="user-box">
Username: <span id="dashUser"></span> | Balance: Rs <span id="dashBalance">0</span>
<button onclick="logout()" style="background:red;color:#fff;margin-top:5px;">Logout</button>
</div>
<h2>Dashboard</h2>
<div class="plan">Deposit/Withdrawal ke baad admin ko screenshot WhatsApp 03705519562 bhejna zaroori hai</div>
</div>

<!-- PLANS -->
<div id="plans" class="page hidden">
<h2>Investment Plans</h2>
<div id="plansList"></div>
</div>

<!-- DEPOSIT -->
<div id="deposit" class="page hidden">
<h2>Deposit</h2>
<label>Choose Method</label>
<select id="depositMethod" onchange="updateDepositNumber()">
  <option value="jazzcash">JazzCash</option>
  <option value="easypaisa">EasyPaisa</option>
</select>
<div class="plan" style="margin-top:10px;">
<span id="depositNumber"></span>
<button class="copy-btn" onclick="copyText(document.getElementById('depositNumber').innerText)">Copy</button>
</div>
<input id="depositAmount" placeholder="Amount">
<input id="depositTxId" placeholder="Transaction ID">
<input type="file" id="depositProof">
<button onclick="submitDeposit()">Submit Deposit</button>
</div>

<!-- WITHDRAWAL -->
<div id="withdrawal" class="page hidden">
<h2>Withdrawal</h2>
<label>Choose Method</label>
<select id="withdrawMethod" onchange="updateWithdrawFields()">
  <option value="jazzcash">JazzCash</option>
  <option value="easypaisa">EasyPaisa</option>
  <option value="bank">Bank</option>
</select>
<input id="withdrawUsername" placeholder="Username" readonly>
<input id="withdrawAccount" placeholder="Account Number" readonly>
<input id="withdrawAmount" placeholder="Amount">
<button onclick="submitWithdraw()">Request Withdrawal</button>
</div>

<!-- SUPPORT -->
<div id="support" class="page hidden">
<h2>Support</h2>
<p>Admin WhatsApp: 03705519562</p>
<p>Email: <a href="mailto:rock.earn92@gmail.com" style="color:#ffb200;">rock.earn92@gmail.com</a></p>
<a href="https://wa.me/923705519562" target="_blank">Open WhatsApp</a>
</div>

<!-- ACTIVITY -->
<div id="activity" class="page hidden">
<h2>Activity</h2>
<div id="activityList">No activity yet.</div>
</div>

<!-- HISTORY -->
<div id="history" class="page hidden">
<h2>History</h2>
<div id="historyList">No history yet.</div>
</div>

<!-- ABOUT VERBOSE -->
<div id="about" class="page hidden">
<h2>About VERBOSE</h2>
<p>VERBOSE is a trusted and professional investment platform, giving high-quality returns to its users. Millions of users and members work with our company. We ensure reliable profits with a transparent system. We provide automatic daily profit updates and real-time balance tracking. Our team is always available for deposits, withdrawals, or any inquiries. Contact us via WhatsApp 03705519562 or Email rock.earn92@gmail.com. Join VERBOSE today for safe and profitable investment opportunities.</p>
</div>

<!-- NAVIGATION -->
<div class="nav hidden" id="bottomNav">
<div onclick="showPage('dashboard')">🏠<br>Home</div>
<div onclick="showPage('plans')">📦<br>Plans</div>
<div onclick="showPage('deposit')">💰<br>Deposit</div>
<div onclick="showPage('withdrawal')">💵<br>Withdraw</div>
<div onclick="showPage('support')">📞<br>Support</div>
<div onclick="showPage('activity')">📊<br>Activity</div>
<div onclick="showPage('history')">📚<br>History</div>
<div onclick="showPage('about')">ℹ️<br>About</div>
</div>

<script>
let currentUser = '';
let balance = 0;
let activityLog = [];
let historyLog = [];

// DEPOSIT NUMBERS
const depositNumbers = { jazzcash: '03705519562', easypaisa: '03379827882' };
// WITHDRAW ACCOUNTS
const withdrawAccounts = {
  jazzcash: ()=>({username: currentUser, account:'03705519562'}),
  easypaisa: ()=>({username: currentUser, account:'03379827882'}),
  bank: ()=>({username: currentUser, account:'1234567890'})
};

// LOGIN
function login(){
    let u = document.getElementById("user").value;
    let p = document.getElementById("pass").value;
    if(u === "" || p === ""){ alert("Username aur password likho sweetie"); return; }
    currentUser = u;
    balance = parseInt(localStorage.getItem(u+'_balance')) || 0;
    document.getElementById("dashUser").innerText = currentUser;
    document.getElementById("dashBalance").innerText = balance;
    localStorage.setItem('verbose_current', currentUser);
    document.getElementById("loginPage").classList.add("hidden");
    document.getElementById("dashboard").classList.remove("hidden");
    document.getElementById("bottomNav").classList.remove("hidden");
    generatePlans();
}

// LOGOUT
function logout(){
    localStorage.removeItem('verbose_current');
    currentUser = '';
    balance = 0;
    document.getElementById("dashboard").classList.add("hidden");
    document.getElementById("bottomNav").classList.add("hidden");
    document.getElementById("loginPage").classList.remove("hidden");
}

// AUTO LOGIN ON REFRESH
window.onload = function(){
    let savedUser = localStorage.getItem('verbose_current');
    if(savedUser){
        document.getElementById("user").value = savedUser;
        login();
    }
};

// SHOW PAGE
function showPage(id){
    let pages = document.querySelectorAll(".page");
    pages.forEach(p => p.classList.add("hidden"));
    document.getElementById(id).classList.remove("hidden");
}

// COPY FUNCTION
function copyText(txt){ navigator.clipboard.writeText(txt); alert("Copied: "+txt); }

// UPDATE DEPOSIT NUMBER
function updateDepositNumber(){
    let method = document.getElementById('depositMethod').value;
    document.getElementById('depositNumber').innerText = depositNumbers[method];
}

// UPDATE WITHDRAWAL FIELDS
function updateWithdrawFields(){
    let method = document.getElementById('withdrawMethod').value;
    let accInfo = withdrawAccounts[method]();
    document.getElementById('withdrawUsername').value = accInfo.username;
    document.getElementById('withdrawAccount').value = accInfo.account;
}

// PLANS DATA
const planNames = [
"Starter Boost","Mini Silver","Quick Profit","Prime Basic","Daily Saver",
"Turbo Start","Ultra 3x","Golden 3x","Royal 3x","Premium 3x",
"Silver Pro","Gold Pro","Max Saver","Profit Stream","Cash Booster",
"Elite Plus","Prime Plus","Turbo Earn","Ultra Return","Mega Plan",
"Diamond Plan","Platinum Plan","Business Pro","Investor Pack","Master King"
];
const planData = [];
for(let i=0;i<25;i++){
    let amt = (i<10)?200 + i*300 : 3500 + (i-10)*1500;
    let multiplier = (i<10)?3:2.5;
    planData.push({name:planNames[i],amount:amt,days:20,multiplier:multiplier});
}

// GENERATE PLANS
function generatePlans(){
    let plansDiv = document.getElementById("plansList");
    plansDiv.innerHTML = "";
    planData.forEach((p,i)=>{
        let daily = Math.round(p.amount*p.multiplier/p.days);
        let offer = (p.multiplier===3)?'<span style="color:#ff0;font-weight:bold;">24h Offer!</span>':'';
        plansDiv.innerHTML += `
            <div class="plan">
                <b>${p.name}</b> ${offer}<br>
                Amount: Rs ${p.amount}<br>
                Days: ${p.days}<br>
                Daily Profit: Rs ${daily}<br>
                Total Profit: Rs ${p.amount*p.multiplier}<br>
                <button onclick="buyPlan(${i})">Buy Now</button>
            </div>
        `;
    });
}

// BUY PLAN
function buyPlan(index){
    let p = planData[index];
    document.getElementById("depositAmount").value = p.amount;
    showPage('deposit');
    activityLog.push(`Plan ${p.name} selected for deposit.`);
    historyLog.push(`Bought plan ${p.name} amount Rs ${p.amount}`);
    updateLogs();
}

// DEPOSIT
function submitDeposit(){
    let amt = parseInt(document.getElementById("depositAmount").value);
    let tx = document.getElementById("depositTxId").value.trim();
    let proof = document.getElementById("depositProof").files[0];
    if(!amt || !tx || !proof){ alert("Fill all deposit details sweetie"); return; }
    balance += amt;
    document.getElementById("dashBalance").innerText = balance;
    localStorage.setItem(currentUser+'_balance', balance);
    activityLog.push(`Deposit Rs ${amt} submitted.`);
    historyLog.push(`Deposit Rs ${amt} TxID: ${tx}`);
    document.getElementById("depositTxId").value=''; document.getElementById("depositProof").value='';
    alert("Deposit submitted! Please send screenshot to admin WhatsApp 03705519562");
    updateLogs();
    showPage('dashboard');
}

// WITHDRAW
function submitWithdraw(){
    let amt = parseInt(document.getElementById("withdrawAmount").value);
    let acc = document.getElementById("withdrawAccount").value.trim();
    if(!amt || !acc){ alert("Enter amount and account number sweetie"); return; }
    if(amt>balance){ alert("Insufficient balance"); return; }
    balance -= amt;
    document.getElementById("dashBalance").innerText = balance;
    localStorage.setItem(currentUser+'_balance', balance);
    activityLog.push(`Withdraw Rs ${amt} requested.`);
    historyLog.push(`Withdraw Rs ${amt} to ${acc}`);
    document.getElementById("withdrawAmount").value='';
    updateLogs();
    alert("Withdraw request sent! Contact admin if needed.");
    showPage('dashboard');
}

// UPDATE ACTIVITY & HISTORY
function updateLogs(){
    document.getElementById("activityList").innerHTML = activityLog.join("<br>");
    document.getElementById("historyList").innerHTML = historyLog.join("<br>");
}
</script>

</body>
</html>
