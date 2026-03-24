<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no"/>
<title>VERBOSE — Premium Asset Management</title>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;800&display=swap" rel="stylesheet">
<style>
  :root { --neon: #00f7ff; --accent: #7000ff; --dark: #020202; --glass: rgba(255,255,255,0.03); --border: rgba(255,255,255,0.08); }
  * { box-sizing: border-box; transition: 0.35s cubic-bezier(0.4, 0, 0.2, 1); font-family: 'Inter', sans-serif; }
  body { margin:0; background: var(--dark); color: #fff; overflow-x: hidden; -webkit-tap-highlight-color: transparent; }
  
  header { text-align: center; padding: 40px 20px 10px; font-size: 22px; font-weight: 800; letter-spacing: 12px; color: #fff; text-transform: uppercase; text-shadow: 0 0 20px rgba(0,247,255,0.4); }
  .page { max-width: 450px; margin: 0 auto 100px; padding: 20px; min-height: 80vh; animation: fadeIn 0.5s ease; }
  @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

  /* Premium Cards */
  .user-card { background: var(--glass); border: 1px solid var(--border); padding: 24px; border-radius: 30px; margin-bottom: 20px; backdrop-filter: blur(30px); position: relative; overflow: hidden; box-shadow: 0 15px 35px rgba(0,0,0,0.5); }
  .user-card::after { content: ""; position: absolute; top: -50%; left: -50%; width: 200%; height: 200%; background: radial-gradient(circle, rgba(0,247,255,0.05) 0%, transparent 70%); pointer-events: none; }
  
  input, select { width: 100%; padding: 18px; margin-top: 12px; border-radius: 16px; border: 1px solid var(--border); background: rgba(0,0,0,0.8); color: #fff; font-size: 14px; outline: none; }
  input:focus { border-color: var(--neon); box-shadow: 0 0 15px rgba(0,247,255,0.2); }

  button { width: 100%; padding: 18px; margin-top: 18px; border-radius: 18px; border: none; background: #fff; color: #000; font-weight: 800; cursor: pointer; text-transform: uppercase; font-size: 11px; letter-spacing: 2px; box-shadow: 0 5px 15px rgba(0,0,0,0.2); }
  button:active { transform: scale(0.97); }
  .btn-neon { background: linear-gradient(135deg, var(--neon), var(--accent)); color: #fff; }
  .btn-logout { background: rgba(255,70,70,0.1); color: #ff4646; border: 1px solid rgba(255,70,70,0.2); }

  /* Navigation */
  .nav { position: fixed; bottom: 0; left: 0; right: 0; background: rgba(0,0,0,0.95); display: flex; justify-content: space-around; padding: 15px 10px 35px; z-index: 1000; border-top: 1px solid var(--border); backdrop-filter: blur(20px); }
  .nav-item { text-align: center; font-size: 9px; cursor: pointer; opacity: 0.3; color: #fff; font-weight: 700; text-transform: uppercase; flex: 1; transition: 0.3s; }
  .nav-item.active { opacity: 1; color: var(--neon); text-shadow: 0 0 10px var(--neon); }

  .hidden { display: none !important; }
  .alert-box { background: rgba(255,70,70,0.05); border: 1px solid rgba(255,70,70,0.2); padding: 18px; border-radius: 22px; margin-bottom: 25px; border-left: 5px solid #ff4646; font-size: 10px; line-height: 1.6; }

  #secretAuth { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.98); z-index: 10001; display: flex; flex-direction: column; justify-content: center; align-items: center; padding: 30px; backdrop-filter: blur(10px); }
  .log-item { font-size: 9px; padding: 10px; border-bottom: 1px solid var(--border); opacity: 0.8; }
</style>
</head>
<body>

<header>VERBOSE</header>

<div id="loginPage" class="page">
  <div style="text-align:center; margin-bottom:45px;">
    <h3 id="adminTrigger" style="margin:0; font-weight:800; user-select:none; letter-spacing: 2px;">Secure Portal Access</h3>
    <p style="font-size:10px; opacity:0.4; letter-spacing:2px; margin-top:8px;">256-BIT END-TO-END ENCRYPTION</p>
  </div>
  <input id="u_name" placeholder="Client Username">
  <input id="u_pass" type="password" placeholder="Access Password">
  <button id="entryBtn" class="btn-neon">Authorize Session</button>
</div>

<div id="secretAuth" class="hidden">
  <h3 style="color:var(--neon); letter-spacing:6px; margin-bottom:25px;">CORE OVERRIDE</h3>
  <input id="adminKey" type="password" placeholder="Master Access Key" style="max-width:320px; text-align: center;">
  <button onclick="verifyAdmin()" class="btn-neon" style="max-width:320px;">Grant Access</button>
  <button onclick="document.getElementById('secretAuth').classList.add('hidden')" style="background:transparent; color:#fff; font-size:10px; margin-top:15px; border: 1px solid #333; width: 120px; padding: 10px;">Abort</button>
</div>

<div id="dashboard" class="page hidden">
  <div class="alert-box">
    <b style="color:#ff4646; letter-spacing: 1px;">SECURITY PROTOCOL:</b><br>
    Your account is currently under real-time protection. Do not disclose your TID or password to third-party individuals.
  </div>
  <div class="user-card">
    <div style="font-size:10px; opacity:0.5; text-transform:uppercase; letter-spacing: 1px;">Current Asset Value</div>
    <div style="font-size:40px; font-weight: 800; margin:12px 0; color: #fff;">PKR <span id="mainBal">0.00</span></div>
    <div style="font-size:9px; color:var(--neon); font-weight:800; letter-spacing: 1.5px;">SYSTEM STATUS: SECURE</div>
  </div>
  <button onclick="tab('plansPage')" class="btn-neon">Portfolio Marketplace</button>
  <button onclick="userLogout()" class="btn-logout" style="margin-top: 40px;">Terminate Session</button>
</div>

<div id="walletPage" class="page hidden">
  <h4 style="text-transform:uppercase; letter-spacing:2px;">Financial Desk</h4>
  <div class="user-card" style="border-left-color:var(--accent);">
    <b style="color:var(--neon); font-size: 11px;">Verified Channels:</b><br>
    <span style="font-size: 13px; opacity: 0.9;">JazzCash: 03705519562<br>EasyPaisa: 03379827882</span>
  </div>
  
  <input id="d_amount" type="number" placeholder="Funding Amount">
  <input id="d_tid" placeholder="Transaction Reference (TID)">
  <label style="font-size:10px; display:block; margin:15px 5px; opacity:0.5;">Upload Digital Receipt (Screenshot):</label>
  <input type="file" id="proofFile" accept="image/*" style="font-size:10px; border:1px dashed var(--border); padding:15px; background: rgba(255,255,255,0.02); border-radius: 12px;">
  <button onclick="handleDeposit()" class="btn-neon">Submit Audit Request</button>

  <hr style="opacity:0.1; margin:40px 0;">
  
  <h4 style="text-transform:uppercase; letter-spacing:2px;">Liquidation</h4>
  <p style="font-size:10px; opacity:0.5;">Minimum Requirement: <b>PKR 200.00</b></p>
  <input id="w_amount" type="number" placeholder="Enter Payout Amount">
  <button id="withdrawBtn" onclick="submitWithdraw()">Execute Liquidation</button>
</div>

<div id="adminPage" class="page hidden" style="border: 2px solid var(--accent); border-radius: 30px;">
  <h3 style="text-align:center; color:var(--neon); letter-spacing: 4px;">EXECUTIVE CONSOLE</h3>
  <div class="user-card" style="border-left-color: orange; padding: 15px;">
    <button onclick="toggleMaintenance()" style="background:orange; color:#000; padding: 10px; font-size: 10px;">Maintenance: ON/OFF</button>
    <button onclick="tab('dashboard')" style="background:#222; color:#fff; margin-top: 10px; font-size: 10px;">Return to User View</button>
  </div>

  <h5 style="margin:25px 0 10px; font-size:12px; letter-spacing: 1px;">Live Audit Queue:</h5>
  <div id="adminRequests"></div>

  <h5 style="margin:25px 0 10px; font-size:12px; letter-spacing: 1px;">Recent Audit Logs:</h5>
  <div id="adminLogs" class="user-card" style="max-height: 250px; overflow-y: auto; padding: 10px; font-family: monospace;"></div>
</div>

<div id="bottomNav" class="nav hidden">
  <div class="nav-item active" onclick="tab('dashboard', this)">Portal</div>
  <div class="nav-item" onclick="tab('plansPage', this)">Plans</div>
  <div class="nav-item" onclick="tab('partnershipPage', this)">Network</div>
  <div class="nav-item" onclick="tab('walletPage', this)">Wallet</div>
</div>

<script type="module">
  import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
  import { getDatabase, ref, set, get, update, onValue, remove, push } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js";

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

  // --- PERSISTENT SESSION ---
  window.onload = () => {
    const user = localStorage.getItem('v_user');
    if(user) sessionStart(user);
    checkMaintenance();
  };

  function sessionStart(u) {
    localStorage.setItem('v_user', u);
    document.getElementById('loginPage').classList.add('hidden');
    document.getElementById('dashboard').classList.remove('hidden');
    document.getElementById('bottomNav').classList.remove('hidden');
    
    onValue(ref(db, 'users/'+u), s => {
      if(s.exists()){
        const bal = parseFloat(s.val().balance);
        document.getElementById('mainBal').innerText = bal.toLocaleString(undefined, {minimumFractionDigits: 2});
        const wBtn = document.getElementById('withdrawBtn');
        wBtn.disabled = bal < 200;
        wBtn.style.opacity = bal < 200 ? 0.3 : 1;
        wBtn.innerText = bal < 200 ? "MIN 200 REQUIRED" : "EXECUTE LIQUIDATION";
      }
    });
  }

  window.userLogout = () => { if(confirm("Terminate secure session?")){ localStorage.clear(); location.reload(); } };

  // --- SECRET ADMIN TRIGGER ---
  let clicks = 0;
  document.getElementById('adminTrigger').onclick = () => {
    clicks++;
    if(clicks >= 7) { document.getElementById('secretAuth').classList.remove('hidden'); clicks = 0; }
    setTimeout(() => clicks = 0, 3000);
  };

  window.verifyAdmin = () => {
    if(document.getElementById('adminKey').value === MASTER_KEY) {
      document.getElementById('secretAuth').classList.add('hidden');
      tab('adminPage');
      loadAdminRequests();
      loadAuditLogs();
    } else alert("Authorization Failed.");
  };

  // --- AUDIT SYSTEM ---
  window.handleDeposit = async () => {
    const u = localStorage.getItem('v_user'), amt = document.getElementById('d_amount').value, tid = document.getElementById('d_tid').value, file = document.getElementById('proofFile').files[0];
    if(!amt || !tid || !file) return alert("All fields are mandatory.");
    const reader = new FileReader(); reader.readAsDataURL(file);
    reader.onload = async () => {
      await set(ref(db, 'requests/'+Date.now()), { user:u, amount:amt, tid:tid, proof:reader.result, type:'Deposit', status:'pending', time:new Date().toLocaleString() });
      alert("Funding audit request submitted.");
    };
  };

  // --- ADMIN CONSOLE LOGIC ---
  function loadAdminRequests() {
    onValue(ref(db, 'requests'), snap => {
      const cont = document.getElementById('adminRequests'); cont.innerHTML = "";
      if(!snap.exists()) return cont.innerHTML = "<p style='font-size:10px; opacity:0.5;'>Audit Queue Clear.</p>";
      Object.keys(snap.val()).forEach(id => {
        const r = snap.val()[id];
        cont.innerHTML += `<div class="user-card" style="font-size:10px; border-left-color: var(--neon);">
          <b>User: ${r.user} | PKR ${r.amount}</b><br>TID: ${r.tid || 'N/A'}<br>
          <img src="${r.proof}" style="width:100%; border-radius:15px; margin:10px 0; border: 1px solid #333;">
          <button onclick="approveAudit('${id}','${r.user}',${r.amount},'${r.type}')" style="background:#00ff88; padding:10px;">APPROVE</button>
        </div>`;
      });
    });
  }

  function loadAuditLogs() {
    onValue(ref(db, 'audit_logs'), snap => {
      const logCont = document.getElementById('adminLogs'); logCont.innerHTML = "";
      if(snap.exists()){
        const logs = Object.values(snap.val()).reverse();
        logs.forEach(l => {
          logCont.innerHTML += `<div class="log-item">[${l.time}] ${l.user}: ${l.type} of ${l.amount} APPROVED</div>`;
        });
      }
    });
  }

  window.approveAudit = async (id, u, amt, type) => {
    const s = await get(ref(db, 'users/'+u));
    let nb = type === 'Deposit' ? parseFloat(s.val().balance) + parseFloat(amt) : parseFloat(s.val().balance) - parseFloat(amt);
    await update(ref(db, 'users/'+u), { balance: nb });
    // Save to Logs
    await push(ref(db, 'audit_logs'), { user:u, amount:amt, type:type, time:new Date().toLocaleTimeString() });
    await remove(ref(db, 'requests/'+id));
    alert("Audit finalized.");
  };

  window.tab = (id, el) => {
    document.querySelectorAll('.page').forEach(p => p.classList.add('hidden'));
    document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
    document.getElementById(id).classList.remove('hidden');
    if(el) el.classList.add('active');
  };

  function checkMaintenance() {
    onValue(ref(db, 'system_config/maintenance'), s => {
      const isM = s.val() || false;
      if(isM && localStorage.getItem('v_user') !== 'admin') document.getElementById('loginPage').innerHTML = "<div style='text-align:center; padding:50px;'><h3>SYSTEM UNDER AUDIT</h3><p style='font-size:11px; opacity:0.6;'>Our nodes are being optimized. Access restored shortly.</p></div>";
    });
  }
</script>
</body>
</html>
