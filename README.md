<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no"/>
<title>VERBOSE — Prime Solutions</title>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;800&display=swap" rel="stylesheet">
<style>
  :root { --neon: #00f7ff; --accent: #7000ff; --dark: #020202; --glass: rgba(255,255,255,0.03); --border: rgba(255,255,255,0.08); }
  * { box-sizing: border-box; transition: 0.3s cubic-bezier(0.4, 0, 0.2, 1); font-family: 'Inter', sans-serif; }
  body { margin:0; background: var(--dark); color: #fff; overflow-x: hidden; }
  
  header { text-align: center; padding: 40px 20px 20px; font-size: 24px; font-weight: 800; letter-spacing: 10px; color: #fff; text-transform: uppercase; text-shadow: 0 0 15px var(--neon); }
  .page { max-width: 450px; margin: 0 auto 100px; padding: 20px; min-height: 80vh; }
  
  /* Modern UI Elements */
  .user-card { background: var(--glass); border: 1px solid var(--border); padding: 22px; border-radius: 28px; margin-bottom: 20px; backdrop-filter: blur(25px); border-left: 4px solid var(--neon); position: relative; }
  input { width: 100%; padding: 16px; margin-top: 12px; border-radius: 14px; border: 1px solid var(--border); background: rgba(0,0,0,0.7); color: #fff; font-size: 14px; outline: none; }
  button { width: 100%; padding: 16px; margin-top: 18px; border-radius: 16px; border: none; background: #fff; color: #000; font-weight: 800; cursor: pointer; text-transform: uppercase; font-size: 11px; letter-spacing: 1.5px; }
  .btn-neon { background: linear-gradient(135deg, var(--neon), var(--accent)); color: #fff; }
  .btn-danger { background: #ff4646 !important; color: #fff !important; }

  /* Navigation */
  .nav { position: fixed; bottom: 0; left: 0; right: 0; background: rgba(0,0,0,0.95); display: flex; justify-content: space-around; padding: 15px 5px 30px; z-index: 1000; border-top: 1px solid var(--border); backdrop-filter: blur(15px); }
  .nav-item { text-align: center; font-size: 9px; cursor: pointer; opacity: 0.4; color: #fff; font-weight: 700; text-transform: uppercase; }
  .nav-item.active { opacity: 1; color: var(--neon); }

  .hidden { display: none !important; }
  .alert-box { background: rgba(255,70,70,0.05); border: 1px solid rgba(255,70,70,0.2); padding: 15px; border-radius: 18px; margin-bottom: 20px; border-left: 4px solid #ff4646; }

  /* Secret Portal UI */
  #secretAuth { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.98); z-index: 10001; display: flex; flex-direction: column; justify-content: center; align-items: center; padding: 30px; }
</style>
</head>
<body>

<header>VERBOSE</header>

<div id="loginPage" class="page">
  <div style="text-align:center; margin-bottom:40px;">
    <h3 id="adminTrigger" style="margin:0; font-weight:800; user-select:none;">Secure Portal Access</h3>
    <p style="font-size:10px; opacity:0.4; letter-spacing:1px; margin-top:5px;">ENTERPRISE GRADE SECURITY</p>
  </div>
  <input id="u_name" placeholder="Official Username">
  <input id="u_pass" type="password" placeholder="Access Password">
  <div style="margin-top:20px; font-size:10px; display:flex; gap:10px; opacity:0.6;">
    <input type="checkbox" id="termsCheck" style="width:18px; margin:0;">
    <label for="termsCheck">I agree to the Digital Asset Policy and Risk Agreement.</label>
  </div>
  <button id="entryBtn" class="btn-neon">Authenticate</button>
</div>

<div id="secretAuth" class="hidden">
  <h2 style="color:var(--neon); letter-spacing:5px;">ADMIN OVERRIDE</h2>
  <input id="adminKey" type="password" placeholder="System Key ID" style="max-width:300px;">
  <button onclick="verifyAdmin()" class="btn-neon" style="max-width:300px;">Unlock Console</button>
  <button onclick="document.getElementById('secretAuth').classList.add('hidden')" style="background:transparent; color:#fff; font-size:10px;">Exit Secure Shell</button>
</div>

<div id="dashboard" class="page hidden">
  <div class="alert-box">
    <b style="font-size:10px; color:#ff4646;">SECURITY WARNING:</b>
    <p style="font-size:9px; margin-top:5px; opacity:0.7; line-height:1.5;">Personal verification data is strictly confidential. Never share your password with anyone representing as administration.</p>
  </div>
  <div class="user-card">
    <div style="font-size:10px; opacity:0.5; text-transform:uppercase;">Current Liquidity</div>
    <div style="font-size:36px; font-weight:800; margin:10px 0;">PKR <span id="mainBal">0.00</span></div>
    <div style="font-size:9px; color:var(--neon); font-weight:800;">ACCOUNT STATUS: VERIFIED</div>
  </div>
  <button onclick="tab('plansPage')" class="btn-neon">View Portfolio Plans</button>
</div>

<div id="walletPage" class="page hidden">
  <h4 style="text-transform:uppercase;">Capital Funding</h4>
  <div class="user-card" style="font-size:12px;">
    JazzCash: 03705519562<br>EasyPaisa: 03379827882
  </div>
  <input id="d_amount" type="number" placeholder="Funding Amount">
  <input id="d_tid" placeholder="Transaction ID (TID)">
  <label style="font-size:10px; margin-top:15px; display:block; opacity:0.6;">Upload Direct Screenshot Proof:</label>
  <input type="file" id="proofFile" accept="image/*" style="font-size:10px; border:1px dashed var(--border);">
  <button onclick="handleDeposit()" class="btn-neon">Submit Audit</button>

  <hr style="opacity:0.1; margin:30px 0;">
  <h4 style="text-transform:uppercase;">Liquidation (Min: 200)</h4>
  <input id="w_amount" type="number" placeholder="Withdrawal Amount">
  <button id="withdrawBtn" onclick="submitWithdraw()">Request Payout</button>
</div>

<div id="adminPage" class="page hidden" style="border: 2px solid var(--accent);">
  <h3 style="text-align:center; color:var(--neon);">EXECUTIVE PANEL</h3>
  <div class="user-card" style="border-left-color:orange;">
    <button onclick="toggleMaintenance()" id="maintBtn" style="background:orange; color:#000;">Maintenance: ON/OFF</button>
    <button onclick="distributeGlobalProfit()" class="btn-neon">Execute Daily Profits</button>
  </div>
  
  <h5 style="margin-top:20px; font-size:12px;">Member Inspection:</h5>
  <input id="modUser" placeholder="Username">
  <button onclick="inspectUser()" style="padding:10px; font-size:10px;">Inspect & Ban</button>
  <div id="modPanel" class="hidden user-card" style="margin-top:10px;">
    <input id="modNewBal" type="number" placeholder="Set Balance">
    <button onclick="applyMod('bal')" style="padding:10px; font-size:10px;">Update Balance</button>
    <button onclick="toggleBan()" id="banBtn" class="btn-danger">Ban Account</button>
  </div>

  <h5 style="margin-top:20px; font-size:12px;">Financial Audit Queue:</h5>
  <div id="adminRequests"></div>
</div>

<div id="bottomNav" class="nav hidden">
  <div class="nav-item active" onclick="tab('dashboard', this)">Home</div>
  <div class="nav-item" onclick="tab('plansPage', this)">Plans</div>
  <div class="nav-item" onclick="tab('partnershipPage', this)">Partners</div>
  <div class="nav-item" onclick="tab('walletPage', this)">Wallet</div>
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

  // --- SECRET ADMIN TRIGGER ---
  let clicks = 0;
  document.getElementById('adminTrigger').onclick = () => {
    clicks++;
    if(clicks >= 7) { 
      document.getElementById('secretAuth').classList.remove('hidden'); 
      clicks = 0; 
    }
    setTimeout(() => clicks = 0, 3000);
  };

  window.verifyAdmin = () => {
    if(document.getElementById('adminKey').value === MASTER_KEY) {
      document.getElementById('secretAuth').classList.add('hidden');
      tab('adminPage');
      loadAdminRequests();
    } else { alert("Unauthorized."); }
  };

  // --- PERSISTENT SESSION ---
  window.onload = () => {
    const user = localStorage.getItem('v_user');
    if(user) sessionStart(user);
  };

  function sessionStart(u) {
    localStorage.setItem('v_user', u);
    document.getElementById('loginPage').classList.add('hidden');
    document.getElementById('dashboard').classList.remove('hidden');
    document.getElementById('bottomNav').classList.remove('hidden');
    onValue(ref(db, 'users/'+u), s => {
      if(s.exists()){
        const bal = parseFloat(s.val().balance);
        document.getElementById('mainBal').innerText = bal.toLocaleString();
        const wBtn = document.getElementById('withdrawBtn');
        if(bal < 200) { wBtn.disabled = true; wBtn.style.opacity = 0.3; wBtn.innerText = "MIN 200 REQUIRED"; }
        else { wBtn.disabled = false; wBtn.style.opacity = 1; wBtn.innerText = "REQUEST PAYOUT"; }
      }
    });
  }

  // --- DEPOSIT WITH DIRECT IMAGE ---
  window.handleDeposit = async () => {
    const u = localStorage.getItem('v_user');
    const amt = document.getElementById('d_amount').value;
    const tid = document.getElementById('d_tid').value;
    const file = document.getElementById('proofFile').files[0];
    if(!amt || !tid || !file) return alert("Fill all fields.");

    const reader = new FileReader();
    reader.readAsDataURL(file);
    reader.onload = async () => {
      const key = Date.now();
      await set(ref(db, 'requests/'+key), { user:u, amount:amt, tid:tid, proof:reader.result, type:'Deposit', status:'pending', time:new Date().toLocaleString() });
      alert("Funding request sent for manual audit.");
    };
  };

  // --- ADMIN AUDIT VIEW ---
  function loadAdminRequests() {
    onValue(ref(db, 'requests'), snap => {
      const cont = document.getElementById('adminRequests'); cont.innerHTML = "";
      if(!snap.exists()) return cont.innerHTML = "<p style='font-size:10px; opacity:0.5;'>Queue Clear.</p>";
      Object.keys(snap.val()).forEach(id => {
        const r = snap.val()[id];
        cont.innerHTML += `
          <div class="user-card" style="font-size:10px;">
            <b>User: ${r.user} | PKR ${r.amount}</b><br>TID: ${r.tid || 'N/A'}<br>
            <img src="${r.proof}" style="width:100%; border-radius:10px; margin:10px 0;">
            <button onclick="approveAudit('${id}','${r.user}',${r.amount},'${r.type}')" style="background:#00ff88; padding:8px;">APPROVE</button>
            <button onclick="rejectAudit('${id}')" style="background:#ff4646; color:#fff; padding:8px;">REJECT</button>
          </div>`;
      });
    });
  }

  window.approveAudit = async (id, u, amt, type) => {
    const s = await get(ref(db, 'users/'+u));
    let nb = (type === 'Deposit') ? parseFloat(s.val().balance) + parseFloat(amt) : parseFloat(s.val().balance) - parseFloat(amt);
    await update(ref(db, 'users/'+u), { balance: nb });
    await remove(ref(db, 'requests/'+id));
    alert("Approved Successfully.");
  };

  window.tab = (id, el) => {
    document.querySelectorAll('.page').forEach(p => p.classList.add('hidden'));
    document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
    document.getElementById(id).classList.remove('hidden');
    if(el) el.classList.add('active');
  };
</script>
</body>
</html>
