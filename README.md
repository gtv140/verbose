<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE Premium Advanced</title>

<style>
*{
  margin:0;
  padding:0;
  box-sizing:border-box;
  font-family:Arial, sans-serif;
}

body{
  background:#000;
  color:#fff;
  overflow-x:hidden;
}

/* HEADER */
.header{
  width:100%;
  padding:18px;
  background:#0f0f0f;
  border-bottom:2px solid #111;
  text-align:center;
  font-size:22px;
  font-weight:bold;
  letter-spacing:1px;
  color:#00f7ff;
  text-shadow:0 0 8px #00f7ff;
}

/* USER BOX */
.userBox{
  width:92%;
  margin:15px auto;
  background:#0d0d0d;
  padding:15px;
  border-radius:12px;
  box-shadow:0 0 12px #00f7ff55;
  border:1px solid #00f7ff33;
}

.userBox h3{
  font-size:18px;
  color:#00f7ff;
  margin-bottom:8px;
  text-shadow:0 0 8px #00f7ff;
}

.user-data{
  font-size:14px;
  margin-top:5px;
  color:#eee;
}

/* DASHBOARD */
.dashboard{
  width:92%;
  margin:20px auto;
}

.box{
  width:100%;
  background:#0c0c0c;
  padding:15px;
  margin-bottom:15px;
  border-radius:12px;
  border:1px solid #00f7ff44;
  box-shadow:0 0 10px #00f7ff44;
}

.box-title{
  font-size:16px;
  color:#00eaff;
  margin-bottom:6px;
}

.box-amount{
  font-size:22px;
  font-weight:bold;
  margin-top:5px;
}

/* BOTTOM NAV */
.bottomNav{
  width:100%;
  position:fixed;
  bottom:0;
  left:0;
  background:#0e0e0e;
  padding:10px 0;
  display:flex;
  justify-content:space-around;
  border-top:1px solid #00f7ff55;
}

.navItem{
  text-align:center;
  font-size:13px;
  color:#fff;
}

.navItem.active{
  color:#00f7ff;
  text-shadow:0 0 6px #00f7ff;
}

.navIcon{
  font-size:20px;
  margin-bottom:3px;
}
</style>
</head>

<body>

<div class="header">VERBOSE PREMIUM</div>

<div class="userBox">
  <h3>User Profile</h3>
  <div class="user-data">Name: <span id="uName">Loading...</span></div>
  <div class="user-data">Member Since: <span id="uSince">--</span></div>
  <div class="user-data">Active Plans: <span id="activePlans">0</span></div>
</div>

<div class="dashboard">
  
  <div class="box">
    <div class="box-title">Main Balance</div>
    <div class="box-amount" id="mainBalance">PKR 0</div>
  </div>

  <div class="box">
    <div class="box-title">Daily Profit</div>
    <div class="box-amount" id="dailyProfit">PKR 0</div>
  </div>

  <div class="box">
    <div class="box-title">Total Profit Earned</div>
    <div class="box-amount" id="totalProfit">PKR 0</div>
  </div>

</div>

<!-- BOTTOM NAVIGATION -->
<div class="bottomNav">
  <div class="navItem active" onclick="showPage('dashboardPage')">
    <div class="navIcon">🏠</div>Home
  </div>
  <div class="navItem" onclick="showPage('plansPage')">
    <div class="navIcon">📦</div>Plans
  </div>
  <div class="navItem" onclick="showPage('historyPage')">
    <div class="navIcon">📜</div>History
  </div>
  <div class="navItem" onclick="showPage('depositPage')">
    <div class="navIcon">💰</div>Deposit
  </div>
  <div class="navItem" onclick="showPage('supportPage')">
    <div class="navIcon">📞</div>Support
  </div>
</div>

<!-- PAGES START -->
<div id="dashboardPage"></div>
<div id="plansPage" style="display:none;"></div>
<div id="historyPage" style="display:none;"></div>
<div id="depositPage" style="display:none;"></div>
<div id="supportPage" style="display:none;"></div>

<!-- SCRIPT START -->
<script>
/* STABLE LOGIN – NO AUTO LOGOUT ON REFRESH */
let user = JSON.parse(localStorage.getItem("verboseUser")) || null;

