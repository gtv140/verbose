<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no"/>
<title>VERBOSE — Quantum Infrastructure</title>
<link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;800&display=swap" rel="stylesheet">
<style>
  :root { --neon: #00f7ff; --accent: #7000ff; --bg: #030303; --card: rgba(255,255,255,0.06); --border: rgba(255,255,255,0.1); --success: #00ff88; --error: #ff4646; --gold: #ffcc00; }
  * { box-sizing: border-box; transition: 0.3s; font-family: 'Plus Jakarta Sans', sans-serif; outline: none; }
  body { margin:0; background: var(--bg); color: #fff; overflow-x: hidden; -webkit-tap-highlight-color: transparent; }

  /* OVERLAYS */
  .system-overlay { position: fixed; inset: 0; background: rgba(0,0,0,0.98); z-index: 1000000; display: none; align-items: center; justify-content: center; text-align: center; padding: 30px; }
  .modal-box { border: 1px solid var(--border); padding: 40px; border-radius: 40px; background: var(--card); backdrop-filter: blur(20px); }

  header { display: flex; align-items: center; padding: 60px 25px 15px; position: sticky; top: 0; background: rgba(3,3,3,0.9); backdrop-filter: blur(15px); z-index: 1000; justify-content: space-between; border-bottom: 1px solid var(--border); }
  .logo-text { font-size: 20px; font-weight: 800; letter-spacing: 5px; text-shadow: 0 0 15px var(--neon); cursor: pointer; }
  .menu-toggle { font-size: 24px; cursor: pointer; color: var(--neon); }

  /* DRAWER */
  .drawer { position: fixed; top: 0; left: -100%; width: 280px; height: 100%; background: rgba(5,5,5,0.98); backdrop-filter: blur(30px); z-index: 50000; border-right: 1px solid var(--border); padding: 50px 20px; }
  .drawer.open { left: 0; }
  .drawer-item { padding: 15px; border-radius: 15px; margin-bottom: 10px; background: var(--card); font-size: 12px; font-weight: 700; cursor: pointer; display: flex; align-items: center; gap: 12px; }

  .page { max-width: 480px; margin: 0 auto; padding: 25px 20px 140px; display: none; }
  .active { display: block !important; animation: fadeIn 0.4s ease; }
  @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

  .node-card { background: var(--card); border: 1px solid var(--border); border-radius: 30px; padding: 25px; margin-bottom: 20px; position: relative; }
  .btn-action { width: 100%; padding: 18px; border-radius: 22px; border: none; font-weight: 800; cursor: pointer; background: linear-gradient(135deg, var(--neon), var(--accent)); color: #fff; text-transform: uppercase; margin-top: 15px; }
  
  /* ADMIN SPECIFIC */
  .admin-log-box { max-height: 150px; overflow-y: auto; background: #000; border-radius: 15px; padding: 10px; font-size: 10px; border: 1px solid var(--gold); }
  .admin-btn { padding: 8px 12px; border-radius: 10px; font-size: 10px; font-weight: 800; border: none; cursor: pointer; margin: 4px; }

  .nav-bar { position: fixed; bottom: 30px; left: 20px; right: 20px; background: rgba(15,15,15,0.95); backdrop-filter: blur(25px); border-radius: 50px; display: flex; justify-content: space-around; padding: 18px; border: 1px solid var(--border); z-index: 1000; }
  .nav-link { font-size: 10px; font-weight: 800; opacity: 0.4; color: #fff; cursor: pointer; }
  .nav-link.active { opacity: 1; color: var(--neon); }
</style>
</head>
<body>

<div id="banOverlay" class="system-overlay">
    <div class="modal-box" style="border-color: var(--error);">
        <h1 style="color:var(--error); letter-spacing:5px;">BANNED</h1>
        <p style="opacity:0.7;">Your access has been suspended due to protocol violations.</p>
        <button class="btn-action" style="background:var(--error);" onclick="logout()">LOGOUT</button>
    </div>
</div>

<header>
    <div class="menu-toggle" onclick="toggleMenu()">☰</div>
    <div class="logo-text" id="adminTrigger">VERBOSE</div>
    <div style="width:24px;"></div>
</header>

<div class="drawer" id="drawer">
    <div class="logo-text" style="font-size:16px; margin-bottom:40px; text-align:center;">CONTROL</div>
    <div class="drawer-item" onclick="tab('dash'); toggleMenu()">Dashboard</div>
    <div class="drawer-item" onclick="tab('historyPage'); toggleMenu()">History</div>
    <div class="drawer-item" onclick="tab('teamPage'); toggleMenu()">Team Squad</div>
    <div class="drawer-item" onclick="tab('salaryPage'); toggleMenu()">Salary Program</div>
    <div class="drawer-item" onclick="tab('infoPage'); toggleMenu()">Company Policy</div>
    <div class="drawer-item" style="color:var(--error); margin-top:60px;" onclick="logout()">Logout</div>
</div>

<div id="dash" class="page active">
    <div class="node-card" style="text-align:center; border-color:var(--neon);">
        <small style="opacity:0.5; letter-spacing:3px;">LIQUID BALANCE</small>
        <h1 style="font-size:45px; margin:10px 0;">PKR <span id="uBal">0.00</span></h1>
        <div id="uNameDisplay" style="font-size:11px; color:var(--neon); font-weight:800;">---</div>
    </div>
    <div id="activeMining" class="node-card" style="display:none; border-color:var(--success); text-align:center;">
        <div id="timer" style="font-size:24px; font-weight:800; color:var(--success);">24:00:00</div>
    </div>
</div>

<div id="historyPage" class="page">
    <h3>TRANSACTION LOGS</h3>
    <div id="userHistoryList"></div>
</div>

<div id="teamPage" class="page">
    <h3>MY TEAM</h3>
    <div class="node-card">Referral Code: <b id="myRefCode" style="color:var(--neon);">---</b></div>
    <div class="node-card">Total Members: <b id="teamSize">0</b></div>
</div>

<div id="salaryPage" class="page">
    <h3>SALARY</h3>
    <div class="node-card">
        <b>Level 1 (20 Users)</b><br>5,000 PKR / Month
        <button class="btn-action" id="claim20" disabled onclick="claimSalary(20)">CLAIM 20</button>
    </div>
    <div class="node-card">
        <b>Level 2 (50 Users)</b><br>15,000 PKR / Month
        <button class="btn-action" id="claim50" disabled onclick="claimSalary(50)">CLAIM 50</button>
    </div>
</div>

<div id="nodesPage" class="page">
    <h3>MINING CLUSTERS</h3>
    <div id="nodeGrid"></div>
</div>

<div id="financePage" class="page">
    <div class="node-card">
        <h3>DEPOSIT</h3>
        <p style="font-size:11px; opacity:0.6;">03705519562 (EasyPaisa/JazzCash)</p>
        <input id="dAmt" style="width:100%; padding:15px; margin-top:10px; border-radius:15px; border:1px solid var(--border); background:#111; color:#fff;" type="number" placeholder="Amount">
        <input id="dTID" style="width:100%; padding:15px; margin-top:10px; border-radius:15px; border:1px solid var(--border); background:#111; color:#fff;" placeholder="TID">
        <button class="btn-action" onclick="sendReq('Deposit')">Submit</button>
    </div>
</div>

<div id="infoPage" class="page">
    <h3>COMPANY INFO</h3>
    <div class="node-card" style="font-size:12px; line-height:1.6;">
        <b>About Us:</b> We provide next-gen cloud mining infrastructure in Rawalpindi.<br><br>
        <b>Privacy:</b> Your data is encrypted and never shared. Assets are secured in cold vaults.<br><br>
        <b>Policy:</b> Withdrawals are audited manually for 100% security.
    </div>
</div>

<div id="adminPage" class="page">
    <h2 style="color:var(--gold); text-align:center;">OVERLORD HUB</h2>
    <div class="node-card"><h4 style="color:var(--neon); margin:0;">ACTIVITY LOGS</h4><div id="adminLogs" class="admin-log-box"></div></div>
    <h4 style="color:var(--gold);">PENDING AUDITS</h4><div id="admReqs"></div>
    <h4 style="color:var(--gold);">USER DIRECTORY</h4>
    <input id="userSearch" oninput="filterUsers()" placeholder="Search Phone..." style="width:100%; padding:12px; margin-bottom:10px; border-radius:10px; border:1px solid var(--border); background:#000; color:#fff;">
    <div id="admUsers"></div>
</div>

<nav class="nav-bar">
    <div class="nav-link active" onclick="tab('dash', this)">Home</div>
    <div class="nav-link" onclick="tab('nodesPage', this)">Nodes</div>
    <div class="nav-link" onclick="tab('financePage', this)">Finance</div>
    <div class="nav-link" onclick="tab('historyPage', this)">History</div>
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
  const doAuth = async () => {
    let p = prompt("Phone:"), pin = prompt("PIN:"), n = prompt("Name (New Users Only):");
    if(!p || !pin) return;
    const s = await get(ref(db, `users/${p}`));
    if(s.exists()) {
        if(s.val().pass === pin) { localStorage.setItem('v_user', p); location.reload(); }
        else alert("Wrong PIN");
    } else {
        const refCode = prompt("Referral Phone (Optional):") || "NONE";
        await set(ref(db, `users/${p}`), { name:n, balance:0, pass:pin, isBanned:false, referredBy:refCode });
        if(refCode !== "NONE") {
            const rSnap = await get(ref(db, `users/${refCode}/team`));
            let team = rSnap.val() || []; team.push(p);
            await update(ref(db, `users/${refCode}`), { team });
        }
        localStorage.setItem('v_user', p); location.reload();
    }
  };

  if(!user) doAuth();

  // --- CORE LOGIC ---
  if(user) {
    onValue(ref(db, `users/${user}`), s => {
        const d = s.val();
        if(!d) return;
        if(d.isBanned) document.getElementById('banOverlay').style.display = 'flex';
        document.getElementById('uBal').innerText = (d.balance || 0).toLocaleString();
        document.getElementById('uNameDisplay').innerText = d.name.toUpperCase();
        document.getElementById('myRefCode').innerText = user;
        document.getElementById('teamSize').innerText = (d.team || []).length;
        if((d.team || []).length >= 20) document.getElementById('claim20').disabled = false;
        if((d.team || []).length >= 50) document.getElementById('claim50').disabled = false;
        
        // History Rendering
        const hDiv = document.getElementById('userHistoryList'); hDiv.innerHTML = "";
        if(d.history) Object.values(d.history).reverse().forEach(x => {
            hDiv.innerHTML += `<div class="node-card" style="font-size:11px; padding:15px; border-left:4px solid ${x.status.includes('Approved')? 'var(--success)':'var(--gold)'}">
                <b>${x.type}</b>: ${x.amt} PKR<br>Status: ${x.status}<br><small>${x.time}</small>
            </div>`;
        });
        renderNodes(d.hasActive);
    });
  }

  const renderNodes = (active) => {
    const g = document.getElementById('nodeGrid'); g.innerHTML = "";
    [{n:"Titan",p:1000,d:125},{n:"Aura",p:2500,d:320},{n:"Zenith",p:5000,d:700}].forEach(x => {
        g.innerHTML += `<div class="node-card"><b>${x.n} Node</b><br>Daily: ${x.d} PKR<br>
        <button class="btn-action" ${active?'disabled':''} onclick="buyNode(${x.p},${x.d})">${active?'ACTIVE':'BUY '+x.p}</button></div>`;
    });
  };

  window.buyNode = async (p, d) => {
    const s = await get(ref(db, `users/${user}`));
    if(s.val().balance < p) return alert("Low Balance");
    await update(ref(db, `users/${user}`), { balance: s.val().balance - p, hasActive:true, daily:d, lastProfit:Date.now() });
    alert("Node Initialized!");
  };

  window.sendReq = (type) => {
    const amt = document.getElementById('dAmt').value, info = document.getElementById('dTID').value;
    const id = push(ref(db, `admin/requests`)).key;
    const data = { user, type, amt, info, status: "Pending", time: new Date().toLocaleString() };
    update(ref(db, `admin/requests/${id}`), data);
    update(ref(db, `users/${user}/history/${id}`), data);
    alert("Audit Queued.");
  };

  window.claimSalary = (lvl) => {
    const id = push(ref(db, `admin/requests`)).key;
    const data = { user, type: "Salary Lvl "+lvl, amt: lvl===20?5000:15000, info: "Team Claim", status: "Pending", time: new Date().toLocaleString() };
    update(ref(db, `admin/requests/${id}`), data);
    update(ref(db, `users/${user}/history/${id}`), data);
    alert("Salary Claim Sent!");
  };

  // --- ADMIN SUPREME ---
  let clks = 0; document.getElementById('adminTrigger').onclick = () => { if(++clks >= 7) { if(prompt("Key?")==="nazim786") { tab('adminPage'); loadAdmin(); } clks=0; } };

  const loadAdmin = () => {
    onValue(ref(db, `admin/requests`), s => {
        const div = document.getElementById('admReqs'); div.innerHTML = "";
        if(s.exists()) Object.entries(s.val()).forEach(([id, r]) => {
            div.innerHTML += `<div class="node-card" style="font-size:11px; border-color:var(--gold);">
                <b>${r.type}</b> | ${r.user} | PKR ${r.amt}<br>Info: ${r.info}
                <div style="margin-top:10px;">
                    <button class="admin-btn" style="background:var(--success);" onclick="approveGod('${id}','${r.user}',${r.amt},'${r.type}')">APPROVE</button>
                    <button class="admin-btn" style="background:var(--error); color:#fff;" onclick="rejectGod('${id}','${r.user}')">REJECT</button>
                </div>
            </div>`;
        });
    });
    onValue(ref(db, `users`), s => { window.allU = s.val(); filterUsers(); });
    onValue(ref(db, `admin/logs`), s => {
        const div = document.getElementById('adminLogs'); div.innerHTML = "";
        if(s.exists()) Object.values(s.val()).reverse().forEach(l => div.innerHTML += `<div>${l}</div>`);
    });
  };

  window.filterUsers = () => {
    const q = document.getElementById('userSearch').value;
    const div = document.getElementById('admUsers'); div.innerHTML = "";
    Object.entries(window.allU).forEach(([p, u]) => {
        if(p.includes(q)) div.innerHTML += `<div class="node-card" style="font-size:10px;">
            <b>${u.name}</b> (${p}) | BAL: ${u.balance}<br>
            <button class="admin-btn" style="background:#444; color:#fff;" onclick="toggleBan('${p}', ${u.isBanned})">${u.isBanned?'UNBAN':'BAN'}</button>
            <button class="admin-btn" style="background:var(--error); color:#fff;" onclick="deleteUser('${p}')">DELETE</button>
        </div>`;
    });
  };

  window.approveGod = async (id, target, amt, type) => {
    const s = await get(ref(db, `users/${target}`));
    const uData = s.val();
    let b = (uData.balance || 0) + Number(amt);
    await update(ref(db, `users/${target}`), { balance: b });
    await update(ref(db, `users/${target}/history/${id}`), { status: "Approved" });
    
    // Auto-Commission 5%
    if(type === 'Deposit' && uData.referredBy && uData.referredBy !== "NONE") {
        const upS = await get(ref(db, `users/${uData.referredBy}`));
        if(upS.exists()) {
            const comm = Number(amt) * 0.05;
            await update(ref(db, `users/${uData.referredBy}`), { balance: (upS.val().balance || 0) + comm });
            push(ref(db, `admin/logs`), `${new Date().toLocaleTimeString()}: Comm ${comm} to ${uData.referredBy}`);
        }
    }
    push(ref(db, `admin/logs`), `${new Date().toLocaleTimeString()}: Approved ${type} for ${target}`);
    await remove(ref(db, `admin/requests/${id}`));
  };

  window.rejectGod = async (id, target) => {
    const reason = prompt("Reason?");
    await update(ref(db, `users/${target}/history/${id}`), { status: "Rejected: " + reason });
    await remove(ref(db, `admin/requests/${id}`));
    push(ref(db, `admin/logs`), `${new Date().toLocaleTimeString()}: Rejected ${target}. Reason: ${reason}`);
  };

  window.toggleBan = (p, curr) => update(ref(db, `users/${p}`), { isBanned: !curr });
  window.deleteUser = (p) => { if(confirm("Delete User?")) remove(ref(db, `users/${p}`)); };

  window.toggleMenu = () => document.getElementById('drawer').classList.toggle('open');
  window.tab = (id, el) => { document.querySelectorAll('.page').forEach(p => p.classList.remove('active')); document.getElementById(id).classList.add('active'); if(el) { document.querySelectorAll('.nav-link').forEach(n => n.classList.remove('active')); el.classList.add('active'); } };
  window.logout = () => { localStorage.clear(); location.reload(); };
</script>
</body>
</html>
