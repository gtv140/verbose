<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no"/>
<title>VERBOSE — Quantum Infrastructure</title>
<link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;800&display=swap" rel="stylesheet">
<style>
  :root { --neon: #00f7ff; --accent: #7000ff; --bg: #030303; --card: rgba(255,255,255,0.07); --border: rgba(255,255,255,0.1); --success: #00ff88; --error: #ff4646; --gold: #ffcc00; }
  * { box-sizing: border-box; transition: 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275); font-family: 'Plus Jakarta Sans', sans-serif; outline: none; }
  body { margin:0; background: var(--bg); color: #fff; overflow-x: hidden; -webkit-tap-highlight-color: transparent; }

  /* BACKGROUND DECOR */
  .orb { position: fixed; width: 300px; height: 300px; background: radial-gradient(circle, rgba(112,0,255,0.15) 0%, transparent 70%); z-index: -1; border-radius: 50%; }
  .orb-1 { top: -100px; right: -50px; }
  .orb-2 { bottom: -100px; left: -50px; background: radial-gradient(circle, rgba(0,247,255,0.1) 0%, transparent 70%); }

  header { display: flex; align-items: center; padding: 65px 25px 20px; position: sticky; top: 0; background: rgba(3,3,3,0.8); backdrop-filter: blur(10px); z-index: 1000; justify-content: space-between; border-bottom: 1px solid var(--border); }
  .logo-text { font-size: 20px; font-weight: 800; letter-spacing: 5px; color: #fff; text-shadow: 0 0 15px var(--neon); }
  .menu-btn { font-size: 24px; color: var(--neon); cursor: pointer; }

  .page { max-width: 480px; margin: 0 auto; padding: 25px 20px 150px; display: none; }
  .active { display: block !important; animation: slideUp 0.5s ease; }
  @keyframes slideUp { from { opacity: 0; transform: translateY(30px); } to { opacity: 1; transform: translateY(0); } }

  /* DASHBOARD CARD */
  .wallet-card { background: linear-gradient(145deg, rgba(20,20,20,0.9), rgba(5,5,5,0.9)); border: 1px solid var(--border); border-radius: 40px; padding: 40px 30px; position: relative; overflow: hidden; box-shadow: 0 20px 50px rgba(0,0,0,0.5); margin-bottom: 30px; }
  .wallet-card::after { content: ''; position: absolute; top: -50%; left: -50%; width: 200%; height: 200%; background: radial-gradient(circle, rgba(255,255,255,0.03) 0%, transparent 50%); pointer-events: none; }
  .balance-label { font-size: 11px; text-transform: uppercase; letter-spacing: 3px; opacity: 0.5; font-weight: 600; }
  .balance-val { font-size: 48px; font-weight: 800; margin: 10px 0; background: linear-gradient(to right, #fff, var(--neon)); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }

  .node-card { background: var(--card); border: 1px solid var(--border); border-radius: 35px; padding: 25px; margin-bottom: 20px; backdrop-filter: blur(20px); }
  .node-card h3 { margin: 0 0 10px; font-size: 18px; letter-spacing: 1px; }

  /* TEXT CONTENT STYLING */
  .details-box { line-height: 1.7; font-size: 13px; color: rgba(255,255,255,0.7); }
  .details-box b { color: var(--neon); display: block; margin-top: 15px; }

  input { width: 100%; padding: 20px; margin-top: 12px; border-radius: 22px; border: 1px solid var(--border); background: rgba(255,255,255,0.03); color: #fff; font-size: 15px; }
  .btn-gold { width: 100%; padding: 20px; border-radius: 25px; border: none; font-weight: 800; cursor: pointer; background: linear-gradient(135deg, var(--neon), var(--accent)); color: #fff; text-transform: uppercase; letter-spacing: 2px; box-shadow: 0 10px 20px rgba(0,247,255,0.2); }

  .nav-bar { position: fixed; bottom: 30px; left: 20px; right: 20px; background: rgba(15,15,15,0.9); backdrop-filter: blur(30px); border-radius: 50px; display: flex; justify-content: space-around; padding: 20px; border: 1px solid var(--border); z-index: 1000; }
  .nav-link { font-size: 10px; font-weight: 800; opacity: 0.4; color: #fff; text-transform: uppercase; cursor: pointer; }
  .nav-link.active { opacity: 1; color: var(--neon); }

  #authScreen { position: fixed; inset: 0; background: var(--bg); z-index: 10000; display: flex; align-items: center; justify-content: center; padding: 20px; }
</style>
</head>
<body>

<div class="orb orb-1"></div>
<div class="orb orb-2"></div>

<div id="authScreen">
    <div class="node-card" style="width:100%; max-width:400px; text-align:center;">
        <h2 style="letter-spacing:5px;">IDENTITY</h2>
        <input id="aName" placeholder="Full Name">
        <input id="aPhone" type="number" placeholder="Phone Number">
        <input id="aPass" type="password" placeholder="Access PIN">
        <button class="btn-gold" style="margin-top:20px;" onclick="handleAuth()">INITIALIZE</button>
    </div>
</div>

<header>
    <div class="menu-btn" onclick="tab('detailsPage')">☰</div>
    <div class="logo-text" id="adminTrigger">VERBOSE</div>
    <div style="width:24px;"></div>
</header>

<div id="dash" class="page active">
    <div class="wallet-card">
        <div class="balance-label">Total Liquid Assets</div>
        <div class="balance-val">PKR <span id="uBal">0.00</span></div>
        <div style="font-size:10px; opacity:0.4; letter-spacing:2px;" id="uNameDisplay">AGENT: ---</div>
    </div>
    
    <div class="node-card" style="border-left: 4px solid var(--success);">
        <h3 style="color:var(--success);">ACTIVE STATUS</h3>
        <p id="statusMsg" style="font-size:12px; opacity:0.7;">No active nodes found in your cluster.</p>
        <div id="timer" style="font-size:24px; font-weight:800; margin-top:10px; display:none;">24:00:00</div>
    </div>
</div>

<div id="detailsPage" class="page">
    <h2 style="color:var(--neon);">QUANTUM PROTOCOL</h2>
    
    <div class="node-card details-box">
        <b>ABOUT VERBOSE</b>
        Verbose Infrastructure is a decentralized cloud mining conglomerate based in Rawalpindi, specializing in high-frequency yield generation. Our mission is to bridge the gap between retail investors and enterprise-grade hardware clusters.
        
        <b>HOW IT WORKS</b>
        By renting a node, you contribute to our hashing power. In return, the system distributes rewards every 24 hours directly to your liquid wallet. All transactions are manually verified to ensure 100% security against sybil attacks.
    </div>

    <div class="node-card details-box">
        <b>PRIVACY POLICY</b>
        1. <b>Data Collection:</b> We only store your phone number for identity and transaction tracking. No third-party sharing.
        2. <b>Asset Protection:</b> All deposits are held in cold storage until node activation.
        3. <b>Zero-Logs:</b> We do not track your IP or location. Your financial privacy is our priority.
        
        <b>TERMS OF SERVICE</b>
        Withdrawals take 1-24 hours for manual audit. One account per user. Violation of protocols results in permanent account suspension.
    </div>
    
    <button class="btn-gold" style="background:var(--error);" onclick="logout()">TERMINATE SESSION</button>
</div>

<div id="nodesPage" class="page">
    <h2 style="text-align:center; letter-spacing:3px;">AVAILABLE CLUSTERS</h2>
    <div id="nodeGrid"></div>
</div>

<div id="financePage" class="page">
    <div class="node-card">
        <h3>INJECT CAPITAL</h3>
        <p style="font-size:11px; opacity:0.6;">JazzCash/EasyPaisa: 03705519562</p>
        <input id="dAmt" type="number" placeholder="Amount">
        <input id="dTID" placeholder="TID (Transaction ID)">
        <button class="btn-gold" onclick="sendReq('Deposit')">Confirm Deposit</button>
    </div>
</div>

<div id="adminPage" class="page">
    <h2 style="color:var(--gold); text-align:center;">GOD MODE HUD</h2>
    <div class="node-card" id="admReqs"></div>
</div>

<nav class="nav-bar">
    <div class="nav-link active" onclick="tab('dash', this)">Home</div>
    <div class="nav-link" onclick="tab('nodesPage', this)">Nodes</div>
    <div class="nav-link" onclick="tab('financePage', this)">Finance</div>
    <div class="nav-link" onclick="tab('detailsPage', this)">Details</div>
</nav>

<script type="module">
  import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
  import { getDatabase, ref, update, onValue, get, set, push, remove } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js";

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

  // --- AUTH ---
  window.handleAuth = async () => {
    const n = document.getElementById('aName').value, p = document.getElementById('aPhone').value, pin = document.getElementById('aPass').value;
    const s = await get(ref(db, `users/${p}`));
    if(s.exists()) {
        if(s.val().pass === pin) { localStorage.setItem('v_user', p); location.reload(); }
        else alert("ACCESS DENIED: WRONG PIN");
    } else {
        if(!n || !p || !pin) return alert("COMPLETE ALL FIELDS");
        await set(ref(db, `users/${p}`), { name:n, balance:0, pass:pin, hasActive:false });
        localStorage.setItem('v_user', p); location.reload();
    }
  };

  // --- UI ENGINE ---
  window.tab = (id, el) => {
    document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
    document.getElementById(id).classList.add('active');
    if(el) { document.querySelectorAll('.nav-link').forEach(n => n.classList.remove('active')); el.classList.add('active'); }
  };

  const nodes = [{n:"Titan Cluster",p:1000,d:125},{n:"Aura Cluster",p:2500,d:320},{n:"Zenith Cluster",p:5000,d:700}];
  const renderNodes = (active) => {
    const g = document.getElementById('nodeGrid'); g.innerHTML = "";
    nodes.forEach(x => {
        g.innerHTML += `<div class="node-card"><b>${x.n}</b><br><small>ROI: PKR ${x.d}/Day</small><br>
        <button class="btn-gold" ${active?'disabled':''} style="margin-top:15px; background:var(--card); border:1px solid var(--neon);" onclick="buyNode(${x.p},${x.d},'${x.n}')">${active?'ACTIVE':'INITIALIZE '+x.p}</button></div>`;
    });
  };

  window.sendReq = (type) => {
    const amt = document.getElementById('dAmt').value, info = document.getElementById('dTID').value;
    const id = push(ref(db, `admin/requests`)).key;
    const data = { user, type, amt, info, status: "Pending", time: new Date().toLocaleString() };
    update(ref(db, `admin/requests/${id}`), data);
    update(ref(db, `users/${user}/history/${id}`), data);
    alert("TRANSMISSION SUCCESSFUL: AWAITING APPROVAL");
  };

  // --- ADMIN ---
  let clks = 0; document.getElementById('adminTrigger').onclick = () => { if(++clks >= 7) { if(prompt("HUD KEY?")==="nazim786") { tab('adminPage'); loadAdmin(); } clks=0; } };
  const loadAdmin = () => {
    onValue(ref(db, `admin/requests`), s => {
        const d = document.getElementById('admReqs'); d.innerHTML = "";
        if(s.exists()) Object.entries(s.val()).forEach(([id, r]) => {
            d.innerHTML += `<div class="node-card" style="border-color:var(--gold); font-size:11px;">
                <b>${r.type}</b> | User: ${r.user} | PKR ${r.amt}<br>TID: ${r.info}
                <button onclick="approveGod('${id}','${r.user}',${r.amt},'${r.type}')" style="background:var(--gold); border:none; padding:5px 10px; margin-top:10px; border-radius:5px; font-weight:800;">APPROVE</button>
            </div>`;
        });
    });
  };

  window.approveGod = async (id, target, amt, type) => {
    const s = await get(ref(db, `users/${target}`));
    let b = (s.val().balance || 0);
    if(type === 'Deposit') b += Number(amt);
    await update(ref(db, `users/${target}`), { balance: b });
    await update(ref(db, `users/${target}/history/${id}`), { status: "Success" });
    await remove(ref(db, `admin/requests/${id}`));
    alert("LEDGER UPDATED");
  };

  window.logout = () => { localStorage.clear(); location.reload(); };

  if(user) {
    document.getElementById('authScreen').style.display = "none";
    onValue(ref(db, `users/${user}`), s => {
        const d = s.val(); document.getElementById('uBal').innerText = (d.balance || 0).toLocaleString();
        document.getElementById('uNameDisplay').innerText = "AGENT: " + d.name.toUpperCase();
        renderNodes(d.hasActive);
    });
  }
</script>
</body>
</html>