function loadUser(){
  if(!user){
     user = {name:"User", since:"2025", balance:0, depositHistory:[], plans:[]};
     localStorage.setItem("verboseUser", JSON.stringify(user));
  }

  document.getElementById("uName").innerText = user.name;
  document.getElementById("uSince").innerText = user.since;
  document.getElementById("mainBalance").innerText = "PKR " + user.balance;
  document.getElementById("activePlans").innerText = user.plans.length;
}
loadUser();

function showPage(pg){
  document.getElementById("dashboardPage").style.display="none";
  document.getElementById("plansPage").style.display="none";
  document.getElementById("historyPage").style.display="none";
  document.getElementById("depositPage").style.display="none";
  document.getElementById("supportPage").style.display="none";
  
  document.getElementById(pg).style.display="block";
}
<script>
/* ===== PLANS DATA ===== */
let plans = [];
for(let i=1;i<=25;i++){
  let invest = Math.round(200 + (i-1)*(30000-200)/24);
  let days = 25 + Math.floor((i-1)*(78-25)/24);
  let multiplier = i<=5 ? 2.8 : 2.3;
  plans.push({
    id:i,
    name:`Plan ${i}`,
    invest,
    days,
    total:Math.round(invest*multiplier),
    offer:i<=5,
    coming:false
  });
}

/* ===== RENDER PLANS ===== */
function renderPlans(){
  const container = document.getElementById("plansPage");
  container.innerHTML = "";
  plans.forEach(p=>{
    const box = document.createElement("div");
    box.style.background="#0d0d0d";
    box.style.border="1px solid #00f7ff44";
    box.style.borderRadius="10px";
    box.style.margin="8px";
    box.style.padding="12px";
    box.style.boxShadow="0 0 12px #00f7ff33";
    const daily = Math.round(p.total/p.days);
    box.innerHTML = `
      <div style="font-weight:bold;color:#00f7ff;margin-bottom:6px;">${p.name}</div>
      <div style="font-size:13px;color:#ccc">${p.offer ? "Special 24h Offer" : p.coming ? "Coming Soon" : "Standard Plan"}</div>
      <div style="font-size:13px;margin-top:6px">Invest: PKR ${p.invest} · Days: ${p.days}</div>
      <div style="font-size:13px">Total: PKR ${p.total} · Daily: PKR ${daily}</div>
      ${p.coming ? '<div style="color:#888;font-size:12px;margin-top:4px">COMING SOON</div>' : `<button onclick="buyPlan(${p.id})" style="margin-top:6px;padding:6px 12px;border:none;border-radius:6px;background:#00f7ff;color:#001;cursor:pointer;">Buy Now</button>`}
      <div id="countdown${p.id}" style="font-size:12px;color:#00f7ff;margin-top:4px;"></div>
    `;
    container.appendChild(box);

    if(p.offer && !p.coming) startCountdown(p.id, 24*3600);
  });
}
renderPlans();

/* ===== COUNTDOWN FOR SPECIAL OFFERS ===== */
let countdownIntervals = {};
function startCountdown(id, seconds){
  const display = document.getElementById("countdown"+id);
  const key = "offer_end_"+id;
  let end = parseInt(localStorage.getItem(key) || "0");
  if(!end || end<Date.now()){
    end = Date.now() + seconds*1000;
    localStorage.setItem(key,end);
  }
  clearInterval(countdownIntervals[id]);
  countdownIntervals[id] = setInterval(()=>{
    const diff = Math.floor((end-Date.now())/1000);
    if(diff<=0){
      display.innerText="Offer Ended";
      clearInterval(countdownIntervals[id]);
      return;
    }
    const h=Math.floor(diff/3600);
    const m=Math.floor((diff%3600)/60);
    const s=diff%60;
    display.innerText=`${h}h ${m}m ${s}s`;
  },1000);
}

