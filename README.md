<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE Trending App</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<style>
/* ---------- Reset & Base ---------- */
*{margin:0;padding:0;box-sizing:border-box;font-family:'Orbitron',sans-serif;}
body, html{height:100%;background:#0a0015;color:#fff;overflow-x:hidden;transition:all 0.3s;}
.hidden{display:none;}
.muted{opacity:0.7;font-size:13px;}

/* ---------- Background ---------- */
.bg-gradient{position:fixed;inset:0;z-index:-2;background:linear-gradient(270deg,#0f0530,#1b0a4a,#061022);background-size:800% 800%;animation:grad 25s ease infinite;filter:blur(28px) saturate(120%);}
@keyframes grad{0%{background-position:0% 50%}50%{background-position:100% 50%}100%{background-position:0% 50%}}

/* ---------- Header ---------- */
.app-header{display:flex;justify-content:space-between;align-items:center;padding:15px;background:#111;box-shadow:0 2px 15px rgba(0,255,240,0.3);}
.app-header h1{color:#0ff;font-size:24px;text-shadow:0 0 14px #0ff;}
.menu-btn{font-size:24px;color:#0ff;cursor:pointer;}

/* ---------- Dashboard Icons ---------- */
.icon-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(90px,1fr));gap:15px;padding:20px;}
.icon-box{background:#111;padding:18px;text-align:center;border-radius:18px;box-shadow:0 0 15px #0ff;transition:0.3s;cursor:pointer;}
.icon-box:hover{transform:scale(1.08);box-shadow:0 0 28px #0ff;}
.icon-box i{font-size:28px;margin-bottom:6px;color:#0ff;}
.icon-box p{font-size:12px;color:#cfe;}

/* ---------- Cards ---------- */
.card{background:rgba(255,255,255,0.02);border:1px solid rgba(0,255,255,0.06);padding:20px;border-radius:16px;box-shadow:0 10px 36px rgba(0,0,0,0.6);backdrop-filter:blur(6px);margin:18px;transition:0.3s;}
.card:hover{transform:translateY(-5px);box-shadow:0 15px 40px rgba(0,255,240,0.18);}

/* ---------- Inputs & Buttons ---------- */
input,select{width:100%;padding:12px;margin-top:10px;border-radius:10px;border:1px solid rgba(255,255,255,0.06);background:rgba(0,0,0,0.35);color:#fff;outline:none;transition:0.2s;}
input:focus,select:focus{border-color:#0ff;}
button.primary{background:#0ff;color:#000;padding:12px 15px;border-radius:12px;border:none;cursor:pointer;font-weight:700;margin-top:12px;transition:0.2s;}
button.primary:hover{background:#0ae;}
button.ghost{background:transparent;border:1px solid rgba(255,255,255,0.06);padding:12px;border-radius:10px;color:#fff;cursor:pointer;margin-left:8px;transition:0.2s;}
button.ghost:hover{background:rgba(0,255,240,0.1);}

/* ---------- Footer Menu ---------- */
.footer-menu{position:fixed;bottom:0;left:0;width:100%;background:#111;display:flex;justify-content:space-around;padding:12px 0;box-shadow:0 -2px 15px rgba(0,255,240,0.3);}
.footer-menu div{text-align:center;color:#fff;font-size:12px;cursor:pointer;}
.footer-menu i{display:block;font-size:22px;margin-bottom:4px;color:#0ff;}

/* ---------- Plans ---------- */
.plan-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(140px,1fr));gap:12px;margin-top:15px;}
.plan{border-radius:14px;padding:16px;border:1px solid rgba(0,255,240,0.06);background:linear-gradient(180deg,rgba(0,255,240,0.02),transparent);position:relative;transition:transform .18s;cursor:pointer;}
.plan:hover{transform:translateY(-6px);box-shadow:0 15px 40px rgba(0,255,240,0.12);}
.badge{position:absolute;top:10px;right:10px;background:#ffea00;color:#000;padding:5px 7px;border-radius:10px;font-weight:800;font-size:11px;}

/* ---------- Table ---------- */
table{width:100%;border-collapse:collapse;margin-top:12px;}
th,td{padding:10px;border-bottom:1px solid rgba(255,255,255,0.03);font-size:13px;color:#0ff;}

/* ---------- Toast ---------- */
.notif{position:fixed;right:18px;bottom:18px;background:#0ff;color:#000;padding:14px 18px;border-radius:14px;font-weight:700;box-shadow:0 12px 35px rgba(0,255,240,0.25);z-index:99;}

/* ---------- Auth ---------- */
.auth-wrap{display:flex;flex-direction:column;align-items:center;justify-content:center;padding:35px;border-radius:16px;box-shadow:0 12px 36px rgba(0,255,240,0.25);}
.auth-wrap h2{text-align:center;margin-bottom:18px;}
</style>
</head>
<body>

<div class="bg-gradient"></div>
<canvas id="particles"></canvas>

<div class="app-header">
<h1>VERBOSE</h1>
<i class="fa fa-bars menu-btn"></i>
</div>

<!-- Login / Signup -->
<div id="authView" class="card auth-wrap">
<h2>Login / Signup</h2>
<input id="authUser" placeholder="Username">
<input id="authPass" placeholder="Password" type="password">
<div style="display:flex;margin-top:12px;">
<button class="primary" onclick="doLogin()">Login</button>
<button class="ghost" onclick="doSignup()">Signup</button>
</div>
<div class="muted small" style="margin-top:12px;">Admin: <strong>AdminKhan</strong> / <strong>SuperSecret123</strong></div>
</div>

<!-- Dashboard -->
<div id="appView" class="hidden">
<div class="icon-grid">
<!-- Multiple icons for trending app feel -->
<div class="icon-box" onclick="navigate('wallet')"><i class="fa fa-wallet"></i><p>Wallet</p></div>
<div class="icon-box" onclick="navigate('plans')"><i class="fa fa-briefcase"></i><p>Plans</p></div>
<div class="icon-box" onclick="navigate('deposit')"><i class="fa fa-money-bill-wave"></i><p>Deposit</p></div>
<div class="icon-box" onclick="navigate('withdraw')"><i class="fa fa-credit-card"></i><p>Withdraw</p></div>
<div class="icon-box" onclick="navigate('transactions')"><i class="fa fa-file-invoice"></i><p>Transactions</p></div>
<div class="icon-box" onclick="navigate('profile')"><i class="fa fa-user"></i><p>Profile</p></div>
<div class="icon-box" onclick="navigate('settings')"><i class="fa fa-cog"></i><p>Settings</p></div>
<div class="icon-box" onclick="navigate('notifications')"><i class="fa fa-bell"></i><p>Notifications</p></div>
<div class="icon-box" onclick="navigate('help')"><i class="fa fa-question-circle"></i><p>Help</p></div>
<div class="icon-box" onclick="navigate('messages')"><i class="fa fa-envelope"></i><p>Messages</p></div>
<div class="icon-box" onclick="navigate('tasks')"><i class="fa fa-list-check"></i><p>Tasks</p></div>
<div class="icon-box" onclick="navigate('achievements')"><i class="fa fa-trophy"></i><p>Achievements</p></div>
<div class="icon-box" onclick="navigate('support')"><i class="fa fa-headset"></i><p>Support</p></div>
<div class="icon-box" onclick="navigate('referral')"><i class="fa fa-users"></i><p>Referral</p></div>
<div class="icon-box" onclick="navigate('rewards')"><i class="fa fa-gift"></i><p>Rewards</p></div>
<div class="icon-box" onclick="navigate('offers')"><i class="fa fa-tags"></i><p>Offers</p></div>
<div class="icon-box" onclick="navigate('stats')"><i class="fa fa-chart-line"></i><p>Stats</p></div>
<div class="icon-box" onclick="navigate('leaderboard')"><i class="fa fa-medal"></i><p>Leaderboard</p></div>
<div class="icon-box" onclick="navigate('chat')"><i class="fa fa-comment"></i><p>Chat</p></div>
<div class="icon-box" onclick="doLogout()"><i class="fa fa-right-from-bracket"></i><p>Logout</p></div>
</div>
</div>

<!-- Footer -->
<div class="footer-menu">
<div onclick="navigate('wallet')"><i class="fa fa-wallet"></i>Wallet</div>
<div onclick="navigate('plans')"><i class="fa fa-briefcase"></i>Plans</div>
<div onclick="navigate('deposit')"><i class="fa fa-money-bill-wave"></i>Deposit</div>
<div onclick="navigate('withdraw')"><i class="fa fa-credit-card"></i>Withdraw</div>
<div onclick="navigate('transactions')"><i class="fa fa-file-invoice"></i>Transactions</div>
</div>

<div id="toastRoot"></div>

<script>
// ---------- Particles ----------
const canvas=document.getElementById('particles'); const ctx=canvas.getContext('2d');
function resizeCanvas(){canvas.width=innerWidth;canvas.height=innerHeight;}
resizeCanvas(); addEventListener('resize',resizeCanvas);
const particles=[]; for(let i=0;i<140;i++){particles.push({x:Math.random()*innerWidth,y:Math.random()*innerHeight,r:Math.random()*1.6+0.6,vx:(Math.random()-0.5)*0.6,vy:(Math.random()-0.5)*0.6,h:180+Math.random()*80});}
function drawParticles(){ctx.clearRect(0,0,canvas.width,canvas.height);for(const p of particles){ctx.beginPath();ctx.fillStyle=`hsla(${p.h},100%,60%,0.12)`;ctx.shadowBlur=12;ctx.shadowColor=`hsla(${p.h},100%,60%,0.14)`;ctx.fillRect(p.x,p.y,p.r*2,p.r*2);p.x+=p.vx;p.y+=p.vy;if(p.x<0)p.x=canvas.width;if(p.x>canvas.width)p.x=0;if(p.y<0)p.y=canvas.height;if(p.y>canvas.height)p.y=0;}requestAnimationFrame(drawParticles);}
drawParticles();

// ---------- Admin & Storage ----------
const ADMIN={user:'AdminKhan',pass:'SuperSecret123'};
const users=JSON.parse(localStorage.getItem('verbose_users')||'[]');
const current=JSON.parse(localStorage.getItem('verbose_current')||'null');

// ---------- Toast ----------
function showToast(text){const n=document.createElement('div');n.className='notif';n.innerText=text;document.body.appendChild(n);setTimeout(()=>n.remove(),2200);}

// ---------- Auth ----------
function doSignup(){const u=document.getElementById('authUser').value.trim();const p=document.getElementById('authPass').value;if(!u||!p){showToast('Enter username & password');return;}let us=users;if(us.find(x=>x.user===u)){showToast('Username exists');return;}us.push({user:u,pass:p,balance:0,admin:false});localStorage.setItem('verbose_users',JSON.stringify(us));localStorage.setItem('verbose_current',JSON.stringify({user:u,admin:false}));showToast('Signup successful');afterLogin();}
function doLogin(){const u=document.getElementById('authUser').value.trim();const p=document.getElementById('authPass').value;if(!u||!p){showToast('Enter username & password');return;}if(u===ADMIN.user&&p===ADMIN.pass){localStorage.setItem('verbose_current',JSON.stringify({user:u,admin:true}));showToast('Admin logged in');afterLogin();return;}const found=users.find(x=>x.user===u&&x.pass===p);if(!found){showToast('Invalid credentials');return;}localStorage.setItem('verbose_current',JSON.stringify({user:found.user,admin:false}));showToast('Login successful');afterLogin();}
function doLogout(){localStorage.removeItem('verbose_current');document.getElementById('appView').classList.add('hidden');document.getElementById('authView').classList.remove('hidden');showToast('Logged out');}
function afterLogin(){document.getElementById('authView').classList.add('hidden');document.getElementById('appView').classList.remove('hidden');}

// ---------- Navigation ----------
function navigate(view){showToast('Navigated to '+view);}
</script>

</body>
</html>
