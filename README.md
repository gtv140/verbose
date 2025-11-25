<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Login - Premium App</title>
<style>
body {
    margin:0;
    padding:0;
    background:#050505;
    font-family: Arial, sans-serif;
    color:#fff;
}

/* Center Box */
.container {
    width: 90%;
    max-width: 350px;
    margin: 80px auto 0;
    padding: 25px;
    background: rgba(20,20,20,0.9);
    border-radius: 18px;
    box-shadow: 0 0 25px #00f2ff;
    text-align:center;
}

/* Heading */
h2 {
    margin: 0 0 15px 0;
    font-size: 26px;
    text-shadow: 0 0 10px #00eaff;
}

/* Inputs */
input {
    width: 90%;
    padding: 12px;
    border-radius:10px;
    border:none;
    margin:10px 0;
    background:#111;
    color:white;
    box-shadow: inset 0 0 10px #0ff;
}

/* Buttons */
button {
    width: 95%;
    padding: 12px;
    border: none;
    margin-top:10px;
    border-radius: 10px;
    background:#00eaff;
    color:#000;
    font-weight:bold;
    font-size:16px;
    box-shadow: 0 0 15px #00eaff;
}
button:hover {
    background:#00bcd4;
    cursor:pointer;
}

/* Switch text */
.switch {
    margin-top:12px;
    font-size:14px;
    color:#bbb;
}
.switch span {
    color:#00eaff;
    text-decoration:underline;
    cursor:pointer;
}
</style>
</head>

<body>

<!-- LOGIN PAGE -->
<div class="container" id="loginBox">
    <h2>Login</h2>
    <input type="text" id="loginUser" placeholder="Enter Username" />
    <input type="password" id="loginPass" placeholder="Enter Password" />
    <button onclick="doLogin()">Login</button>

    <p class="switch">
        Don't have an account? <span onclick="showSignup()">Signup</span>
    </p>
</div>


<!-- SIGNUP PAGE -->
<div class="container" id="signupBox" style="display:none;">
    <h2>Signup</h2>
    <input type="text" id="newUser" placeholder="Create Username" />
    <input type="password" id="newPass" placeholder="Create Password" />
    <button onclick="doSignup()">Create Account</button>

    <p class="switch">
        Already have an account? <span onclick="showLogin()">Login</span>
    </p>
</div>


<script>
/* Switch Pages */
function showSignup(){
    document.getElementById("loginBox").style.display = "none";
    document.getElementById("signupBox").style.display = "block";
}

function showLogin(){
    document.getElementById("signupBox").style.display = "none";
    document.getElementById("loginBox").style.display = "block";
}

/* Signup Save */
function doSignup(){
    let u = document.getElementById("newUser").value;
    let p = document.getElementById("newPass").value;

    if(u=="" || p==""){ alert("Fill all fields"); return; }

    let user = {username:u, password:p, balance:0};
    localStorage.setItem("appUser", JSON.stringify(user));

    alert("Account Created Successfully");
    showLogin();
}

/* Login Check */
function doLogin(){
    let u = document.getElementById("loginUser").value;
    let p = document.getElementById("loginPass").value;

    let saved = localStorage.getItem("appUser");
    if(!saved){ alert("No account found"); return; }

    let user = JSON.parse(saved);

    if(u === user.username && p === user.password){
        alert("Login Successful");
        window.location.href = "dashboard.html";
    } else {
        alert("Invalid Credentials");
    }
}
</script>

</body>
</html><!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Dashboard - Premium App</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<style>
body{
    margin:0;
    font-family:Arial,sans-serif;
    background:#0d0d0d;
    color:#fff;
}

/* Header */
.app-header{
    display:flex;
    justify-content:space-between;
    align-items:center;
    padding:15px;
    background:#111;
    box-shadow:0 2px 10px rgba(0,0,0,0.5);
}
.app-header h1{
    font-size:20px;
    margin:0;
    color:#0af;
}