/* ===== BUY PLAN ===== */
function buyPlan(id){
  const plan = plans.find(x=>x.id===id);
  if(!plan) return;
  
  alert(`You selected ${plan.name}. Please deposit PKR ${plan.invest} to proceed.`);
  
  // add to user plans
  if(!user.plans.find(x=>x.id===id)){
    user.plans.push({id:id,lastUpdate:Date.now(),dailyProfit:Math.round(plan.total/plan.days)});
    localStorage.setItem("verboseUser",JSON.stringify(user));
  }

  document.getElementById("depositPage").innerHTML=`
    <div style="margin:12px;">
      <div style="font-weight:bold;margin-bottom:8px;color:#00f7ff;">Deposit for ${plan.name}</div>
      <div style="margin-bottom:6px;">Amount: PKR ${plan.invest}</div>
      <div style="margin-bottom:6px;">Method: JazzCash / EasyPaisa</div>
      <button onclick="submitDeposit(${plan.invest})" style="padding:6px 12px;border:none;border-radius:6px;background:#00f7ff;color:#001;cursor:pointer;">Submit Deposit</button>
    </div>
  `;
  showPage("depositPage");
}

/* ===== DEPOSIT SUBMISSION ===== */
function submitDeposit(amount){
  user.balance += amount;
  localStorage.setItem("verboseUser",JSON.stringify(user));
  document.getElementById("mainBalance").innerText="PKR "+user.balance;
  alert("Deposit successful! Admin will verify your transaction.");
  showPage("dashboardPage");
}

/* ===== DAILY PROFIT ENGINE ===== */
function addDailyProfit(){
  const now=Date.now();
  user.plans.forEach(p=>{
    const last=p.lastUpdate||now;
    const daysPassed=Math.floor((now-last)/(1000*60*60*24));
    if(daysPassed>0){
      user.balance += p.dailyProfit*daysPassed;
      p.lastUpdate=now;
    }
  });
  localStorage.setItem("verboseUser",JSON.stringify(user));
  document.getElementById("mainBalance").innerText="PKR "+user.balance;
}
setInterval(addDailyProfit,60000); // every 1 min auto-update
<script>
/* ===== REFERRAL COPY ===== */
function copyReferral(){
  const ref = document.getElementById('refLink');
  if(!ref) return;
  navigator.clipboard?.writeText(ref.value) || ref.select() && document.execCommand('copy');
  alert("Referral link copied!");
}

/* ===== WITHDRAWAL ===== */
function submitWithdraw(){
  const amt = parseFloat(document.getElementById('withdrawAmount').value);
  const acc = document.getElementById('withdrawAccount').value.trim();
  if(!amt || !acc){ alert("Enter amount & account"); return; }
  if(amt > user.balance){ alert("Insufficient balance"); return; }
  user.balance -= amt;
  localStorage.setItem("verboseUser", JSON.stringify(user));
  document.getElementById("mainBalance").innerText="PKR "+user.balance;
  alert(`Withdrawal request of PKR ${amt} received. Admin will process it.`);
  document.getElementById('withdrawAmount').value='';
  document.getElementById('withdrawAccount').value='';
}

/* ===== SUPPORT LINKS ===== */
function openSupport(type){
  if(type==="whatsapp"){
    window.open("https://chat.whatsapp.com/Kmaiv3VdSo09rio4qcRTRM", "_blank");
  } else if(type==="email"){
    window.open("mailto:rock.earn92@gmail.com");
  }
}

/* ===== LOGOUT ===== */
function logout(){
  localStorage.removeItem("verbose_user");
  showPage("loginPage");
}

/* ===== INITIAL SETUP ===== */
let user = JSON.parse(localStorage.getItem("verboseUser")) || {username:null,balance:0,dailyProfit:0,plans:[]};
if(user.username){
  document.getElementById("dashUser").innerText=user.username;
  document.getElementById("mainBalance").innerText="PKR "+user.balance;
  document.getElementById("bottomNav").classList.remove("hidden");
  showPage("dashboardPage");
}

/* ===== NAVIGATION ===== */
function showPage(id){
  document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));
  document.getElementById(id).classList.remove('hidden');
}

/* ===== AUTO DAILY PROFIT UPDATE ===== */
setInterval(()=>{
  const now = Date.now();
  user.plans.forEach(p=>{
    const last = p.lastUpdate || now;
    const daysPassed = Math.floor((now-last)/(1000*60*60*24));
    if(daysPassed>0){
      user.balance += p.dailyProfit*daysPassed;
      p.lastUpdate = now;
    }
  });
  localStorage.setItem("verboseUser",JSON.stringify(user));
  const balEl = document.getElementById("mainBalance");
  if(balEl) balEl.innerText="PKR "+user.balance;
},60000); // every 1 minute
</script>
