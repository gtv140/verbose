<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE - Rock Earn</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.2/css/all.min.css">
<style>
body{margin:0;font-family:Arial,sans-serif;background:#f4f4f4;color:#333;}
header{padding:20px;text-align:center;font-weight:900;font-size:24px;background:#fff;border-bottom:1px solid #ddd;}
.wrap{max-width:480px;margin:12px auto;padding:12px;background:#fff;border-radius:10px;box-shadow:0 2px 6px rgba(0,0,0,0.1);}
.card{padding:12px;border-radius:8px;margin-bottom:12px;border:1px solid #ddd;background:#fff;}
input,select,button{width:100%;padding:10px;margin-top:6px;border-radius:6px;border:1px solid #ccc;font-size:14px;}
.btn{background:#007bff;border:none;color:#fff;font-weight:600;cursor:pointer;padding:10px;border-radius:6px;}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;padding:10px;background:#fff;border-top:1px solid #ddd;}
.nav div{color:#007bff;text-align:center;font-size:13px;cursor:pointer;}
.hidden{display:none;}
.plan{display:flex;justify-content:space-between;gap:10px;padding:10px;border-radius:6px;border:1px solid #ddd;margin-bottom:8px;background:#f9f9f9;}
.plan .meta{flex:1;}
.plan .actions{width:110px;text-align:right;}
.user-box{display:flex;justify-content:space-between;align-items:center;margin-bottom:12px;}
.user-box .badge{background:#e0e0e0;padding:4px 8px;border-radius:999px;color:#007bff;}
.countdown{font-weight:600;color:#ff0000;margin-top:6px;}
.referral-box{display:flex;justify-content:space-between;align-items:center;margin-bottom:12px;}
.referral-box input{flex:1;}
</style>
</head>
<body>
<header><i class="fas fa-bolt"></i> VERBOSE <i class="fas fa-bolt"></i></header>
<div class="wrap">

<!-- LOGIN / SIGNUP -->
<div id="loginCard" class="card">
<h3 style="margin:0 0 8px 0;"><i class="fas fa-user"></i> Login / Signup</h3>
<select id="authMode">
<option value="login">Login</option>
<option value="signup">New User</option>
</select>
<input id="inputUser" placeholder="Username"/>
<input id="inputPass" placeholder="Password" type="password"/>
<input id="referralInput" placeholder="Referral Code (optional)"/>
<button class="btn" onclick="doAuth()"><i class="fas fa-sign-in-alt"></i> Submit</button>
<p style="font-size:13px;color:#666;">Tip: Data stored locally, refresh will not reset your data.</p>
</div>

<!-- DASHBOARD -->
<div id="dashboardCard" class="card hidden">
<div class="user-box">
<div>
<div id="welcomeText" style="font-weight:800;">Welcome —</div>
<div id="memberSince" style="font-size:13px;color:#666;">Member since —</div>
</div>
<div style="text-align:right;">
<div style="font-size:13px;color:#666;">Balance</div>
<div style="font-weight:900;font-size:18px;"><i class="fas fa-coins"></i> Rs <span id="balanceText">0</span></div>
<div class="badge"><i class="fas fa-calendar-day"></i> Daily: Rs <span id="dailyText">0</span></div>
<div class="btn" style="font-size:13px;padding:6px 12px;margin-top:6px;" onclick="doLogout()"><i class="fas fa-sign-out-alt"></i> Logout</div>
</div>
</div>

<!-- REFERRAL -->
<div class="referral-box">
<input id="referralLink" readonly/>
<button class="btn" style="width:auto;padding:6px 12px;margin-left:4px;" onclick="copyReferral()"><i class="fas fa-copy"></i> Copy</button>
</div>

<!-- SUPPORT & ACTIVITY -->
<div class="user-box">
<div><i class="fas fa-headset"></i> Support: WhatsApp 03705519562, Email Rock.earn92@gmail.com</div>
<div><i class="fas fa-chart-line"></i> Activity: Coming Soon</div>
</div>

<!-- PLANS -->
<div id="plansCard" class="card hidden">
<h3 style="margin-top:0;"><i class="fas fa-gift"></i> Plans</h3>
<div id="plansList"></div>
</div>

<!-- DEPOSIT -->
<div id="depositCard" class="card hidden">
<h3><i class="fas fa-wallet"></i> Deposit</h3>
<label>Method</label>
<select id="depositMethod" onchange="updateDepositNumber()">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<label>Number</label>
<input id="depositNumber" readonly/>
<label>Amount (PKR)</label>
<input id="depositAmount" readonly/>
<label>Transaction ID</label>
<input id="depositTx" placeholder="Enter TX ID"/>
<label>Upload Proof</label>
<input id="depositProof" type="file"/>
<button class="btn" onclick="submitDeposit()"><i class="fas fa-paper-plane"></i> Submit Deposit</button>
</div>

<!-- WITHDRAW -->
<div id="withdrawCard" class="card hidden">
<h3><i class="fas fa-money-bill-wave"></i> Withdrawal</h3>
<label>Method</label>
<select id="withdrawMethod">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
<option value="bank">Bank</option>
</select>
<label>Username</label>
<input id="withdrawUsername" readonly/>
<label>Account / Mobile Number</label>
<input id="withdrawAccount" placeholder="Enter account or mobile number"/>
<label>Amount (PKR)</label>
<input id="withdrawAmount" placeholder="Enter amount"/>
<button class="btn" onclick="submitWithdraw()"><i class="fas fa-paper-plane"></i> Request Withdrawal</button>
<p style="font-size:13px;color:#666;margin-top:6px;text-align:center;">⚠️ Withdrawal manually approved. Contact support if urgent.</p>
</div>

</div>

<!-- NAV -->
<div class="nav">
<div onclick="nav('dashboardCard')"><i class="fas fa-home"></i> Home</div>
<div onclick="nav('plansCard')"><i class="fas fa-box"></i> Plans</div>
<div onclick="nav('depositCard')"><i class="fas fa-wallet"></i> Deposit</div>
<div onclick="nav('withdrawCard')"><i class="fas fa-hand-holding-usd"></i> Withdraw</div>
</div>

<script>
const KEY_USER='verbose_user';
const KEY_BAL='verbose_balance_';
const KEY_DAILY='verbose_daily_';
const KEY_USER_PLANS='verbose_plans_';
const KEY_DEPOSITS='verbose_deposits';
const KEY_WITHDRAWS='verbose_withdraws';
let currentUser=localStorage.getItem(KEY_USER)||null;
let plans=[];
for(let i=1;i<=7;i++){let invest=200*i;let days=20+Math.floor(Math.random()*51);plans.push({id:i,name:'Special Plan '+i,invest:invest,multiplier:3,total:invest*3,days:days,offer:true});}
for(let i=8;i<=32;i++){let invest=Math.round(3000+(i-8)*(30000-3000)/24);let duration=20+Math.floor(Math.random()*51);let total=Math.round(invest*2.5);plans.push({id:i,name:'Plan '+(i-7),invest:invest,multiplier:2.5,total:total,days:duration,offer:false});}
function fmt(n){return Number(n).toLocaleString('en-US');}
function doAuth(){const mode=document.getElementById('authMode').value;const u=(document.getElementById('inputUser').value||'').trim();const p=(document.getElementById('inputPass').value||'').trim();const ref=document.getElementById('referralInput').value.trim();if(!u||!p){alert('Enter username & password');return;}const credKey='verbose_cred_'+u;if(mode==='signup'){if(localStorage.getItem(credKey)){alert('Username exists');return;}localStorage.setItem(credKey,p);localStorage.setItem(KEY_BAL+u,'0');localStorage.setItem(KEY_DAILY+u,'0');localStorage.setItem(KEY_USER_PLANS+u,'[]');if(ref && localStorage.getItem('verbose_cred_'+ref)){let bal=Number(localStorage.getItem(KEY_BAL+ref)||0);bal+=30;localStorage.setItem(KEY_BAL+ref,bal);alert(`Referral bonus Rs 30 added to ${ref}`);}}else{if(localStorage.getItem(credKey)!==p){alert('Wrong username/password');return;}}localStorage.setItem(KEY_USER,u);currentUser=u;afterLoginUI();}
function afterLoginUI(){nav('dashboardCard');renderPlans();updateReferralLink();}
function updateReferralLink(){if(!currentUser) return;document.getElementById('referralLink').value=window.location.href+'?ref='+currentUser;}
function copyReferral(){const link=document.getElementById('referralLink');link.select();document.execCommand('copy');alert('Referral link copied!');}
function doLogout(){localStorage.removeItem(KEY_USER);currentUser=null;nav('loginCard');}
function nav(cardId){['loginCard','dashboardCard','plansCard','depositCard','withdrawCard'].forEach(id=>document.getElementById(id).classList.add('hidden'));document.getElementById(cardId).classList.remove('hidden');renderDashboard();fillWithdrawUser();}
function renderDashboard(){if(!currentUser)return;document.getElementById('welcomeText').innerText='Welcome, '+currentUser;document.getElementById('memberSince').innerText='Member since: '+new Date().toLocaleDateString();document.getElementById('balanceText').innerText=fmt(Number(localStorage.getItem(KEY_BAL+currentUser)||0));document.getElementById('dailyText').innerText=fmt(Number(localStorage.getItem(KEY_DAILY+currentUser)||0));}
function renderPlans(){const container=document.getElementById('plansList');container.innerHTML='';plans.forEach(plan=>{const div=document.createElement('div');div.className='plan';let dailyProfit=plan.days>0?Math.round(plan.total/plan.days):0;div.innerHTML=`<div class="meta"><div style="font-weight:800"><i class="fas fa-gift"></i>${plan.name}</div><div style="font-size:13px;color:#666;margin-top:4px">Invest: Rs ${fmt(plan.invest)} · Total: Rs ${fmt(plan.total)} · Days: ${plan.days} · Daily: Rs ${fmt(dailyProfit)}</div></div><div class="actions">${plan.offer?'<div class="countdown" id="countdown_'+plan.id+'">Loading...</div>':'<button class="btn" onclick="buyPlan('+plan.id+')"><i class="fas fa-shopping-cart"></i> Buy</button>'}</div>`;container.appendChild(div);if(plan.offer){let endTs=Date.now()+24*3600*1000;const el=document.getElementById('countdown_'+plan.id);function tick(){const diff=Math.floor((endTs-Date.now())/1000);if(diff<=0){el.innerText='Ended';return;}const h=Math.floor(diff/3600),m=Math.floor((diff%3600)/60),s=diff%60;el.innerText=`${String(h).padStart(2,'0')}h:${String(m).padStart(2,'0')}m:${String(s).padStart(2,'0')}s`;requestAnimationFrame(tick);}tick();}});}
function buyPlan(id){if(!currentUser){alert('Login first');return;}const plan=plans.find(p=>p.id===id);if(!plan){alert('Plan not available');return;}let userPlans=JSON.parse(localStorage.getItem(KEY_USER_PLANS+currentUser)||'[]');const dailyProfit=Math.round(plan.total/plan.days);userPlans.push({planId:plan.id,dailyProfit,lastCredit:Date.now()});localStorage.setItem(KEY_USER_PLANS+currentUser,JSON.stringify(userPlans));document.getElementById('depositAmount').value=plan.invest;updateDepositNumber();nav('depositCard');alert(`Plan ${plan.name} selected. Submit deposit.`);}
function updateDepositNumber(){const method=document.getElementById('depositMethod').value;document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';}
function submitDeposit(){if(!currentUser){alert('Login first');return;}const tx=(document.getElementById('depositTx').value||'').trim();const proof=document.getElementById('depositProof').files[0];const amount=Number(document.getElementById('depositAmount').value)||0;if(!tx||!proof||!amount){alert('All fields required');return;}const deposits=JSON.parse(localStorage.getItem(KEY_DEPOSITS)||'[]');deposits.push({user:currentUser,method:document.getElementById('depositMethod').value,amount,tx,proof:proof.name,time:Date.now(),approved:false});localStorage.setItem(KEY_DEPOSITS,JSON.stringify(deposits));alert('Deposit submitted!');document.getElementById('depositTx').value='';document.getElementById('depositProof').value='';nav('dashboardCard');}
function fillWithdrawUser(){if(!currentUser) return;document.getElementById('withdrawUsername').value=currentUser;}
function submitWithdraw(){if(!currentUser){alert('Login first');return;}const account=document.getElementById('withdrawAccount').value.trim();const amount=Number(document.getElementById('withdrawAmount').value);if(!account||!amount){alert('Enter account and amount');return;}const withdraws=JSON.parse(localStorage.getItem(KEY_WITHDRAWS)||'[]');withdraws.push({user:currentUser,method:document.getElementById('withdrawMethod').value,account,amount,time:Date.now(),approved:false});localStorage.setItem(KEY_WITHDRAWS,JSON.stringify(withdraws));alert('Withdrawal request submitted!');document.getElementById('withdrawAccount').value='';document.getElementById('withdrawAmount').value='';nav('dashboardCard');}
</script>
</body>
</html>