/* Balance Card */
.balance-card{
    background:#111;
    padding:15px;
    margin:15px;
    border-radius:15px;
    box-shadow:0 0 20px #0af;
    display:flex;
    justify-content:space-between;
}
.balance-card div{ text-align:center;}
.balance-card h3{ margin:5px 0; color:#0ff; }
.balance-card p{ margin:0; color:#aaa; }

/* Icon Grid */
.icon-grid{
    display:grid;
    grid-template-columns:repeat(3,1fr);
    gap:12px;
    padding:15px;
}
.icon-box{
    background:#1a1a1a;
    padding:18px;
    text-align:center;
    border-radius:15px;
    box-shadow:0 0 15px #00f2ff;
    transition:0.3s;
}
.icon-box:hover{
    transform:scale(1.05);
    background:#222;
}
.icon-box i{
    font-size:28px;
    margin-bottom:10px;
    color:#0af;
}
.icon-box p{
    margin:0;
    font-size:14px;
}

/* Plans Section */
.plan-grid{
    display:grid;
    grid-template-columns:repeat(auto-fill,minmax(150px,1fr));
    gap:12px;
    padding:15px;
}
.plan-box{
    background:#111;
    border-radius:12px;
    padding:12px;
    box-shadow:0 0 15px #0ff;
    text-align:center;
    transition:0.3s;
}
.plan-box:hover{
    transform:translateY(-5px);
    box-shadow:0 0 25px #0ff;
}
.plan-box h4{
    margin:5px 0;
    color:#0af;
}
.plan-box p{
    font-size:12px;
    margin:2px 0;
    color:#ccc;
}
.plan-box .badge{
    display:inline-block;
    background:#ffea00;
    color:#000;
    font-size:10px;
    padding:3px 5px;
    border-radius:6px;
    margin-top:5px;
}

/* Footer Menu */
.footer-menu{
    position:fixed;
    bottom:0;
    left:0;
    width:100%;
    background:#111;
    display:flex;
    justify-content:space-around;
    padding:10px 0;
    box-shadow:0 -2px 10px rgba(0,0,0,0.5);
}
.footer-menu div{
    text-align:center;
    color:#fff;
    font-size:12px;
}
.footer-menu i{
    display:block;
    font-size:22px;
    margin-bottom:5px;
    color:#0af;
}

/* Responsive Scroll */
body, html{overflow-x:hidden;}
</style>
</head>
<body>

<!-- Header -->
<div class="app-header">
    <h1>Dashboard</h1>
    <i class="fa fa-user" style="font-size:22px"></i>
</div>

<!-- Balance Cards -->
<div class="balance-card">
    <div>
        <h3 id="walletBalance">0 PKR</h3>
        <p>Wallet</p>
    </div>
    <div>
        <h3 id="profitBalance">0 PKR</h3>
        <p>Profit</p>
    </div>
</div>

<!-- Icon Grid -->
<div class="icon-grid">
    <div class="icon-box" onclick="alert('Wallet Clicked')">
        <i class="fa fa-wallet"></i>
        <p>Wallet</p>
    </div>
    <div class="icon-box" onclick="alert('Plans Clicked')">
        <i class="fa fa-briefcase"></i>
        <p>Plans</p>
    </div>
    <div class="icon-box" onclick="alert('Deposit Clicked')">
        <i class="fa fa-money-bill"></i>
        <p>Deposit</p>
    </div>
    <div class="icon-box" onclick="alert('Withdraw Clicked')">
        <i class="fa fa-hand-holding-dollar"></i>
        <p>Withdraw</p>
    </div>
    <div class="icon-box" onclick="alert('Transactions Clicked')">
        <i class="fa fa-receipt"></i>
        <p>Transactions</p>
    </div>
    <div class="icon-box" onclick="alert('Referral Clicked')">
        <i class="fa fa-user-plus"></i>
        <p>Referral</p>
    </div>
</div>

<!-- Plans Section -->
<h3 style="padding-left:15px;">Available Plans</h3>
<div class="plan-grid">
    <div class="plan-box">
        <h4>Plan 1</h4>
        <p>Invest: 500 PKR</p>
        <p>Days: 7</p>
        <p>Profit: 1200 PKR</p>
        <span class="badge">Special</span>
    </div>
    <div class="plan-box">
        <h4>Plan 2</h4>
        <p>Invest: 1000 PKR</p>
        <p>Days: 14</p>
        <p>Profit: 2500 PKR</p>
    </div>
    <div class="plan-box">
        <h4>Plan 3</h4>
        <p>Invest: 2000 PKR</p>
        <p>Days: 21</p>
        <p>Profit: 5000 PKR</p>
    </div>
    <div class="plan-box">
        <h4>Plan 4</h4>
        <p>Invest: 5000 PKR</p>
        <p>Days: 28</p>
        <p>Profit: 12000 PKR</p>
        <span class="badge">Special</span>
    </div>
</div>

<!-- Footer Menu -->
<div class="footer-menu">
    <div onclick="alert('Home')"><i class="fa fa-home"></i>Home</div>
    <div onclick="alert('Plans')"><i class="fa fa-layer-group"></i>Plans</div>
    <div onclick="alert('Account')"><i class="fa fa-user"></i>Account</div>
</div>

<script>
/* Load wallet data */
let user = JSON.parse(localStorage.getItem("appUser"));
if(user){
    document.getElementById("walletBalance").innerText = user.balance + " PKR";
    document.getElementById("profitBalance").innerText = user.profit ? user.profit + " PKR" : "0 PKR";
}
</script>

</body>
</html>
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Transactions & Referral - Premium App</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<style>
body{
    margin:0;
    font-family:Arial,sans-serif;
    background:#0d0d0d;
    color:#fff;
}

/* Header */
.app-header{
    display:flex;
    justify-content:space-between;
    align-items:center;
    padding:15px;
    background:#111;
    box-shadow:0 2px 10px rgba(0,0,0,0.5);
}
.app-header h1{
    font-size:20px;
    margin:0;
    color:#0af;
}

/* Form Cards */
.card{
    background:#111;
    margin:15px;
    padding:15px;
    border-radius:15px;
    box-shadow:0 0 20px #0ff;
}
.card h3{
    margin-top:0;
    color:#0af;
}
.card input, .card select{
    width:100%;
    padding:10px;
    margin-top:8px;
    border-radius:10px;
    border:none;
    background:#1a1a1a;
    color:#fff;
}
.card button{
    margin-top:10px;
    padding:10px;
    width:48%;
    border:none;
    border-radius:10px;
    cursor:pointer;
    font-weight:700;
}
.card button.primary{
    background:#0af;
    color:#000;
}
.card button.ghost{
    background:transparent;
    color:#0af;
    border:1px solid #0af;
}

/* Transactions Table */
.tx-table{
    width:100%;
    border-collapse:collapse;
    margin-top:10px;
}
.tx-table th, .tx-table td{
    padding:8px;
    border-bottom:1px solid rgba(0,255,255,0.1);
    font-size:12px;
    text-align:center;
}

/* Referral */
.referral{
    background:#111;
    margin:15px;
    padding:15px;
    border-radius:15px;
    box-shadow:0 0 20px #0ff;
}
.referral input{
    width:100%;
    padding:10px;
    border-radius:10px;
    border:none;
    background:#1a1a1a;
    color:#0af;
    font-weight:700;
}

/* Footer Menu */
.footer-menu{
    position:fixed;
    bottom:0;
    left:0;
    width:100%;
    background:#111;
    display:flex;
    justify-content:space-around;
    padding:10px 0;
    box-shadow:0 -2px 10px rgba(0,0,0,0.5);
}
.footer-menu div{
    text-align:center;
    color:#fff;
    font-size:12px;
}
.footer-menu i{
    display:block;
    font-size:22px;
    margin-bottom:5px;
    color:#0af;
}
</style>
</head>
<body>

<!-- Header -->
<div class="app-header">
    <h1>Transactions & Referral</h1>
    <i class="fa fa-user"></i>
</div>

<!-- Deposit Form -->
<div class="card">
    <h3>Deposit</h3>
    <select id="depositPlan">
        <option value="1">Plan 1 - 500 PKR</option>
        <option value="2">Plan 2 - 1000 PKR</option>
        <option value="3">Plan 3 - 2000 PKR</option>
    </select>
    <input type="text" id="depositMethod" placeholder="Payment Method">
    <input type="text" id="depositTx" placeholder="Transaction ID">
    <div style="display:flex;gap:10px;">
        <button class="primary" onclick="submitDeposit()">Deposit</button>
        <button class="ghost" onclick="alert('Cancel Deposit')">Cancel</button>
    </div>
</div>

<!-- Withdraw Form -->
<div class="card">
    <h3>Withdraw</h3>
    <select id="withdrawMethod">
        <option value="jazzcash">JazzCash</option>
        <option value="easypaisa">EasyPaisa</option>
    </select>
    <input type="text" id="withdrawAccount" placeholder="Account Number">
    <input type="number" id="withdrawAmount" placeholder="Amount PKR">
    <div style="display:flex;gap:10px;">
        <button class="primary" onclick="submitWithdraw()">Withdraw</button>
        <button class="ghost" onclick="alert('Cancel Withdraw')">Cancel</button>
    </div>
</div>

<!-- Transactions Table -->
<div class="card">
    <h3>Transactions</h3>
    <table class="tx-table" id="txTable">
        <thead>
            <tr>
                <th>Type</th>
                <th>Amount</th>
                <th>Plan</th>
                <th>Status</th>
                <th>Time</th>
            </tr>
        </thead>
        <tbody></tbody>
    </table>
</div>

<!-- Referral Section -->
<div class="referral">
    <h3>Referral</h3>
    <input type="text" id="refLink" readonly value="https://app.com/?ref=YourUserID">
    <p style="color:#aaa; font-size:12px; margin-top:5px;">Share your referral link to earn bonus!</p>
</div>

<!-- Footer Menu -->
<div class="footer-menu">
    <div onclick="alert('Home')"><i class="fa fa-home"></i>Home</div>
    <div onclick="alert('Plans')"><i class="fa fa-layer-group"></i>Plans</div>
    <div onclick="alert('Account')"><i class="fa fa-user"></i>Account</div>
</div>

<script>
/* Sample Storage */
let user = JSON.parse(localStorage.getItem("appUser")) || {balance:0,profit:0,transactions:[],referrals:0};

/* Submit Deposit */
function submitDeposit(){
    const plan = document.getElementById("depositPlan").value;
    const method = document.getElementById("depositMethod").value;
    const tx = document.getElementById("depositTx").value;
    if(!method || !tx){ alert('Fill all deposit fields'); return; }
    const amount = plan==1?500:plan==2?1000:2000;
    user.balance += amount;
    user.transactions.push({type:'Deposit',amount,plan:'Plan '+plan,status:'Success',time:new Date().toLocaleString()});
    localStorage.setItem("appUser", JSON.stringify(user));
    alert('Deposit Successful');
    loadTx();
}

/* Submit Withdraw */
function submitWithdraw(){
    const method = document.getElementById("withdrawMethod").value;
    const acc = document.getElementById("withdrawAccount").value;
    const amount = parseInt(document.getElementById("withdrawAmount").value);
    if(!method || !acc || !amount){ alert('Fill all withdraw fields'); return; }
    if(amount>user.balance){ alert('Insufficient balance'); return; }
    user.balance -= amount;
    user.transactions.push({type:'Withdraw',amount,plan:'-',status:'Pending',time:new Date().toLocaleString()});
    localStorage.setItem("appUser", JSON.stringify(user));
    alert('Withdraw Requested');
    loadTx();
}

/* Load Transactions */
function loadTx(){
    const tbody = document.getElementById("txTable").querySelector('tbody');
    tbody.innerHTML = '';
    user.transactions.forEach(tx=>{
        const tr = document.createElement('tr');
        tr.innerHTML=`<td>${tx.type}</td><td>${tx.amount}</td><td>${tx.plan}</td><td>${tx.status}</td><td>${tx.time}</td>`;
        tbody.appendChild(tr);
    });
}
loadTx();

/* Referral Link Auto Fill */
document.getElementById("refLink").value = `https://app.com/?ref=YourUserID&bonus=${user.referrals}`;
</script>

</body>
</html>
