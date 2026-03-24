<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no"/>
<title>VERBOSE — Elite Ecosystem</title>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;800&display=swap" rel="stylesheet">
<style>
  :root { --neon: #00f7ff; --accent: #7000ff; --dark: #020202; --glass: rgba(255,255,255,0.03); --border: rgba(255,255,255,0.08); --success: #00ff88; }
  * { box-sizing: border-box; transition: 0.3s cubic-bezier(0.4, 0, 0.2, 1); font-family: 'Inter', sans-serif; }
  body { margin:0; background: var(--dark); color: #fff; overflow-x: hidden; }
  
  /* Live Ticker Style */
  .ticker-wrap { background: rgba(0,247,255,0.05); padding: 8px 0; overflow: hidden; border-bottom: 1px solid var(--border); }
  .ticker { display: flex; white-space: nowrap; animation: ticker 30s linear infinite; }
  .ticker-item { margin-right: 50px; font-size: 10px; color: var(--success); font-weight: 600; text-transform: uppercase; }
  @keyframes ticker { 0% { transform: translateX(100%); } 100% { transform: translateX(-100%); } }

  header { text-align: center; padding: 25px; font-size: 22px; font-weight: 800; letter-spacing: 10px; color: #fff; text-transform: uppercase; text-shadow: 0 0 15px var(--neon); cursor: pointer; }
  .page { max-width: 450px; margin: 0 auto 100px; padding: 20px; min-height: 85vh; animation: fadeIn 0.5s ease; }
  @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

  /* Universal Components */
  .user-card { background: var(--glass); border: 1px solid var(--border); padding: 25px; border-radius: 30px; margin-bottom: 20px; backdrop-filter: blur(30px); position: relative; border-left: 4px solid var(--neon); }
  .plan-img { width: 100%; height: 200px; border-radius: 22px; object-fit: cover; margin-bottom: 15px; border: 1px solid var(--border); }
  input, select { width: 100%; padding: 16px; margin-top: 12px; border-radius: 15px; border: 1px solid var(--border); background: rgba(0,0,0,0.8); color: #fff; outline: none; font-size: 13px; }
  button { width: 100%; padding: 18px; margin-top: 15px; border-radius: 18px; border: none; background: #fff; color: #000; font-weight: 800; cursor: pointer; text-transform: uppercase; font-size: 10px; letter-spacing: 1.5px; }
  .btn-neon { background: linear-gradient(135deg, var(--neon), var(--accent)); color: #fff; box-shadow: 0 10px 20px rgba(0,0,0,0.3); }

  /* Navigation */
  .nav { position: fixed; bottom: 0; left: 0; right: 0; background: rgba(0,0,0,0.95); display: flex; justify-content: space-around; padding: 15px 5px 35px; z-index: 1000; border-top: 1px solid var(--border); backdrop-filter: blur(15px); }
  .nav-item { text-align: center; font-size: 9px; cursor: pointer; opacity: 0.3; color: #fff; font-weight: 700; text-transform: uppercase; flex: 1; }
  .nav-item.active { opacity: 1; color: var(--neon); text-shadow: 0 0 10px var(--neon); }
  .hidden { display: none !important; }

  /* History Items */
  .history-item { display: flex; justify-content: space-between; padding: 12px; border-bottom: 1px solid var(--border); font-size: 11px; }
  .status-p { color: orange; } .status-s { color: var(--success); }

  /* Secret Overlay */
  #secretAuth { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.98); z-index: 10001; display: flex; flex-direction: column; justify-content: center; align-items: center; padding: 30px; }
</style>
</head>
<body>

<div class="ticker-wrap">
  <div class="ticker" id="liveTicker">
    <span class="ticker-item">User @Aman just earned PKR 3,200</span>
    <span class="ticker-item">New Asset "Phoenix Core" is now LIVE</span>
    <span class="ticker-item">Withdrawal of PKR 15,000 processed for @Nazim</span>
    <span class="ticker-item">Market Status: Highly Bullish</span>
  </div>
</div>

<header id="adminTrigger">VERBOSE</header>

<div id="noticeBar" style="background:rgba(112,0,255,0.1); padding:12px; font-size:10px; text-align:center; color:var(--neon); margin:10px 20px; border-radius:15px;" class="hidden">
  📢 <span id="noticeText"></span>
</div>

<div id="dashboard" class="page">
  <div class="user-card">
    <div style="font-size:9px; opacity:0.5; letter-spacing:2px;">SECURED EQUITY BALANCE</div>
    <div style="font-size:38px; font-weight:800; margin:5px 0;">PKR <span id="mainBal">0.00</span></div>
    <div style="font-size:9px; color:var(--success); font-weight:800;">● LIVE SYNC ACTIVE</div>
  </div>
  <div id="userPlansDisplay"></div>
</div>

<div id="walletPage" class="page hidden">
  <h4>FUNDING HUB</h4>
  <div class="user-card">
    <p id="jazzDisp" style="font-size:12px;">JazzCash: Loading...</p>
    <p id="easyDisp" style="font-size:12px;">EasyPaisa: Loading...</p>
  </div>
  <input id="d_amt" type="number" placeholder="Deposit Amount">
  <input id="d_tid" placeholder="Transaction ID">
  <button onclick="sendDeposit()" class="btn-neon">Verify Transaction</button>
  <h5 style="margin-top:25px; font-size:11px; opacity:0.5;">ACTIVITY HISTORY</h5>
  <div id="userHistory"></div>
</div>

<div id="adminPage" class="page hidden" style="border: 2px solid var(--accent); border-radius: 35px;">
  <h3 style="text-align:center; color:var(--neon);">CORE CONSOLE</h3>
  
  <div class="user-card" style="border-left-color: #ff00ff;">
    <b>SYSTEM UPDATES</b>
    <input id="admNotice" placeholder="Push Notice">
    <input id="admJazz" placeholder="Update JazzCash">
    <input id="admEasy" placeholder="Update EasyPaisa">
    <button onclick="updateSystem()" class="btn-neon" style="background:#ff00ff;">Push Changes</button>
  </div>

  <div class="user-card">
    <b>DEPLOY ASSET MACHINE</b>
    <input id="pName" placeholder="Machine Name">
    <input id="pPrice" type="number" placeholder="Price">
    <input id="pProfit" type="number" placeholder="Daily Profit">
    <input type="file" id="pImg" accept="image/*" style="font-size:10px; margin-top:10px;">
    <button onclick="deployPlan()" class="btn-neon">Verify & Launch</button>
  </div>

  <div id="adminPlans"></div>
  <h5 style="margin-top:20px;">PENDING REQUESTS:</h5>
  <div id="adminRequests"></div>
</div>

<div id="secretAuth" class="hidden">
  <h2 style="letter-spacing:10px;">OVERRIDE</h2>
  <input id="admKey" type="password" placeholder="System Key" style="max-width:300px; text-align:center;">
  <button onclick="checkAdmin()" class="btn-neon" style="max-width:300px;">Unlock Shell</button>
</div>

<div class="nav">
  <div class="nav-item active" onclick="tab('dashboard', this)">Vault</div>
  <div class="nav-item" onclick="tab('walletPage', this)">Wallet</div>
  <div id="navAdmin" class="nav-item" onclick="tab('adminPage', this)">Control</div>
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
  const user = localStorage.getItem('v_user') || "demo_user";

  // --- AUTO LOGOUT ---
  let timer = 0;
  document.onmousedown = () => timer = 0;
  setInterval(() => {
    timer++;
    if(timer >= 10) { localStorage.clear(); location.reload(); }
  }, 60000);

  // --- ADMIN SECRET ---
  let clicks = 0;
  document.getElementById('adminTrigger').onclick = () => {
    clicks++; if(clicks >= 7) { document.getElementById('secretAuth').classList.remove('hidden'); clicks = 0; }
  };
  window.checkAdmin = () => {
    if(document.getElementById('admKey').value === "NAZIM786") {
      document.getElementById('secretAuth').classList.add('hidden');
      tab('adminPage', document.getElementById('navAdmin'));
    } else alert("Invalid Key.");
  };

  // --- CORE LOGIC ---
  onValue(ref(db, 'system_config'), s => {
    if(s.exists()){
      const c = s.val();
      document.getElementById('jazzDisp').innerText = "JazzCash: " + c.gateways.jazz;
      document.getElementById('easyDisp').innerText = "EasyPaisa: " + c.gateways.easy;
      if(c.notice) {
        document.getElementById('noticeBar').classList.remove('hidden');
        document.getElementById('noticeText').innerText = c.notice;
      } else document.getElementById('noticeBar').classList.add('hidden');
    }
  });

  window.updateSystem = async () => {
    const n = document.getElementById('admNotice').value;
    const j = document.getElementById('admJazz').value;
    const e = document.getElementById('admEasy').value;
    await update(ref(db, 'system_config'), { notice: n });
    if(j && e) await update(ref(db, 'system_config/gateways'), { jazz:j, easy:e });
    alert("System Upgraded.");
  };

  window.deployPlan = async () => {
    const file = document.getElementById('pImg').files[0];
    const name = document.getElementById('pName').value;
    const price = document.getElementById('pPrice').value;
    const profit = document.getElementById('pProfit').value;
    if(!file || !name) return;
    const r = new FileReader(); r.readAsDataURL(file);
    r.onload = async () => {
      await set(ref(db, 'plans/' + Date.now()), { name, price, profit, img: r.result });
      alert("Asset Deployed!");
    };
  };

  onValue(ref(db, 'plans'), snap => {
    const uC = document.getElementById('userPlansDisplay');
    const aC = document.getElementById('adminPlans');
    uC.innerHTML = ""; aC.innerHTML = "";
    if(snap.exists()){
      Object.keys(snap.val()).forEach(id => {
        const p = snap.val()[id];
        uC.innerHTML += `<div class="user-card" style="border-left:none;"><img src="${p.img}" class="plan-img"><h4>${p.name}</h4><div style="font-size:24px; color:var(--neon); font-weight:800;">PKR ${p.price}</div><button class="btn-neon">Invest Now</button></div>`;
        aC.innerHTML += `<div class="user-card" style="padding:10px;">${p.name} <button onclick="removePlan('${id}')" style="background:red; color:#fff; width:auto; padding:5px; margin:0;">Del</button></div>`;
      });
    }
  });

  window.tab = (id, el) => {
    document.querySelectorAll('.page').forEach(p => p.classList.add('hidden'));
    document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
    document.getElementById(id).classList.remove('hidden');
    el.classList.add('active');
  };
</script>
</body>
</html>
