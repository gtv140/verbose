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
<h2>Dashboard</h2>
<p>Welcome to VERBOSE!</p>
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
<div class="plan">
JazzCash: 03705519562 <button class="copy-btn" onclick="copyText('03705519562')">Copy</button><br>
EasyPaisa: 03379827882 <button class="copy-btn" onclick="copyText('03379827882')">Copy</button>
</div>
<input placeholder="Enter Amount">
<input placeholder="Transaction ID">
<input type="file">
<button>Submit Deposit</button>
</div>

<!-- WITHDRAWAL -->
<div id="withdrawal" class="page hidden">
<h2>Withdrawal</h2>
<input placeholder="Enter Amount">
<input placeholder="Account Number">
<button>Request Withdrawal</button>
</div>

<!-- SUPPORT -->
<div id="support" class="page hidden">
<h2>Support</h2>
<p>Admin WhatsApp: 03705519562</p>
<a href="https://wa.me/923705519562" target="_blank">Open WhatsApp</a>
</div>

<!-- ACTIVITY -->
<div id="activity" class="page hidden">
<h2>Activity</h2>
<p>No activity yet.</p>
</div>

<!-- HISTORY -->
<div id="history" class="page hidden">
<h2>History</h2>
<p>No history yet.</p>
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
</div>

<script>
// LOGIN FUNCTION
function login(){
    let u = document.getElementById("user").value;
    let p = document.getElementById("pass").value;
    if(u === "" || p === ""){ alert("Username aur password likho sweetie"); return; }
    document.getElementById("loginPage").classList.add("hidden");
    document.getElementById("dashboard").classList.remove("hidden");
    document.getElementById("bottomNav").classList.remove("hidden");
    generatePlans();
}

// SHOW PAGE
function showPage(id){
    let pages = document.querySelectorAll(".page");
    pages.forEach(p => p.classList.add("hidden"));
    document.getElementById(id).classList.remove("hidden");
}

// COPY FUNCTION
function copyText(txt){ navigator.clipboard.writeText(txt); alert("Copied: "+txt); }

// GENERATE 25 PLANS
function generatePlans(){
    let plansDiv = document.getElementById("plansList");
    plansDiv.innerHTML = "";
    let names = [
        "Starter Boost","Mini Silver","Quick Profit","Prime Basic","Daily Saver",
        "Turbo Start","Ultra 3x","Golden 3x","Royal 3x","Premium 3x",
        "Silver Pro","Gold Pro","Max Saver","Profit Stream","Cash Booster",
        "Elite Plus","Prime Plus","Turbo Earn","Ultra Return","Mega Plan",
        "Diamond Plan","Platinum Plan","Business Pro","Investor Pack","Master King"
    ];
    let amount = 200;
    for(let i=1;i<=25;i++){
        if(i<=10) amount += 300;
        else amount += 1500;
        let multiplier = (i<=10)?3:2.5;
        let totalProfit = amount*multiplier;
        let dailyProfit = (totalProfit/20).toFixed(0);
        plansDiv.innerHTML += `
            <div class="plan">
                <b>${names[i-1]}</b><br>
                Amount: Rs ${amount}<br>
                Days: 20<br>
                Daily Profit: Rs ${dailyProfit}<br>
                Total Profit: Rs ${totalProfit}<br>
                Multiplier: ${multiplier}x
            </div>
        `;
    }
}
</script>

</body>
</html>
