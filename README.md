<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no"/>
<title>VERBOSE — Trusted Asset Management</title>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;800&display=swap" rel="stylesheet">
<style>
  :root { --neon: #00f7ff; --accent: #7000ff; --dark: #020202; --glass: rgba(255,255,255,0.03); --border: rgba(255,255,255,0.08); --success: #00ff88; }
  * { box-sizing: border-box; transition: 0.3s cubic-bezier(0.4, 0, 0.2, 1); font-family: 'Inter', sans-serif; }
  body { margin:0; background: var(--dark); color: #fff; overflow-x: hidden; }
  
  header { text-align: center; padding: 35px 20px 10px; font-size: 24px; font-weight: 800; letter-spacing: 12px; color: #fff; text-transform: uppercase; text-shadow: 0 0 20px var(--neon); }
  .page { max-width: 450px; margin: 0 auto 100px; padding: 20px; min-height: 80vh; animation: fadeIn 0.6s ease; }
  @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

  /* Announcement Bar */
  #noticeBar { background: linear-gradient(90deg, transparent, rgba(0,247,255,0.1), transparent); border-top: 1px solid var(--border); border-bottom: 1px solid var(--border); padding: 10px; text-align: center; font-size: 10px; color: var(--neon); letter-spacing: 1px; margin-bottom: 20px; font-weight: 600; }

  /* Trust Cards */
  .user-card { background: var(--glass); border: 1px solid var(--border); padding: 25px; border-radius: 30px; margin-bottom: 20px; backdrop-filter: blur(30px); position: relative; border-left: 4px solid var(--neon); }
  .trust-badge { display: flex; align-items: center; gap: 5px; font-size: 9px; opacity: 0.5; margin-bottom: 15px; text-transform: uppercase; letter-spacing: 1px; }
  .trust-badge svg { color: var(--success); }

  /* Plan & Buttons */
  .plan-img { width: 100%; height: 200px; border-radius: 22px; object-fit: cover; margin-bottom: 15px; filter: contrast(1.1); }
  input { width: 100%; padding: 16px; margin-top: 12px; border-radius: 15px; border: 1px solid var(--border); background: rgba(0,0,0,0.8); color: #fff; outline: none; font-size: 13px; }
  button { width: 100%; padding: 18px; margin-top: 15px; border-radius: 18px; border: none; background: #fff; color: #000; font-weight: 800; cursor: pointer; text-transform: uppercase; font-size: 11px; letter-spacing: 2px; box-shadow: 0 10px 20px rgba(0,0,0,0.4); }
  .btn-neon { background: linear-gradient(135deg, var(--neon), var(--accent)); color: #fff; }

  /* Bottom Navigation */
  .nav { position: fixed; bottom: 0; left: 0; right: 0; background: rgba(0,0,0,0.96); display: flex; justify-content: space-around; padding: 15px 5px 35px; z-index: 1000; border-top: 1px solid var(--border); backdrop-filter: blur(20px); }
  .nav-item { text-align: center; font-size: 9px; cursor: pointer; opacity: 0.3; color: #fff; font-weight: 700; text-transform: uppercase; flex: 1; }
  .nav-item.active { opacity: 1; color: var(--neon); text-shadow: 0 0 10px var(--neon); }

  .hidden { display: none !important; }
</style>
</head>
<body>

<header>VERBOSE</header>

<div id="noticeBar" class="hidden">📢 SYSTEM NOTICE: <span id="noticeContent">Welcome to the Elite Asset Network.</span></div>

<div id="dashboard" class="page hidden">
  <div class="trust-badge">
    <svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3"><path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"></path><polyline points="22 4 12 14.01 9 11.01"></polyline></svg>
    SECURED BY 256-BIT QUANTUM SHIELD
  </div>

  <div class="user-card">
    <div style="font-size:10px; opacity:0.5; letter-spacing:1px; margin-bottom:5px;">CURRENT LIQUID ASSETS</div>
    <div style="font-size:38px; font-weight:800; margin:10px 0;">PKR <span id="mainBal">0.00</span></div>
    <div style="font-size:9px; color:var(--success); font-weight:800; display:flex; align-items:center; gap:5px;">
      <span style="width:6px; height:6px; background:var(--success); border-radius:50%; display:inline-block; animation:pulse 1s infinite;"></span>
      LIVE MARKET CONNECTION
    </div>
  </div>
  
  <h4 style="letter-spacing:3px; font-size:12px; margin: 30px 0 15px; font-weight:800;">ACTIVE CORE ASSETS</h4>
  <div id="userPlansDisplay"></div>
</div>

<div id="adminPage" class="page hidden" style="border: 1px solid var(--accent); border-radius: 35px;">
  <h3 style="text-align:center; color:var(--neon); letter-spacing:4px;">EXECUTIVE CONSOLE</h3>
  
  <div class="user-card" style="border-left: 4px solid #ff00ff;">
    <b style="font-size:10px;">GLOBAL NOTIFICATION CENTER</b>
    <input id="adminNotice" placeholder="Write message for all users...">
    <button onclick="postNotice()" class="btn-neon" style="background:linear-gradient(135deg, #ff00ff, var(--accent));">Push Announcement</button>
    <button onclick="clearNotice()" style="background:transparent; border:1px solid #333; color:#fff; font-size:9px;">Remove Notice</button>
  </div>

  <div class="user-card">
    <b style="font-size:10px;">LAUNCH NEW ASSET CORE</b>
    <input id="pName" placeholder="Asset Name">
    <input id="pPrice" type="number" placeholder="Entry Price (PKR)">
    <input id="pProfit" type="number" placeholder="Daily Yield (PKR)">
    <label style="font-size:9px; display:block; margin:15px 0 5px; opacity:0.5;">UPLOAD CORE IMAGE:</label>
    <input type="file" id="pImgFile" accept="image/*" style="font-size:10px; border:1px dashed #444; padding:10px;">
    <button onclick="createPlan()" class="btn-neon">Authorize & Deploy</button>
  </div>

  <div id="adminPlansList"></div>
</div>

<div id="bottomNav" class="nav hidden">
  <div class="nav-item active" onclick="tab('dashboard', this)">Asset</div>
  <div class="nav-item" onclick="tab('plansPage', this)">Plans</div>
  <div class="nav-item" onclick="tab('walletPage', this)">Wallet</div>
  <div class="nav-item" onclick="tab('adminPage', this)">Admin</div>
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

  window.onload = () => {
    const user = localStorage.getItem('v_user');
    if(user) sessionStart(user);
    syncGlobalNotice();
  };

  function sessionStart(u) {
    localStorage.setItem('v_user', u);
    document.getElementById('dashboard').classList.remove('hidden');
    document.getElementById('bottomNav').classList.remove('hidden');
    onValue(ref(db, 'users/'+u), s => {
      if(s.exists()) document.getElementById('mainBal').innerText = parseFloat(s.val().balance).toLocaleString();
    });
  }

  // --- ANNOUNCEMENT SYSTEM ---
  window.postNotice = async () => {
    const msg = document.getElementById('adminNotice').value;
    if(!msg) return;
    await set(ref(db, 'system_config/notice'), msg);
    alert("Announcement Pushed!");
  };

  window.clearNotice = async () => {
    await set(ref(db, 'system_config/notice'), "");
    alert("Notice Removed.");
  };

  function syncGlobalNotice() {
    onValue(ref(db, 'system_config/notice'), snap => {
      const bar = document.getElementById('noticeBar');
      if(snap.exists() && snap.val() !== "") {
        bar.classList.remove('hidden');
        document.getElementById('noticeContent').innerText = snap.val();
      } else {
        bar.classList.add('hidden');
      }
    });
  }

  // --- ASSET MANAGEMENT ---
  window.createPlan = async () => {
    const name = document.getElementById('pName').value, price = document.getElementById('pPrice').value, profit = document.getElementById('pProfit').value, file = document.getElementById('pImgFile').files[0];
    if(!name || !price || !profit || !file) return alert("All fields required.");
    const reader = new FileReader(); reader.readAsDataURL(file);
    reader.onload = async () => {
      await set(ref(db, 'plans/' + Date.now()), { name, price, profit, img: reader.result });
      alert("Asset Deployed!");
    };
  };

  onValue(ref(db, 'plans'), snap => {
    const userCont = document.getElementById('userPlansDisplay');
    userCont.innerHTML = "";
    if(snap.exists()){
      Object.values(snap.val()).forEach(p => {
        userCont.innerHTML += `
          <div class="user-card" style="border-left:none; padding:15px;">
            <img src="${p.img}" class="plan-img">
            <h4 style="margin:0; font-weight:800; letter-spacing:1px;">${p.name}</h4>
            <div style="font-size:24px; font-weight:800; color:var(--neon); margin:10px 0;">PKR ${parseFloat(p.price).toLocaleString()}</div>
            <div style="font-size:10px; opacity:0.6; margin-bottom:15px;">Yield: PKR ${p.profit} / Daily</div>
            <button onclick="invest(${p.price})" class="btn-neon" style="padding:15px; font-size:10px;">Activate Asset Core</button>
          </div>`;
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
