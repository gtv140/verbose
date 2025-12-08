<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE Earning System</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.2/css/all.min.css">
<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:'Orbitron',sans-serif;}
body,html{height:100%;background:#050012;color:#fff;overflow-x:hidden;}
.hidden{display:none;}
.bg-gradient{position:fixed;inset:0;z-index:-1;background:linear-gradient(270deg,#0f0530,#1b0a4a,#061022);background-size:800% 800%;animation:grad 20s ease infinite;filter:blur(28px) saturate(120%);}
@keyframes grad{0%{background-position:0% 50%}50%{background-position:100% 50%}100%{background-position:0% 50%}}
.app-header{display:flex;justify-content:space-between;align-items:center;padding:15px;background:#111;box-shadow:0 2px 12px rgba(0,255,240,0.3);}
.app-header h1{color:#0ff;font-size:22px;text-shadow:0 0 12px #0ff;}
.menu-btn{font-size:22px;color:#0ff;cursor:pointer;}
.icon-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(100px,1fr));gap:15px;padding:20px;}
.icon-box{background:#111;padding:20px;text-align:center;border-radius:15px;box-shadow:0 0 12px #0ff;transition:0.3s;cursor:pointer;}
.icon-box:hover{transform:scale(1.05);box-shadow:0 0 24px #0ff;}
.icon-box i{font-size:28px;margin-bottom:8px;color:#0ff;}
.icon-box p{font-size:14px;color:#cfe;}
.card{background:rgba(255,255,255,0.03);border:1px solid rgba(0,255,255,0.06);padding:18px;border-radius:14px;box-shadow:0 8px 30px rgba(0,0,0,0.6);backdrop-filter:blur(6px);margin:15px;transition:0.3s;}
.card:hover{transform:translateY(-4px);box-shadow:0 12px 36px rgba(0,255,240,0.15);}
input,select{width:100%;padding:10px;margin-top:8px;border-radius:8px;border:1px solid rgba(255,255,255,0.06);background:rgba(0,0,0,0.35);color:#fff;outline:none;transition:0.2s;}
input:focus,select:focus{border-color:#0ff;}
button.primary{background:#0ff;color:#000;padding:10px 12px;border-radius:10px;border:none;cursor:pointer;font-weight:700;margin-top:10px;transition:0.2s;}
button.primary:hover{background:#0ae;}
button.ghost{background:transparent;border:1px solid rgba(255,255,255,0.06);padding:10px;border-radius:8px;color:#fff;cursor:pointer;margin-left:8px;transition:0.2s;}
button.ghost:hover{background:rgba(0,255,240,0.1);}
.footer-menu{position:fixed;bottom:0;left:0;width:100%;background:#111;display:flex;justify-content:space-around;padding:10px 0;box-shadow:0 -2px 12px rgba(0,255,240,0.3);}
.footer-menu div{text-align:center;color:#fff;font-size:12px;cursor:pointer;}
.footer-menu i{display:block;font-size:22px;margin-bottom:4px;color:#0ff;}
table{width:100%;border-collapse:collapse;margin-top:10px;}
th,td{padding:8px;border-bottom:1px solid rgba(255,255,255,0.03);font-size:13px;color:#0ff;}
.notif{position:fixed;right:18px;bottom:18px;background:#0ff;color:#000;padding:12px 16px;border-radius:12px;font-weight:700;box-shadow:0 10px 30px rgba(0,255,240,0.2);z-index:99;}
.auth-wrap{display:flex;flex-direction:column;align-items:center;justify-content:center;padding:30px;border-radius:14px;box-shadow:0 8px 30px rgba(0,255,240,0.2);}
.auth-wrap h2{text-align:center;margin-bottom:15px;}
.muted{opacity:0.7;font-size:13px;}
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
<div class="app-header">
<h1>VERBOSE</h1>
<i class="fa fa-bars menu-btn"></i>
</div>

<div class="icon-grid" id="dashboardIcons"></div>

<div id="depositView" class="card hidden">
<h2>Deposit</h2>
<label>Username</label>
<input id="depositUser" readonly>
<label>Account Number</label>
<input id="depositAcc" readonly>
<label>Amount PKR</label>
<input id="depositAmount" readonly>
<label>Method</label>
<select id="depositMethod">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<button class="ghost" onclick="copyText(document.getElementById('depositAcc').value)">Copy Account</button>
<label>Transaction ID</label>
<input id="depositTxId" placeholder="TX ID">
<button class="primary" onclick="submitDeposit()">Submit Deposit</button>
</div>

<div id="withdrawView" class="card hidden">
<h2>Withdraw</h2>
<label>Username</label>
<input id="withdrawUser" readonly>
<label>Account Number</label>
<input id="withdrawAcc" readonly>
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
<thead><tr><th>Type</th><th>Username</th><th>Account</th><th>Amount</th><th>Time</th><th>Status</th></tr></thead>
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
<div onclick="navigate('deposit')"><i class="fas fa-money-bill-wave"></i>Deposit</div>
<div onclick="navigate('withdraw')"><i class="fas fa-credit-card"></i>Withdraw</div>
<div onclick="navigate('transactions')"><i class="fas fa-file-invoice"></i>Transactions</div>
<div onclick="navigate('profile')"><i class="fas fa-user"></i>Profile</div>
</div>

<div id="toastRoot"></div>

<script>
// ---------- Admin & Users ----------
const ADMIN={user:'AdminKhan',pass:'SuperSecret123',acc:'000123456789'};
const usersKey='verbose_users';
const curKey='verbose_current';
if(!localStorage.getItem(usersKey)) localStorage.setItem(usersKey,JSON.stringify([ADMIN]));

// ---------- Utility ----------
function getUsers(){return JSON.parse(localStorage.getItem(usersKey)||'[]');}
function setUsers(u){localStorage.setItem(usersKey,JSON.stringify(u));}
function getCurrent(){return JSON.parse(localStorage.getItem(curKey)||'null');}
function setCurrent(u){localStorage.setItem(curKey,JSON.stringify(u));}
function showToast(txt){const n=document.createElement('div');n.className='notif';n.innerText=txt;document.body.appendChild(n);setTimeout(()=>n.remove(),2000);}
function copyText(txt){navigator.clipboard.writeText(txt).then(()=>showToast('Copied!'));}

// ---------- Auth ----------
function afterLogin(){
 const cur=getCurrent();
 if(!cur)return;
 document.getElementById('loginPage').classList.add('hidden');
 document.getElementById('dashboardPage').classList.remove('hidden');
 renderDashboard();
 updateDepositWithdrawFields();
 renderTransactions();
 renderProfile();
}
function doSignup(){
 const u=document.getElementById('authUser').value.trim();
 const p=document.getElementById('authPass').value.trim();
 if(!u||!p){showToast('Enter username & password');return;}
 const users=getUsers();
 if(users.find(x=>x.user===u)){showToast('Username exists');return;}
 users.push({user:u,pass:p,acc:'0'+Math.floor(Math.random()*999999999),balance:0,active:[],profit:0});
 setUsers(users);
 setCurrent(users.find(x=>x.user===u));
 showToast('Signup successful');afterLogin();
}
function doLogin(){
 const u=document.getElementById('authUser').value.trim();
 const p=document.getElementById('authPass').value.trim();
 const users=getUsers();
 const found=users.find(x=>x.user===u && x.pass===p);
 if(!found){showToast('Invalid credentials');return;}
 setCurrent(found);
 showToast('Login successful');afterLogin();
}
function doLogout(){
 localStorage.removeItem(curKey);
 document.getElementById('dashboardPage').classList.add('hidden');
 document.getElementById('loginPage').classList.remove('hidden');
 showToast('Logged out');
}

// ---------- Dashboard ----------
const dashboardIcons=[
{icon:'fas fa-wallet',name:'Wallet',view:'deposit'},
{icon:'fas fa-money-bill-wave',name:'Deposit',view:'deposit'},
{icon:'fas fa-credit-card',name:'Withdraw',view:'withdraw'},
{icon:'fas fa-file-invoice',name:'Transactions',view:'transactions'},
{icon:'fas fa-user',name:'Profile',view:'profile'}
];
function renderDashboard(){
 const grid=document.getElementById('dashboardIcons');grid.innerHTML='';
 dashboardIcons.forEach(d=>{
   const div=document.createElement('div');div.className='icon-box';
   div.innerHTML=`<i class="${d.icon}"></i><p>${d.name}</p>`;
   div.onclick=()=>navigate(d.view);grid.appendChild(div);
 });
}

// ---------- Navigation ----------
function navigate(view){
['depositView','withdrawView','txView','profileView'].forEach(v=>document.getElementById(v).classList.add('hidden'));
if(view==='deposit') document.getElementById('depositView').classList.remove('hidden');
if(view==='withdraw') document.getElementById('withdrawView').classList.remove('hidden');
if(view==='transactions') document.getElementById('txView').classList.remove('hidden');
if(view==='profile') document.getElementById('profileView').classList.remove('hidden');
}

// ---------- Deposit / Withdraw ----------
function updateDepositWithdrawFields(){
 const cur=getCurrent();
 if(!cur) return;
 document.getElementById('depositUser').value=cur.user;
 document.getElementById('depositAcc').value=cur.acc;
 document.getElementById('withdrawUser').value=cur.user;
 document.getElementById('withdrawAcc').value=cur.acc;
}
function submitDeposit(){
 const amt=100; // Fixed demo
 const cur=getCurrent(); const users=getUsers();
 const user=users.find(u=>u.user===cur.user);
 if(!user.active) user.active=[];
 user.active.push({type:'deposit',user:cur.user,acc:cur.acc,amount:amt,time:new Date().toLocaleString(),status:'Pending'});
 setUsers(users); showToast('Deposit submitted'); renderTransactions();
}
function submitWithdraw(){
 const amt=parseInt(document.getElementById('withdrawAmount').value);
 if(!amt || amt<=0){showToast('Enter valid amount'); return;}
 const cur=getCurrent(); const users=getUsers();
 const user=users.find(u=>u.user===cur.user);
 if(user.balance<amt){showToast('Insufficient balance'); return;}
 user.balance-=amt;
 if(!user.active) user.active=[];
 user.active.push({type:'withdraw',user:cur.user,acc:cur.acc,amount:amt,time:new Date().toLocaleString(),status:'Pending'});
 setUsers(users); showToast('Withdraw requested'); renderTransactions();
}
function renderTransactions(){
 const cur=getCurrent(); const users=getUsers();
 const user=users.find(u=>u.user===cur.user);
 const tbody=document.getElementById('txTableBody'); tbody.innerHTML='';
 if(user.active && user.active.length>0){
   user.active.forEach(tx=>{
     let tr=document.createElement('tr');
     tr.innerHTML=`<td>${tx.type}</td><td>${tx.user}</td><td>${tx.acc}</td><td>${tx.amount}</td><td>${tx.time}</td><td>${tx.status}</td>`;
     tbody.appendChild(tr);
   });
 }
}

// ---------- Profile ----------
function renderProfile(){
 const cur=getCurrent(); const users=getUsers();
 const user=users.find(u=>u.user===cur.user);
 document.getElementById('profileName').value=user.user;
 document.getElementById('profileStats').innerText=`Balance: ${user.balance || 0} | Active Tx: ${(user.active && user.active.length)||0}`;
}
function updateProfile(){
 const name=document.getElementById('profileName').value.trim();
 if(!name){showToast('Name required');return;}
 const cur=getCurrent(); const users=getUsers();
 const user=users.find(u=>u.user===cur.user);
 user.user=name; setUsers(users); setCurrent(user); showToast('Profile updated'); renderProfile(); updateDepositWithdrawFields();
}

// ---------- Init ----------
afterLogin();
</script>
</body>
</html>
