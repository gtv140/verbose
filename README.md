<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE Earning System</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:'Orbitron',sans-serif;}
body,html{height:100%;background:#0b0b1c;color:#fff;overflow-x:hidden;transition:all 0.3s;}
.hidden{display:none;}
.bg-gradient{position:fixed;inset:0;z-index:-2;background:linear-gradient(270deg,#1a1a40,#0c0c25,#1b1b4d);background-size:800% 800%;animation:grad 25s ease infinite;filter:blur(25px);}
@keyframes grad{0%{background-position:0% 50%}50%{background-position:100% 50%}100%{background-position:0% 50%}}
.card{background:rgba(255,255,255,0.02);border:1px solid rgba(0,255,240,0.06);padding:20px;border-radius:15px;margin:15px;backdrop-filter:blur(6px);}
input,select{width:100%;padding:10px;margin-top:8px;border-radius:8px;border:1px solid rgba(255,255,255,0.06);background:rgba(0,0,0,0.35);color:#fff;outline:none;}
input:focus,select:focus{border-color:#0ff;}
button.primary{background:#0ff;color:#000;padding:10px 12px;border-radius:10px;border:none;cursor:pointer;font-weight:700;margin-top:10px;}
button.ghost{background:transparent;border:1px solid rgba(255,255,255,0.06);padding:10px;border-radius:8px;color:#fff;cursor:pointer;margin-left:8px;}
.icon-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(100px,1fr));gap:15px;padding:20px;}
.icon-box{background:#111;padding:20px;text-align:center;border-radius:15px;cursor:pointer;transition:0.3s;}
.icon-box:hover{transform:scale(1.05);box-shadow:0 0 20px #0ff;}
.icon-box i{font-size:28px;margin-bottom:8px;color:#0ff;}
.icon-box p{font-size:14px;color:#cfe;}
table{width:100%;border-collapse:collapse;margin-top:10px;}
th,td{padding:8px;border-bottom:1px solid rgba(255,255,255,0.03);font-size:13px;color:#0ff;}
.notif{position:fixed;right:18px;bottom:18px;background:#0ff;color:#000;padding:12px 16px;border-radius:12px;font-weight:700;z-index:99;}
</style>
</head>
<body>
<div class="bg-gradient"></div>

<!-- Login Page -->
<div id="loginPage" class="card">
<h2>Login / Signup</h2>
<input id="authUser" placeholder="Username">
<input id="authPass" placeholder="Password" type="password">
<div style="display:flex;margin-top:10px;">
<button class="primary" onclick="doLogin()">Login</button>
<button class="ghost" onclick="doSignup()">Signup</button>
</div>
</div>

<!-- Dashboard -->
<div id="dashboardPage" class="hidden">
<h1 style="text-align:center;margin-top:10px;">VERBOSE Dashboard</h1>

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
<label>Amount PKR</label>
<input id="withdrawAmount" type="number">
<label>Username</label>
<input id="withdrawUser" readonly>
<label>Account Number</label>
<input id="withdrawAccount" readonly>
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
<button class="primary" onclick="updateProfile()">Update Profile</button>
</div>

<div id="toastRoot"></div>

<script>
// ---------- Admin, Users ----------
const ADMIN={user:'AdminKhan',pass:'SuperSecret123'};
const depositNumbers={jazzcash:'03705519562',easypaisa:'03379827882'};
let users=JSON.parse(localStorage.getItem('verbose_users')||'[]');
if(!users.find(u=>u.user===ADMIN.user)){users.push({user:ADMIN.user,pass:ADMIN.pass,balance:0,active:[],profit:0,admin:true});localStorage.setItem('verbose_users',JSON.stringify(users));}

// ---------- Helpers ----------
function getUsers(){return JSON.parse(localStorage.getItem('verbose_users')||'[]');}
function setUsers(u){localStorage.setItem('verbose_users',JSON.stringify(u));}
function getCurrent(){return JSON.parse(localStorage.getItem('verbose_current')||'null');}
function setCurrent(u){localStorage.setItem('verbose_current',JSON.stringify(u));}
function showToast(txt){const n=document.createElement('div');n.className='notif';n.innerText=txt;document.body.appendChild(n);setTimeout(()=>n.remove(),2000);}

// ---------- Auth ----------
function afterLogin(){
    const cur=getCurrent();
    if(!cur) return;
    document.getElementById('loginPage').classList.add('hidden');
    document.getElementById('dashboardPage').classList.remove('hidden');
    renderDashboard();
    renderDepositPlans();
    renderTransactions();
    renderProfile();
    document.getElementById('withdrawUser').value=cur.user;
    document.getElementById('withdrawAccount').value='03312345678';
}
function doSignup(){
    const u=document.getElementById('authUser').value.trim();
    const p=document.getElementById('authPass').value;
    if(!u||!p){showToast('Enter username & password');return;}
    const users=getUsers();
    if(users.find(x=>x.user===u)){showToast('Username exists');return;}
    users.push({user:u,pass:p,balance:0,active:[],profit:0,admin:false});
    setUsers(users);
    setCurrent({user:u,admin:false});
    showToast('Signup successful');afterLogin();
}
function doLogin(){
    const u=document.getElementById('authUser').value.trim();
    const p=document.getElementById('authPass').value;
    const users=getUsers();
    const found=users.find(x=>x.user===u&&x.pass===p);
    if(!found){showToast('Invalid credentials');return;}
    setCurrent({user:found.user,admin:false});
    showToast('Login successful');afterLogin();
}
function doLogout(){
    localStorage.removeItem('verbose_current');
    document.getElementById('dashboardPage').classList.add('hidden');
    document.getElementById('loginPage').classList.remove('hidden');
    showToast('Logged out');
}

// ---------- Dashboard ----------
const dashboardIcons=[
{icon:'fa-wallet',name:'Wallet',view:'wallet'},
{icon:'fa-briefcase',name:'Plans',view:'plans'},
{icon:'fa-money-bill-wave',name:'Deposit',view:'deposit'},
{icon:'fa-credit-card',name:'Withdraw',view:'withdraw'},
{icon:'fa-file-invoice',name:'transactions'},
{icon:'fa-user',name:'Profile',view:'profile'},
{icon:'fa-star',name:'VIP',view:'vip'},
{icon:'fa-gift',name:'Rewards',view:'rewards'},
{icon:'fa-chart-line',name:'Stats',view:'stats'},
{icon:'fa-headset',name:'Support',view:'support'},
{icon:'fa-bell',name:'Notifications',view:'notifications'},
{icon:'fa-envelope',name:'Messages',view:'messages'},
{icon:'fa-shield-alt',name:'Security',view:'security'},
{icon:'fa-cogs',name:'Tools',view:'tools'},
{icon:'fa-heart',name:'Favorites',view:'favorites'},
{icon:'fa-globe',name:'Global',view:'global'},
{icon:'fa-calendar',name:'Events',view:'events'},
{icon:'fa-user-shield',name:'Account',view:'account'},
{icon:'fa-bolt',name:'Bonus',view:'bonus'}
];
function navigate(view){
    const views=['depositView','withdrawView','txView','profileView'];
    views.forEach(v=>document.getElementById(v).classList.add('hidden'));
    if(view==='deposit') document.getElementById('depositView').classList.remove('hidden');
    if(view==='withdraw') document.getElementById('withdrawView').classList.remove('hidden');
    if(view==='transactions') {document.getElementById('txView').classList.remove('hidden'); renderTransactions();}
    if(view==='profile') document.getElementById('profileView').classList.remove('hidden');
}
function renderDashboard(){
    const grid=document.getElementById('dashboardIcons'); grid.innerHTML='';
    dashboardIcons.forEach(d=>{
        const div=document.createElement('div'); div.className='icon-box';
        div.innerHTML=`<i class="fa ${d.icon}"></i><p>${d.name}</p>`;
        div.onclick=()=>navigate(d.view);
        grid.appendChild(div);
    });
}

// ---------- Deposit ----------
const plans=[]; for(let i=1;i<=20;i++){plans.push({id:i,name:`Plan ${i}`,invest:100*i});}
function renderDepositPlans(){
    const select=document.getElementById('depositPlan'); select.innerHTML='';
    plans.forEach(p=>{ const opt=document.createElement('option'); opt.value=p.id; opt.text=`${p.name} - ${p.invest} PKR`; select.appendChild(opt);});
    updateDepositAmount();updateDepositNumber();
}
function updateDepositAmount(){ const planId=parseInt(document.getElementById('depositPlan').value); const plan=plans.find(p=>p.id===planId); if(plan) document.getElementById('depositAmount').value=plan.invest;}
function updateDepositNumber(){ const method=document.getElementById('depositMethod').value; document.getElementById('depositNumber').value=depositNumbers[method];}
function copyText(txt){navigator.clipboard.writeText(txt).then(()=>showToast('Copied!'));}
function submitDeposit(){
    const amount=parseInt(document.getElementById('depositAmount').value);
    const planId=parseInt(document.getElementById('depositPlan').value);
    const method=document.getElementById('depositMethod').value;
    const tx=document.getElementById('depositTxId').value.trim();
    const proof=document.getElementById('depositProof').files[0];
    if(!tx || !proof){showToast('Fill TX ID and upload proof');return;}
    const cur=getCurrent(); const users=getUsers();
    const user=users.find(u=>u.user===cur.user);
    if(!user.active) user.active=[];
    user.active.push({type:'deposit',plan:planId,amount:amount,method,tx,time:new Date().toLocaleString(),status:'Pending'});
    setUsers(users); showToast('Deposit submitted');
    document.getElementById('depositTxId').value=''; document.getElementById('depositProof').value='';
    renderTransactions();
}

// ---------- Withdraw ----------
function submitWithdraw(){
    const amt=parseInt(document.getElementById('withdrawAmount').value);
    const cur=getCurrent(); const users=getUsers();
    const user=users.find(u=>u.user===cur.user);
    if(!amt || amt<=0){showToast('Enter valid amount'); return;}
    if(user.balance<amt){showToast('Insufficient balance'); return;}
    user.balance-=amt;
    if(!user.active) user.active=[];
    user.active.push({type:'withdraw',username:cur.user,account:'03312345678',amount:amt,time:new Date().toLocaleString(),status:'Pending'});
    setUsers(users); showToast('Withdraw request sent');
    document.getElementById('withdrawAmount').value='';
    renderTransactions();
}

// ---------- Transactions ----------
function renderTransactions(){
    const cur=getCurrent(); const users=getUsers(); const user=users.find(u=>u.user===cur.user);
    const tbody=document.getElementById('txTableBody'); tbody.innerHTML='';
    if(user.active && user.active.length>0){
        user.active.forEach(tx=>{
            let tr=document.createElement('tr');
            tr.innerHTML=`<td>${tx.type}</td><td>${tx.username||tx.user}</td><td>${tx.account||'-'}</td><td>${tx.amount||'-'}</td><td>${tx.time}</td><td>${tx.status}</td>`;
            tbody.appendChild(tr);
        });
    }
}

// ---------- Profile ----------
function renderProfile(){const cur=getCurrent();document.getElementById('profileName').value=cur.user;}
function updateProfile(){const name=document.getElementById('profileName').value.trim();if(!name){showToast('Name cannot be empty');return;}const cur=getCurrent();const users=getUsers();const user=users.find(u=>u.user===cur.user);user.user=name;setUsers(users);setCurrent({user:name,admin:cur.admin});showToast('Profile updated');renderProfile();}

// ---------- Init ----------
window.onload = afterLogin;
</script>
</body>
</html>
