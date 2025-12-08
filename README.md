<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:'Orbitron',sans-serif;}
body,html{height:100%;background:#01021a;color:#fff;overflow-x:hidden;transition:all 0.3s;}
.hidden{display:none;}
.bg-gradient{position:fixed;inset:0;z-index:-2;background:linear-gradient(270deg,#01031f,#00102b,#020518);background-size:800% 800%;animation:grad 25s ease infinite;filter:blur(10px) saturate(110%);}
@keyframes grad{0%{background-position:0% 50%}50%{background-position:100% 50%}100%{background-position:0% 50%}}
canvas{position:fixed;top:0;left:0;z-index:-1;}
.auth-wrap{display:flex;flex-direction:column;align-items:center;justify-content:center;height:100vh;}
.auth-wrap input{width:250px;padding:12px;margin:8px 0;border-radius:8px;border:1px solid rgba(255,255,255,0.2);background:rgba(0,0,0,0.3);color:#fff;outline:none;}
.auth-wrap button{width:120px;padding:10px;margin:10px;border:none;border-radius:8px;cursor:pointer;font-weight:700;transition:0.2s;}
.auth-wrap .primary{background:#0ff;color:#000;}
.auth-wrap .primary:hover{background:#0ae;}
.auth-wrap .ghost{background:transparent;color:#0ff;border:1px solid #0ff;}
.auth-wrap .ghost:hover{background:rgba(0,255,240,0.1);}
.dashboard-header{padding:20px;text-align:center;}
.dashboard-header h1{color:#0ff;font-size:28px;text-shadow:0 0 12px #0ff;margin-bottom:8px;}
.dashboard-header p{color:#cfe;font-size:16px;}
.icon-box{background:#111;padding:15px;text-align:center;border-radius:12px;box-shadow:0 0 12px #0ff;cursor:pointer;transition:0.2s;}
.icon-box:hover{transform:scale(1.05);box-shadow:0 0 24px #0ff;}
.icon-box i{font-size:28px;margin-bottom:6px;color:#0ff;}
.icon-box p{font-size:14px;color:#cfe;}
.plan-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(160px,1fr));gap:12px;margin-top:12px;}
.card{background:rgba(255,255,255,0.03);border-radius:14px;padding:12px;margin:10px;}
.footer-menu{position:fixed;bottom:0;left:0;width:100%;background:#111;display:flex;justify-content:space-around;padding:10px 0;box-shadow:0 -2px 12px rgba(0,255,240,0.3);}
.footer-menu div{text-align:center;color:#fff;font-size:12px;cursor:pointer;}
.footer-menu i{display:block;font-size:22px;margin-bottom:4px;color:#0ff;}
.notif{position:fixed;right:18px;bottom:18px;background:#0ff;color:#000;padding:12px 16px;border-radius:12px;font-weight:700;box-shadow:0 10px 30px rgba(0,255,240,0.2);z-index:99;}
input,select{width:100%;padding:10px;margin-top:8px;border-radius:8px;border:1px solid rgba(255,255,255,0.06);background:rgba(0,0,0,0.35);color:#fff;outline:none;transition:0.2s;}
input:focus,select:focus{border-color:#0ff;}
button.primary{background:#0ff;color:#000;padding:10px 12px;border-radius:10px;border:none;cursor:pointer;font-weight:700;margin-top:10px;transition:0.2s;}
button.primary:hover{background:#0ae;}
button.ghost{background:transparent;border:1px solid rgba(255,255,255,0.06);padding:10px;border-radius:8px;color:#fff;cursor:pointer;margin-left:8px;transition:0.2s;}
button.ghost:hover{background:rgba(0,255,240,0.1);}
</style>
</head>
<body>
<div class="bg-gradient"></div>
<canvas id="particles"></canvas>

<!-- Login Page -->
<div id="loginPage" class="auth-wrap">
<h2 style="color:#0ff;margin-bottom:15px;">VERBOSE</h2>
<input id="authUser" placeholder="Username">
<input id="authPass" placeholder="Password" type="password">
<div>
<button class="primary" onclick="doLogin()">Login</button>
<button class="ghost" onclick="doSignup()">Signup</button>
</div>
</div>

<!-- Dashboard -->
<div id="dashboardPage" class="hidden">
<div class="dashboard-header card">
<h1>Welcome to VERBOSE</h1>
<p>Username: <span id="dashUser"></span> | Balance: <span id="dashBalance">0</span> PKR | Daily Profit: <span id="dashProfit">0</span> PKR</p>
</div>
<div id="dashboardIcons" class="plan-grid"></div>
</div>

<div class="footer-menu">
<div onclick="showToast('Wallet clicked')"><i class="fa fa-wallet"></i>Wallet</div>
<div onclick="showToast('Plans clicked')"><i class="fa fa-briefcase"></i>Plans</div>
<div onclick="showToast('Deposit clicked')"><i class="fa fa-money-bill-wave"></i>Deposit</div>
<div onclick="showToast('Withdraw clicked')"><i class="fa fa-credit-card"></i>Withdraw</div>
<div onclick="showToast('Transactions clicked')"><i class="fa fa-file-invoice"></i>Transactions</div>
</div>

<div id="toastRoot"></div>

<script>
// ---------- Particles ----------
const canvas=document.getElementById('particles'); 
const ctx=canvas.getContext('2d');
function resizeCanvas(){canvas.width=innerWidth;canvas.height=innerHeight;}
resizeCanvas(); addEventListener('resize',resizeCanvas);
const particles=[]; for(let i=0;i<120;i++){particles.push({x:Math.random()*innerWidth,y:Math.random()*innerHeight,r:Math.random()*1.2+0.5,vx:(Math.random()-0.5)*0.3,vy:(Math.random()-0.5)*0.3,h:180+Math.random()*80});}
function drawParticles(){ctx.clearRect(0,0,canvas.width,canvas.height);for(const p of particles){ctx.beginPath();ctx.fillStyle=`hsla(${p.h},100%,60%,0.12)`;ctx.shadowBlur=8;ctx.shadowColor=`hsla(${p.h},100%,60%,0.14)`;ctx.fillRect(p.x,p.y,p.r*2,p.r*2);p.x+=p.vx;p.y+=p.vy;if(p.x<0)p.x=canvas.width;if(p.x>canvas.width)p.x=0;if(p.y<0)p.y=canvas.height;if(p.y>canvas.height)p.y=0;}requestAnimationFrame(drawParticles);}
drawParticles();

// ---------- Admin & Users ----------
const ADMIN={user:'AdminKhan',pass:'SuperSecret123'};
const dashboardIcons=[
{icon:'fa-wallet',name:'Wallet',view:'wallet'},
{icon:'fa-briefcase',name:'Plans',view:'plans'},
{icon:'fa-money-bill-wave',name:'Deposit',view:'deposit'},
{icon:'fa-credit-card',name:'Withdraw',view:'withdraw'},
{icon:'fa-file-invoice',name:'Transactions',view:'transactions'}
];
function getUsers(){return JSON.parse(localStorage.getItem('verbose_users')||'[]');}
function setUsers(u){localStorage.setItem('verbose_users',JSON.stringify(u));}
function getCurrent(){return JSON.parse(localStorage.getItem('verbose_current')||'null');}
function setCurrent(u){localStorage.setItem('verbose_current',JSON.stringify(u));}
function showToast(txt){const n=document.createElement('div');n.className='notif';n.innerText=txt;document.body.appendChild(n);setTimeout(()=>n.remove(),2000);}
(function(){let u=getUsers();if(!u.find(x=>x.user===ADMIN.user)){u.push({user:ADMIN.user,pass:ADMIN.pass,balance:0,profit:0,admin:true});setUsers(u);}})();

// ---------- Auth ----------
function afterLogin(){
 const cur=getCurrent(); 
 if(!cur)return;
 document.getElementById('loginPage').classList.add('hidden');
 document.getElementById('dashboardPage').classList.remove('hidden');
 document.getElementById('dashUser').innerText=cur.user;
 document.getElementById('dashBalance').innerText=cur.balance||0;
 document.getElementById('dashProfit').innerText=cur.profit||0;
 renderDashboard();
}
function doSignup(){
 const u=document.getElementById('authUser').value.trim();
 const p=document.getElementById('authPass').value;
 if(!u||!p){showToast('Enter username & password');return;}
 const users=getUsers();
 if(users.find(x=>x.user===u)){showToast('Username exists');return;}
 users.push({user:u,pass:p,balance:0,profit:0,admin:false});
 setUsers(users);
 setCurrent({user:u,admin:false,balance:0,profit:0});
 showToast('Signup successful');afterLogin();
}
function doLogin(){
 const u=document.getElementById('authUser').value.trim();
 const p=document.getElementById('authPass').value;
 if(!u||!p){showToast('Enter username & password');return;}
 if(u===ADMIN.user&&p===ADMIN.pass){setCurrent({user:ADMIN.user,admin:true,balance:0,profit:0});showToast('Admin logged in');afterLogin();return;}
 const users=getUsers();
 const found=users.find(x=>x.user===u&&x.pass===p);
 if(!found){showToast('Invalid credentials');return;}
 setCurrent({user:found.user,admin:false,balance:found.balance||0,profit:found.profit||0});
 showToast('Login successful');afterLogin();
}
function doLogout(){
 localStorage.removeItem('verbose_current');
 document.getElementById('dashboardPage').classList.add('hidden');
 document.getElementById('loginPage').classList.remove('hidden');
 showToast('Logged out');
}

// ---------- Dashboard ----------
function renderDashboard(){
 const grid=document.getElementById('dashboardIcons');
 if(!grid) return;
 grid.innerHTML='';
 dashboardIcons.forEach(d=>{
   const div=document.createElement('div');
   div.className='icon-box';
   div.innerHTML=`<i class="fa ${d.icon}"></i><p>${d.name}</p>`;
   div.onclick=()=>showToast(`${d.name} clicked`);
   grid.appendChild(div);
 });
}
</script>
</body>
</html>
