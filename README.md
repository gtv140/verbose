<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no"/>
<title>VERBOSE — Quantum Cloud</title>
<link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;800&display=swap" rel="stylesheet">
<style>
  :root { --neon: #00f7ff; --accent: #7000ff; --bg: #030303; --card: rgba(255,255,255,0.03); --border: rgba(255,255,255,0.08); --success: #00ff88; --error: #ff4646; --gold: #ffcc00; }
  * { box-sizing: border-box; transition: 0.3s; font-family: 'Plus Jakarta Sans', sans-serif; outline: none; }
  body { margin:0; background: var(--bg); color: #fff; overflow-x: hidden; -webkit-tap-highlight-color: transparent; }

  /* LOGIN & SIGNUP SCREEN */
  #authPage { position: fixed; inset: 0; background: var(--bg); z-index: 20000; padding: 20px; display: flex; flex-direction: column; justify-content: center; align-items: center; }
  .auth-card { width: 100%; max-width: 400px; background: var(--card); border: 1px solid var(--border); padding: 40px 25px; border-radius: 40px; backdrop-filter: blur(25px); text-align: center; }
  .auth-logo { font-size: 32px; font-weight: 800; letter-spacing: 12px; text-shadow: 0 0 20px var(--neon); margin-bottom: 10px; }
  
  /* INPUTS & BUTTONS */
  input { width: 100%; padding: 18px; margin-top: 15px; border-radius: 20px; border: 1px solid var(--border); background: rgba(255,255,255,0.02); color: #fff; font-size: 14px; }
  .btn-primary { width: 100%; padding: 18px; border-radius: 20px; border: none; font-weight: 800; cursor: pointer; background: linear-gradient(135deg, var(--neon), var(--accent)); color: #fff; margin-top: 20px; box-shadow: 0 10px 20px rgba(112,0,255,0.2); }
  .help-link { font-size: 11px; margin-top: 25px; display: block; color: var(--success); text-decoration: none; font-weight: 600; }

  /* HOME PAGE UI */
  header { text-align: center; padding: 40px 20px 10px; cursor: pointer; }
  .page { max-width: 450px; margin: 0 auto; padding: 10px 20px 140px; display: none; }
  .active { display: block !important; animation: fadeIn 0.4s ease; }
  @keyframes fadeIn { from { opacity: 0; transform: scale(0.98); } to { opacity: 1; transform: scale(1); } }

  .balance-card { background: linear-gradient(135deg, #0a0a0a, #000); border: 1px solid var(--neon); padding: 30px; border-radius: 35px; margin-bottom: 30px; text-align: left; }
  
  /* NODE LISTING */
  .node-card { background: var(--card); border: 1px solid var(--border); border-radius: 35px; padding: 20px; display: flex; align-items: center; gap: 20px; margin-bottom: 15px; border-left: 4px solid var(--accent); }
  .node-icon { width: 55px; height: 55px; background: rgba(0,247,255,0.05); border: 1px solid var(--neon); border-radius: 20px; display: flex; align-items: center; justify-content: center; font-size: 26px; }
  .node-info h4 { margin: 0; font-size: 14px; letter-spacing: 0.5px; }
  .node-info p { margin: 4px 0 0; font-size: 10px; opacity: 0.5; }
  .btn-activate { background: var(--neon); color: #000; padding: 10px 16px; border-radius: 12px; font-size: 10px; font-weight: 800; cursor: pointer; margin-left: auto; border: none; }

  /* NAV */
  .nav { position: fixed; bottom: 30px; left: 20px; right: 20px; background: rgba(10,10,10,0.9); backdrop-filter: blur(20px); display: none; justify-content: space-around; padding: 22px; border-radius: 40px; border: 1px solid var(--border); z-index: 1000; }
  .nav-item { font-size: 9px; opacity: 0.4; font-weight: 800; cursor: pointer; color: #fff; text-transform: uppercase; }
  .nav-item.active { opacity: 1; color: var(--neon); }
</style>
</head>
<body>

<div id="authPage">
    <div class="auth-card">
        <div class="auth-logo">VERBOSE</div>
        <p style="font-size:9px; opacity:0.4; letter-spacing:3px;">QUANTUM DATA CENTER</p>
        <div id="authBox">
            <input id="authPhone" type="number" placeholder="Mobile Node ID">
            <input id="authPass" type="password" placeholder="Cloud Key (Password)">
            <button class="btn-primary" onclick="handleAuth()">LOGIN SYSTEM</button>
            
            <p id="authSwitch" style="font-size:10px; margin-top:20px; cursor:pointer; color:var(--neon);" onclick="toggleAuth()">New Operator? Create Account</p>
            
            <a href="https://wa.me/923705519562?text=Hi%20Admin,%20I%20forgot%20my%20VERBOSE%20password." class="help-link">
                ⚡ FORGOT PASSWORD? CHAT WITH HELP DESK
            </a>
        </div>
    </div>
</div>

<header id="adminTrigger">
    <div class="logo-text">VERBOSE</div>
    <div style="font-size:8px; opacity:0.5; margin-top:8px; letter-spacing:4px;">NYC CLOUD v15.0</div>
</header>

<div id="dash" class="page">
    <div class="balance-card">
        <small style="opacity:0.5; text-transform:uppercase; letter-spacing:1px;">Quantum Balance</small>
        <div style="font-size:32px; font-weight:800; margin-top:10px;">PKR <span id="uBal">0.00</span></div>
    </div>
    
    <h3 style="letter-spacing:2px; font-size:11px; margin: 0 0 20px 5px; opacity:0.7;">ACTIVE MINING SERVERS</h3>
    <div id="nodeGrid"></div>
</div>

<nav class="nav" id="mainNav">
    <div class="nav-item active" onclick="tab('dash', this)">Nodes</div>
    <div class="nav-item" onclick="tab('chatPage', this)">Chat</div>
    <div class="nav-item" onclick="tab('finance', this)">Wallet</div>
    <div class="nav-item" onclick="logout()">Exit</div>
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
  let isLogin = true;

  // --- RENDER 25 NODES ---
  const icons = ["📡", "🛰️", "🚀", "🧬", "💎", "⚡", "🌀"];
  const grid = document.getElementById('nodeGrid');
  for(let i=1; i<=25; i++) {
    let price = 500 * i;
    let icon = icons[i % icons.length];
    grid.innerHTML += `
      <div class="node-card">
        <div class="node-icon">${icon}</div>
        <div class="node-info">
            <h4>Quantum Server v.${i}</h4>
            <p>Investment: PKR ${price} | Daily Yield: PKR ${50*i}</p>
        </div>
        <button class="btn-activate" onclick="buyNode(${price})">BUY</button>
      </div>`;
  }

  // --- LOGIN LOGIC ---
  window.handleAuth = async () => {
    const phone = document.getElementById('authPhone').value;
    const pass = document.getElementById('authPass').value;
    if(!phone || !pass) return alert("Sweetie, please fill all fields!");

    const userRef = ref(db, `users/${phone}`);
    const snap = await get(userRef);

    if(isLogin) {
        if(snap.exists() && snap.val().pass === pass) {
            localStorage.setItem('v_user', phone);
            location.reload();
        } else alert("Error: Security Key Mismatch!");
    } else {
        if(snap.exists()) return alert("This Node ID is already active!");
        await set(userRef, { pass, balance: 0, status: 'Active' });
        localStorage.setItem('v_user', phone);
        location.reload();
    }
  };

  window.toggleAuth = () => {
    isLogin = !isLogin;
    document.getElementById('authSwitch').innerText = isLogin ? "New Operator? Create Account" : "Access Existing Node ID";
  };

  window.logout = () => { localStorage.clear(); location.reload(); };

  // --- UI INIT ---
  const user = localStorage.getItem('v_user');
  if(user) {
    document.getElementById('authPage').style.display = 'none';
    document.getElementById('mainNav').style.display = 'flex';
    document.getElementById('dash').classList.add('active');
    onValue(ref(db, `users/${user}`), s => {
        document.getElementById('uBal').innerText = (s.val().balance || 0).toLocaleString();
    });
  }

  window.tab = (id, el) => {
    document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
    document.getElementById(id).classList.add('active');
    document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
    el.classList.add('active');
  };
</script>
</body>
</html>
