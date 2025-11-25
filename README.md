<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Premium Earning App Panel</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<style>
  /* Global Styles */
  body{margin:0;padding:0;font-family:Arial,sans-serif;background:#0b0b0f;color:#fff}
  .hidden{display:none}
  button{cursor:pointer;border:none;border-radius:8px;outline:none}
  input,select{border-radius:8px;border:none;padding:10px;width:100%;margin-top:8px;background:#111;color:#fff}
  /* Login Screen */
  #loginScreen{display:flex;align-items:center;justify-content:center;height:100vh;background:linear-gradient(135deg,#0d0d1a,#050012);flex-direction:column}
  #loginScreen h2{margin:0 0 15px 0;color:#0af;text-shadow:0 0 10px #0af}
  #loginScreen .auth-box{background:#111;padding:20px;border-radius:15px;box-shadow:0 0 20px #000;width:320px;max-width:90%;}
  #loginScreen .auth-box button.primary{background:#0af;color:#000;width:48%;font-weight:700}
  #loginScreen .auth-box button.ghost{background:transparent;border:1px solid #0af;color:#0af;width:48%}
  /* App Header */
  .app-header{display:flex;justify-content:space-between;align-items:center;padding:15px;background:#111;box-shadow:0 2px 10px rgba(0,0,0,0.5)}
  .app-header h1{margin:0;font-size:20px;color:#0af}
  .menu-btn{font-size:22px;color:#0af}
  /* Dashboard Icons Grid */
  .icon-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(100px,1fr));gap:12px;padding:15px}
  .icon-box{background:#1a1a1a;padding:20px;text-align:center;border-radius:15px;transition:0.3s;box-shadow:0 0 10px #000}
  .icon-box:hover{transform:scale(1.05);background:#222}
  .icon-box i{font-size:28px;margin-bottom:8px;color:#0af}
  .icon-box p{margin:0;font-size:12px;color:#fff}
  /* Bottom Navigation */
  .footer-menu{position:fixed;bottom:0;left:0;width:100%;background:#111;display:flex;justify-content:space-around;padding:8px 0;box-shadow:0 -2px 10px rgba(0,0,0,0.5)}
  .footer-menu div{text-align:center;color:#fff;font-size:10px}
  .footer-menu i{display:block;font-size:18px;margin-bottom:4px;color:#0af}
  /* Cards */
  .card{background:rgba(15,15,20,0.8);padding:12px;border-radius:12px;margin-bottom:12px;box-shadow:0 4px 12px rgba(0,0,0,0.5)}
  /* Table */
  table{width:100%;border-collapse:collapse;color:#fff;font-size:12px}
  th,td{padding:6px;border-bottom:1px solid rgba(255,255,255,0.1)}
  th{color:#0af}
  /* Toast Notification */
  .toast{position:fixed;bottom:18px;right:18px;background:#0af;color:#000;padding:12px 16px;border-radius:12px;font-weight:700;z-index:999;box-shadow:0 8px 20px rgba(0,255,240,0.2)}
</style>
</head>
<body>

<!-- Login / Signup Screen -->
<div id="loginScreen">
  <h2>Premium Panel Login</h2>
  <div class="auth-box">
    <input type="text" id="authUser" placeholder="Username">
    <input type="password" id="authPass" placeholder="Password">
    <div style="display:flex;justify-content:space-between;margin-top:10px">
      <button class="primary" onclick="doLogin()">Login</button>
      <button class="ghost" onclick="doSignup()">Signup</button>
    </div>
    <p style="font-size:10px;margin-top:8px;color:#aaa">Signup saved locally. Admin: <strong>AdminKhan</strong> / <strong>SuperSecret123</strong></p>
  </div>
</div>

<!-- App Main Screen -->
<div id="appScreen" class="hidden">
  <div class="app-header">
    <h1>Premium Panel</h1>
    <i class="fa fa-bars menu-btn"></i>
    <button onclick="doLogout()" style="background:#ff4d4d;color:#000;padding:6px 12px;border-radius:10px;">Logout</button>
  </div>

  <div class="icon-grid">
    <div class="icon-box"><i class="fa fa-wallet"></i><p>Wallet</p></div>
    <div class="icon-box"><i class="fa fa-gift"></i><p>Rewards</p></div>
    <div class="icon-box"><i class="fa fa-chart-line"></i><p>Stats</p></div>
    <div class="icon-box"><i class="fa fa-cog"></i><p>Settings</p></div>
    <div class="icon-box"><i class="fa fa-user"></i><p>Profile</p></div>
    <div class="icon-box"><i class="fa fa-bolt"></i><p>Boost</p></div>
    <div class="icon-box"><i class="fa fa-hand-holding-dollar"></i><p>Deposit</p></div>
    <div class="icon-box"><i class="fa fa-credit-card"></i><p>Withdraw</p></div>
    <div class="icon-box"><i class="fa fa-users"></i><p>Referrals</p></div>
    <div class="icon-box"><i class="fa fa-receipt"></i><p>Transactions</p></div>
    <div class="icon-box"><i class="fa fa-info-circle"></i><p>About</p></div>
  </div>

  <div class="footer-menu">
    <div><i class="fa fa-home"></i>Home</div>
    <div><i class="fa fa-layer-group"></i>Menu</div>
    <div><i class="fa fa-user"></i>Account</div>
  </div>
</div>

<div id="toastRoot"></div>

<script>
  /* ---------- App Data ---------- */
  const ADMIN={user:'AdminKhan',pass:'SuperSecret123'};
  const depositNumbers={jazzcash:'03705519562',easypaisa:'03379827882'};
  const plans=[ 
    {id:1,name:'Plan 1',days:25,invest:250,totalProfit:1200,offer:true},
    {id:2,name:'Plan 2',days:28,invest:500,totalProfit:2500,offer:true},
    {id:3,name:'Plan 3',days:30,invest:750,totalProfit:3750,offer:true},
    {id:4,name:'Plan 4',days:33,invest:1000,totalProfit:4000,offer:true},
    {id:5,name:'Plan 5',days:35,invest:1500,totalProfit:7700,offer:true},
    {id:6,name:'Plan 6',days:38,invest:2000,totalProfit:5700,offer:false},
    {id:7,name:'Plan 7',days:40,invest:2500,totalProfit:7200,offer:false},
    {id:8,name:'Plan 8',days:43,invest:3000,totalProfit:9000,offer:false},
    {id:9,name:'Plan 9',days:45,invest:3500,totalProfit:10800,offer:false},
    {id:10,name:'Plan 10',days:48,invest:4000,totalProfit:13200,offer:false},
    {id:11,name:'Plan 11',days:50,invest:5000,totalProfit:15000,offer:false},
    {id:12,name:'Plan 12',days:53,invest:6000,totalProfit:18000,offer:false},
    {id:13,name:'Plan 13',days:55,invest:7000,totalProfit:21000,offer:false},
    {id:14,name:'Plan 14',days:58,invest:8000,totalProfit:24000,offer:false},
    {id:15,name:'Plan 15',days:60,invest:9000,totalProfit:27000,offer:false},
    {id:16,name:'Plan 16',days:63,invest:10000,totalProfit:30000,offer:false},
    {id:17,name:'Plan 17',days:65,invest:12000,totalProfit:33000,offer:false},
    {id:18,name:'Plan 18',days:68,invest:14000,totalProfit:36000,offer:false},
    {id:19,name:'Plan 19',days:70,invest:16000,totalProfit:39000,offer:false},
    {id:20,name:'Plan 20',days:72,invest:18000,totalProfit:42000,offer:false},
    {id:21,name:'Plan 21',days:75,invest:20000,totalProfit:45000,offer:false},
    {id:22,name:'Plan 22',days:77,invest:25000,totalProfit:60000,offer:false},
    {id:23,name:'Plan 23',days:79,invest:30000,totalProfit:80000,offer:false},
    {id:24,name:'Plan 24',days:80,invest:40000,totalProfit:100000,offer:false},
    {id:25,name:'Plan 25',days:85,invest:60000,totalProfit:150000,offer:false}
  ];
  for(const p of plans)p.dailyProfit=Math.round(p.totalProfit/p.days);
</script><script>
/* ---------- Storage Helpers ---------- */
function getUsers(){return JSON.parse(localStorage.getItem('verbose_users')||'[]');}
function setUsers(u){localStorage.setItem('verbose_users',JSON.stringify(u));}
function getTx(){return JSON.parse(localStorage.getItem('verbose_tx')||'[]');}
function setTx(t){localStorage.setItem('verbose_tx',JSON.stringify(t));}
function getCurrent(){return JSON.parse(localStorage.getItem('verbose_current')||'null');}
function setCurrent(u){localStorage.setItem('verbose_current',JSON.stringify(u));}

/* ---------- Init Admin ---------- */
(function initUsers(){
  let u=getUsers();
  if(!u.find(x=>x.user===ADMIN.user)) {
    u.push({user:ADMIN.user,pass:ADMIN.pass,balance:0,active:[],profit:0,admin:true});
    setUsers(u);
  }
})();

/* ---------- Toast ---------- */
function showToast(txt){
  const n=document.createElement('div');n.className='toast';n.innerText=txt;
  document.body.appendChild(n);
  setTimeout(()=>n.remove(),2500);
}

/* ---------- Auth: Login / Signup / Logout ---------- */
function afterLogin(){
  const cur=getCurrent();
  if(!cur)return;
  document.getElementById('loginScreen').classList.add('hidden');
  document.getElementById('appScreen').classList.remove('hidden');
  if(!cur.admin){
    // hide admin icons if any (currently no admin icon)
  }
}

function doSignup(){
  const u=document.getElementById('authUser').value.trim();
  const p=document.getElementById('authPass').value;
  if(!u||!p){showToast('Enter username & password');return;}
  const users=getUsers();
  if(users.find(x=>x.user===u)){showToast('Username exists');return;}
  users.push({user:u,pass:p,balance:0,active:[],profit:0,referralCode:u+'_ref',referredBy:null});
  setUsers(users);
  setCurrent({user:u,admin:false});
  showToast('Signup successful');
  afterLogin();
}

function doLogin(){
  const u=document.getElementById('authUser').value.trim();
  const p=document.getElementById('authPass').value;
  if(!u||!p){showToast('Enter username & password');return;}
  if(u===ADMIN.user&&p===ADMIN.pass){setCurrent({user:u,admin:true});showToast('Admin logged in');afterLogin();return;}
  const users=getUsers();
  const found=users.find(x=>x.user===u&&x.pass===p);
  if(!found){showToast('Invalid credentials');return;}
  setCurrent({user:found.user,admin:false});
  showToast('Login successful');
  afterLogin();
}

function doLogout(){
  localStorage.removeItem('verbose_current');
  document.getElementById('appScreen').classList.add('hidden');
  document.getElementById('loginScreen').classList.remove('hidden');
}

/* ---------- Copy Number Helper ---------- */
function copyText(val){
  navigator.clipboard.writeText(val).then(()=>showToast('Copied'));
}

/* ---------- Deposit / Withdraw Simulation ---------- */
function submitDeposit(){
  const cur=getCurrent();
  if(!cur){showToast('Login first');return;}
  const amount=prompt('Enter deposit amount PKR');
  if(!amount||isNaN(amount)||amount<=0){showToast('Invalid amount');return;}
  let tx=getTx();
  tx.push({user:cur.user,type:'Deposit',amount:parseFloat(amount),time:Date.now(),status:'Pending'});
  setTx(tx);
  showToast('Deposit submitted (simulated)');
}

function submitWithdraw(){
  const cur=getCurrent();
  if(!cur){showToast('Login first');return;}
  const amount=prompt('Enter withdrawal amount PKR');
  if(!amount||isNaN(amount)||amount<=0){showToast('Invalid amount');return;}
  let tx=getTx();
  tx.push({user:cur.user,type:'Withdrawal',amount:parseFloat(amount),time:Date.now(),status:'Pending'});
  setTx(tx);
  showToast('Withdrawal requested (simulated)');
}

/* ---------- Render Plans ---------- */
function renderPlans(){
  const grid=document.createElement('div');grid.className='card';
  grid.innerHTML='<h3>Available Plans</h3>';
  plans.forEach(p=>{
    const div=document.createElement('div');div.className='card';
    div.innerHTML=`<strong>${p.name}</strong> <br>Invest: ${p.invest} PKR <br>Days: ${p.days} <br>Profit: ${p.totalProfit} PKR`;
    grid.appendChild(div);
  });
  document.getElementById('appScreen').appendChild(grid);
}

/* ---------- Referrals ---------- */
function generateReferral(){
  const cur=getCurrent();
  if(!cur||cur.admin)return '';
  const users=getUsers();
  const me=users.find(u=>u.user===cur.user);
  return me?.referralCode||'';
}

function applyReferral(code){
  const cur=getCurrent();
  if(!cur||cur.admin){showToast('Invalid');return;}
  const users=getUsers();
  const me=users.find(u=>u.user===cur.user);
  const refUser=users.find(u=>u.referralCode===code);
  if(!refUser){showToast('Invalid referral');return;}
  if(me.referredBy){showToast('Already applied');return;}
  me.referredBy=code;
  refUser.balance+=50; // bonus PKR
  setUsers(users);
  showToast('Referral applied, referrer gets 50 PKR bonus');
}

/* ---------- Transactions Display ---------- */
function renderTx(){
  const cur=getCurrent();
  if(!cur)return;
  const tx=getTx().filter(t=>t.user===cur.user);
  const table=document.createElement('table');
  table.innerHTML='<thead><tr><th>Type</th><th>Amount</th><th>Status</th><th>Time</th></tr></thead>';
  const tbody=document.createElement('tbody');
  tx.forEach(t=>{
    const tr=document.createElement('tr');
    tr.innerHTML=`<td>${t.type}</td><td>${t.amount}</td><td>${t.status}</td><td>${new Date(t.time).toLocaleString()}</td>`;
    tbody.appendChild(tr);
  });
  table.appendChild(tbody);
  document.getElementById('appScreen').appendChild(table);
}

/* ---------- Init ---------- */
afterLogin();
renderPlans();
renderTx();
</script><!-- ---------- Part 3: About & Footer ---------- -->
<div id="aboutScreen" class="card hidden" style="margin:20px">
    <h2>About VERBOSE</h2>
    <p style="color:#bfe;line-height:1.5">
        VERBOSE ek premium earning platform hai jo modern digital finance ko fully mobile app style me present karta hai.
        <br><br>
        <strong>Owner:</strong> John Wilson<br>
        <strong>Launch Date:</strong> 17 Nov 2025<br>
        <strong>Features:</strong> Login/Signup, Wallet, Deposit, Withdrawal, Referral Bonus, Transactions, Active Plans
        <br><br>
        Enjoy the neon-inspired interface and easy navigation. Designed for mobile and desktop users!
    </p>
</div>

<footer style="position:fixed;bottom:0;width:100%;background:#111;color:#0af;text-align:center;padding:10px 0;font-size:12px;box-shadow:0 -2px 10px rgba(0,0,0,0.5)">
    Made with ✨ neon style by VERBOSE
</footer>

<style>
/* Toast */
.toast {
    position:fixed;
    bottom:60px;
    left:50%;
    transform:translateX(-50%);
    background:#0af;
    color:#000;
    padding:12px 20px;
    border-radius:12px;
    font-weight:700;
    box-shadow:0 10px 30px rgba(0,255,255,0.12);
    z-index:9999;
}

/* Responsive App-Style */
@media(max-width:480px){
    .icon-grid{
        grid-template-columns:repeat(2,1fr);
        gap:12px;
        padding:15px;
    }
    .icon-box{
        padding:15px;
        border-radius:12px;
    }
    .footer-menu i{
        font-size:18px;
    }
    .footer-menu div{
        font-size:10px;
    }
    .app-header h1{
        font-size:18px;
    }
}
</style>

<script>
/* ---------- Footer Menu Navigation ---------- */
document.querySelectorAll('.footer-menu div').forEach(item=>{
    item.addEventListener('click',()=>{
        const txt=item.innerText.toLowerCase();
        document.getElementById('appScreen').innerHTML=''; // clear main
        if(txt==='home'){renderPlans();renderTx();}
        if(txt==='menu'){showToast('Menu section - coming soon');}
        if(txt==='account'){document.getElementById('appScreen').appendChild(document.getElementById('aboutScreen'));aboutScreen.classList.remove('hidden');}
    });
});
</script>
