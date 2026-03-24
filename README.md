<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no"/>
<title>VERBOSE — Prime Solutions Professional</title>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;800&display=swap" rel="stylesheet">
<style>
  :root { --neon: #00f7ff; --accent: #7000ff; --dark: #020202; --glass: rgba(255,255,255,0.03); --card-border: rgba(255,255,255,0.08); }
  * { box-sizing: border-box; transition: 0.25s cubic-bezier(0.4, 0, 0.2, 1); font-family: 'Inter', sans-serif; }
  body { margin:0; background: var(--dark); color: #fff; overflow-x: hidden; -webkit-font-smoothing: antialiased; }
  
  /* Header Modernization */
  header { text-align: center; padding: 40px 20px 20px; font-size: 24px; font-weight: 800; letter-spacing: 8px; color: #fff; text-transform: uppercase; border-bottom: 1px solid var(--card-border); }
  
  .page { max-width: 450px; margin: 0 auto 100px; padding: 25px; position: relative; min-height: 85vh; }
  
  /* Modern Card Styling */
  .user-card { background: var(--glass); border: 1px solid var(--card-border); padding: 20px; border-radius: 24px; margin-bottom: 20px; backdrop-filter: blur(20px); box-shadow: 0 10px 30px rgba(0,0,0,0.5); }
  
  /* Form Inputs */
  input, select { width: 100%; padding: 16px; margin-top: 12px; border-radius: 12px; border: 1px solid var(--card-border); background: rgba(0,0,0,0.6); color: #fff; font-size: 14px; outline: none; transition: 0.3s; }
  input:focus { border-color: var(--neon); box-shadow: 0 0 15px rgba(0, 247, 255, 0.2); }
  
  /* Dynamic Buttons */
  button { width: 100%; padding: 16px; margin-top: 20px; border-radius: 14px; border: none; background: #fff; color: #000; font-weight: 700; cursor: pointer; text-transform: uppercase; font-size: 12px; letter-spacing: 1px; }
  button:active { transform: scale(0.96); }
  .btn-accent { background: linear-gradient(135deg, var(--neon), var(--accent)); color: #fff; }
  .btn-disabled { opacity: 0.3; cursor: not-allowed; filter: grayscale(1); }

  /* Navigation Bar */
  .nav { position: fixed; bottom: 0; left: 0; right: 0; background: rgba(5,5,5,0.95); display: flex; justify-content: space-around; padding: 15px 10px 25px; z-index: 1000; border-top: 1px solid var(--card-border); backdrop-filter: blur(10px); }
  .nav-item { text-align: center; font-size: 9px; cursor: pointer; opacity: 0.4; color: #fff; font-weight: 600; text-transform: uppercase; }
  .nav-item.active { opacity: 1; color: var(--neon); }

  /* Notification Alerts */
  .security-alert { background: rgba(255, 70, 70, 0.08); border: 1px solid rgba(255, 70, 70, 0.2); border-radius: 16px; padding: 15px; margin-bottom: 20px; }
  
  .hidden { display: none !important; }
  #maintenanceScreen { position:fixed; top:0; left:0; width:100%; height:100%; background:var(--dark); z-index:9999; display:flex; flex-direction:column; justify-content:center; align-items:center; text-align:center; padding:30px; }
</style>
</head>
<body>

<div id="maintenanceScreen" class="hidden">
  <div style="width:40px; height:40px; border:3px solid var(--card-border); border-top-color:var(--neon); border-radius:50%; animation:spin 1s linear infinite;"></div>
  <h2 style="letter-spacing:4px; margin-top:20px;">SYSTEM AUDIT</h2>
  <p style="font-size:12px; opacity:0.6; line-height:1.6;">Verbose is currently undergoing a security upgrade. Access will be restored shortly.</p>
</div>

<header id="mainHeader">VERBOSE</header>

<div id="loginPage" class="page">
  <div style="text-align:center; margin-bottom:30px;">
    <h3 style="margin:0; font-weight:800;">Secure Portal</h3>
    <p style="font-size:11px; opacity:0.5;">Enterprise Grade Encryption Active</p>
  </div>
  <input id="u_name" placeholder="Official Username">
  <input id="u_pass" type="password" placeholder="Access Password">
  <div style="margin-top:20px; font-size:11px; display:flex; gap:10px; align-items:flex-start; opacity:0.7;">
    <input type="checkbox" id="termsCheck" style="width:20px; margin:0;">
    <label for="termsCheck">I agree to the Professional Service Agreement and Risk Disclosure.</label>
  </div>
  <button id="entryBtn" class="btn-accent">Authenticate Access</button>
</div>

<div id="dashboard" class="page hidden">
  <div class="security-alert">
    <div style="display:flex; align-items:center; gap:8px; color:#ff4646; font-size:11px; font-weight:800;">
      <span>SECURITY PROTOCOL</span>
    </div>
    <p style="font-size:10px; margin-top:5px; opacity:0.7; line-height:1.5;">Do not share your credentials. Official support will never ask for your password. Report suspicious activity immediately.</p>
  </div>

  <div class="user-card">
    <div style="font-size:10px; opacity:0.5; text-transform:uppercase; letter-spacing:1px;">Available Liquidity</div>
    <div style="font-size:36px; font-weight:800; margin:8px 0;">PKR <span id="mainBal">0.00</span></div>
    <div id="accountStatus" style="font-size:9px; color:var(--neon); font-weight:800;">STATUS: ACTIVE</div>
  </div>
  <button onclick="tab('plansPage')" class="btn-accent">Portfolio Marketplace</button>
</div>

<div id="walletPage" class="page hidden">
  <h4 style="text-transform:uppercase; letter-spacing:1px;">Capital Management</h4>
  
  <div class="user-card" style="font-size:12px;">
    <b style="color:var(--neon)">Deposit Instructions:</b><br>
    JazzCash/SadaPay: 03705519562<br>
    EasyPaisa: 03379827882
  </div>

  <input id="d_amount" type="number" placeholder="Funding Amount (PKR)">
  <input id="d_tid" placeholder="Transaction Reference (TID)">
  <input type="file" id="proofFile" accept="image/*" style="font-size:11px; border:1px dashed var(--card-border);">
  <button onclick="handleDeposit()" class="btn-accent">Submit Funding Audit</button>

  <div style="margin:40px 0 20px; border-top:1px solid var(--card-border); padding-top:20px;">
    <h4 style="text-transform:uppercase; letter-spacing:1px;">Liquidation</h4>
    <p style="font-size:10px; opacity:0.5;">Minimum withdrawal requirement: <b>PKR 200.00</b></p>
    <input id="w_amount" type="number" placeholder="Withdrawal Amount">
    <button id="withdrawBtn" onclick="submitWithdraw()" style="background:#fff; color:#000;">Execute Payout</button>
  </div>
</div>

<div id="plansPage" class="page hidden"><div id="plansList"></div></div>
<div id="partnershipPage" class="page hidden">
  <h4 style="text-transform:uppercase;">Network Statistics</h4>
  <div id="referralList"></div>
  <button onclick="copyRefLink()" class="btn-accent" style="margin-top:20px;">Copy Invite Link</button>
</div>

<div id="bottomNav" class="nav hidden">
  <div class="nav-item active" onclick="tab('dashboard', this)">Home</div>
  <div class="nav-item" onclick="tab('plansPage', this)">Plans</div>
  <div class="nav-item" onclick="tab('partnershipPage', this)">Network</div>
  <div class="nav-item" onclick="tab('walletPage', this)">Wallet</div>
  <div class="nav-item" onclick="tab('adminPage', this)" id="adminNavItem">Control</div>
</div>

<script type="module">
  import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
  import { getDatabase, ref, set, get, update, onValue } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js";

  // Firebase Init (Configs hidden for security)
  const app = initializeApp(firebaseConfig);
  const db = getDatabase(app);

  // --- PERSISTENT SESSION SYSTEM ---
  window.onload = () => {
    const activeSession = localStorage.getItem('v_user');
    if (activeSession) sessionStart(activeSession);
  };

  function sessionStart(u) {
    localStorage.setItem('v_user', u);
    document.getElementById('loginPage').classList.add('hidden');
    document.getElementById('dashboard').classList.remove('hidden');
    document.getElementById('bottomNav').classList.remove('hidden');
    
    // Real-time Balance & Withdrawal Limit Check
    onValue(ref(db, 'users/' + u), snap => {
      if(snap.exists()){
        const bal = parseFloat(snap.val().balance);
        document.getElementById('mainBal').innerText = bal.toLocaleString(undefined, {minimumFractionDigits: 2});
        
        // Disable withdrawal if below 200
        const wBtn = document.getElementById('withdrawBtn');
        if(bal < 200) {
          wBtn.classList.add('btn-disabled');
          wBtn.disabled = true;
          wBtn.innerText = "MINIMUM 200 REQUIRED";
        } else {
          wBtn.classList.remove('btn-disabled');
          wBtn.disabled = false;
          wBtn.innerText = "EXECUTE PAYOUT";
        }
      }
    });
  }

  // --- WITHDRAWAL LOGIC ---
  window.submitWithdraw = async () => {
    const amt = parseFloat(document.getElementById('w_amount').value);
    const u = localStorage.getItem('v_user');
    const s = await get(ref(db, 'users/' + u));
    const bal = parseFloat(s.val().balance);

    if(amt < 200) return alert("Withdrawal amount must be at least PKR 200.");
    if(amt > bal) return alert("Insufficient balance for this transaction.");

    if(confirm(`Confirm withdrawal request of PKR ${amt}?`)) {
      const key = Date.now();
      await set(ref(db, 'requests/' + key), { user:u, amount:amt, type:'Withdraw', status:'pending', time:new Date().toLocaleString() });
      alert("Withdrawal request submitted for manual audit.");
    }
  };

  // --- NAVIGATION FIX ---
  window.tab = (id, el) => {
    document.querySelectorAll('.page').forEach(p => p.classList.add('hidden'));
    document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
    document.getElementById(id).classList.remove('hidden');
    if(el) el.classList.add('active');
  };

  // --- HELPERS ---
  window.copyRefLink = () => {
    const link = `${window.location.origin}${window.location.pathname}?ref=${localStorage.getItem('v_user')}`;
    navigator.clipboard.writeText(link);
    alert("Professional invitation link copied.");
  };

  @keyframes spin { to { transform: rotate(360deg); } }
</script>
</body>
</html>
