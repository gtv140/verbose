<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>VERBOSE — Prime Solutions Professional</title>
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;800&display=swap" rel="stylesheet">
<style>
  :root { --neon: #00f7ff; --accent: #ff5cff; --dark: #050505; --glass: rgba(255,255,255,0.05); --text: #fff; }
  .light-mode { --dark: #f0f2f5; --glass: rgba(0,0,0,0.05); --text: #222; }
  * { box-sizing: border-box; transition: 0.3s; font-family: 'Poppins', sans-serif; -webkit-tap-highlight-color: transparent; }
  body { margin:0; background: var(--dark); color: var(--text); overflow-x: hidden; }
  
  header { text-align: center; padding: 30px 20px; font-size: 28px; font-weight: 800; background: linear-gradient(90deg, var(--neon), var(--accent)); -webkit-background-clip: text; -webkit-text-fill-color: transparent; letter-spacing: 6px; cursor: pointer; text-transform: uppercase; }
  
  .page { max-width: 420px; margin: 10px auto 100px; background: var(--glass); padding: 25px; border-radius: 20px; border: 1px solid rgba(255,255,255,0.05); backdrop-filter: blur(15px); min-height: 520px; box-shadow: 0 20px 40px rgba(0,0,0,0.4); }
  
  .user-card { background: rgba(255,255,255,0.03); padding: 20px; border-radius: 15px; margin-bottom: 15px; border-left: 4px solid var(--neon); }
  
  input, select, textarea { width: 100%; padding: 14px; margin-top: 10px; border-radius: 8px; border: 1px solid rgba(255,255,255,0.1); background: rgba(0,0,0,0.4); color: #fff; font-size: 13px; outline: none; }
  input:focus { border-color: var(--neon); }
  
  button { width: 100%; padding: 15px; margin-top: 15px; border-radius: 8px; border: none; background: linear-gradient(90deg, var(--neon), var(--accent)); color: #000; font-weight: 700; cursor: pointer; text-transform: uppercase; font-size: 13px; letter-spacing: 1px; }
  button:disabled { opacity: 0.5; cursor: not-allowed; }

  .nav { position: fixed; bottom: 0; left: 0; right: 0; background: rgba(5,5,7,0.98); display: flex; justify-content: space-around; padding: 12px; z-index: 1000; border-top: 1px solid rgba(255,255,255,0.05); }
  .nav-item { text-align: center; font-size: 10px; cursor: pointer; opacity: 0.4; color: #fff; flex:1; text-transform: uppercase; font-weight: 600; }
  .nav-item.active { opacity: 1; color: var(--neon); }

  .plan-card { background: rgba(255, 255, 255, 0.02); border-radius: 18px; padding: 20px; margin-bottom: 20px; border: 1px solid rgba(255,255,255,0.05); }
  .progress-container { width: 100%; height: 4px; background: rgba(255,255,255,0.1); border-radius: 10px; margin: 12px 0; overflow: hidden; }
  .progress-bar { height: 100%; background: var(--neon); width: 0%; transition: width 1s linear; }

  .status-tag { font-size: 9px; padding: 2px 8px; border-radius: 4px; float: right; font-weight: bold; }
  .approved { background: #00ff88; color: #000; } .pending { background: #ffa500; color: #000; } .rejected { background: #ff4444; color: #fff; }
  
  .hidden { display: none; }
  .theme-toggle { position: absolute; top: 20px; right: 20px; cursor: pointer; font-size: 18px; opacity: 0.6; }
</style>
</head>
<body>

<div class="theme-toggle" onclick="toggleTheme()">THEME</div>
<header id="mainHeader">VERBOSE</header>

<div id="loginPage" class="page">
  <h3 style="text-align:center; font-weight: 400;">Secure Portal Access</h3>
  <input id="u_name" type="text" placeholder="Username">
  <input id="u_pass" type="password" placeholder="Password">
  <button id="entryBtn">Authenticate</button>
  <p style="font-size: 10px; text-align: center; opacity: 0.5; margin-top: 20px;">By logging in, you agree to our Terms of Service.</p>
</div>

<div id="dashboard" class="page hidden">
  <div class="user-card">
    <div id="welcomeMsg" style="opacity:0.6; font-size:11px; text-transform: uppercase;">Account Holder</div>
    <div style="font-size:30px; font-weight:800; margin:5px 0;">PKR <span id="mainBal">0.00</span></div>
    <div style="font-size:9px; color:var(--neon); font-weight:bold; letter-spacing: 1px;">TOTAL CAPITAL BALANCE</div>
  </div>
  <div id="newsTicker" style="background: rgba(0,247,255,0.05); padding: 10px; border-radius: 10px; font-size: 11px;">
    <marquee id="newsText">System Status: Operational. Welcome to Verbose Prime Solutions.</marquee>
  </div>
  <button onclick="tab('plansPage')">Invest in Portfolios</button>
</div>

<div id="plansPage" class="page hidden">
  <h4 style="margin:0; text-transform: uppercase; letter-spacing: 1px;">Investment Portfolios</h4>
  <p style="font-size:10px; opacity:0.5; margin-bottom:20px;">Automated daily yield generation.</p>
  <div id="plansList"></div>
</div>

<div id="walletPage" class="page hidden">
  <div class="user-card" style="font-size:11px;">
    <b style="color: var(--neon);">Official Deposit Channels:</b><br>
    JazzCash / SadaPay: 03705519562<br>
    EasyPaisa: 03379827882<br>
    <small>Beneficiary: Muhammad Nazim</small>
  </div>
  <input id="d_amount" type="number" placeholder="Deposit Amount (PKR)">
  <input id="d_tid" placeholder="Transaction ID (TID)">
  <input id="d_proof" type="text" placeholder="Screenshot Reference Link">
  <button onclick="submitReq('Deposit')">Initiate Deposit</button>
  <hr style="opacity:0.05; margin:25px 0;">
  <input id="w_amount" type="number" placeholder="Withdrawal Amount (PKR)">
  <button onclick="submitReq('Withdraw')" style="background:var(--accent)">Request Payout</button>
</div>

<div id="historyPage" class="page hidden">
  <h4 style="margin:0; text-transform: uppercase;">Audit Logs</h4>
  <div id="userHistoryList" style="margin-top:15px;"></div>
</div>

<div id="detailsPage" class="page hidden">
  <h4 style="text-transform: uppercase;">Corporate Information</h4>
  <div style="font-size:11px; line-height:1.8; opacity:0.8;">
    <b>About Verbose:</b> Prime Solutions is a certified digital asset management firm based in Rawalpindi, Pakistan.<br><br>
    <b>Privacy Protocol:</b> All user data is encrypted via 256-bit SSL protocols. We prioritize member confidentiality.<br><br>
    <b>Operations:</b> Withdrawals are audited and processed within 24 business hours.<br><br>
    <b>Support:</b> For technical inquiries, contact <u>webhub262@gmail.com</u>
  </div>
</div>

<div id="bottomNav" class="nav hidden">
  <div class="nav-item active" onclick="tab('dashboard', this)">Home</div>
  <div class="nav-item" onclick="tab('plansPage', this)">Plans</div>
  <div class="nav-item" onclick="tab('walletPage', this)">Wallet</div>
  <div class="nav-item" onclick="tab('historyPage', this)">History</div>
  <div class="nav-item" onclick="tab('detailsPage', this)">Details</div>
  <div id="adminNav" class="nav-item hidden" onclick="tab('adminPage', this)">Admin</div>
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

  document.getElementById('entryBtn').onclick = async () => {
    const u = document.getElementById('u_name').value.trim().toLowerCase();
    const p = document.getElementById('u_pass').value.trim();
    if(!u || !p) return alert("Credentials required.");
    const userRef = ref(db, 'users/' + u);
    const snap = await get(userRef);
    if(snap.exists()){
      if(snap.val().password === p) sessionStart(u); else alert("Invalid Password.");
    } else {
      await set(userRef, { username:u, password:p, balance:0, isBanned: false });
      sessionStart(u);
    }
  };

  function sessionStart(u) {
    localStorage.setItem('v_user', u);
    document.getElementById('loginPage').classList.add('hidden');
    document.getElementById('dashboard').classList.remove('hidden');
    document.getElementById('bottomNav').classList.remove('hidden');
    onValue(ref(db, 'users/' + u), s => { 
        document.getElementById('mainBal').innerText = parseFloat(s.val().balance).toLocaleString(undefined, {minimumFractionDigits: 2}); 
    });
    renderPlans();
  }

  function renderPlans() {
    const cont = document.getElementById('plansList'); cont.innerHTML = "";
    for(let i=1; i<=25; i++) {
      let cost = (i<=20) ? 200 + (i-1)*1000 : 25000 + (i-21)*15000;
      let daily = (i<=20) ? Math.round(cost*0.1) : Math.round(cost*0.18);
      cont.innerHTML += `
        <div class="plan-card">
          <div style="display:flex; justify-content:space-between; align-items:center;">
            <b style="font-size:14px;">Portfolio Tier ${i}</b>
            <span style="color:var(--neon); font-weight:800;">PKR ${cost.toLocaleString()}</span>
          </div>
          <div style="font-size:11px; margin-top:8px; opacity:0.7;">Daily Yield: PKR ${daily.toLocaleString()}</div>
          <div class="progress-container"><div class="progress-bar" id="pb-${i}"></div></div>
          <button onclick="executeInvestment(${cost}, ${i})" style="padding:10px; font-size:11px;">Activate Contract</button>
        </div>`;
    }
  }

  window.executeInvestment = async (cost, level) => {
    const u = localStorage.getItem('v_user');
    const s = await get(ref(db, 'users/' + u));
    const currentBal = parseFloat(s.val().balance);
    if(currentBal >= cost) {
      if(confirm("Confirm investment activation?")) {
        await update(ref(db, 'users/' + u), { balance: currentBal - cost });
        alert("Contract successfully activated.");
      }
    } else {
      alert("Insufficient Capital. Please initiate a deposit to proceed.");
      tab('walletPage');
    }
  };

  window.submitReq = (type) => {
    const u = localStorage.getItem('v_user');
    const amt = (type=='Deposit')? document.getElementById('d_amount').value : document.getElementById('w_amount').value;
    if(!amt) return alert("Amount field cannot be empty.");
    const key = Date.now();
    set(ref(db, 'requests/' + key), { user:u, amount:amt, type:type, status:'pending', time:new Date().toLocaleString() });
    set(ref(db, 'history/' + u + '/' + key), { amount:amt, type:type, status:'pending', time:new Date().toLocaleString() });
    alert("Request transmitted to audit department.");
  };

  window.tab = (id, el) => {
    document.querySelectorAll('.page').forEach(p => p.classList.add('hidden'));
    document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
    document.getElementById(id).classList.remove('hidden');
    if(el) el.classList.add('active');
    if(id==='historyPage') fetchHistory(localStorage.getItem('v_user'));
  };

  function fetchHistory(u) {
    onValue(ref(db, 'history/' + u), snap => {
      const cont = document.getElementById('userHistoryList'); cont.innerHTML = "";
      const data = snap.val();
      if(!data) return cont.innerHTML = "<p style='font-size:11px; opacity:0.5;'>No transaction logs found.</p>";
      Object.keys(data).reverse().forEach(id => {
        cont.innerHTML += `<div class="user-card" style="font-size:10px;">
          <span class="status-tag ${data[id].status}">${data[id].status}</span>
          <b>${data[id].type}</b>: PKR ${parseFloat(data[id].amount).toLocaleString()}<br>
          <span style="opacity:0.5;">${data[id].time}</span>
        </div>`;
      });
    });
  }

  window.toggleTheme = () => document.body.classList.toggle('light-mode');
  
  let clicks = 0; document.getElementById('mainHeader').onclick = () => { if(++clicks===7){ document.getElementById('adminNav').classList.remove('hidden'); alert("Administrator Privileges Enabled."); clicks=0; } };
</script>
</body>
</html>
