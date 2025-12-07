<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>VERBOSE</title>
<style>
:root{--neon:#00f7ff;--accent:#ff5cff;--dark:#070707}
*{box-sizing:border-box;margin:0;padding:0;font-family:Arial,sans-serif}
body{background:var(--dark);color:#fff;overflow-x:hidden}
header{text-align:center;padding:20px;font-size:28px;font-weight:800;background:linear-gradient(90deg,var(--neon),var(--accent));-webkit-background-clip:text;-webkit-text-fill-color:transparent}
.login-box,.page{max-width:430px;margin:20px auto;padding:18px;border-radius:12px;background:rgba(255,255,255,0.03);border:1px solid rgba(0,255,240,0.06)}
input,button,select{width:100%;padding:10px;margin-top:10px;border-radius:8px;border:1px solid rgba(0,255,240,0.08);background:transparent;color:#e6f7fb;outline:none;font-size:14px}
button{background:linear-gradient(90deg,var(--neon),var(--accent));border:none;color:#001;font-weight:700;cursor:pointer}
.nav{position:fixed;bottom:0;left:0;right:0;background:#00000080;display:flex;justify-content:space-around;padding:10px;border-top:1px solid rgba(0,255,240,0.06)}
.nav div{text-align:center;cursor:pointer;width:60px}
.hidden{display:none}
.plan-box{border:1px solid rgba(0,255,240,0.08);padding:12px;margin:10px 0;border-radius:10px;background:rgba(255,255,255,0.03)}
.plan-btn{margin-top:8px;background:#00f7ff;color:#000;font-weight:700;border-radius:8px;padding:8px;width:100%}
.countdown{color:#00f7ff;font-weight:700;margin-top:5px}
.user-box{background:#00f7ff10;padding:12px;border-radius:12px;margin-bottom:12px}
</style>
</head>
<body>

<header>VERBOSE</header>

<!-- LOGIN PAGE -->
<div id="loginPage" class="login-box">
<h2>Login / Signup</h2>
<select id="userOption"><option value="login">Login</option><option value="signup">Signup</option></select>
<input id="user" placeholder="Username"/>
<input id="pass" type="password" placeholder="Password"/>
<button onclick="login()">Submit</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
<div class="user-box">
<b id="dashUser"></b><br>
Balance: Rs <span id="dashBalance">0</span><br>
Daily: Rs <span id="dashDaily">0</span>
</div>
<button onclick="logout()">Logout</button>
</div>

<!-- PLANS -->
<div id="plans" class="page hidden">
<h2>Plans</h2>
<div id="plansList"></div>
</div>

<!-- DEPOSIT -->
<div id="deposit" class="page hidden">
<h2>Deposit</h2>
<select id="depositMethod" onchange="updateDepositNumber()">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<input id="depositNumber" readonly/>
<input id="depositAmount" readonly/>
<input id="depositTx" placeholder="Transaction ID"/>
<button onclick="submitDeposit()">Submit</button>
</div>

<!-- WITHDRAW -->
<div id="withdraw" class="page hidden">
<h2>Withdraw</h2>
<input id="withdrawAmount" placeholder="Amount"/>
<input id="withdrawAccount" placeholder="Account Number"/>
<button onclick="submitWithdraw()">Withdraw</button>
</div>

<!-- NAVBAR -->
<div class="nav">
<div onclick="showPage('dashboard')">🏠</div>
<div onclick="showPage('plans')">📦</div>
<div onclick="showPage('deposit')">💰</div>
<div onclick="showPage('withdraw')">💵</div>
</div>

<script>
let currentUser = localStorage.getItem("user") || null;
let balance = Number(localStorage.getItem("balance")) || 0;

// LOGIN
function login(){
 let u=document.getElementById("user").value;
 let p=document.getElementById("pass").value;
 if(!u||!p){alert("Enter");return;}

 currentUser=u;
 localStorage.setItem("user",u);

 if(document.getElementById("userOption").value==="signup"){
   balance=0;
   localStorage.setItem("balance",0);
 }

 loadDashboard();
 document.getElementById("loginPage").classList.add("hidden");
 document.getElementById("dashboard").classList.remove("hidden");
}

// LOGOUT
function logout(){
 currentUser=null;
 localStorage.removeItem("user");
 document.getElementById("dashboard").classList.add("hidden");
 document.getElementById("loginPage").classList.remove("hidden");
}

// SHOW PAGE
function showPage(x){
 document.querySelectorAll('.page').forEach(e=>e.classList.add('hidden'));
 document.getElementById(x).classList.remove('hidden');
 loadDashboard();
}

// DASHBOARD UPDATE
function loadDashboard(){
 if(!currentUser) return;
 document.getElementById("dashUser").innerText=currentUser;
 document.getElementById("dashBalance").innerText=localStorage.getItem("balance");
}

// PLANS GENERATION
let plans=[];
for(let i=1;i<=7;i++){
 let invest=200*i;
 let total=invest*3;
 let days=5+i;
 plans.push({id:i,invest,total,days,offer:true,expiry:Date.now()+24*3600*1000});
}
for(let i=8;i<=25;i++){
 let invest=i*1000;
 let total=Math.round(invest*2.5);
 let days=10+i;
 plans.push({id:i,invest,total,days,offer:false});
}

renderPlans();

// RENDER PLAN LIST
function renderPlans(){
 let box=document.getElementById("plansList");
 box.innerHTML="";
 plans.forEach(p=>{
   let t = p.offer 
     ? `<div class='countdown' id='cd${p.id}'></div>` 
     : "";

   box.innerHTML+=`
    <div class="plan-box">
      <b>Plan ${p.id}</b><br>
      Invest: Rs ${p.invest}<br>
      Total: Rs ${p.total}<br>
      Days: ${p.days}<br>
      ${t}
      <button class="plan-btn" onclick="buyNow(${p.id})">Buy Now</button>
    </div>
   `;
   if(p.offer) startCountdown(p.id,p.expiry);
 });
}

// COUNTDOWN
function startCountdown(id,expiry){
 let el=document.getElementById("cd"+id);
 let timer=setInterval(()=>{
   let d=expiry-Date.now();
   if(d<=0){el.innerText="Expired";clearInterval(timer);return;}
   let h=Math.floor(d/3600000);
   let m=Math.floor((d%3600000)/60000);
   let s=Math.floor((d%60000)/1000);
   el.innerText=`${h}h ${m}m ${s}s`;
 },1000);
}

// BUY NOW → deposit page
function buyNow(id){
 let plan=plans.find(p=>p.id===id);
 document.getElementById("depositAmount").value=plan.invest;
 showPage("deposit");
}

// DEPOSIT NUMBER
function updateDepositNumber(){
 let m=document.getElementById("depositMethod").value;
 document.getElementById("depositNumber").value =
   m==="jazzcash" ? "03705519562" : "03379827882";
}
updateDepositNumber();

// SUBMIT DEPOSIT
function submitDeposit(){
 let amt=Number(document.getElementById("depositAmount").value);
 if(amt<=0){alert("No amount");return;}

 balance+=amt;
 localStorage.setItem("balance",balance);

 alert("Deposit successful!");
 loadDashboard();
 showPage("dashboard");
}

// WITHDRAW
function submitWithdraw(){
 let amt=Number(document.getElementById("withdrawAmount").value);
 if(amt<=0 || amt>balance){alert("Invalid");return;}

 balance-=amt;
 localStorage.setItem("balance",balance);

 alert("Withdrawal submitted!");
 loadDashboard();
 showPage("dashboard");
}

// AUTO LOGIN
if(currentUser){
 document.getElementById("loginPage").classList.add("hidden");
 document.getElementById("dashboard").classList.remove("hidden");
 loadDashboard();
}
</script>

</body>
</html>
