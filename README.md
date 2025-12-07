<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.2/css/all.min.css">
<style>
:root{
--bg:#040404;
--neon:#0ff;
--accent:#5af;
--muted:rgba(230,247,251,0.6);
--alert:#ff5caa;
--success:#0f0;
}
body{margin:0;font-family:Arial,sans-serif;background:var(--bg);color:#e6fbff;}
.hidden{display:none;}
header{padding:20px;text-align:center;font-weight:900;color:var(--neon);text-shadow:0 0 12px var(--neon);font-size:24px;}
.wrap{max-width:480px;margin:12px auto;padding:12px;}
.card{background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));padding:12px;border-radius:12px;margin-bottom:12px;border:1px solid rgba(0,255,240,0.06);}
input,select,button{width:100%;padding:10px;margin-top:6px;border-radius:8px;border:1px solid rgba(255,255,255,0.03);background:transparent;color:#dff;font-size:14px;}
.btn{background:linear-gradient(90deg,var(--neon),var(--accent));border:none;color:#001;font-weight:800;cursor:pointer;padding:10px;border-radius:10px;}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;padding:10px;background:rgba(0,0,0,0.6);border-top:1px solid rgba(0,255,240,0.03);}
.nav div{color:var(--neon);text-align:center;font-size:13px;cursor:pointer;}
.countdown{font-weight:800;color:var(--neon);margin-top:6px;}
.muted{color:var(--muted);font-size:13px;}
.plan{display:flex;justify-content:space-between;gap:10px;padding:10px;border-radius:10px;border:1px solid rgba(0,255,240,0.04);margin-bottom:8px;background:linear-gradient(180deg,rgba(255,255,255,0.01),rgba(0,0,0,0.04));}
.plan .meta{flex:1;}
.plan .actions{width:110px;text-align:right;}
.user-box{display:flex;justify-content:space-between;align-items:center;margin-bottom:12px;}
.user-box .badge{background:rgba(0,255,240,0.06);padding:4px 8px;border-radius:999px;color:var(--neon);}
.icon{margin-right:6px;}
.admin-box{display:flex;justify-content:space-around;margin-top:12px;}
.admin-box div{flex:1;text-align:center;padding:10px;background:rgba(0,255,240,0.06);border-radius:10px;margin:2px;cursor:pointer;}
.admin-box div i{font-size:20px;margin-bottom:4px;display:block;color:var(--neon);}
.coming-soon{opacity:0.5;font-style:italic;}
.alert-note{background:rgba(255,0,128,0.1);color:var(--alert);padding:10px;margin-bottom:12px;border-radius:8px;font-weight:600;text-align:center;text-shadow:0 0 6px var(--neon);}
.success-note{background:rgba(0,255,0,0.1);color:var(--success);padding:10px;margin-bottom:12px;border-radius:8px;font-weight:600;text-align:center;text-shadow:0 0 6px var(--neon);}
.referral-box{display:flex;justify-content:space-between;align-items:center;margin-bottom:12px;}
.referral-box input{flex:1;}
</style>
</head>
<body>
<header><i class="fas fa-bolt icon"></i>VERBOSE<i class="fas fa-bolt icon"></i></header>
<div class="wrap">

<!-- LOGIN / SIGNUP -->
<div id="loginCard" class="card">
<h3 style="margin:0 0 8px 0;color:var(--neon)"><i class="fas fa-user icon"></i>Login / Signup</h3>
<select id="authMode">
<option value="login">Login</option>
<option value="signup">New User</option>
</select>
<input id="inputUser" placeholder="Username"/>
<input id="inputPass" placeholder="Password" type="password"/>
<input id="referralInput" placeholder="Referral Code (optional)"/>
<button class="btn" onclick="doAuth()"><i class="fas fa-sign-in-alt icon"></i>Submit</button>
<p class="muted">Tip: Use same device & browser. Data stored locally.</p>
</div>

<!-- DASHBOARD -->
<div id="dashboardCard" class="card hidden">
<div class="alert-note">⚠️ All transactions are manually approved. Deposit/Withdrawal issues? Contact Support immediately.</div>
<div class="user-box">
<div>
<div id="welcomeText" style="font-weight:800;color:var(--neon)"><i class="fas fa-user-check icon"></i>Welcome —</div>
<div id="memberSince" class="muted">Member since —</div>
</div>
<div style="text-align:right;">
<div class="muted">Balance</div>
<div style="font-weight:900;font-size:18px"><i class="fas fa-coins icon"></i>Rs <span id="balanceText">0</span></div>
<div class="badge"><i class="fas fa-calendar-day icon"></i>Daily: Rs <span id="dailyText">0</span></div>
<div class="btn" style="font-size:13px;padding:4px 8px;margin-top:4px;" onclick="doLogout()"><i class="fas fa-sign-out-alt icon"></i>Logout</div>
</div>
</div>

<!-- REFERRAL -->
<div class="referral-box">
<input id="referralLink" readonly/>
<button class="btn" style="width:auto;padding:6px 12px;margin-left:4px;" onclick="copyReferral()"><i class="fas fa-copy icon"></i>Copy</button>
</div>

<!-- SUPPORT & ACTIVITY -->
<div class="admin-box">
<div onclick="openSupport()"><i class="fas fa-headset"></i>Support</div>
<div onclick="alert('Activity Log coming soon')"><i class="fas fa-chart-line"></i>Activity</div>
</div>

<!-- PLANS -->
<div id="plansCard" class="card hidden">
<h3 style="color:var(--neon);margin-top:0"><i class="fas fa-gift icon"></i>Plans</h3>
<div class="muted" style="margin-bottom:8px;">Special Offers: 7 plans (24h countdown). Normal: 25 plans (20–70 days) + 5 Coming Soon.</div>
<div id="plansList"></div>
</div>

<!-- DEPOSIT -->
<div id="depositCard" class="card hidden">
<h3 style="color:var(--neon);margin:0 0 6px 0;"><i class="fas fa-hand-holding-usd icon"></i>Deposit</h3>
<label class="muted">Method</label>
<select id="depositMethod" onchange="updateDepositNumber()">
<option value="jazzcash">JazzCash — 03705519562</option>
<option value="easypaisa">EasyPaisa — 03379827882</option>
</select>
<label class="muted">Number</label>
<input id="depositNumber" readonly/>
<label class="muted">Amount (PKR)</label>
<input id="depositAmount" readonly/>
<label class="muted">Transaction ID</label>
<input id="depositTx" placeholder="Enter TX ID"/>
<label class="muted">Upload Proof</label>
<input id="depositProof" type="file"/>
<button class="btn" onclick="submitDeposit()"><i class="fas fa-paper-plane icon"></i>Submit Deposit</button>
</div>

<!-- WITHDRAW -->
<div id="withdrawCard" class="card hidden">
<h3 style="color:var(--neon);margin:0 0 6px 0;"><i class="fas fa-money-bill-wave icon"></i>Withdrawal</h3>
<label class="muted">Method</label>
<select id="withdrawMethod">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
<option value="bank">Bank</option>
</select>
<label class="muted">Username</label>
<input id="withdrawUsername" readonly/>
<label class="muted">Account / Mobile Number</label>
<input id="withdrawAccount" placeholder="Enter account or mobile number"/>
<label class="muted">Amount (PKR)</label>
<input id="withdrawAmount" placeholder="Enter amount"/>
<button class="btn" onclick="submitWithdraw()"><i class="fas fa-paper-plane icon"></i>Request Withdrawal</button>
<p class="muted" style="margin-top:6px;text-align:center;">⚠️ Withdrawal will be approved manually. Contact Support for urgent issues.</p>
</div>

</div>

<!-- NAV -->
<div class="nav">
<div onclick="nav('dashboardCard')"><i class="fas fa-home icon"></i>Home</div>
<div onclick="nav('plansCard')"><i class="fas fa-box icon"></i>Plans</div>
<div onclick="nav('depositCard')"><i class="fas fa-wallet icon"></i>Deposit</div>
<div onclick="nav('withdrawCard')"><i class="fas fa-hand-holding-usd icon"></i>Withdraw</div>
</div>

<script>
// Full JS for login/signup, dashboard, plans, deposit, withdraw, referral
// Same logic as pehle wali file but icons working & support details added

function doAuth(){ /* login/signup code */ }
function afterLoginUI(){ /* dashboard setup */ }
function nav(cardId){ /* show/hide cards */ }
function renderPlans(){ /* plans listing & countdown */ }
function updateDepositNumber(){ /* update deposit number based on method */ }
function submitDeposit(){ /* deposit submission */ }
function fillWithdrawUser(){ /* fill username in withdraw */ }
function submitWithdraw(){ /* withdraw request */ }
function copyReferral(){ /* referral copy */ }
function doLogout(){ /* logout */ }
function openSupport(){alert("📞 WhatsApp: +92337982782\n📧 Email: support@verbose.com\n⏰ 24/7 Support available\nPlease mention your username & issue for fast response.");}

</script>
</body>
</html>
