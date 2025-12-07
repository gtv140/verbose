<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE</title>
<style>
body{margin:0;font-family:Arial,sans-serif;background:#f7f7f7;color:#111;}
header{padding:20px;text-align:center;font-weight:900;font-size:28px;color:#222;background:#fff;border-bottom:1px solid #ccc;}
.wrap{max-width:480px;margin:12px auto;padding:12px;background:#fff;border-radius:10px;box-shadow:0 2px 10px rgba(0,0,0,0.1);}
.card{padding:12px;margin-bottom:12px;border:1px solid #ddd;border-radius:8px;}
input,select,button{width:100%;padding:10px;margin-top:6px;border-radius:6px;border:1px solid #ccc;font-size:14px;}
button{background:#28a745;color:#fff;border:none;cursor:pointer;}
button:hover{opacity:0.9;}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;padding:10px;background:#fff;border-top:1px solid #ccc;}
.nav div{cursor:pointer;font-size:14px;text-align:center;}
.hidden{display:none;}
.plan{border:1px solid #ccc;padding:8px;border-radius:8px;margin-bottom:8px;}
.plan button{width:auto;padding:6px 12px;margin-top:4px;}
.countdown{font-weight:600;color:#d9534f;margin-top:4px;}
.alert{background:#ffeeba;padding:8px;border-radius:6px;margin-bottom:10px;}
</style>
</head>
<body>
<header>VERBOSE</header>
<div class="wrap">

<!-- LOGIN -->
<div id="loginCard" class="card">
<h3>Login / Signup</h3>
<select id="authMode">
<option value="login">Login</option>
<option value="signup">New User</option>
</select>
<input id="inputUser" placeholder="Username"/>
<input id="inputPass" placeholder="Password" type="password"/>
<button onclick="doAuth()">Submit</button>
</div>

<!-- DASHBOARD -->
<div id="dashboardCard" class="card hidden">
<div class="alert">All transactions manually approved. Contact Support if needed.</div>
<div id="welcomeText" style="font-weight:bold;">Welcome —</div>
<div id="balanceText">Balance: 0</div>
<button id="logoutBtn" onclick="doLogout()">Logout</button>
</div>

<!-- PLANS -->
<div id="plansCard" class="card hidden">
<h3>Plans</h3>
<div id="plansList"></div>
</div>

<!-- DEPOSIT -->
<div id="depositCard" class="card hidden">
<h3>Deposit</h3>
<label>Amount</label>
<input id="depositAmount" readonly/>
<label>Method</label>
<select id="depositMethod">
<option value="jazzcash">JazzCash — 03705519562</option>
<option value="easypaisa">EasyPaisa — 03379827882</option>
</select>
<label>Transaction ID</label>
<input id="depositTx" placeholder="Enter TX ID"/>
<label>Upload Proof</label>
<input id="depositProof" type="file"/>
<button onclick="submitDeposit()">Submit Deposit</button>
</div>

<!-- WITHDRAW -->
<div id="withdrawCard" class="card hidden">
<h3>Withdrawal</h3>
<label>Amount</label>
<input id="withdrawAmount" placeholder="Enter amount"/>
<button onclick="submitWithdraw()">Request Withdrawal</button>
</div>

</div>

<div class="nav">
<div onclick="nav('dashboardCard')">Home</div>
<div onclick="nav('plansCard')">Plans</div>
<div onclick="nav('depositCard')">Deposit</div>
<div onclick="nav('withdrawCard')">Withdraw</div>
</div>

<script>
// STORAGE KEYS
const KEY_USER='verbose_user';
const KEY_BAL='verbose_balance_';
let currentUser=localStorage.getItem(KEY_USER)||null;
let plans=[];

// SETUP PLANS
for(let i=1;i<=7;i++){
  let invest=200*i; if(invest>3000) invest=3000;
  let days=20+Math.floor(Math.random()*51);
  plans.push({id:i,name:'Special Plan '+i,invest:invest,days:days,special:true});
}
for(let i=8;i<=30;i++){
  let invest=3000 + (i-8)*1000;
  let days=20+Math.floor(Math.random()*51);
  plans.push({id:i,name:'Plan '+(i-7),invest:invest,days:days,special:false});
}
for(let i=31;i<=35;i++){
  plans.push({id:i,name:'Coming Soon',invest:0,days:0,special:false});
}

function fmt(n){return Number(n).toLocaleString();}

// AUTH
function doAuth(){
  const u=document.getElementById('inputUser').value.trim();
  const p=document.getElementById('inputPass').value.trim();
  if(!u||!p){alert('Enter username & password');return;}
  let users=JSON.parse(localStorage.getItem('verbose_users')||'{}');
  const mode=document.getElementById('authMode').value;
  if(mode==='signup'){
    if(users[u]){alert('Username exists');return;}
    users[u]={pass:p,balance:0};
    localStorage.setItem('verbose_users',JSON.stringify(users));
  }else{
    if(!users[u]||users[u].pass!==p){alert('Wrong username/password');return;}
  }
  localStorage.setItem(KEY_USER,u);
  currentUser=u;
  renderDashboard();
  nav('dashboardCard');
  renderPlans();
}

// LOGOUT
function doLogout(){localStorage.removeItem(KEY_USER);currentUser=null;nav('loginCard');}

// NAV
function nav(cardId){
  ['loginCard','dashboardCard','plansCard','depositCard','withdrawCard'].forEach(id=>document.getElementById(id).classList.add('hidden'));
  document.getElementById(cardId).classList.remove('hidden');
  if(cardId==='dashboardCard') document.getElementById('logoutBtn').style.display='block';
  else document.getElementById('logoutBtn').style.display='none';
  if(currentUser) renderDashboard();
}

// DASHBOARD
function renderDashboard(){
  if(!currentUser) return;
  let users=JSON.parse(localStorage.getItem('verbose_users')||'{}');
  document.getElementById('welcomeText').innerText='Welcome, '+currentUser;
  document.getElementById('balanceText').innerText='Balance: Rs '+fmt(users[currentUser].balance);
}

// PLANS
function renderPlans(){
  const container=document.getElementById('plansList');
  container.innerHTML='';
  plans.forEach(plan=>{
    const div=document.createElement('div');
    div.className='plan';
    let daily=Math.round(plan.invest/plan.days);
    let timerHtml='';
    if(plan.special){
      timerHtml=`<div class="countdown" id="countdown_${plan.id}"></div>`;
      startCountdown(plan.id);
    }
    div.innerHTML=`<b>${plan.name}</b> - Rs ${fmt(plan.invest)} - ${plan.days} days - Daily: Rs ${fmt(daily)} ${timerHtml}<br>
      ${plan.name!=='Coming Soon'?'<button onclick="buyPlan('+plan.id+')">Buy Now</button>':''}`;
    container.appendChild(div);
  });
}

// BUY PLAN
function buyPlan(id){
  const plan=plans.find(p=>p.id===id);
  if(!plan) return;
  document.getElementById('depositAmount').value=plan.invest;
  nav('depositCard');
}

// DEPOSIT
function submitDeposit(){
  if(!currentUser){alert('Login first');return;}
  let users=JSON.parse(localStorage.getItem('verbose_users')||'{}');
  let amt=Number(document.getElementById('depositAmount').value)||0;
  users[currentUser].balance+=amt;
  localStorage.setItem('verbose_users',JSON.stringify(users));
  alert('Deposit submitted and balance updated!');
  nav('dashboardCard');
  renderDashboard();
}

// WITHDRAW
function submitWithdraw(){
  if(!currentUser){alert('Login first');return;}
  let amt=Number(document.getElementById('withdrawAmount').value);
  let users=JSON.parse(localStorage.getItem('verbose_users')||'{}');
  if(amt>users[currentUser].balance){alert('Insufficient balance'); return;}
  users[currentUser].balance-=amt;
  localStorage.setItem('verbose_users',JSON.stringify(users));
  alert('Withdrawal requested!');
  renderDashboard();
}

// COUNTDOWN
function startCountdown(planId){
  const el=document.getElementById('countdown_'+planId);
  let endTs=Date.now() + 24*3600*1000;
  function tick(){
    let diff=Math.floor((endTs-Date.now())/1000);
    if(diff<=0){el.innerText='Offer Ended'; return;}
    let h=Math.floor(diff/3600), m=Math.floor((diff%3600)/60), s=diff%60;
    el.innerText=`${h}h:${m}m:${s}s`;
    requestAnimationFrame(tick);
  }
  tick();
}

// INITIAL LOAD
if(currentUser) nav('dashboardCard');
else nav('loginCard');

</script>
</body>
</html>
