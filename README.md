<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.2/css/all.min.css">
<style>
:root{
--bg:#0a0a0a;
--primary:#1e90ff;
--secondary:#4da6ff;
--text:#e0e0e0;
--muted:#888;
--alert:#ff4d4d;
--success:#4dff4d;
}
body{margin:0;font-family:Arial,sans-serif;background:var(--bg);color:var(--text);}
header{padding:20px;text-align:center;font-weight:900;font-size:26px;color:var(--primary);}
.wrap{max-width:480px;margin:12px auto;padding:12px;}
.card{background:#111;padding:12px;border-radius:12px;margin-bottom:12px;border:1px solid #222;}
input,select,button{width:100%;padding:10px;margin-top:6px;border-radius:8px;border:1px solid #333;background:#111;color:var(--text);font-size:14px;}
.btn{background:var(--primary);border:none;color:#fff;font-weight:800;cursor:pointer;padding:10px;border-radius:10px;transition:0.3s;}
.btn:hover{background:var(--secondary);}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;padding:10px;background:#111;border-top:1px solid #222;}
.nav div{color:var(--text);text-align:center;font-size:13px;cursor:pointer;}
.countdown{font-weight:800;color:var(--primary);margin-top:6px;}
.muted{color:var(--muted);font-size:13px;}
.plan{display:flex;justify-content:space-between;gap:10px;padding:10px;border-radius:10px;border:1px solid #222;margin-bottom:8px;background:#111;}
.plan .meta{flex:1;}
.plan .actions{width:110px;text-align:right;}
.user-box{display:flex;justify-content:space-between;align-items:center;margin-bottom:12px;}
.user-box .badge{background:#222;padding:4px 8px;border-radius:999px;color:var(--primary);}
.admin-box{display:flex;justify-content:space-around;margin-top:12px;}
.admin-box div{flex:1;text-align:center;padding:10px;background:#222;border-radius:10px;margin:2px;cursor:pointer;}
.admin-box div i{font-size:20px;margin-bottom:4px;display:block;color:var(--primary);}
.alert-note{background:#330000;color:var(--alert);padding:10px;margin-bottom:12px;border-radius:8px;font-weight:600;text-align:center;}
.success-note{background:#003300;color:var(--success);padding:10px;margin-bottom:12px;border-radius:8px;font-weight:600;text-align:center;}
.referral-box{display:flex;justify-content:space-between;align-items:center;margin-bottom:12px;}
.referral-box input{flex:1;}
.logout-btn{position:fixed;bottom:60px;right:20px;background:var(--alert);color:#fff;padding:10px 14px;border-radius:50%;cursor:pointer;font-size:18px;border:none;}
.logout-btn:hover{background:#ff1a1a;}
</style>
</head>
<body>
<header><i class="fas fa-rocket"></i> VERBOSE</header>
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
<p class="muted">Tip: Use same device & browser. Data stored locally.</p>
</div>

<!-- DASHBOARD -->
<div id="dashboardCard" class="card hidden">
<div class="alert-note">⚠️ If any issue with Deposit/Withdrawal, contact VERBOSE Administration immediately.</div>
<div class="user-box">
<div>
<div id="welcomeText" style="font-weight:800;"><i class="fas fa-user-check"></i> Welcome —</div>
<div id="memberSince" class="muted">Member since —</div>
</div>
<div style="text-align:right;">
<div class="muted">Balance</div>
<div style="font-weight:900;font-size:18px"><i class="fas fa-coins"></i> Rs <span id="balanceText">0</span></div>
<div class="badge"><i class="fas fa-calendar-day"></i> Daily: Rs <span id="dailyText">0</span></div>
</div>
</div>

<!-- REFERRAL -->
<div class="referral-box">
<input id="referralLink" readonly/>
<button class="btn" onclick="copyReferral()"><i class="fas fa-copy"></i> Copy</button>
</div>

<!-- ADMIN -->
<div class="card">
<h3>Administration / Support</h3>
<p>WhatsApp: <a href="https://wa.me/93705519562" target="_blank">03705519562</a></p>
<p>Email: <a href="mailto:rock.earn92@gmail.com">rock.earn92@gmail.com</a></p>
<p>VERBOSE — Million+ Users Platform</p>
</div>

<!-- PLANS -->
<div id="plansCard" class="card hidden">
<h3><i class="fas fa-gift"></i> Plans</h3>
<div class="muted" style="margin-bottom:8px;">7 Special Offers (24h), 25 Normal Plans + 5 Coming Soon</div>
<div id="plansList"></div>
</div>

<!-- DEPOSIT -->
<div id="depositCard" class="card hidden">
<h3><i class="fas fa-wallet"></i> Deposit</h3>
<label class="muted">Method</label>
<select id="depositMethod" onchange="updateDepositNumber()">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<label class="muted">Number</label>
<input id="depositNumber" readonly/>
<label class="muted">Amount (PKR)</label>
<input id="depositAmount" readonly/>
<label class="muted">Transaction ID</label>
<input id="depositTx" placeholder="Enter TX ID"/>
<label class="muted">Upload Proof</label>
<input id="depositProof" type="file"/>
<button class="btn" onclick="submitDeposit()"><i class="fas fa-paper-plane"></i> Submit Deposit</button>
</div>

<!-- WITHDRAW -->
<div id="withdrawCard" class="card hidden">
<h3><i class="fas fa-money-bill-wave"></i> Withdrawal</h3>
<label class="muted">Method</label>
<select id="withdrawMethod">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
<option value="bank">Bank</option>
</select>
<label class="muted">Account / Mobile Number</label>
<input id="withdrawAccount" placeholder="Enter account or mobile number"/>
<label class="muted">Amount (PKR)</label>
<input id="withdrawAmount" placeholder="Enter amount"/>
<button class="btn" onclick="submitWithdraw()"><i class="fas fa-paper-plane"></i> Request Withdrawal</button>
<p class="muted" style="margin-top:6px;text-align:center;">⚠️ Withdrawal will be approved manually. Contact support for urgent issues.</p>
</div>

</div>

<!-- NAV -->
<div class="nav">
<div onclick="nav('dashboardCard')"><i class="fas fa-home"></i> Home</div>
<div onclick="nav('plansCard')"><i class="fas fa-box"></i> Plans</div>
<div onclick="nav('depositCard')"><i class="fas fa-wallet"></i> Deposit</div>
<div onclick="nav('withdrawCard')"><i class="fas fa-hand-holding-usd"></i> Withdraw</div>
</div>

<button class="logout-btn" onclick="doLogout()" title="Logout"><i class="fas fa-sign-out-alt"></i></button>

<script>
// STORAGE KEYS
const KEY_USER='verbose_user';
const KEY_BAL='verbose_balance_';
const KEY_DAILY='verbose_daily_';
const KEY_USER_PLANS='verbose_plans_';
const KEY_OFFERS='verbose_offer_';
const KEY_DEPOSITS='verbose_deposits';
const KEY_WITHDRAWS='verbose_withdraws';
let currentUser=localStorage.getItem(KEY_USER)||null;
let plans=[],offerIntervals={};

// GENERATE PLANS
for(let i=1;i<=7;i++){
let invest=200*i;if(invest>3000) invest=3000; let days=20+Math.floor(Math.random()*51);
plans.push({id:i,name:'Special Plan '+i,invest:invest,multiplier:3,total:invest*3,days:days,offer:true});
}
for(let i=8;i<=32;i++){
let invest=Math.round(3000+(i-8)*(30000-3000)/24);
let duration=20+Math.floor(Math.random()*51);
let total=Math.round(invest*2.5);
plans.push({id:i,name:'Plan '+(i-7),invest:invest,multiplier:2.5,total:total,days:duration,offer:false});
}
for(let i=33;i<=37;i++){
plans.push({id:i,name:'Coming Soon',invest:0,multiplier:0,total:0,days:0,offer:false});
}
function fmt(n){return Number(n).toLocaleString('en-US');}

// AUTH
function doAuth(){
const mode=document.getElementById('authMode').value;
const u=(document.getElementById('inputUser').value||'').trim();
const p=(document.getElementById('inputPass').value||'').trim();
const ref=document.getElementById('referralInput').value.trim();
if(!u||!p){alert('Enter username & password');return;}
const credKey='verbose_cred_'+u;
if(mode==='signup'){
if(localStorage.getItem(credKey)){alert('Username exists');return;}
localStorage.setItem(credKey,p);localStorage.setItem(KEY_BAL+u,'0');localStorage.setItem(KEY_DAILY+u,'0');localStorage.setItem(KEY_USER_PLANS+u,'[]');
if(ref && localStorage.getItem('verbose_cred_'+ref)){let bal=Number(localStorage.getItem(KEY_BAL+ref)||0);bal+=30;localStorage.setItem(KEY_BAL+ref,bal);alert(`Referral bonus Rs 30 added to ${ref}`);}
}else{if(localStorage.getItem(credKey)!==p){alert('Wrong username/password');return;}}
localStorage.setItem(KEY_USER,u);currentUser=u;afterLoginUI();
}

function afterLoginUI(){nav('dashboardCard');renderPlans();startOffers();updateReferralLink();}
function updateReferralLink(){if(!currentUser) return;document.getElementById('referralLink').value=window.location.href+'?ref='+currentUser;}
function copyReferral(){const link=document.getElementById('referralLink');link.select();document.execCommand('copy');alert('Referral link copied!');}
function doLogout(){localStorage.removeItem(KEY_USER);currentUser=null;nav('loginCard');Object.values(offerIntervals).forEach(i=>clearInterval(i));}

// NAV
function nav(cardId){['loginCard','dashboardCard','plansCard','depositCard','withdrawCard'].forEach(id=>document.getElementById(id).classList.add('hidden'));document.getElementById(cardId).classList.remove('hidden');renderDashboard();fillWithdrawUser();updateReferralLink();}

// DASHBOARD
function renderDashboard(){if(!currentUser)return;document.getElementById('welcomeText').innerText='Welcome, '+currentUser;document.getElementById('memberSince').innerText='Member since: '+new Date().toLocaleDateString();
document.getElementById('balanceText').innerText=fmt(Number(localStorage.getItem(KEY_BAL+currentUser)||0));document.getElementById('dailyText').innerText=fmt(Number(localStorage.getItem(KEY_DAILY+currentUser)||0));}

// PLANS
function renderPlans(){
const container=document.getElementById('plansList');container.innerHTML='';
plans.forEach(plan=>{
const div=document.createElement('div');div.className='plan';if(plan.name==='Coming Soon') div.className+=' coming-soon';
let dailyProfit=plan.days>0?Math.round(plan.total/plan.days):0;
div.innerHTML=`<div class="meta"><div style="font-weight:800"><i class="fas fa-gift"></i>${plan.name}</div>
<div class="muted" style="margin-top:4px">Invest: Rs ${fmt(plan.invest)} · Total: Rs ${fmt(plan.total)} · Days: ${plan.days} · Daily: Rs ${fmt(dailyProfit)}</div>
${plan.offer?`<div class="countdown" id="countdown_${plan.id}">Loading timer...</div>`:''}</div>
<div class="actions">${plan.offer||plan.name!=='Coming Soon'?'<button class="btn" onclick="buyPlan('+plan.id+')"><i class="fas fa-shopping-cart"></i> Buy Now</button>':''}</div>`;container.appendChild(div);
});
}

// OFFER TIMER
function startOffers(){plans.forEach(plan=>{if(!plan.offer)return;
const key=KEY_OFFERS+plan.id;let endTs=Number(localStorage.getItem(key)||0);if(!endTs||endTs<Date.now()){endTs=Date.now()+24*3600*1000;localStorage.setItem(key,endTs);}
const el=document.getElementById('countdown_'+plan.id);if(!el)return;function tick(){const diff=Math.floor((endTs-Date.now())/1000);if(diff<=0){el.innerText='Offer ended';clearInterval(offerIntervals[plan.id]);return;}
const h=Math.floor(diff/3600),m=Math.floor((diff%3600)/60),s=diff%60;el.innerText=`${String(h).padStart(2,'0')}h:${String(m).padStart(2,'0')}m:${String(s).padStart(2,'0')}s`;}tick();offerIntervals[plan.id]=setInterval(tick,1000);});}

// BUY PLAN
function buyPlan(id){if(!currentUser){alert('Login first');return;}const plan=plans.find(p=>p.id===id);if(!plan || plan.name==='Coming Soon'){alert('Plan not available');return;}
let userPlans=JSON.parse(localStorage.getItem(KEY_USER_PLANS+currentUser)||'[]');
const dailyProfit=Math.round(plan.total/plan.days);userPlans.push({planId:plan.id,dailyProfit,lastCredit:Date.now()});localStorage.setItem(KEY_USER_PLANS+currentUser,JSON.stringify(userPlans));
document.getElementById('depositAmount').value=plan.invest;updateDepositNumber();nav('depositCard');alert(`Plan ${plan.name} selected. Submit your deposit. Admin will manually approve.`);}

// DEPOSIT
function updateDepositNumber(){const method=document.getElementById('depositMethod').value;document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';}
function submitDeposit(){if(!currentUser){alert('Login first');return;}
const tx=(document.getElementById('depositTx').value||'').trim();const proof=document.getElementById('depositProof').files[0];const amount=Number(document.getElementById('depositAmount').value)||0;
if(!tx||!proof||!amount){alert('All fields required');return;}
let bal=Number(localStorage.getItem(KEY_BAL+currentUser)||0);bal+=amount;localStorage.setItem(KEY_BAL+currentUser,bal);
const deposits=JSON.parse(localStorage.getItem(KEY_DEPOSITS)||'[]');deposits.push({user:currentUser,method:document.getElementById('depositMethod').value,amount,tx,proof:proof.name,time:Date.now(),approved:false});
localStorage.setItem(KEY_DEPOSITS,JSON.stringify(deposits));alert('Deposit submitted! Admin will approve manually.');document.getElementById('depositTx').value='';document.getElementById('depositProof').value='';nav('dashboardCard');renderDashboard();}

// WITHDRAW
function fillWithdrawUser(){if(!currentUser) return;}
function submitWithdraw(){if(!currentUser){alert('Login first');return;}
const method=document.getElementById('withdrawMethod').value;
const account=document.getElementById('withdrawAccount').value.trim();
const amount=Number(document.getElementById('withdrawAmount').value);
if(!account||!amount){alert('Enter account and amount');return;}
let bal=Number(localStorage.getItem(KEY_BAL+currentUser)||0);if(amount>bal){alert('Insufficient balance');return;}
bal-=amount;localStorage.setItem(KEY_BAL+currentUser,bal);
const withdraws=JSON.parse(localStorage.getItem(KEY_WITHDRAWS)||'[]');withdraws.push({user:currentUser,method,account,amount,time:Date.now(),approved:false});
localStorage.setItem(KEY_WITHDRAWS,JSON.stringify(withdraws));
alert('Withdrawal request submitted! Admin will approve manually.');document.getElementById('withdrawAccount').value='';document.getElementById('withdrawAmount').value='';nav('dashboardCard');renderDashboard();}
</script>
</body>
</html>
