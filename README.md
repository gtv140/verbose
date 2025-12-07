<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE</title>
<style>
body { font-family: Arial; margin:0; background:#111; color:#fff; }
.container { padding:15px; }
.box { background:#222; padding:15px; margin:10px 0; border-radius:10px; }
input, button, select {
 width:100%; padding:10px; margin-top:10px; border-radius:8px;
 border:none; font-size:16px;
}
button { background:#ffb200; font-weight:bold; }
a { color:#ffb200; text-decoration:none; font-weight:bold; }
.nav { position:fixed; bottom:0; left:0; right:0; background:#000;
 display:flex; justify-content:space-around; padding:10px; }
.nav a { color:#fff; font-size:14px; }
.plan { background:#1d1d1d; padding:12px; margin:10px 0; border-radius:10px; border:1px solid #333; }
</style>
</head>

<body>

<!-- LOGIN PAGE -->
<div id="loginPage" class="container">
<h2>VERBOSE Login</h2>
<div class="box">
<input id="user" placeholder="Username">
<input id="pass" type="password" placeholder="Password">
<button onclick="login()">Login</button>
</div>
</div>

<!-- DASHBOARD -->
<div id="home" class="container" style="display:none;">
<h2>VERBOSE Dashboard</h2>

<div class="box" style="background:#330;">
<b>NOTE:</b> Deposit/Withdrawal k bad <br>
Admin ko screenshot WhatsApp py send karo.<br>
<b>WhatsApp: 03705519562</b>
</div>

<div class="box"><a href="#" onclick="showPlans()">📦 View Plans</a></div>
<div class="box"><a href="#" onclick="showDeposit()">💰 Deposit</a></div>
<div class="box"><a href="#" onclick="showWithdraw()">📤 Withdrawal</a></div>
<div class="box"><a href="#" onclick="showSupport()">📞 Support</a></div>
<div class="box"><a href="#" onclick="showActivity()">📋 Activity</a></div>
<div class="box"><a href="#" onclick="showHistory()">📜 History</a></div>

</div>

<!-- PLANS PAGE -->
<div id="plansPage" class="container" style="display:none;">
<h2>25 Plans (200 to 30000)</h2>
<div id="plansList"></div>
<button onclick="back()">Back</button>
</div>

<!-- DEPOSIT -->
<div id="depositPage" class="container" style="display:none;">
<h2>Deposit</h2>

<div class="box">
<b>JazzCash:</b> 03705519562  
<button onclick="copyText('03705519562')">Copy</button>
</div>

<div class="box">
<b>Easypaisa:</b> 03379827882  
<button onclick="copyText('03379827882')">Copy</button>
</div>

<input placeholder="Enter Amount">
<input placeholder="Transaction ID">
<input type="file">

<button>Submit Deposit</button>
<button onclick="back()">Back</button>
</div>

<!-- WITHDRAW -->
<div id="withdrawPage" class="container" style="display:none;">
<h2>Withdrawal</h2>
<input placeholder="Enter Amount">
<input placeholder="JazzCash/Easypaisa Number">
<button>Request Withdrawal</button>
<button onclick="back()">Back</button>
</div>

<!-- SUPPORT -->
<div id="supportPage" class="container" style="display:none;">
<h2>Support</h2>
<div class="box">
Admin WhatsApp:  
<b>03705519562</b><br><br>
<a href="https://wa.me/923705519562">Open WhatsApp</a>
</div>
<button onclick="back()">Back</button>
</div>

<!-- ACTIVITY -->
<div id="activityPage" class="container" style="display:none;">
<h2>Activity</h2>
<div class="box">No activity found.</div>
<button onclick="back()">Back</button>
</div>

<!-- HISTORY -->
<div id="historyPage" class="container" style="display:none;">
<h2>History</h2>
<div class="box">No history yet.</div>
<button onclick="back()">Back</button>
</div>

<div class="nav">
<a href="#" onclick="home()">Home</a>
<a href="#" onclick="showPlans()">Plans</a>
<a href="#" onclick="showDeposit()">Deposit</a>
<a href="#" onclick="showWithdraw()">Withdraw</a>
<a href="#" onclick="showSupport()">Support</a>
</div>

<script>
function login(){
document.getElementById("loginPage").style.display="none";
document.getElementById("home").style.display="block";
generatePlans();
}

function back(){
home();
}

function home(){
hideAll();
document.getElementById("home").style.display="block";
}

function hideAll(){
document.getElementById("home").style.display="none";
document.getElementById("plansPage").style.display="none";
document.getElementById("depositPage").style.display="none";
document.getElementById("withdrawPage").style.display="none";
document.getElementById("supportPage").style.display="none";
document.getElementById("activityPage").style.display="none";
document.getElementById("historyPage").style.display="none";
}

function showPlans(){
hideAll();
document.getElementById("plansPage").style.display="block";
}

function showDeposit(){
hideAll();
document.getElementById("depositPage").style.display="block";
}

function showWithdraw(){
hideAll();
document.getElementById("withdrawPage").style.display="block";
}

function showSupport(){
hideAll();
document.getElementById("supportPage").style.display="block";
}

function showActivity(){
hideAll();
document.getElementById("activityPage").style.display="block";
}

function showHistory(){
hideAll();
document.getElementById("historyPage").style.display="block";
}

function copyText(txt){
navigator.clipboard.writeText(txt);
alert("Copied: "+txt);
}

function generatePlans(){
let list="";
let price = 200;
let planDays = 20;
for(let i=1; i<=25; i++){
let total = price<=3000 ? price*3 : price*2.5;
let daily = (total/planDays).toFixed(2);

list += `
<div class="plan">
<b>Plan ${i}</b><br>
Amount: Rs ${price}<br>
Days: ${planDays}<br>
Daily Profit: Rs ${daily}<br>
Total Profit: Rs ${total}
</div>
`;

price += Math.floor( (30000-200)/25 );
planDays += 2;
}
document.getElementById("plansList").innerHTML = list;
}
</script>

</body>
</html>
