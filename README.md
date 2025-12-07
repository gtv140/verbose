<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE Premium App</title>

<style>
body{font-family:Arial;background:#f2f2f2;margin:0;padding:0}
header{background:#4b49ac;color:#fff;padding:15px;text-align:center;font-size:22px}
.card{background:#fff;margin:12px;padding:15px;border-radius:10px;box-shadow:0 0 4px #ccc}
input,select{width:100%;padding:10px;margin-top:6px;border:1px solid #999;border-radius:6px}
.btn{background:#4b49ac;color:#fff;padding:12px;text-align:center;border-radius:8px;margin-top:10px}
.btn:hover{opacity:0.9}
.nav{position:fixed;bottom:0;left:0;width:100%;background:#4b49ac;color:#fff;display:flex;justify-content:space-around;padding:10px 0}
.nav div{text-align:center;font-size:13px}
h3{margin:0 0 10px 0}
.planBox{border:1px solid #999;padding:10px;border-radius:8px;margin-bottom:10px;background:#fafafa}
.timer{color:red;font-weight:bold}
.hide{display:none}
.ico{font-size:18px;margin-right:4px}
</style>
</head>

<body>

<header>⚡ VERBOSE — Premium App</header>

<!-- LOGIN -->
<div class="card" id="loginCard">
<h3>👤 Login / Signup</h3>
<input id="userInput" placeholder="Enter Username">
<div class="btn" onclick="loginNow()">Continue</div>
</div>

<!-- DASHBOARD -->
<div class="card hide" id="dashboard">
<h3>🏠 Welcome <span id="userName"></span></h3>

<div class="card">
<div><b>Balance 💎:</b> PKR <span id="balance">0</span></div>
<div><b>Daily Profit:</b> PKR <span id="dailyProfit">0</span></div>
<div style="margin-top:10px;color:#c00">
<b>Alert:</b> Deposit/Withdraw issue?  
Contact Administration.
</div>
</div>

</div>

<!-- PLANS -->
<div class="card hide" id="plansCard">
<h3>📦 Investment Plans</h3>

<div id="plansList"></div>
</div>

<!-- DEPOSIT -->
<div class="card hide" id="depositCard">
<h3>📥 Deposit</h3>

<b>Amount</b>
<input id="depAmount" readonly>

<b>Method</b>
<select id="depMethod">
<option>JazzCash (03705519562)</option>
<option>EasyPaisa (03379827882)</option>
</select>

<b>Transaction ID</b>
<input id="depID">

<b>Upload Proof</b>
<input type="file">

<div class="btn" onclick="depositAdd()">Submit Deposit</div>
</div>

<!-- WITHDRAW -->
<div class="card hide" id="withdrawCard">
<h3>📤 Withdraw</h3>

<b>Username</b>
<input id="wUser" readonly>

<b>Amount</b>
<input id="wAmount">

<b>Method</b>
<select id="wMethod">
<option>JazzCash</option>
<option>EasyPaisa</option>
<option>Bank</option>
</select>

<b>Your Account Number</b>
<input id="wAcc">

<div class="btn" onclick="withdrawNow()">Submit Withdraw</div>
</div>

<!-- SUPPORT -->
<div class="card hide" id="supportCard">
<h3>📞 Contact Administration</h3>

<p><b>🟢 WhatsApp:</b> 03705519562</p>
<p><b>✉️ Email:</b> rock.earn92@gmail.com</p>

<p style="margin-top:10px">
Our company “VERBOSE Technologies Pvt Ltd”  
provides secure earning solutions, daily automated payouts,  
24/7 customer support & verified financial partners.
</p>
</div>

<!-- NAVIGATION -->
<div class="nav">
<div onclick="navTo('dashboard')">🏠<br>Home</div>
<div onclick="navTo('plansCard')">📦<br>Plans</div>
<div onclick="navTo('depositCard')">💰<br>Deposit</div>
<div onclick="navTo('withdrawCard')">💵<br>Withdraw</div>
<div onclick="navTo('supportCard')">📞<br>Support</div>
</div>

<script>
let user = "";
let balance = 0;
let dailyProfit = 0;

/* LOGIN */
function loginNow(){
    user = document.getElementById("userInput").value;
    if(user==="") return;
    document.getElementById("wUser").value = user;
    document.getElementById("userName").innerText = user;
    document.getElementById("loginCard").classList.add("hide");
    document.getElementById("dashboard").classList.remove("hide");
}

/* NAVIGATION FIX (NO LOGOUT ON REFRESH) */
function navTo(id){
    document.querySelectorAll(".card").forEach(x=>x.classList.add("hide"));
    document.getElementById(id).classList.remove("hide");
}

/* PLANS (7 Special Offers + 25 Normal) */
let plans = [];
let special = [200,400,600,800,1500,2000,3000];
special.forEach(p=>{
    plans.push({
        amount:p,
        daily:(p*3)/100,
        total:p*3,
        days:1,
        special:true
    });
});
for(let i=1;i<=25;i++){
    let amt = 3000 + i*1000;
    plans.push({
        amount:amt,
        daily:(amt*2.5)/100,
        total:amt*2.5,
        days:1,
        special:false
    });
}

let planHTML = "";
plans.forEach((p,i)=>{
planHTML += `
<div class="planBox">
<b>Plan PKR ${p.amount}</b><br>
Daily Profit: PKR ${p.daily}<br>
Total Profit: PKR ${p.total}<br>
Days: ${p.days}<br>
<span class="timer">⏳ 24h</span><br>
<div class="btn" onclick="buyNow(${p.amount}, ${p.daily})">Buy Now</div>
</div>
`;
});
document.getElementById("plansList").innerHTML = planHTML;

/* BUY NOW → DEPOSIT PAGE AUTO AMOUNT */
function buyNow(amount, dp){
    dailyProfit = dp;
    document.getElementById("dailyProfit").innerText = dp;
    document.getElementById("depAmount").value = amount;
    navTo("depositCard");
}

/* DEPOSIT ADD */
function depositAdd(){
    let amt = parseInt(document.getElementById("depAmount").value);
    balance += amt;
    document.getElementById("balance").innerText = balance;
    alert("Deposit Added Successfully!");
    navTo("dashboard");
}

/* WITHDRAW */
function withdrawNow(){
    let amt = parseInt(document.getElementById("wAmount").value);
    if(amt > balance){ alert("Low Balance"); return; }
    balance -= amt;
    document.getElementById("balance").innerText = balance;
    alert("Withdrawal Request Submitted!");
}
</script>

</body>
</html>
