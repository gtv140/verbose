<html lang="ur">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>VERBOSE — The Final Masterpiece</title>
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --neon: #00f7ff; --accent: #7000ff; --bg: #020202; --card: rgba(255,255,255,0.08); --border: rgba(255,255,255,0.1); --success: #00ff88; --error: #ff4646; --gold: #ffcc00; }
        * { box-sizing: border-box; transition: 0.3s cubic-bezier(0.4, 0, 0.2, 1); font-family: 'Plus Jakarta Sans', sans-serif; -webkit-tap-highlight-color: transparent; }
        body { margin:0; background: var(--bg); color: #fff; overflow-x: hidden; }
        
        /* Premium Header */
        header { display: flex; align-items: center; padding: 55px 25px 15px; position: sticky; top: 0; background: rgba(2,2,2,0.9); backdrop-filter: blur(20px); z-index: 1000; justify-content: space-between; border-bottom: 1px solid var(--border); }
        .logo-text { font-size: 20px; font-weight: 800; letter-spacing: 5px; text-shadow: 0 0 15px var(--neon); cursor: pointer; }
        
        /* Layout */
        .page { max-width: 480px; margin: 0 auto; padding: 20px 20px 140px; display: none; }
        .active { display: block !important; animation: fadeIn 0.4s ease; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(15px); } to { opacity: 1; transform: translateY(0); } }

        /* Dashboard Components */
        .prime-card { background: linear-gradient(145deg, #111, #050505); border: 1px solid var(--border); border-radius: 35px; padding: 35px 20px; margin-bottom: 20px; text-align: center; box-shadow: 0 25px 50px rgba(0,0,0,0.5); }
        .node-card { background: var(--card); border: 1px solid var(--border); border-radius: 25px; padding: 20px; margin-bottom: 15px; position: relative; }
        .btn-prime { width: 100%; padding: 18px; border-radius: 22px; border: none; font-weight: 800; cursor: pointer; background: linear-gradient(135deg, var(--neon), var(--accent)); color: #fff; text-transform: uppercase; margin-top: 10px; box-shadow: 0 10px 20px rgba(0,247,255,0.15); }
        input { width: 100%; padding: 18px; margin-top: 12px; border-radius: 20px; border: 1px solid var(--border); background: rgba(255,255,255,0.03); color: #fff; }
        
        /* Stats & Nav */
        .stat-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-bottom: 20px; }
        .stat-box { background: var(--card); padding: 15px; border-radius: 22px; text-align: center; border: 1px solid var(--border); }
        .stat-box b { color: var(--neon); font-size: 18px; }
        .nav-bar { position: fixed; bottom: 25px; left: 20px; right: 20px; background: rgba(15,15,15,0.95); backdrop-filter: blur(25px); border-radius: 50px; display: flex; justify-content: space-around; padding: 18px; border: 1px solid var(--border); z-index: 1000; box-shadow: 0 -10px 40px rgba(0,0,0,0.5); }
        .nav-link { font-size: 9px; font-weight: 800; opacity: 0.4; color: #fff; text-align: center; cursor: pointer; text-transform: uppercase; }
        .nav-link.active { opacity: 1; color: var(--neon); }

        /* Admin Visuals */
        .admin-img { width: 100%; border-radius: 15px; margin-top: 12px; border: 1px solid var(--gold); }
        #toast { position: fixed; bottom: 110px; left: 50%; transform: translateX(-50%); background: rgba(0,0,0,0.95); border: 1px solid var(--neon); padding: 10px 20px; border-radius: 20px; font-size: 11px; z-index: 10001; display: none; }
    </style>
</head>
<body>

<div id="toast">User 03xx*** successfully withdrew 5,000 PKR ✓</div>

<header>
    <div style="font-size:22px; color:var(--gold);" onclick="tab('salary')">🎖️</div>
    <div class="logo-text" id="adminTrigger">VERBOSE</div>
    <div style="font-size:22px; opacity:0.5;" onclick="logout()">🚪</div>
</header>

<div id="dash" class="page active">
    <div class="prime-card">
        <small style="opacity:0.5; letter-spacing:4px;">TOTAL BALANCE</small>
        <h1 style="font-size:48px; margin:10px 0; font-weight:800;">PKR <span id="uBal">0.00</span></h1>
        <div style="color:var(--neon); font-size:11px; font-weight:800;"><span id="uName">PRIME INVESTOR</span> ✓</div>
    </div>
    
    <div class="stat-grid">
        <div class="stat-box"><small>Monthly Salary</small><br><b id="uSal">0</b></div>
        <div class="stat-box"><small>Team Bonus</small><br><b id="uBon">0</b></div>
        <div class="stat-box"><small>Direct Team</small><br><b id="uDirect">0</b></div>
        <div class="stat-box"><small>Total Team</small><br><b id="uTotal">0</b></div>
    </div>

    <div class="node-card">
        <h3>Referral Program</h3>
        <p id="refLink" style="font-size:10px; opacity:0.5; word-break:break-all;"></p>
        <button class="btn-prime" style="padding:12px; font-size:12px;" onclick="copyRef()">Copy Invite Link</button>
    </div>
</div>

<div id="salary" class="page">
    <h2 style="text-align:center; color:var(--gold);">🏅 SALARY CLUB</h2>
    <div class="node-card" style="border-top:3px solid var(--gold); text-align:center;">
        <h4>Salary Eligibility</h4>
        <div id="salStatus" style="font-size:22px; font-weight:800; color:var(--neon);">NOT ELIGIBLE</div>
        <p style="font-size:11px; opacity:0.6;">Need 20 active direct referrals to claim 5,000 PKR Salary.</p>
    </div>
    <div class="node-card">
        <h4>Referral Commissions</h4>
        <div style="display:flex; justify-content:space-between; margin-bottom:10px;"><span>Level 1 (Direct)</span> <b style="color:var(--success);">10%</b></div>
        <div style="display:flex; justify-content:space-between;"><span>Level 2 (Indirect)</span> <b style="color:var(--neon);">5%</b></div>
    </div>
</div>

<div id="plans" class="page">
    <h3 style="text-align:center; letter-spacing:2px;">MINING NODES</h3>
    <div id="planContainer"></div>
</div>

<div id="wallet" class="page">
    <div class="node-card" style="border-top:3px solid var(--success);">
        <h3>DEPOSIT</h3>
        <p style="font-size:11px; opacity:0.7;">JazzCash/SadaPay: <b>03705519562</b><br>EasyPaisa: <b>03379827882</b></p>
        <input id="dAmt" type="number" placeholder="Amount">
        <input id="dTID" placeholder="TID (Transaction ID)">
        <small style="display:block; margin-top:15px;">Upload Payment Screenshot:</small>
        <input type="file" id="dFile" accept="image/*">
        <button class="btn-prime" onclick="depositReq()">Verify Deposit</button>
    </div>

    <div class="node-card" style="border-top:3px solid var(--error); margin-top:25px;">
        <h3>WITHDRAW</h3>
        <input id="wAmt" type="number" placeholder="Amount">
        <input id="wNum" placeholder="Account Number">
        <button class="btn-prime" style="background:var(--error);" onclick="withdrawReq()">Request Payout</button>
    </div>
</div>

<div id="admin" class="page">
    <h2 style="color:var(--gold); text-align:center;">SUPREME COMMAND</h2>
    <div id="adminFeed"></div>
    <hr style="opacity:0.1; margin:25px 0;">
    <h3>User Manager</h3>
    <input id="sPhone" placeholder="Search Phone Number">
    <button class="btn-prime" onclick="searchUser()">Find User</button>
    <div id="uEdit" class="node-card" style="margin-top:20px; display:none;"></div>
</div>

<nav class="nav-bar">
    <div class="nav-link active" onclick="tab('dash', this)">Home</div>
    <div class="nav-link" onclick="tab('plans', this)">Plans</div>
    <div class="nav-link" onclick="tab('wallet', this)">Wallet</div>
</nav>

<script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
    import { getDatabase, ref, set, get, update, onValue, push, remove } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js";

    const firebaseConfig = {
      apiKey: "AIzaSyBe5Q5jXpx3UvrHC9WOky9UWeDnP9SPfZI",
      authDomain: "verbose-6c008.firebaseapp.com",
      databaseURL: "https://verbose-6c008-default-rtdb.firebaseio.com",
      projectId: "verbose-6c008",
      storageBucket: "verbose-6c008.firebasestorage.app",
      messagingSenderId: "867100945312",
      appId: "1:867100945312:web:315dfb48fb34496cee12c5"
    };

    const app = initializeApp(firebaseConfig);
    const db = getDatabase(app);
    let currentUser = localStorage.getItem('v_user_auth');

    // Persistence Logic
    const init = async () => {
        if(currentUser) return sync();
        let p = prompt("Phone Number:"), pin = prompt("PIN:");
        if(!p || !pin) return;
        const s = await get(ref(db, `users/${p}`));
        if(s.exists()){
            if(s.val().pass === pin) { localStorage.setItem('v_user_auth', p); location.reload(); }
            else { alert("Wrong PIN"); init(); }
        } else {
            await set(ref(db, `users/${p}`), {balance:0, pass:pin, salary:0, bonus:0, directCount:0, teamSize:0, history:{}});
            localStorage.setItem('v_user_auth', p); location.reload();
        }
    };
    init();

    function sync() {
        document.getElementById('refLink').innerText = window.location.href + "?ref=" + currentUser;
        onValue(ref(db, `users/${currentUser}`), s => {
            const d = s.val(); if(!d) return;
            document.getElementById('uBal').innerText = Number(d.balance).toLocaleString();
            document.getElementById('uSal').innerText = d.salary || 0;
            document.getElementById('uBon').innerText = d.bonus || 0;
            document.getElementById('uDirect').innerText = d.directCount || 0;
            document.getElementById('uTotal').innerText = d.teamSize || 0;
            if(d.directCount >= 20) document.getElementById('salStatus').innerText = "ELIGIBLE (PKR 5,000)";
        });
    }

    // Build 25 Plans
    const container = document.getElementById('planContainer');
    for(let i=1; i<=25; i++){
        let price = i <= 20 ? i*250 : (i-20)*5000 + 5000;
        container.innerHTML += `<div class="node-card">
            <b>${i > 20 ? 'VIP Node' : 'Standard Node'} ${i}</b><br><small>Price: PKR ${price}</small>
            <button class="btn-prime" onclick="buy(${price})">Activate</button></div>`;
    }

    window.buy = async (price) => {
        const s = await get(ref(db, `users/${currentUser}`));
        let b = Number(s.val().balance);
        if(b < price) return alert("Low Balance!");
        await update(ref(db, `users/${currentUser}`), { balance: b - price });
        alert("Activation Successful!");
    };

    // Finance & Proof
    window.depositReq = () => {
        const amt = document.getElementById('dAmt').value, tid = document.getElementById('dTID').value, file = document.getElementById('dFile').files[0];
        if(!amt || !tid || !file) return alert("Puri detail bharein!");
        const r = new FileReader(); r.readAsDataURL(file);
        r.onload = async () => {
            const id = push(ref(db, `admin/requests`)).key;
            await update(ref(db, `admin/requests/${id}`), { user: currentUser, type: 'Deposit', amt, tid, proof: r.result, status: 'Pending', time: new Date().toLocaleString() });
            alert("Sent to Admin for Audit!");
        };
    };

    window.withdrawReq = async () => {
        const amt = Number(document.getElementById('wAmt').value), num = document.getElementById('wNum').value;
        const s = await get(ref(db, `users/${currentUser}`));
        if(amt > s.val().balance) return alert("Low Balance!");
        const id = push(ref(db, `admin/requests`)).key;
        await update(ref(db, `admin/requests/${id}`), { user: currentUser, type: 'Withdraw', amt, info: num, status: 'Pending', time: new Date().toLocaleString() });
        alert("Withdrawal Pending Approval.");
    };

    // Admin HUD
    let logoClk = 0; document.getElementById('adminTrigger').onclick = () => { if(++logoClk >= 7) { if(prompt("HUD KEY?")==="nazim786") { tab('admin'); loadAdmin(); } logoClk=0; } };

    function loadAdmin() {
        onValue(ref(db, `admin/requests`), s => {
            const d = document.getElementById('adminFeed'); d.innerHTML = "";
            if(!s.exists()) d.innerHTML = "No pending audits.";
            Object.entries(s.val() || {}).forEach(([id, r]) => {
                d.innerHTML += `<div class="node-card">
                    <b>${r.type}: ${r.amt}</b><br>User: ${r.user} | TID: ${r.tid || r.info}
                    ${r.proof ? `<img src="${r.proof}" class="admin-img">` : ''}
                    <div style="display:flex; gap:10px; margin-top:10px;">
                        <button class="btn-prime" style="background:var(--success); color:#000;" onclick="audit('${id}','${r.user}',${r.amt},'Approved','${r.type}')">Approve</button>
                        <button class="btn-prime" style="background:var(--error);" onclick="audit('${id}','${r.user}',0,'Rejected','${r.type}')">Reject</button>
                    </div></div>`;
            });
        });
    }

    window.audit = async (id, target, amt, status, type) => {
        if(status === 'Approved') {
            const s = await get(ref(db, `users/${target}`));
            let nb = Number(s.val().balance) + (type==='Deposit' ? Number(amt) : -Number(amt));
            await update(ref(db, `users/${target}`), { balance: nb });
        }
        await remove(ref(db, `admin/requests/${id}`));
        alert("Action Completed.");
    };

    window.searchUser = async () => {
        const p = document.getElementById('sPhone').value;
        const s = await get(ref(db, `users/${p}`));
        if(!s.exists()) return alert("User not found!");
        const div = document.getElementById('uEdit'); div.style.display = 'block';
        div.innerHTML = `<h4>User: ${p}</h4><input id="eBal" value="${s.val().balance}" placeholder="Balance"><button class="btn-prime" onclick="saveU('${p}')">Save</button>`;
    };
    window.saveU = async (p) => { await update(ref(db, `users/${p}`), { balance: Number(document.getElementById('eBal').value) }); alert("Updated!"); };

    window.tab = (id, el) => {
        document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
        document.getElementById(id).classList.add('active');
        if(el) { document.querySelectorAll('.nav-link').forEach(n => n.classList.remove('active')); el.classList.add('active'); }
    };
    window.logout = () => { localStorage.clear(); location.reload(); };
    window.copyRef = () => { navigator.clipboard.writeText(document.getElementById('refLink').innerText); alert("Link Copied!"); };

    setInterval(() => {
        const t = document.getElementById('toast'); t.style.display = 'block'; setTimeout(() => t.style.display='none', 3000);
    }, 15000);
</script>
</body>
</html>
