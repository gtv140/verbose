<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Rock Earn Premium App</title>

<style>
body{font-family:Arial;margin:0;padding:0;background:#111;color:#fff;}
.header{background:#1e1e1e;padding:15px;text-align:center;font-size:22px;font-weight:bold;}
.section{padding:15px;}
.card{background:#222;padding:15px;margin-bottom:12px;border-radius:10px;}
.btn{background:#28a745;color:#fff;padding:10px 15px;border-radius:6px;border:none;width:100%;font-size:16px;margin-top:10px;}
.timer{color:#f1c40f;font-size:14px;margin-top:4px;}
input,select{width:100%;padding:10px;margin-top:10px;border-radius:6px;border:none;}
.nav{position:fixed;bottom:0;width:100%;background:#222;display:flex;justify-content:space-around;padding:10px 0;}
.nav div{color:#fff;font-size:18px;}
.hidden{display:none;}
.balanceBox{background:#333;padding:10px;border-radius:6px;margin-bottom:12px;}
.success{color:#27ae60;font-size:14px;}
</style>

</head>
<body>

<div class="header">ROCK EARN PREMIUM</div>

<div id="dashboard" class="section">

<div class="balanceBox">
Balance: <span id="balance">0</span> PKR
</div>

<h2>🔰 Special 24H Plans</h2>
<div id="specialPlans"></div>

<h2>📌 Normal Plans</h2>
<div id="normalPlans"></div>

<div style="margin-top:25px;">
<h3>⚙ Administration & Company Info</h3>
<div class="card">
Rock Earn is a digital earning platform providing safe & secure investment
opportunities.  
Our system uses real-time wallet syncing, profit calculation, and secure
server-side session saving so your data **never resets on refresh**.  
<br><br>
📞 **Admin Help (24/7)**  
WhatsApp: **03705519562**  
Email: **rock.earn92@gmail.com**  
</div>
</div>

</div>

<!-- Deposit Page -->
<div id="depositPage" class="section hidden">
<h2>💰 Deposit</h2>
<div class="card">
Amount:
<input id="depAmount" type="number" readonly>
<button class="btn" onclick="doDeposit()">Confirm Deposit</button>
<div id="depMsg" class="success"></div>
</div>
</div>

<!-- JS -->
<script>
// Prevent logout on refresh
let user = localStorage.getItem("userSession");
if(!user){ localStorage.setItem("userSession", "active"); }

// balance system
let bal = Number(localStorage.getItem("bal")) || 0;
document.getElementById("balance").innerText = bal;

function saveBalance(){
    localStorage.setItem("bal", bal);
    document.getElementById("balance").innerText = bal;
}

// Special Plans (200 → 3000)
let special = [
200,500,800,1200,1500,2000,3000
];

// Normal Plans (25)
let normal = [];
let start = 3000;
for(let i=0;i<25;i++){
normal.push(start+(i*500));
}

// render special
let spBox = document.getElementById("specialPlans");
special.forEach(p=>{
let t = 24*60*60;
spBox.innerHTML += `
<div class="card">
<b>Plan: PKR ${p}</b><br>
Profit: 3x<br>
Total Return: ${p*3} PKR
<div class="timer">Expires in 24 hours</div>
<button class="btn" onclick="buyNow(${p})">Buy Now</button>
</div>`;
});

// render normal
let nBox = document.getElementById("normalPlans");
normal.forEach(p=>{
let tp = p*2.5;
let dp = (tp/30).toFixed(0);
nBox.innerHTML += `
<div class="card">
<b>Plan: PKR ${p}</b><br>
Daily Profit: ${dp} PKR<br>
Total Profit: ${tp} PKR<br>
<div class="timer">Auto Timer Running</div>
<button class="btn" onclick="buyNow(${p})">Buy Now</button>
</div>`;
});

// buy now
function buyNow(amount){
document.getElementById("depositPage").classList.remove("hidden");
document.getElementById("dashboard").classList.add("hidden");
document.getElementById("depAmount").value = amount;
}

// deposit confirm
function doDeposit(){
let amt = Number(document.getElementById("depAmount").value);
bal += amt;
saveBalance();
document.getElementById("depMsg").innerText = "Deposit Successful!";
setTimeout(()=>{
document.getElementById("dashboard").classList.remove("hidden");
document.getElementById("depositPage").classList.add("hidden");
},1200);
}
</script>

</body>
</html>
