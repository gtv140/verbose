<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE Modern Earning</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:'Orbitron',sans-serif;}
body,html{height:100%;background:#0b0c1a;color:#fff;overflow-x:hidden;}
.bg-gradient{position:fixed;inset:0;z-index:-1;background:linear-gradient(120deg,#0f0530,#1b0a4a,#061022);background-size:600% 600%;animation:grad 25s ease infinite;}
@keyframes grad{0%{background-position:0% 50%}50%{background-position:100% 50%}100%{background-position:0% 50%}}
.card{background:rgba(255,255,255,0.05);border:1px solid rgba(0,255,255,0.1);padding:20px;border-radius:15px;margin:15px;transition:0.3s;backdrop-filter:blur(6px);}
.card:hover{transform:translateY(-5px);box-shadow:0 12px 30px rgba(0,255,240,0.2);}
input,select{width:100%;padding:10px;margin-top:8px;border-radius:8px;border:1px solid rgba(255,255,255,0.1);background:rgba(0,0,0,0.4);color:#fff;outline:none;transition:0.2s;}
input:focus,select:focus{border-color:#0ff;}
button.primary{background:#0ff;color:#000;padding:10px 15px;border-radius:10px;border:none;cursor:pointer;font-weight:700;margin-top:10px;transition:0.2s;}
button.primary:hover{background:#0ae;}
button.ghost{background:transparent;border:1px solid rgba(255,255,255,0.1);padding:10px;border-radius:8px;color:#fff;cursor:pointer;margin-left:8px;transition:0.2s;}
button.ghost:hover{background:rgba(0,255,240,0.1);}
.icon-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(100px,1fr));gap:15px;padding:20px;}
.icon-box{text-align:center;padding:15px;background:rgba(0,255,255,0.05);border-radius:15px;cursor:pointer;transition:0.3s;}
.icon-box:hover{transform:scale(1.05);box-shadow:0 0 20px #0ff;}
.icon-box i{font-size:28px;margin-bottom:8px;color:#0ff;}
.icon-box p{font-size:14px;color:#cfe;}
.footer-menu{position:fixed;bottom:0;width:100%;display:flex;justify-content:space-around;background:#111;padding:10px 0;}
.footer-menu div{text-align:center;color:#fff;font-size:12px;cursor:pointer;}
.footer-menu i{display:block;font-size:22px;margin-bottom:4px;color:#0ff;}
table{width:100%;border-collapse:collapse;margin-top:10px;}
th,td{padding:8px;border-bottom:1px solid rgba(255,255,255,0.1);font-size:13px;color:#0ff;}
.notif{position:fixed;right:18px;bottom:18px;background:#0ff;color:#000;padding:12px 16px;border-radius:12px;font-weight:700;box-shadow:0 10px 30px rgba(0,255,240,0.2);z-index:99;}
.auth-wrap{display:flex;flex-direction:column;align-items:center;justify-content:center;padding:30px;border-radius:14px;box-shadow:0 8px 30px rgba(0,255,240,0.2);}
.auth-wrap h2{text-align:center;margin-bottom:15px;}
</style>
</head>
<body>
<div class="bg-gradient"></div>

<!-- Login Page -->
<div id="loginPage" class="card auth-wrap">
<h2>Login / Signup</h2>
<input id="authUser" placeholder="Username">
<input id="authPass" placeholder="Password" type="password">
<div style="display:flex;margin-top:10px;">
<button class="primary" onclick="doLogin()">Login</button>
<button class="ghost" onclick="doSignup()">Signup</button>
</div>
<div class="muted small" style="margin-top:10px;">Admin: <strong>AdminKhan</strong> / <strong>SuperSecret123</strong></div>
</div>

<!-- Dashboard -->
<div id="dashboardPage" class="hidden">
<div class="icon-grid" id="dashboardIcons"></div>

<div id="depositView" class="card hidden">
<h2>Deposit</h2>
<label>Choose Plan</label>
<select id="depositPlan" onchange="updateDepositAmount()"></select>
<label>Amount</label>
<input id="depositAmount" readonly>
<label>Method</label>
<select id="depositMethod" onchange="updateDepositNumber()">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<input id="depositNumber" readonly>
<button class="ghost" onclick="copyText(document.getElementById('depositNumber').value)">Copy</button>
<label>Transaction ID</label>
<input id="depositTxId" placeholder="TX ID">
<label>Upload Proof</label>
<input type="file" id="depositProof">
<button class="primary" onclick="submitDeposit()">Submit Deposit</button>
</div>

<div id="withdrawView" class="card hidden">
<h2>Withdraw</h2>
<label>Username</label>
<input id="withdrawUser" placeholder="Your Username">
<label>Account Number</label>
<input id="withdrawAccount" placeholder="Account Number">
<label>Amount PKR</label>
<input id="withdrawAmount" type="number">
<label>Method</label>
<select id="withdrawMethod">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<button class="primary" onclick="submitWithdraw()">Request Withdraw</button>
</div>

<div id="txView" class="card hidden">
<h2>Transactions</h2>
<table>
<thead><tr><th>Type</th><th>User</th><th>Account</th><th>Amount</th><th>Plan</th><th>Time</th><th>Status</th></tr></thead>
<tbody id="txTableBody"></tbody>
</table>
</div>

<div id="profileView" class="card hidden">
<h2>Profile</h2>
<label>Display Name</label>
<input id="profileName">
<div id="profileStats" class="muted small"></div>
<button class="primary" onclick="updateProfile()">Update Profile</button>
</div>

<div class="footer-menu">
<div onclick="navigate('wallet')"><i class="fa fa-wallet"></i>Wallet</div>
<div onclick="navigate('plans')"><i class="fa fa-briefcase"></i>Plans</div>
<div onclick="navigate('deposit')"><i class="fa fa-money-bill-wave"></i>Deposit</div>
<div onclick="navigate('withdraw')"><i class="fa fa-credit-card"></i>Withdraw</div>
<div onclick="navigate('transactions')"><i class="fa fa-file-invoice"></i>Transactions</div>
</div>

<div id="toastRoot"></div>

<script>
// ---------- Storage ----------
const ADMIN={user:'AdminKhan',pass:'SuperSecret123'};
const depositNumbers={jazzcash:'03705519562',easypaisa:'03379827882'};
const plans=[]; for(let i=1;i<=20;i++){plans.push({id:i,name:`Plan ${i}`,days:10+i,invest:100*i});}
const dashboardIcons=[
{icon:'fa-wallet',name:'Wallet',view:'wallet'},
{icon:'fa-briefcase',name:'Plans',view:'plans'},
{icon:'fa-money-bill-wave',name:'Deposit',view:'deposit'},
{icon:'fa-credit-card',name:'Withdraw',view:'withdraw'},
{icon:'fa-file-invoice',name:'Transactions',view:'transactions'},
{icon:'fa-user',name:'Profile',view:'profile'}
];

function getUsers(){return JSON.parse(localStorage.getItem('verbose_users')||'[]');}
function setUsers(u){localStorage.setItem('verbose_users',JSON.stringify(u));}
function getCurrent(){return JSON.parse(localStorage.getItem('verbose_current')||'null');}
function setCurrent(u){localStorage.setItem('verbose_current',JSON.stringify(u));}
function showToast(txt){const n=document.createElement('div');n.className='notif';n.innerText=txt;document.body.appendChild(n);setTimeout(()=>n.remove(),2000);}
(function(){let u=getUsers();if(!u.find(x=>x.user===ADMIN.user)){u.push({user:ADMIN.user,pass:ADMIN.pass,balance:0,active:[],admin:true});setUsers(u);}})();

// ---------- Auth ----------
function afterLogin(){const cur=getCurrent();if(!cur)return;document.getElementById('loginPage').classList.add('hidden');document.getElementById('dashboardPage').classList.remove('hidden');renderDashboard();renderDepositPlans();renderTransactions();}
function doSignup(){const u=document.getElementById('authUser').value.trim();const p=document.getElementById('authPass').value;if(!u||!p){showToast('Enter username & password');return;}const users=getUsers();if(users.find(x=>x.user===u)){showToast('Username exists');return;}users.push({user:u,pass:p,balance:0,active:[],admin:false});setUsers(users);setCurrent({user:u,admin:false});showToast('Signup successful');afterLogin();}
function doLogin(){const u=document.getElementById('authUser').value.trim();const p=document.getElementById('authPass').value;if(!u||!p){showToast('Enter username & password');return;}if(u===ADMIN.user&&p===ADMIN.pass){setCurrent({user:ADMIN.user,admin:true});showToast('Admin logged in');afterLogin();return;}const users=getUsers();const found=users.find(x=>x.user===u&&x.pass===p);if(!found){showToast('Invalid credentials');return;}setCurrent({user:found.user,admin:false});showToast('Login successful');afterLogin();}
function doLogout(){localStorage.removeItem('verbose_current');document.getElementById('dashboardPage').classList.add('hidden');document.getElementById('loginPage').classList.remove('hidden');showToast('Logged out');}

// ---------- Navigation ----------
function navigate(view){const views=['depositView','withdrawView','txView','profileView'];views.forEach(v=>{let el=document.getElementById(v);if(el) el.classList.add('hidden');});if(view==='deposit') document.getElementById('depositView').classList.remove('hidden');if(view==='withdraw') document.getElementById('withdrawView').classList.remove('hidden');if(view==='transactions'){document.getElementById('txView').classList.remove('hidden');renderTransactions();}if(view==='profile'){document.getElementById('profileView').classList.remove('hidden');renderProfile();}}

// ---------- Dashboard ----------
function renderDashboard(){const grid=document.getElementById('dashboardIcons');grid.innerHTML='';dashboardIcons.forEach(d=>{const div=document.createElement('div');div.className='icon-box';div.innerHTML=`<i class="fa ${d.icon}"></i><p>${d.name}</p>`;div.onclick=()=>navigate(d.view);grid.appendChild(div);});}

// ---------- Deposit ----------
function renderDepositPlans(){const select=document.getElementById('depositPlan');select.innerHTML='';plans.forEach(p=>{const opt=document.createElement('option');opt.value=p.id;opt.text=`${p.name} - ${p.invest} PKR`;select.appendChild(opt);});updateDepositAmount();updateDepositNumber();}
function updateDepositAmount(){const planId=parseInt(document.getElementById('depositPlan').value);const plan=plans.find(p=>p.id===planId);if(plan) document.getElementById('depositAmount').value=plan.invest;}
function updateDepositNumber(){const method=document.getElementById('depositMethod').value;document.getElementById('depositNumber').value=depositNumbers[method];}
function copyText(txt){navigator.clipboard.writeText(txt).then(()=>showToast('Copied!'));}
function submitDeposit(){const planId=parseInt(document.getElementById('depositPlan').value);const amount=parseInt(document.getElementById('depositAmount').value);const method=document.getElementById('depositMethod').value;const tx=document.getElementById('depositTxId').value.trim();const proof=document.getElementById('depositProof').files[0];if(!tx||!proof){showToast('Fill TX ID & upload proof');return;}const cur=getCurrent();const users=getUsers();const user=users.find(u=>u.user===cur.user);if(!user.active) user.active=[];user.active.push({type:'deposit',amount:amount,plan:planId,method,tx,proof:proof.name,time:new Date().toLocaleString(),status:'Pending'});setUsers(users);showToast('Deposit submitted');document.getElementById('depositTxId').value='';document.getElementById('depositProof').value='';renderTransactions();}

// ---------- Withdraw ----------
function submitWithdraw(){const u=document.getElementById('withdrawUser').value.trim();const acc=document.getElementById('withdrawAccount').value.trim();const amt=parseInt(document.getElementById('withdrawAmount').value);const method=document.getElementById('withdrawMethod').value;if(!u||!acc||!amt||amt<=0){showToast('Fill all withdrawal details');return;}const cur=getCurrent();const users=getUsers();const user=users.find(x=>x.user===cur.user);if(user.balance<amt){showToast('Insufficient balance');return;}user.balance-=amt;if(!user.active) user.active=[];user.active.push({type:'withdraw',user:u,account:acc,amount:amt,method,time:new Date().toLocaleString(),status:'Pending'});setUsers(users);showToast('Withdraw request sent');document.getElementById('withdrawUser').value='';document.getElementById('withdrawAccount').value='';document.getElementById('withdrawAmount').value='';renderTransactions();}

// ---------- Transactions ----------
function renderTransactions(){const cur=getCurrent();const users=getUsers();const user=users.find(u=>u.user===cur.user);const tbody=document.getElementById('txTableBody');tbody.innerHTML='';if(user.active&&user.active.length>0){user.active.forEach(tx=>{let tr=document.createElement('tr');tr.innerHTML=`<td>${tx.type}</td><td>${tx.user||user.user}</td><td>${tx.account||'-'}</td><td>${tx.amount||'-'}</td><td>${tx.plan||'-'}</td><td>${tx.time}</td><td>${tx.status}</td>`;tbody.appendChild(tr);});}}

// ---------- Profile ----------
function renderProfile(){const cur=getCurrent();const users=getUsers();const user=users.find(u=>u.user===cur.user);document.getElementById('profileName').value=user.user;document.getElementById('profileStats').innerHTML=`Balance: ${user.balance||0} PKR | Active Tx: ${(user.active&&user.active.length)||0}`;}
function updateProfile(){const name=document.getElementById('profileName').value.trim();if(!name){showToast('Name cannot be empty');return;}const cur=getCurrent();const users=getUsers();const user=users.find(u=>u.user===cur.user);user.user=name;setUsers(users);setCurrent({user:name,admin:cur.admin});showToast('Profile updated');renderProfile();renderDashboard();}

// ---------- Init ----------
afterLogin();
</script>
</body>
</html>
