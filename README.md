<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.2/css/all.min.css">
<style>
body{margin:0;font-family:Arial,sans-serif;background:#f5f5f5;color:#333;}
header{padding:20px;text-align:center;font-weight:900;font-size:24px;background:#ddd;}
.wrap{max-width:600px;margin:12px auto;padding:12px;}
.card{background:#fff;padding:12px;border-radius:12px;margin-bottom:12px;border:1px solid #ccc;}
input,select,button{width:100%;padding:10px;margin-top:6px;border-radius:6px;border:1px solid #ccc;font-size:14px;}
button{cursor:pointer;background:#007bff;color:#fff;border:none;}
button:hover{background:#0056b3;}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;padding:10px;background:#eee;border-top:1px solid #ccc;}
.nav div{cursor:pointer;text-align:center;font-size:12px;}
.nav div i{display:block;margin-bottom:4px;}
.plan{padding:10px;border:1px solid #ccc;border-radius:8px;margin-bottom:8px;background:#fafafa;}
.countdown{font-size:12px;color:#555;margin-top:4px;}
</style>
</head>
<body>

<header><i class="fas fa-bolt"></i> VERBOSE</header>

<div class="wrap">

<!-- LOGIN / SIGNUP -->
<div id="loginCard" class="card">
<h3><i class="fas fa-user"></i> Login / Signup</h3>
<select id="authMode">
<option value="login">Login</option>
<option value="signup">New User</option>
</select>
<input id="inputUser" placeholder="Username"/>
<input id="inputPass" placeholder="Password" type="password"/>
<button onclick="doAuth()"><i class="fas fa-sign-in-alt"></i> Submit</button>
</div>

<!-- DASHBOARD -->
<div id="dashboardCard" class="card" style="display:none;">
<h3><i class="fas fa-user-check"></i> Welcome <span id="welcomeText"></span></h3>
<p>Member since: <span id="memberSince"></span></p>
<p><i class="fas fa-coins"></i> Balance: Rs <span id="balanceText">0</span></p>
<p><i class="fas fa-calendar-day"></i> Daily: Rs <span id="dailyText">0</span></p>
<button onclick="doLogout()" style="width:120px;margin:auto;display:block;"><i class="fas fa-sign-out-alt"></i> Logout</button>
</div>

<!-- PLANS -->
<div id="plansCard" class="card" style="display:none;">
<h3><i class="fas fa-gift"></i> Special Plans</h3>
<div id="plansList"></div>
</div>

<!-- DEPOSIT -->
<div id="depositCard" class="card" style="display:none;">
<h3><i class="fas fa-hand-holding-usd"></i> Deposit</h3>
<select id="depositMethod" onchange="updateDepositNumber()">
<option value="jazzcash">JazzCash — 03705519562</option>
<option value="easypaisa">EasyPaisa — 03379827882</option>
</select>
<input id="depositNumber" readonly/>
<input id="depositAmount" placeholder="Amount"/>
<input id="depositTx" placeholder="Transaction ID"/>
<button onclick="submitDeposit()"><i class="fas fa-paper-plane"></i> Submit Deposit</button>
</div>

<!-- WITHDRAW -->
<div id="withdrawCard" class="card" style="display:none;">
<h3><i class="fas fa-money-bill-wave"></i> Withdrawal</h3>
<select id="withdrawMethod">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
<option value="bank">Bank</option>
</select>
<input id="withdrawUsername" readonly/>
<input id="withdrawAccount" placeholder="Account / Mobile"/>
<input id="withdrawAmount" placeholder="Amount"/>
<button onclick="submitWithdraw()"><i class="fas fa-paper-plane"></i> Request Withdrawal</button>
</div>

<!-- SUPPORT -->
<div id="supportCard" class="card" style="display:none;">
<h3><i class="fas fa-headset"></i> Support / Administration</h3>
<p><i class="fas fa-phone"></i> Whatsapp: 03705519562</p>
<p><i class="fas fa-envelope"></i> Email: Rock.earn92@gmail.com</p>
<p><i class="fas fa-building"></i> Company: VERBOSE — Rock Earn Branch</p>
<p>Millions of users working worldwide.</p>
</div>

</div>

<div class="nav">
<div onclick="nav('dashboardCard')"><i class="fas fa-home"></i>Home</div>
<div onclick="nav('plansCard')"><i class="fas fa-box"></i>Plans</div>
<div onclick="nav('depositCard')"><i class="fas fa-wallet"></i>Deposit</div>
<div onclick="nav('withdrawCard')"><i class="fas fa-hand-holding-usd"></i>Withdraw</div>
<div onclick="nav('supportCard')"><i class="fas fa-headset"></i>Support</div>
</div>

<script>
// STORAGE KEYS
const KEY_USER='verbose_user';
const KEY_BAL='verbose_balance_';
const KEY_DAILY='verbose_daily_';
const KEY_USER_PLANS='verbose_plans_';
let currentUser=localStorage.getItem(KEY_USER)||null;

// PLANS
let plans=[];
for(let i=1;i<=7;i++){
  let invest=200*i;
  let days=20+Math.floor(Math.random()*51);
  let total=invest*3;
  let dailyProfit=Math.round(total/days);
  let offerEnd=Date.now()+24*3600*1000;
  plans.push({id:i,name:'Special Plan '+i,invest,total,days,dailyProfit,offerEnd});
}

// AUTH
function doAuth(){
  const mode=document.getElementById('authMode').value;
  const u=document.getElementById('inputUser').value.trim();
  const p=document.getElementById('inputPass').value.trim();
  if(!u||!p){alert('Enter username & password');return;}
  const credKey='verbose_cred_'+u;
  if(mode==='signup'){
    if(localStorage.getItem(credKey)){alert('Username exists');return;}
    localStorage.setItem(credKey,p);
    localStorage.setItem(KEY_BAL+u,'0');
    localStorage.setItem(KEY_DAILY+u,'0');
    localStorage.setItem(KEY_USER_PLANS+u,'[]');
  } else {
    if(localStorage.getItem(credKey)!==p){alert('Wrong username/password');return;}
  }
  localStorage.setItem(KEY_USER,u);currentUser=u;
  afterLoginUI();
}

function afterLoginUI(){nav('dashboardCard');renderDashboard();renderPlans();updateDepositNumber();}

function renderDashboard(){
  if(!currentUser) return;
  document.getElementById('welcomeText').innerText=currentUser;
  document.getElementById('memberSince').innerText=new Date().toLocaleDateString();
  document.getElementById('balanceText').innerText=localStorage.getItem(KEY_BAL+currentUser)||'0';
  document.getElementById('dailyText').innerText=localStorage.getItem(KEY_DAILY+currentUser)||'0';
}

// NAV
function nav(cardId){
  ['loginCard','dashboardCard','plansCard','depositCard','withdrawCard','supportCard'].forEach(id=>document.getElementById(id).style.display='none');
  document.getElementById(cardId).style.display='block';
  if(cardId==='withdrawCard') fillWithdrawUser();
}

// PLANS
function renderPlans(){
  const container=document.getElementById('plansList');container.innerHTML='';
  plans.forEach(plan=>{
    const div=document.createElement('div');div.className='plan';
    div.innerHTML=`<h4><i class="fas fa-gift"></i> ${plan.name}</h4>
      <p>Invest: Rs ${plan.invest} | Total: Rs ${plan.total} | Days: ${plan.days} | Daily: Rs ${plan.dailyProfit}</p>
      <p class="countdown" id="countdown_${plan.id}"></p>
      <button onclick="buyPlan(${plan.id})"><i class="fas fa-shopping-cart"></i> Buy Plan</button>`;
    container.appendChild(div);
    startCountdown(plan.id,plan.offerEnd);
  });
}

function startCountdown(id,endTs){
  const el=document.getElementById('countdown_'+id);
  function tick(){
    const diff=Math.floor((endTs-Date.now())/1000);
    if(diff<=0){el.innerText='Offer ended';return;}
    const h=Math.floor(diff/3600), m=Math.floor((diff%3600)/60), s=diff%60;
    el.innerText=`Offer ends in: ${String(h).padStart(2,'0')}h:${String(m).padStart(2,'0')}m:${String(s).padStart(2,'0')}s`;
    setTimeout(tick,1000);
  }
  tick();
}

function buyPlan(id){
  if(!currentUser){alert('Login first');return;}
  const plan=plans.find(p=>p.id===id);
  if(!plan){alert('Plan not available');return;}
  let userPlans=JSON.parse(localStorage.getItem(KEY_USER_PLANS+currentUser)||'[]');
  userPlans.push({planId:plan.id,dailyProfit:plan.dailyProfit,lastCredit:Date.now()});
  localStorage.setItem(KEY_USER_PLANS+currentUser,JSON.stringify(userPlans));
  document.getElementById('depositAmount').value=plan.invest;
  updateDepositNumber();
  nav('depositCard');
  alert('Plan selected. Submit deposit. Admin will approve manually.');
}

// DEPOSIT
function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}

function submitDeposit(){
  if(!currentUser){alert('Login first');return;}
  const amount=document.getElementById('depositAmount').value;
  const tx=document.getElementById('depositTx').value.trim();
  if(!amount||!tx){alert('Enter amount and TX ID');return;}
  let deposits=JSON.parse(localStorage.getItem('verbose_deposits')||'[]');
  deposits.push({user:currentUser,method:document.getElementById('depositMethod').value,amount,tx,time:Date.now(),approved:false});
  localStorage.setItem('verbose_deposits',JSON.stringify(deposits));
  alert('Deposit submitted. Admin will approve.');
  document.getElementById('depositTx').value='';
  nav('dashboardCard');
}

// WITHDRAW
function fillWithdrawUser(){if(currentUser) document.getElementById('withdrawUsername').value=currentUser;}
function submitWithdraw(){
  if(!currentUser){alert('Login first');return;}
  const account=document.getElementById('withdrawAccount').value.trim();
  const amount=document.getElementById('withdrawAmount').value;
  if(!account||!amount){alert('Enter account and amount');return;}
  let withdraws=JSON.parse(localStorage.getItem('verbose_withdraws')||'[]');
  withdraws.push({user:currentUser,method:document.getElementById('withdrawMethod').value,account,amount,time:Date.now(),approved:false});
  localStorage.setItem('verbose_withdraws',JSON.stringify(withdraws));
  alert('Withdrawal requested. Admin will approve.');
  document.getElementById('withdrawAccount').value='';
  document.getElementById('withdrawAmount').value='';
  nav('dashboardCard');
}

// LOGOUT
function doLogout(){currentUser=null;localStorage.removeItem(KEY_USER);nav('loginCard');}

</script>

</body>
</html>
