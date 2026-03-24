<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>VERBOSE — Prime Solutions Professional</title>
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;800&display=swap" rel="stylesheet">
<style>
  :root { --neon: #00f7ff; --accent: #ff5cff; --dark: #050505; --glass: rgba(255,255,255,0.05); --text: #fff; }
  .light-mode { --dark: #f0f2f5; --glass: rgba(0,0,0,0.08); --text: #222; }
  * { box-sizing: border-box; transition: 0.3s; font-family: 'Poppins', sans-serif; -webkit-tap-highlight-color: transparent; }
  body { margin:0; background: var(--dark); color: var(--text); overflow-x: hidden; }
  header { text-align: center; padding: 25px; font-size: 26px; font-weight: 800; background: linear-gradient(90deg, var(--neon), var(--accent)); -webkit-background-clip: text; -webkit-text-fill-color: transparent; letter-spacing: 5px; cursor: pointer; text-transform: uppercase; }
  .page { max-width: 420px; margin: 10px auto 100px; background: var(--glass); padding: 25px; border-radius: 20px; border: 1px solid rgba(255,255,255,0.05); backdrop-filter: blur(15px); min-height: 520px; position: relative; }
  .user-card { background: rgba(255,255,255,0.03); padding: 18px; border-radius: 15px; margin-bottom: 15px; border-left: 4px solid var(--neon); position: relative; }
  input, select { width: 100%; padding: 14px; margin-top: 10px; border-radius: 8px; border: 1px solid rgba(255,255,255,0.1); background: rgba(0,0,0,0.4); color: #fff; font-size: 13px; outline: none; }
  button { width: 100%; padding: 15px; margin-top: 15px; border-radius: 8px; border: none; background: linear-gradient(90deg, var(--neon), var(--accent)); color: #000; font-weight: 700; cursor: pointer; text-transform: uppercase; font-size: 12px; }
  .nav { position: fixed; bottom: 0; left: 0; right: 0; background: rgba(5,5,7,0.98); display: flex; justify-content: space-around; padding: 12px; z-index: 1000; border-top: 1px solid rgba(255,255,255,0.05); }
  .nav-item { text-align: center; font-size: 9px; cursor: pointer; opacity: 0.4; color: #fff; flex:1; text-transform: uppercase; font-weight: 600; }
  .nav-item.active { opacity: 1; color: var(--neon); }
  .status-tag { font-size: 9px; padding: 2px 8px; border-radius: 4px; float: right; font-weight: bold; text-transform: uppercase; }
  .approved { background: #00ff88; color: #000; } .pending { background: #ffa500; color: #000; } .rejected { background: #ff4444; color: #fff; }
  .hidden { display: none !important; }
  #maintenanceScreen { position:fixed; top:0; left:0; width:100%; height:100%; background:#050505; z-index:9999; display:flex; flex-direction:column; justify-content:center; align-items:center; text-align:center; padding:20px; }
  .progress-bar { height: 4px; background: var(--neon); width: 0%; border-radius: 10px; margin-top: 10px; }
</style>
</head>
<body>

<div id="maintenanceScreen" class="hidden">
  <h2 style="color:var(--neon); letter-spacing:3px;">MAINTENANCE</h2>
  <p style="font-size:12px; opacity:0.7;">System optimization in progress. Please check back later.</p>
</div>

<header id="mainHeader">VERBOSE</header>

<div id="loginPage" class="page">
  <h4 style="text-align:center;">Secure Portal Login</h4>
  <input id="u_name" placeholder="Username">
  <input id="u_pass" type="password" placeholder="Password">
  <div style="font-size:10px; margin-top:15px; display:flex; align-items:center; gap:8px; opacity:0.6;">
    <input type="checkbox" id="termsCheck" style="width:auto; margin:0;"> Accept Corporate Terms
  </div>
  <button id="entryBtn">Authenticate</button>
</div>

<div id="dashboard" class="page hidden">
  <div class="user-card">
    <div style="opacity:0.6; font-size:10px; text-transform: uppercase;">Capital Balance</div>
    <div style="font-size:32px; font-weight:800; margin:5px 0;">PKR <span id="mainBal">0.00</span></div>
  </div>
  <button onclick="tab('plansPage')">Invest in Portfolios</button>
</div>

<div id="plansPage" class="page hidden">
  <h4 style="margin:0; text-transform:uppercase;">Yield Portfolios</h4>
  <div id="plansList" style="margin-top:15px;"></div>
</div>

<div id="partnershipPage" class="page hidden">
  <h4 style="text-transform:uppercase;">Partnership Network</h4>
  <div class="user-card" style="border-left-color:var(--accent);">
    <div style="font-size:9px; opacity:0.6;">REFERRAL LINK</div>
    <button onclick="copyRefLink()" style="padding:10px; font-size:10px;">Copy Invitation Link</button>
  </div>
  <div id="referralList"></div>
</div>

<div id="walletPage" class="page hidden">
  <div class="user-card" style="font-size:11px;">
    <b>Deposit Accounts:</b><br>
    JazzCash: 03705519562 | EasyPaisa: 03379827882
  </div>
  <input id="d_amount" type="number" placeholder="Deposit Amount">
  <input id="d_tid" placeholder="Transaction ID">
  <label style="font-size:10px; margin-top:10px; display:block;">Attach Screenshot Proof:</label>
  <input type="file" id="proofFile" accept="image/*" style="font-size:10px;">
  <button onclick="handleDeposit()">Submit for Audit</button>
  <hr style="opacity:0.1; margin:20px 0;">
  <input id="w_amount" type="number" placeholder="Withdrawal Amount">
  <button onclick="submitWithdraw()" style="background:var(--accent)">Request Payout</button>
</div>

<div id="adminPage" class="page hidden" style="border:2px solid var(--accent);">
  <div id="adminLock">
    <input id="adminSecretKey" type="password" placeholder="System Key">
    <button onclick="unlockAdmin()">Authorize Admin</button>
  </div>
  <div id="adminContent" class="hidden">
    <button onclick="toggleMaintenance()" id="maintBtn" style="background:orange;">Maintenance: OFF</button>
    <button onclick="distributeGlobalProfit()" style="background:var(--neon);">Global Profit Payout</button>
    <div id="adminRequests"></div>
  </div>
</div>

<div id="bottomNav" class="nav hidden">
  <div class="nav-item active" onclick="tab('dashboard', this)">Home</div>
  <div class="nav-item" onclick="tab('plansPage', this)">Plans</div>
  <div class="nav-item" onclick="tab('partnershipPage', this)">Partners</div>
  <div class="nav-item" onclick="tab('walletPage', this)">Wallet</div>
  <div class="nav-item" onclick="tab('adminPage', this)" id="adminNavItem">Admin</div>
</div>

<script type="module">
  import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
  import { getDatabase, ref, set, get, update, onValue, remove } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js";

  const firebaseConfig = {
    apiKey: "AIzaSyBe5Q5jXpx3UvrHC9WOky9UWeDnP9SPfZI",
    authDomain: "verbose-6c008.firebaseapp.com",
    projectId: "verbose-6c008",
    databaseURL: "https://verbose-6c008-default-rtdb.firebaseio.com",
    appId: "1:867100945312:web:315dfb48fb34496cee12c5"
  };
  const app = initializeApp(firebaseConfig);
  const db = getDatabase(app);

  const MASTER_KEY = "NAZIM786";

  // AUTH & REGISTRATION
  document.getElementById('entryBtn').onclick = async () => {
    const u = document.getElementById('u_name').value.trim().toLowerCase();
    const p = document.getElementById('u_pass').value.trim();
    if(!u || !p || !document.getElementById('termsCheck').checked) return alert("Fill all fields and accept terms.");
    
    const urlParams = new URLSearchParams(window.location.search);
    const referredBy = urlParams.get('ref') || "none";

    const userRef = ref(db, 'users/' + u);
    const snap = await get(userRef);
    if(snap.exists()){
      if(snap.val().password === p) sessionStart(u); else alert("Invalid Credentials.");
    } else {
      await set(userRef, { username:u, password:p, balance:0, referredBy: referredBy });
      if(referredBy !== "none") await set(ref(db, 'referrals/'+referredBy+'/'+u), { status: 'Inactive', joined: new Date().toLocaleDateString() });
      sessionStart(u);
    }
  };

  function sessionStart(u) {
    localStorage.setItem('v_user', u);
    document.getElementById('loginPage').classList.add('hidden');
    document.getElementById('dashboard').classList.remove('hidden');
    document.getElementById('bottomNav').classList.remove('hidden');
    onValue(ref(db, 'users/'+u), s => { document.getElementById('mainBal').innerText = parseFloat(s.val().balance).toLocaleString(); });
    checkMaintenance();
    renderPlans();
  }

  // DEPOSIT WITH IMAGE UPLOAD
  window.handleDeposit = async () => {
    const u = localStorage.getItem('v_user');
    const amt = document.getElementById('d_amount').value;
    const tid = document.getElementById('d_tid').value;
    const file = document.getElementById('proofFile').files[0];
    if(!amt || !tid || !file) return alert("All fields are mandatory.");

    const reader = new FileReader();
    reader.readAsDataURL(file);
    reader.onload = async () => {
      const key = Date.now();
      await set(ref(db, 'requests/'+key), { user:u, amount:amt, tid:tid, proof:reader.result, type:'Deposit', status:'pending', time:new Date().toLocaleString() });
      alert("Deposit transmitted for manual audit.");
    };
  };

  // ADMIN POWERS
  window.unlockAdmin = () => {
    if(document.getElementById('adminSecretKey').value === MASTER_KEY) {
      document.getElementById('adminLock').classList.add('hidden');
      document.getElementById('adminContent').classList.remove('hidden');
      loadAdminRequests();
    } else { alert("Unauthorized."); }
  };

  function loadAdminRequests() {
    onValue(ref(db, 'requests'), snap => {
      const cont = document.getElementById('adminRequests'); cont.innerHTML = "";
      if(!snap.exists()) return cont.innerHTML = "<p>No pending audits.</p>";
      Object.keys(snap.val()).forEach(id => {
        const r = snap.val()[id];
        cont.innerHTML += `
          <div class="user-card" style="font-size:11px;">
            <b>User:</b> ${r.user} | <b>PKR ${r.amount}</b><br>
            <b>TID:</b> ${r.tid}<br>
            <img src="${r.proof}" style="width:100%; border-radius:10px; margin:10px 0;">
            <button onclick="approveAudit('${id}','${r.user}',${r.amount},'${r.type}')" style="background:#00ff88; padding:8px;">Approve</button>
            <button onclick="rejectAudit('${id}')" style="background:#ff4444; color:#fff; padding:8px;">Reject</button>
          </div>`;
      });
    });
  }

  window.approveAudit = async (id, u, amt, type) => {
    const s = await get(ref(db, 'users/'+u));
    let nb = (type === 'Deposit') ? parseFloat(s.val().balance) + parseFloat(amt) : parseFloat(s.val().balance) - parseFloat(amt);
    await update(ref(db, 'users/'+u), { balance: nb });
    
    // Referral Commission (10%)
    if(type === 'Deposit' && s.val().referredBy !== 'none') {
      const r = s.val().referredBy;
      const rSnap = await get(ref(db, 'users/'+r));
      if(rSnap.exists()){
        await update(ref(db, 'users/'+r), { balance: parseFloat(rSnap.val().balance) + (amt*0.1) });
        await update(ref(db, 'referrals/'+r+'/'+u), { status: 'Active' });
      }
    }
    await remove(ref(db, 'requests/'+id));
    alert("Audit approved.");
  };

  // MAINTENANCE SYSTEM
  window.toggleMaintenance = async () => {
    const s = await get(ref(db, 'system_config/maintenance'));
    const status = s.exists() ? s.val() : false;
    await set(ref(db, 'system_config/maintenance'), !status);
  };

  function checkMaintenance() {
    onValue(ref(db, 'system_config/maintenance'), snap => {
      const isM = snap.val() || false;
      if(isM) document.getElementById('maintenanceScreen').classList.remove('hidden');
      else document.getElementById('maintenanceScreen').classList.add('hidden');
    });
  }

  window.tab = (id, el) => {
    document.querySelectorAll('.page').forEach(p => p.classList.add('hidden'));
    document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
    document.getElementById(id).classList.remove('hidden');
    if(el) el.classList.add('active');
  };

  window.copyRefLink = () => {
    const link = window.location.origin + window.location.pathname + "?ref=" + localStorage.getItem('v_user');
    navigator.clipboard.writeText(link);
    alert("Link Copied.");
  };
</script>
</body>
</html>
