<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE</title>
<style>
body{margin:0;font-family:Arial,sans-serif;background:#f0f0f0;color:#333;}
header{padding:20px;text-align:center;font-weight:900;font-size:28px;background:#222;color:#fff;}
.card{max-width:500px;margin:20px auto;padding:20px;background:#fff;border-radius:10px;box-shadow:0 4px 8px rgba(0,0,0,0.1);}
input,select,button{width:100%;padding:10px;margin-top:6px;border-radius:6px;border:1px solid #ccc;font-size:14px;}
button{cursor:pointer;background:#28a745;color:#fff;border:none;font-weight:600;}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;padding:10px;background:#eee;border-top:1px solid #ccc;}
.nav div{cursor:pointer;text-align:center;font-size:14px;}
.plan{display:flex;justify-content:space-between;align-items:center;padding:10px;border:1px solid #ccc;border-radius:8px;margin-bottom:8px;background:#fafafa;}
.hidden{display:none;}
.countdown{font-weight:600;color:#d9534f;}
.copy-btn{padding:6px 12px;margin-left:6px;background:#007bff;color:#fff;border:none;border-radius:4px;cursor:pointer;}
</style>
</head>
<body>

<header>VERBOSE</header>

<div class="card" id="loginCard">
<h3>Login / Signup</h3>
<select id="authMode">
<option value="login">Login</option>
<option value="signup">New User</option>
</select>
<input id="inputUser" placeholder="Username"/>
<input id="inputPass" placeholder="Password" type="password"/>
<input id="referralInput" placeholder="Referral Code (optional)"/>
<button onclick="doAuth()">Submit</button>
</div>

<div class="card hidden" id="dashboardCard">
<div id="welcomeText" style="font-weight:800;font-size:18px;margin-bottom:8px;"></div>
<div id="balanceText" style="margin-bottom:8px;">Balance: Rs 0</div>
<div>
<button onclick="doLogout()" style="width:100%;margin-top:10px;background:#dc3545;">Logout</button>
</div>
</div>

<div class="card hidden" id="plansCard">
<h3>Special & Normal Plans</h3>
<div id="plansList"></div>
</div>

<div class="card hidden" id="depositCard">
<h3>Deposit</h3>
<label>Method</label>
<select id="depositMethod" onchange="updateDepositNumber()">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<label>Number</label>
<div style="display:flex;align-items:center;">
<input id="depositNumber" readonly/>
<button class="copy-btn" onclick="copyNumber()">Copy</button>
</div>
<label>Amount (PKR)</label>
<input id="depositAmount" readonly/>
<label>Transaction ID</label>
<input id="depositTx" placeholder="Enter TX ID"/>
<label>Upload Proof</label>
<input id="depositProof" type="file"/>
<button onclick="submitDeposit()">Submit Deposit</button>
</div>

<script>
// STORAGE
const KEY_USER='verbose_user';
const KEY_BAL='verbose_balance_';
let currentUser=localStorage.getItem(KEY_USER)||null;

// PLANS DATA
let plans=[];
for(let i=1;i<=7;i++){plans.push({id:i,name:'Special Plan '+i,invest:200*i,total:(200*i)*3,days:20+Math.floor(Math.random()*51),special:true});}
for(let i=8;i<=32;i++){plans.push({id:i,name:'Plan '+(i-7),invest:3000+(i-8)*1000,total:(3000+(i-8)*1000)*2.5,days:20+Math.floor(Math.random()*51),special:false});}
for(let i=33;i<=37;i++){plans.push({id:i,name:'Coming Soon',invest:0,total:0,days:0,special:false});}

// AUTH
function doAuth(){
const mode=document.getElementById('authMode').value;
const u=document.getElementById('inputUser').value.trim();
const p=document.getElementById('inputPass').value.trim();
if(!u||!p){alert('Enter username & password');return;}
if(mode==='signup'){localStorage.setItem('cred_'+u,p);localStorage.setItem(KEY_BAL+u,'0');} 
else if(localStorage.getItem('cred_'+u)!==p){alert('Wrong username/password');return;}
localStorage.setItem(KEY_USER,u);currentUser=u;afterLoginUI();
}

function afterLoginUI(){
document.getElementById('loginCard').classList.add('hidden');
document.getElementById('dashboardCard').classList.remove('hidden');
document.getElementById('plansCard').classList.remove('hidden');
renderDashboard();
renderPlans();
}

// LOGOUT
function doLogout(){
localStorage.removeItem(KEY_USER);
currentUser=null;
document.getElementById('dashboardCard').classList.add('hidden');
document.getElementById('plansCard').classList.add('hidden');
document.getElementById('loginCard').classList.remove('hidden');
}

// DASHBOARD
function renderDashboard(){
if(!currentUser)return;
document.getElementById('welcomeText').innerText='Welcome, '+currentUser;
document.getElementById('balanceText').innerText='Balance: Rs '+(localStorage.getItem(KEY_BAL+currentUser)||0);
}

// PLANS
function renderPlans(){
const container=document.getElementById('plansList');container.innerHTML='';
plans.forEach(plan=>{
const div=document.createElement('div');div.className='plan';
let daily=Math.floor(plan.total/plan.days)||0;
div.innerHTML=`<div>
<strong>${plan.name}</strong><br>
Invest: Rs ${plan.invest} | Total: Rs ${plan.total} | Days: ${plan.days} | Daily: Rs ${daily}
${plan.special?`<br><span class="countdown" id="cd_${plan.id}"></span>`:''}
</div>
<div>
${plan.name!=='Coming Soon'?`<button onclick="buyPlan(${plan.id})">Buy Now</button>`:''}
</div>`;
container.appendChild(div);
if(plan.special) startCountdown(plan.id);
});
}

// COUNTDOWN
function startCountdown(id){
let end=Date.now()+24*3600*1000;
function tick(){const diff=Math.floor((end-Date.now())/1000);if(diff<=0){document.getElementById('cd_'+id).innerText='Offer ended';return;}
let h=Math.floor(diff/3600),m=Math.floor((diff%3600)/60),s=diff%60;
document.getElementById('cd_'+id).innerText=`Ends in ${h}h:${m}m:${s}s`;}
tick();setInterval(tick,1000);
}

// BUY PLAN
function buyPlan(id){
if(!currentUser){alert('Login first');return;}
let plan=plans.find(p=>p.id===id);
document.getElementById('depositAmount').value=plan.invest;
document.getElementById('depositCard').classList.remove('hidden');
document.getElementById('plansCard').classList.add('hidden');
}

// DEPOSIT
function updateDepositNumber(){
const method=document.getElementById('depositMethod').value;
document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}
function copyNumber(){
const input=document.getElementById('depositNumber');input.select();document.execCommand('copy');alert('Number copied!');
}
function submitDeposit(){
const amount=Number(document.getElementById('depositAmount').value);
let bal=Number(localStorage.getItem(KEY_BAL+currentUser)||0);
bal+=amount;
localStorage.setItem(KEY_BAL+currentUser,bal);
alert('Deposit submitted! Balance updated.');
document.getElementById('depositCard').classList.add('hidden');
document.getElementById('dashboardCard').classList.remove('hidden');
renderDashboard();
}

// INITIAL CHECK
if(currentUser) afterLoginUI();
</script>

</body>
</html>
