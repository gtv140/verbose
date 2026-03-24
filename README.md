<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no"/>
<title>VERBOSE — Quantum Terminal</title>
<link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;800&display=swap" rel="stylesheet">
<style>
  :root { --neon: #00f7ff; --accent: #7000ff; --bg: #000; --card: rgba(255,255,255,0.03); --border: rgba(255,255,255,0.08); --success: #00ff88; }
  * { box-sizing: border-box; transition: all 0.5s cubic-bezier(0.19, 1, 0.22, 1); font-family: 'Plus Jakarta Sans', sans-serif; -webkit-tap-highlight-color: transparent; }
  body { margin:0; background: var(--bg); color: #fff; overflow-x: hidden; height: 100vh; }

  /* Interactive Background */
  #canvas { position: fixed; top: 0; left: 0; z-index: -1; opacity: 0.3; }
  .glow-overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: radial-gradient(circle at 50% 0%, rgba(112,0,255,0.1), transparent 60%); z-index: -1; }

  header { text-align: center; padding: 60px 20px 10px; font-size: 28px; font-weight: 800; letter-spacing: 15px; color: #fff; text-transform: uppercase; text-shadow: 0 0 30px var(--neon); cursor: pointer; }
  
  /* Market Ticker */
  .ticker-wrap { background: rgba(0,247,255,0.05); border-bottom: 1px solid var(--border); padding: 8px 0; overflow: hidden; white-space: nowrap; }
  .ticker-move { display: inline-block; animation: ticker 25s linear infinite; font-size: 10px; font-weight: 800; letter-spacing: 2px; color: var(--neon); }
  @keyframes ticker { 0% { transform: translateX(100%); } 100% { transform: translateX(-100%); } }

  .page { max-width: 480px; margin: 0 auto; height: 80vh; overflow-y: auto; padding: 20px; padding-bottom: 150px; scrollbar-width: none; }
  .page::-webkit-scrollbar { display: none; }

  /* Premium Cards */
  .quantum-card { background: var(--card); border: 1px solid var(--border); padding: 30px; border-radius: 40px; backdrop-filter: blur(60px); margin-bottom: 25px; position: relative; box-shadow: 0 30px 60px rgba(0,0,0,0.8); }
  .balance-hide { filter: blur(12px); cursor: pointer; opacity: 0.5; }

  /* Admin Terminal Style */
  .terminal-box { background: #000; border: 1px solid var(--success); border-radius: 20px; padding: 15px; font-family: 'Courier New', monospace; font-size: 11px; color: var(--success); margin-bottom: 20px; line-height: 1.6; }

  input { width: 100%; padding: 20px; margin-top: 15px; border-radius: 25px; border: 1px solid var(--border); background: rgba(255,255,255,0.04); color: #fff; outline: none; font-size: 14px; }
  button { width: 100%; padding: 22px; margin-top: 20px; border-radius: 25px; border: none; font-weight: 800; text-transform: uppercase; font-size: 11px; cursor: pointer; letter-spacing: 3px; }
  .btn-quantum { background: linear-gradient(135deg, var(--neon), var(--accent)); color: #fff; box-shadow: 0 15px 40px rgba(112,0,255,0.4); }

  /* Bottom Navigation */
  .nav { position: fixed; bottom: 30px; left: 25px; right: 25px; background: rgba(10,10,10,0.92); display: flex; justify-content: space-around; padding: 22px; border-radius: 40px; border: 1px solid var(--border); backdrop-filter: blur(30px); z-index: 1000; box-shadow: 0 -10px 40px rgba(0,0,0,0.5); }
  .nav-item { font-size: 9px; opacity: 0.3; color: #fff; font-weight: 800; text-transform: uppercase; letter-spacing: 1px; }
  .nav-item.active { opacity: 1; color: var(--neon); text-shadow: 0 0 15px var(--neon); transform: translateY(-3px); }

  .hidden { display: none !important; }
</style>
</head>
<body>

<canvas id="canvas"></canvas>
<div class="glow-overlay"></div>

<div class="ticker-wrap">
    <div class="ticker-move">
        BTC: $67,432.10 (+2.4%) • GOLD: $2,156.40 (-0.15%) • ETH: $3,402.12 (+1.8%) • USDT: 280.45 PKR • SOL: $145.20 (+5.2%) • BTC: $67,432.10 (+2.4%)
    </div>
</div>

<header id="adminTrigger">VERBOSE</header>

<div id="dashboard" class="page">
  <div class="quantum-card">
    <div style="display:flex; justify-content:space-between; align-items:center;">
        <span id="welcomeMsg" style="font-size:10px; opacity:0.6; letter-spacing:2px;">AUTHORIZING...</span>
        <span style="font-size:8px; color:var(--success); border:1px solid var(--success); padding:2px 6px; border-radius:10px;">SECURE</span>
    </div>
    <div style="font-size:48px; font-weight:800; margin:20px 0; letter-spacing:-2px;" onclick="toggleBal()">
        PKR <span id="mainBal">0.00</span>
    </div>
    <div style="display:flex; gap:10px; align-items:center;">
        <div style="width:100%; height:2px; background:var(--border); border-radius:2px; overflow:hidden;">
            <div style="width:75%; height:100%; background:var(--neon); box-shadow:0 0 10px var(--neon);"></div>
        </div>
        <span style="font-size:9px; opacity:0.5;">DENSITY</span>
    </div>
  </div>

  <h4 style="letter-spacing:5px; font-size:11px; margin: 10px 15px 20px; opacity:0.8;">ELITE QUANTUM ASSETS</h4>
  <div id="userPlansDisplay"></div>
</div>

<div id="adminPage" class="page hidden">
  <div class="terminal-box">
    > ROOT_ACCESS: GRANTED<br>
    > BYPASS_STATUS: ACTIVE<br>
    > MASTER_NODE_ONLINE_24/7
  </div>

  <div class="quantum-card" style="border-color: var(--accent);">
    <b style="font-size:10px; color:var(--neon);">QUANTUM DATA INJECTION</b>
    <input id="targetUser" placeholder="Client Identification">
    <input id="editBal" type="number" placeholder="Override Capital Value">
    <button onclick="overrideUser()" class="btn-quantum">Execute Override</button>
  </div>
</div>

<div id="secretAuth" class="hidden" style="position:fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.99); z-index:10001; display:flex; flex-direction:column; justify-content:center; align-items:center; backdrop-filter:blur(30px);">
  <div style="width:60px; height:60px; border:3px solid var(--neon); border-radius:50%; border-top-color:transparent; animation: spin 1s linear infinite; margin-bottom:30px;"></div>
  <h2 style="letter-spacing:20px; color:var(--neon);">AUTH_KEY</h2>
  <input id="admKey" type="password" placeholder="System Code" style="max-width:320px; text-align:center;">
  <button onclick="unlockAdmin()" class="btn-quantum" style="max-width:320px;">Authorize</button>
</div>

<div class="nav">
  <div class="nav-item active" onclick="tab('dashboard', this)">Portfolio</div>
  <div class="nav-item" onclick="tab('walletPage', this)">Vault</div>
  <div id="navAdmin" class="nav-item" onclick="tab('adminPage', this)">Admin</div>
</div>

<script type="module">
  import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
  import { getDatabase, ref, set, get, update, onValue } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js";

  const firebaseConfig = {
    apiKey: "AIzaSyBe5Q5jXpx3UvrHC9WOky9UWeDnP9SPfZI",
    authDomain: "verbose-6c008.firebaseapp.com",
    projectId: "verbose-6c008",
    databaseURL: "https://verbose-6c008-default-rtdb.firebaseio.com",
    appId: "1:867100945312:web:315dfb48fb34496cee12c5"
  };
  const app = initializeApp(firebaseConfig);
  const db = getDatabase(app);

  const uName = localStorage.getItem('v_user') || "Operator";

  // --- VOICE & UI GREETING ---
  window.addEventListener('load', () => {
    document.getElementById('welcomeMsg').innerText = "SYSTEM_READY: " + uName.toUpperCase();
    const voice = new SpeechSynthesisUtterance(`Welcome back ${uName}, your assets are fully optimized.`);
    voice.pitch = 0.8; voice.rate = 1; window.speechSynthesis.speak(voice);
  });

  window.toggleBal = () => document.getElementById('mainBal').classList.toggle('balance-hide');

  // --- ADMIN SYSTEM ---
  let clicks = 0;
  document.getElementById('adminTrigger').onclick = () => {
    clicks++; if(clicks >= 7) { document.getElementById('secretAuth').classList.remove('hidden'); clicks = 0; }
  };
  window.unlockAdmin = () => {
    if(document.getElementById('admKey').value === "NAZIM786") {
      document.getElementById('secretAuth').classList.add('hidden');
      tab('adminPage', document.getElementById('navAdmin'));
    } else alert("Biometric Identity Failed.");
  };

  window.overrideUser = async () => {
    const u = document.getElementById('targetUser').value.trim().toLowerCase();
    const b = document.getElementById('editBal').value;
    if(!u || !b) return;
    await update(ref(db, 'users/' + u), { balance: parseFloat(b) });
    alert("Central Node Overwritten.");
  };

  window.tab = (id, el) => {
    document.querySelectorAll('.page').forEach(p => p.classList.add('hidden'));
    document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
    document.getElementById(id).classList.remove('hidden');
    el.classList.add('active');
  };

  // --- PARTICLE SYSTEM ---
  const canvas = document.getElementById('canvas');
  const ctx = canvas.getContext('2d');
  canvas.width = window.innerWidth; canvas.height = window.innerHeight;
  let pts = [];
  for(let i=0; i<40; i++) pts.push({x:Math.random()*canvas.width, y:Math.random()*canvas.height, s:Math.random()*1.5, vx:Math.random()-0.5, vy:Math.random()-0.5});
  function anim() {
    ctx.clearRect(0,0,canvas.width,canvas.height); ctx.fillStyle = '#00f7ff';
    pts.forEach(p => {
        p.x+=p.vx; p.y+=p.vy;
        if(p.x<0 || p.x>canvas.width) p.vx*=-1; if(p.y<0 || p.y>canvas.height) p.vy*=-1;
        ctx.beginPath(); ctx.arc(p.x, p.y, p.s, 0, 7); ctx.fill();
    });
    requestAnimationFrame(anim);
  }
  anim();
</script>
<style>@keyframes spin { to { transform: rotate(360deg); } }</style>
</body>
</html>
