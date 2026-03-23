<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>VERBOSE - Secure Investment Portal</title>
<style>
  :root{ --neon:#00f7ff; --accent:#ff5cff; --dark:#070707; --panel:#0f1114; }
  *{box-sizing:border-box}
  body { margin:0; font-family:Inter, Arial, sans-serif; background:var(--dark); color:#fff; overflow-x:hidden; -webkit-font-smoothing:antialiased; }
  header { text-align:center; padding:24px 12px; font-size:28px; font-weight:800; letter-spacing:2px; background:linear-gradient(90deg,var(--neon),var(--accent)); -webkit-background-clip:text; -webkit-text-fill-color:transparent; cursor:pointer; }
  
  .login-box, .page { max-width:430px; margin:18px auto; background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01)); padding:18px; border-radius:12px; border:1px solid rgba(0,255,240,0.06); box-shadow:0 8px 30px rgba(0,0,0,0.6); }
  
  input,button,select { width:100%; padding:10px; margin-top:10px; border-radius:8px; border:1px solid rgba(0,255,240,0.08); background:transparent; color:#e6f7fb; outline:none; font-size:14px; }
  
  button { background:linear-gradient(90deg,var(--neon),var(--accent)); border:none; color:#001; font-weight:700; cursor:pointer; transition:transform .15s ease; }
  button:hover{ transform:translateY(-2px); }
  
  .nav { position:fixed; bottom:0; left:0; right:0; background:rgba(15,17,20,0.95); display:flex; justify-content:space-around; padding:12px; border-top:1px solid rgba(0,255,240,0.04); font-size:11px; z-index:1000; }
  .nav div{text-align:center;cursor:pointer; width:60px;}
  
  .hidden{display:none;}
  .user-box { background:linear-gradient(90deg, rgba(0,255,240,0.06), rgba(255,92,255,0.02)); padding:14px; border-radius:12px; margin-bottom:12px; border:1px solid rgba(0,255,240,0.06); }
  .plan-box { border:1px solid rgba(0,255,240,0.06); padding:12px; margin:10px 0; border-radius:10px; background:rgba(255,255,255,0.01); display:flex; justify-content:space-between; align-items:center; }
  .history-item { border-bottom:1px solid rgba(255,255,255,0.05); padding:8px 0; font-size:12px; display:flex; justify-content:space-between; }
  .small {font-size:12px; color:rgba(230,247,251,0.6);}
  .status-tag { padding:2px 6px; border-radius:4px; font-size:10px; }
</style>
</head>
<body>

<header id="mainHeader">VERBOSE</header>

<div id="loginPage" class="login-box">
  <h2 style="margin:0 0 10px 0">Login / Signup</h2>
  <input id="user" placeholder="Username" />
  <input id="pass" placeholder="Password" type="password" />
  <button onclick="handleAuth()">Access Account</button>
  <p class="small" style="margin-top:10px; text-align:center;">Simple username login enabled for Prime Solutions clients.</p>
</div>

<div id="dashboard" class="page">
  <div class="user-box">
    <div style="font-size:18px; font-weight:800;" id="dashUser">Welcome Back</div>
    <div style="margin-top:5px;">Current Balance: Rs <span id="dashBalance">0</span></div>
    <div class="small" style="color:var(--neon)">Daily Profit: Rs <span id="dashDaily">0</span></div>
  </div>
  
  <div class="alert-box" style="background:rgba(0,247,255,0.05); padding:10px; border-radius:8px; border:1px solid var(--neon); font-size:12px;">
    📢 Announcement: All withdrawal requests are processed within 24 hours.
  </div>

  <div style="margin-top:15px;">
    <h3>Active Plans</h3>
    <div id="activePlansList" class="small">No active plans yet.</div>
  </div>
</div>

<div id="plans" class="page hidden">
  <h3>Investment Plans</h3>
  <div id="plansList"></div>
</div>

<div id="history" class="page hidden">
  <h3>Transaction History</h3>
  <div id="historyList">
    <p class="small">Loading history...</p>
  </div>
</div>

<div id="about" class="page hidden">
  <h3>About Verbose</h3>
  <p class="small">Verbose is a leading digital investment platform powered by <b>Prime Solutions</b>. We offer transparent and secure profit-sharing models for web-based assets.</p>
  
  <h3>Privacy Policy</h3>
  <p class="small">1. Your data is encrypted and never shared.<br>2. Withdrawals require manual admin verification for security.<br>3. One account per device is recommended.</p>
  
  <h3>Contact Admin</h3>
  <p class="small">WhatsApp: 03705519562<br>Email: support@primesolutions.com</p>
</div>

<div id="godMode" class="page hidden" style="border: 2px dashed var(--neon);">
  <h2 style="color:var(--neon); text-align:center;">⚡ GOD MODE ⚡</h2>
  <div class="user-box">
    <input id="adminSearchUser" placeholder="Target Username">
    <button onclick="fetchUserAdmin()">Get User Data</button>
    <div id="adminEditor" class="hidden">
      <input id="editBalance" type="number" placeholder="New Balance">
      <input id="editDaily" type="number" placeholder="New Daily Profit">
      <button onclick="saveAdminChanges()" style="background:var(--accent);">Update User</button>
    </div>
  </div>
  <div class="user-box">
    <h3>Pending Deposits</h3>
    <div id="pendingList" class="small">No requests...</div>
  </div>
  <button onclick="showPage('dashboard')">Back</button>
</div>

<div id="bottomNav" class="nav hidden">
  <div onclick="showPage('dashboard')">🏠<br>Home</div>
  <div onclick="showPage('plans')">📦<br>Plans</div>
  <div onclick="showPage('history')">📜<br>History</div>
  <div onclick="showPage('about')">ℹ️<br>About</div>
  <div onclick="logout()">🚪<br>Logout</div>
  <div onclick="showPage('godMode')" id="adminBtn" class="hidden">⚡<br>Admin</div>
</div>

<script type="module">
  import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
  import { getDatabase, ref, set, get, update, onValue } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js";

  const firebaseConfig = {
    apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk",
    authDomain: "dark-web-9.firebaseapp.com",
    projectId: "dark-web-9",
    storageBucket: "dark-web-9.firebasestorage.app",
    messagingSenderId: "564328425161",
    appId: "1:564328425161:web:eb109ab77356dafe7f4f18"
  };

  const app = initializeApp(firebaseConfig);
  const db = getDatabase(app);

  // --- AUTH ---
  window.handleAuth = async () => {
    const u = document.getElementById('user').value.trim();
    const p = document.getElementById('pass').value.trim();
    if(!u || !p) return alert("Please fill all details!");

    const userRef = ref(db, 'users/' + u);
    const snap = await get(userRef);

    if(snap.exists()) {
      if(snap.val().password === p) loginSuccess(u, snap.val());
      else alert("Incorrect password!");
    } else {
      const newUser = { username:u, password:p, balance:0, dailyProfit:0, joined: new Date().toLocaleDateString() };
      await set(userRef, newUser);
      loginSuccess(u, newUser);
    }
  };

  function loginSuccess(u, data) {
    localStorage.setItem('v_user', u);
    document.getElementById('dashUser').innerText = "Hello, " + u;
    document.getElementById('dashBalance').innerText = data.balance;
    document.getElementById('dashDaily').innerText = data.dailyProfit;
    
    document.getElementById('loginPage').classList.add('hidden');
    document.getElementById('dashboard').classList.remove('hidden');
    document.getElementById('bottomNav').classList.remove('hidden');
    
    renderPlans();
    loadHistory(u);
    listenToAdmin();
  }

  // --- NAVIGATION ---
  window.showPage = (id) => {
    document.querySelectorAll('.page').forEach(p => p.classList.add('hidden'));
    document.getElementById(id).classList.remove('hidden');
  };

  // --- GOD MODE TOGGLE ---
  let clicks = 0;
  document.getElementById('mainHeader').onclick = () => {
    clicks++;
    if(clicks === 7) {
      const key = prompt("Enter Master Admin Key:");
      if(key === "prime786") {
        document.getElementById('adminBtn').classList.remove('hidden');
        showPage('godMode');
      }
      clicks = 0;
    }
  };

  // --- DATA LOADING ---
  function renderPlans() {
    const list = document.getElementById('plansList');
    list.innerHTML = "";
    for(let i=1; i<=12; i++) {
        const amt = i * 500;
        const profit = Math.round(amt * 0.15);
        list.innerHTML += `<div class="plan-box">
          <div><b>Elite Plan ${i}</b><br><span class="small">Invest: Rs ${amt} | Daily: Rs ${profit}</span></div>
          <button onclick="requestDeposit(${amt})" style="width:70px; font-size:10px;">Buy</button>
        </div>`;
    }
  }

  window.requestDeposit = (amt) => {
    const u = localStorage.getItem('v_user');
    alert(`Please send Rs ${amt} to JazzCash: 03705519562. After payment, inform admin on WhatsApp.`);
    // Example: Push to Firebase pending list
    const dId = Date.now();
    set(ref(db, 'deposits/' + dId), { user: u, amount: amt, status: 'pending', time: new Date().toLocaleString() });
  };

  async function loadHistory(u) {
    const histRef = ref(db, 'deposits');
    onValue(histRef, (snap) => {
      const list = document.getElementById('historyList');
      list.innerHTML = "";
      const data = snap.val();
      for(let id in data) {
        if(data[id].user === u) {
          list.innerHTML += `<div class="history-item">
            <span>Deposit Rs ${data[id].amount}</span>
            <span class="status-tag" style="background:${data[id].status==='pending'?'#555':'#00ff00'}">${data[id].status}</span>
          </div>`;
        }
      }
    });
  }

  // --- ADMIN FUNCTIONS ---
  window.fetchUserAdmin = async () => {
    const u = document.getElementById('adminSearchUser').value;
    const snap = await get(ref(db, 'users/' + u));
    if(snap.exists()) {
      document.getElementById('editBalance').value = snap.val().balance;
      document.getElementById('editDaily').value = snap.val().dailyProfit;
      document.getElementById('adminEditor').classList.remove('hidden');
    }
  };

  window.saveAdminChanges = async () => {
    const u = document.getElementById('adminSearchUser').value;
    await update(ref(db, 'users/' + u), {
      balance: parseFloat(document.getElementById('editBalance').value),
      dailyProfit: parseFloat(document.getElementById('editDaily').value)
    });
    alert("User Data Updated, Sweetie!");
  };

  function listenToAdmin() {
    onValue(ref(db, 'deposits'), (snap) => {
        const pList = document.getElementById('pendingList');
        pList.innerHTML = "";
        const data = snap.val();
        for(let id in data) {
            if(data[id].status === 'pending') {
                pList.innerHTML += `<div>${data[id].user}: Rs ${data[id].amount} <button onclick="approveDep('${id}','${data[id].user}',${data[id].amount})">Approve</button></div>`;
            }
        }
    });
  }

  window.approveDep = async (id, u, amt) => {
    const userSnap = await get(ref(db, 'users/' + u));
    const newBal = (userSnap.val().balance || 0) + amt;
    await update(ref(db, 'users/' + u), { balance: newBal });
    await update(ref(db, 'deposits/' + id), { status: 'success' });
    alert("Approved!");
  };

  window.logout = () => { localStorage.clear(); location.reload(); };

</script>
</body>
</html>
