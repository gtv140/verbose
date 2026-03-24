<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no"/>
<title>VERBOSE — Absolute Enterprise</title>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;800&display=swap" rel="stylesheet">
<style>
  :root { --neon: #00f7ff; --accent: #7000ff; --dark: #020202; --glass: rgba(255,255,255,0.03); --border: rgba(255,255,255,0.08); --success: #00ff88; }
  * { box-sizing: border-box; transition: 0.3s cubic-bezier(0.4, 0, 0.2, 1); font-family: 'Inter', sans-serif; }
  body { margin:0; background: var(--dark); color: #fff; overflow-x: hidden; }
  
  /* Live Ticker */
  .ticker-wrap { background: rgba(0,247,255,0.05); padding: 8px 0; overflow: hidden; border-bottom: 1px solid var(--border); }
  .ticker { display: flex; white-space: nowrap; animation: ticker 25s linear infinite; }
  .ticker-item { margin-right: 50px; font-size: 10px; color: var(--success); font-weight: 600; text-transform: uppercase; }
  @keyframes ticker { 0% { transform: translateX(100%); } 100% { transform: translateX(-100%); } }

  header { text-align: center; padding: 25px 20px 5px; font-size: 22px; font-weight: 800; letter-spacing: 10px; color: #fff; text-transform: uppercase; text-shadow: 0 0 15px var(--neon); cursor: pointer; }
  .page { max-width: 450px; margin: 0 auto 100px; padding: 20px; min-height: 85vh; animation: fadeIn 0.5s ease; }
  @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

  #noticeBar { background: linear-gradient(90deg, transparent, rgba(112,0,255,0.1), transparent); border: 1px solid var(--border); padding: 12px; text-align: center; font-size: 10px; color: var(--neon); margin: 10px 20px; border-radius: 15px; }

  .user-card { background: var(--glass); border: 1px solid var(--border); padding: 25px; border-radius: 30px; margin-bottom: 20px; backdrop-filter: blur(30px); position: relative; border-left: 4px solid var(--neon); }
  .plan-img { width: 100%; height: 200px; border-radius: 22px; object-fit: cover; margin-bottom: 15px; border: 1px solid var(--border); }
  
  input, select { width: 100%; padding: 16px; margin-top: 12px; border-radius: 15px; border: 1px solid var(--border); background: rgba(0,0,0,0.8); color: #fff; outline: none; font-size: 13px; }
  button { width: 100%; padding: 18px; margin-top: 15px; border-radius: 18px; border: none; background: #fff; color: #000; font-weight: 800; cursor: pointer; text-transform: uppercase; font-size: 10px; letter-spacing: 2px; }
  .btn-neon { background: linear-gradient(135deg, var(--neon), var(--accent)); color: #fff; box-shadow: 0 10px 20px rgba(0,0,0,0.3); }

  .nav { position: fixed; bottom: 0; left: 0; right: 0; background: rgba(0,0,0,0.95); display: flex; justify-content: space-around; padding: 15px 5px 35px; z-index: 1000; border-top: 1px solid var(--border); backdrop-filter: blur(15px); }
  .nav-item { text-align: center; font-size: 9px; cursor: pointer; opacity: 0.3; color: #fff; font-weight: 700; text-transform: uppercase; flex: 1; }
  .nav-item.active { opacity: 1; color: var(--neon); }
  .hidden { display: none !important; }

  /* Admin Secret Login */
  #adminSecretOverlay { position: fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.98); z-index:10001; display:flex; flex-direction:column; justify-content:center; align-items:center; padding:30px; }
</style>
</head>
<body>

<div class="ticker-wrap">
  <div class="ticker" id="liveTicker">
    <span class="ticker-item">User @Ali just earned PKR 5,400</span>
    <span class="ticker-item">User @Sana withdrew PKR 12,000</span>
    <span class="ticker-item">New Core Asset "Titan" Deployed</span>
    <span class="ticker-item">User @Khan upgraded to VIP Tier</span>
  </div>
</div>

<header id="masterHeader">VERBOSE</header>

<div id="noticeBar" class="hidden">📢 <span id="noticeContent">Loading official notice...</span></div>

<div id="loginPage" class="page">
  <div style="text-align:center; margin:40px 0;">
    <h2 style="letter-spacing:5px;">AUTHENTICATE</h2>
    <p style="font-size:9px; opacity:0.4;">SECURE ACCESS PROTOCOL</p>
  </div>
  <input id="u_name" placeholder="Client Username">
  <input id="u_pass" type="password" placeholder="Access Password">
  <button onclick="handleLogin()" class="btn-neon">Enter Vault</button>
</div>

<div id="dashboard" class="page hidden">
  <div class="user-card">
    <div style="font-size:9px; opacity:0.5; letter-spacing:2px; margin-bottom:10px;">🛡️ ENCRYPTED ASSET BALANCE</div>
    <div style="font-size:38px; font-weight:800; margin:5px 0;">PKR <span id="mainBal">0.00</span></div>
    <div style="font-size:9px; color:var(--success); font-weight:800; display:flex; align-items:center; gap:5px;">
      <span style="width:6px; height:6px; background:var(--success); border-radius:50%; animation:pulse 1s infinite;"></span>
      LIVE CONNECTION: VERIFIED
    </div>
  </div>
  <div id="userPlansDisplay"></div>
</div>

<div id="walletPage" class="page hidden">
  <h4 style="letter-spacing:2px; font-size:12px;">FUNDING & WITHDRAWAL</h4>
  <div class="user-card" id="gatewayInfo">
    <p id="jazzDisp" style="font-size:12px; margin:0;">JazzCash: Loading...</p>
    <p id="easyDisp" style="font-size:12px; margin:5px 0 0;">EasyPaisa: Loading...</p>
  </div>
  <input id="d_amt" type="number" placeholder="Amount">
  <input id="d_tid" placeholder="Transaction ID">
  <button onclick="submitDeposit()" class="btn-neon">Submit Audit Request</button>
  <div id="userHistory" style="margin-top:20px;"></div>
</div>

<div id="adminPage" class="page hidden" style="border: 2px solid var(--accent); border-radius: 35px;">
  <h3 style="text-align:center; color:var(--neon); letter-spacing:4px;">EXECUTIVE CONSOLE</h3>
  
  <div class="user-card" style="border-left-color:#ff00ff;">
    <b>ANNOUNCEMENT & GATEWAYS</b>
    <input id="adminNotice" placeholder="Push Global Notice">
    <input id="newJazz" placeholder="Update JazzCash">
    <input id="newEasy" placeholder="Update EasyPaisa">
    <button onclick="updateSystem()" class="btn-neon" style="background:#ff00ff; color:#fff;">Update System</button>
  </div>

  <div class="user-card">
    <b>DEPLOY NEW ASSET CORE</b>
    <input id="pName" placeholder="Asset Name">
    <input id="pPrice" type="number" placeholder="Price">
    <input id="pProfit" type="number" placeholder="Daily Yield">
    <input type="file" id="pImgFile" accept="image/*" style="font-size:10px; margin-top:10px;">
    <button onclick="createPlan()" class="btn-neon">Authorize Deployment</button>
  </div>

  <div id="adminPlansList"></div>
  <h5 style="margin-top:20px;">PENDING AUDITS:</h5>
  <div id="adminRequests"></div>
</div>

<div id="adminSecretOverlay" class="hidden">
  <h2 style="color:var(--neon); letter-spacing:8px;">OVERRIDE</h2>
  <input id="adminKey" type="password" placeholder="Enter System Key" style="max-width:300px; text-align:center;">
  <button onclick="unlockAdmin()" class="btn-neon" style="max-width:300px;">Verify Identity</button>
</div>

<div id="bottomNav" class="nav hidden">
  <div class="nav-item active" onclick="tab('dashboard', this)">Vault</div>
  <div class="nav-item" onclick="tab('walletPage', this)">Wallet</div>
  <div class="nav-item" onclick="tab('adminPage', this)" id="navAdmin">Control</div>
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

  // --- AUTO LOGOUT (10 MIN) ---
  let idleTime = 0;
  document.onmousedown = () => idleTime = 0;
  setInterval(() => {
    idleTime++;
    if(idleTime >= 10 && localStorage.getItem('v_user')) { 
      alert("Session Terminated for Security."); 
      localStorage.clear(); location.reload(); 
    }
  }, 60000);

  // --- LOGIN LOGIC ---
  window.handleLogin = async () => {
    const u = document.getElementById('u_name').value.trim().toLowerCase();
    const p = document.getElementById('u_pass').value;
    if(!u || !p) return;
    const s = await get(ref(db, 'users/'+u));
    if(s.exists() && s.val().password === p) sessionStart(u);
    else alert("Invalid Credentials.");
  };

  function sessionStart(u) {
    localStorage.setItem('v_user', u);
    document.getElementById('loginPage').classList.add('hidden');
    document.getElementById('dashboard').classList.remove('hidden');
    document.getElementById('bottomNav').classList.remove('hidden');
    
    onValue(ref(db, 'users/'+u), s => {
      if(s.exists()) document.getElementById('mainBal').innerText = parseFloat(s.val().balance).toLocaleString();
    });
    loadPlans();
    syncSystem();
  }

  // --- ADMIN SECRET (7 CLICKS) ---
  let headerClicks = 0;
  document.getElementById('masterHeader').onclick = () => {
    headerClicks++;
    if(headerClicks >= 7) { document.getElementById('adminSecretOverlay').classList.remove('hidden'); headerClicks = 0; }
  };
  window.unlockAdmin = () => {
    if(document.getElementById('adminKey').value === "NAZIM786") {
      document.getElementById('adminSecretOverlay').classList.add('hidden');
      tab('adminPage', document.getElementById('navAdmin'));
    } else alert("Access Denied.");
  };

  // --- SYSTEM SYNC ---
  function syncSystem() {
    onValue(ref(db, 'system_config'), snap => {
      if(snap.exists()){
        const c = snap.val();
        if(c.notice) { 
          document.getElementById('noticeBar').classList.remove('hidden');
          document.getElementById('noticeContent').innerText = c.notice;
        } else document.getElementById('noticeBar').classList.add('hidden');
        
        document.getElementById('jazzDisp').innerText = "JazzCash: " + c.gateways.jazz;
        document.getElementById('easyDisp').innerText = "EasyPaisa: " + c.gateways.easy;
      }
    });
  }

  window.updateSystem = async () => {
    const n = document.getElementById('adminNotice').value;
    const j = document.getElementById('newJazz').value;
    const e = document.getElementById('newEasy').value;
    await update(ref(db, 'system_config'), { notice: n });
    if(j && e) await update(ref(db, 'system_config/gateways'), { jazz:j, easy:e });
    alert("System Overhauled!");
  };

  // --- PLAN DEPLOYMENT ---
  window.createPlan = async () => {
    const name = document.getElementById('pName').value, price = document.getElementById('pPrice').value, profit = document.getElementById('pProfit').value, file = document.getElementById('pImgFile').files[0];
    if(!name || !file) return;
    const reader = new FileReader(); reader.readAsDataURL(file);
    reader.onload = async () => {
      await set(ref(db, 'plans/' + Date.now()), { name, price, profit, img: reader.result });
      alert("Asset Deployed Live!");
    };
  };

  function loadPlans() {
    onValue(ref(db, 'plans'), snap => {
      const uCont = document.getElementById('userPlansDisplay');
      const aCont = document.getElementById('adminPlansList');
      uCont.innerHTML = ""; aCont.innerHTML = "";
      if(snap.exists()){
        Object.keys(snap.val()).forEach(id => {
          const p = snap.val()[id];
          uCont.innerHTML += `
            <div class="user-card" style="border-left:none; padding:15px;">
              <img src="${p.img}" class="plan-img">
              <h4 style="margin:0;">${p.name}</h4>
              <div style="font-size:24px; color:var(--neon); margin:10px 0; font-weight:800;">PKR ${parseFloat(p.price).toLocaleString()}</div>
              <button class="btn-neon" style="padding:15px;">Activate Asset</button>
            </div>`;
          aCont.innerHTML += `<div class="user-card" style="padding:10px; font-size:10px;">${p.name} <button onclick="deletePlan('${id}')" style="background:red; color:#fff; padding:5px; border-radius:5px;">Del</button></div>`;
        });
      }
    });
  }

  window.tab = (id, el) => {
    document.querySelectorAll('.page').forEach(p => p.classList.add('hidden'));
    document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
    document.getElementById(id).classList.remove('hidden');
    if(el) el.classList.add('active');
  };
</script>
</body>
</html>
