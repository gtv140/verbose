<VERBOSE><html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE</title>
<style>
body{margin:0;font-family:Arial,sans-serif;background:#111;color:#0ff;}
header{text-align:center;font-size:26px;font-weight:bold;padding:20px;text-shadow:0 0 5px #0ff;}
.container{max-width:400px;margin:20px auto;padding:20px;background:#222;border-radius:8px;}
input,button,select{width:100%;padding:10px;margin:5px 0;border-radius:5px;border:1px solid #0ff;background:#111;color:#0ff;}
button{cursor:pointer;background:#0ff;color:#000;font-weight:bold;}
button:hover{opacity:0.8;}
.hidden{display:none;}
.user-box{background:#001f2f;padding:10px;border-radius:5px;margin-bottom:10px;}
.plan-box{border:1px solid #0ff;padding:10px;margin:10px 0;border-radius:5px;background:#111;}
.offer{color:#ff0;font-weight:bold;}
.copy-btn{background:#0ff;color:#000;padding:5px;border:none;margin-top:5px;border-radius:5px;cursor:pointer;}
#countdown{font-weight:bold;color:#0ff;}
.nav{display:flex;justify-content:space-around;padding:10px 0;border-top:1px solid #0ff;position:fixed;bottom:0;width:100%;background:#111;}
.nav div{text-align:center;font-size:12px;cursor:pointer;}
</style>
</head>
<body><header>V<span style="color:#0ff;">E</span>R<span style="color:#0ff;">B</span>O<span style="color:#0ff;">S</span>E</header><div id="loginPage" class="container">
<h2>Login / Signup</h2>
<input id="user" placeholder="Username">
<input id="pass" placeholder="Password" type="password">
<button onclick="login()">Login</button>
</div><div id="dashboard" class="container hidden">
<div class="user-box">Username: <span id="dashUser"></span> | Balance: Rs <span id="dashBalance">0</span></div>
<h2>Dashboard</h2>
<p>Welcome to VERBOSE! Trusted, secure, and reliable platform.</p>
<button onclick="logout()">Logout</button>
</div><div id="plans" class="container hidden">
<h2>Plans</h2>
<div id="plansList"></div>
</div><div id="deposit" class="container hidden">
<h2>Deposit</h2>
<label>Method</label>
<select id="depositMethod" onchange="updateDepositNumber()">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<input id="depositNumber" readonly>
<button class="copy-btn" onclick="copyDepositNumber()">Copy Number</button>
<label>Amount</label>
<input id="depositAmount" readonly>
<label>Transaction ID</label>
<input id="depositTxId" placeholder="TX ID">
<label>Upload Proof</label>
<input type="file" id="depositProof">
<button onclick="submitDeposit()">Submit Deposit</button>
</div><div id="withdrawal" class="container hidden">
<h2>Withdrawal</h2>
<label>Method</label>
<select id="withdrawMethod">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
<option value="bank">Bank</option>
</select>
<input id="withdrawUsername" readonly>
<input id="withdrawAccount" placeholder="Account Number">
<input id="withdrawAmount" placeholder="Amount">
<button onclick="submitWithdraw()">Request Withdrawal</button>
</div><div id="support" class="container hidden">
<h2>Contact Support</h2>
<p>For any deposit/withdraw issues, contact:</p>
<p>WhatsApp: <a href="https://chat.whatsapp.com/Kmaiv3VdSo09rio4qcRTRM" target="_blank">Join Group</a></p>
<p>Email: <a href="mailto:rock.earn92@gmail.com">rock.earn92@gmail.com</a></p>
</div><div class="nav hidden" id="bottomNav">
<div onclick="showPage('dashboard')">🏠<br>Home</div>
<div onclick="showPage('plans')">📦<br>Plans</div>
<div onclick="showPage('deposit')">💰<br>Deposit</div>
<div onclick="showPage('withdrawal')">💵<br>Withdraw</div>
<div onclick="showPage('support')">📞<br>Support</div>
</div><script>
let currentUser=localStorage.getItem('verbose_user')||null;
let balance=parseFloat(localStorage.getItem('verbose_balance'))||0;
let plansData=[];
let userPlans=JSON.parse(localStorage.getItem('verbose_userPlans')||'[]');

for(let i=1;i<=25;i++){
  let invest=200+i*100;
  let days=20+i;
  let multiplier=i<=7?3:2.5;
  let offerEnd = Date.now() + (i<=7?24*60*60*1000:0);
  plansData.push({id:i,name:`Plan ${i}`,invest:invest,days:days,total:Math.round(invest*multiplier),multiplier:multiplier,offer:i<=7,countdownEnd:offerEnd});
}

function login(){
  let u=document.getElementById("user").value;
  let p=document.getElementById("pass").value;
  if(!u||!p){alert("Enter username & password");return;}
  currentUser=u;
  localStorage.setItem('verbose_user',currentUser);
  if(!localStorage.getItem('verbose_balance')) localStorage.setItem('verbose_balance','5000');
  balance=parseFloat(localStorage.getItem('verbose_balance'));
  document.getElementById("dashUser").innerText=currentUser;
  document.getElementById("dashBalance").innerText=balance;
  document.getElementById("loginPage").classList.add("hidden");
  document.getElementById("dashboard").classList.remove("hidden");
  document.getElementById("bottomNav").classList.remove("hidden");
  renderPlans();
  updateWithdrawUsername();
}

function logout(){
  currentUser=null;
  localStorage.removeItem('verbose_user');
  document.getElementById("loginPage").classList.remove("hidden");
  document.getElementById("dashboard").classList.add("hidden");
  document.getElementById("bottomNav").classList.add("hidden");
  document.getElementById("user").value='';
  document.getElementById("pass").value='';
}

function showPage(id){
  document.querySelectorAll(".container").forEach(p=>p.classList.add("hidden"));
  document.getElementById(id).classList.remove("hidden");
}

function renderPlans(){
  let list=document.getElementById("plansList"); list.innerHTML='';
  plansData.forEach(p=>{
    let div=document.createElement('div'); div.className='plan-box';
    let countdown=p.offer?`<div id="countdown_${p.id}">Offer ends in: <span id="time_${p.id}"></span></div>`:'';
    div.innerHTML=`<b>${p.name}</b> ${p.offer?'<span class="offer">🔥 24h Offer</span>':''}<br>
    Invest: Rs ${p.invest}<br>
    Days: ${p.days}<br>
    Total Profit: Rs ${p.total}<br>
    Daily Profit: Rs ${Math.round(p.total/p.days)}<br>
    ${countdown}<button onclick="buyPlan(${p.id})">Buy Now</button>`;
    list.appendChild(div);
  });
  startCountdowns();
}

function startCountdowns(){
  plansData.forEach(p=>{
    if(p.offer){
      let timerId=setInterval(()=>{
        let distance=p.countdownEnd-Date.now();
        if(distance<0){distance=0; clearInterval(timerId);}
        let h=Math.floor(distance/(1000*60*60));
        let m=Math.floor((distance%(1000*60*60))/(1000*60));
        let s=Math.floor((distance%(1000*60))/1000);
        let el=document.getElementById('time_'+p.id);
        if(el) el.innerText=`${h}h ${m}m ${s}s`;
      },1000);
    }
  });
}

function buyPlan(id){
  let plan=plansData.find(p=>p.id===id);
  document.getElementById('depositAmount').value=plan.invest;
  document.getElementById('depositMethod').value='jazzcash';
  updateDepositNumber();
  showPage('deposit');
}

const depositNumbers={jazzcash:'03705519562',easypaisa:'03379827882'};
function updateDepositNumber(){document.getElementById('depositNumber').value=depositNumbers[document.getElementById('depositMethod').value];}
function copyDepositNumber(){let num=document.getElementById('depositNumber');num.select();document.execCommand("copy");alert("Deposit number copied!");}

function submitDeposit(){
  let tx=document.getElementById('depositTxId').value.trim();
  let proof=document.getElementById('depositProof').files[0];
  let amount=parseFloat(document.getElementById('depositAmount').value);
  if(!tx||!proof){alert("Fill TX ID & upload proof");return;}
  balance+=amount; localStorage.setItem('verbose_balance',balance);
  document.getElementById('dashBalance').innerText=balance;
  alert("Deposit submitted! Our team will verify and process it shortly.");
  document.getElementById('depositTxId').value=''; document.getElementById('depositProof').value='';
  showPage('dashboard');
}

function updateWithdrawUsername(){document.getElementById('withdrawUsername').value=currentUser;}
function submitWithdraw(){
  let amt=parseFloat(document.getElementById('withdrawAmount').value);
  let acc=document.getElementById('withdrawAccount').value.trim();
  if(!amt||!acc){alert("Enter amount & account"); return;}
  if(amt>balance){alert("Insufficient balance"); return;}
  balance-=amt; localStorage.setItem('verbose_balance',balance);
  document.getElementById('dashBalance').innerText=balance;
  alert(`Withdrawal request of Rs ${amt} received. Our support team will process it.`);
  document.getElementById('withdrawAmount').value=''; document.getElementById('withdrawAccount').value='';
  showPage('dashboard');
}

window.onload=function(){
  if(currentUser){
    document.getElementById("dashUser").innerText=currentUser;
    balance=parseFloat(localStorage.getItem('verbose_balance'));
    document.getElementById("dashBalance").innerText=balance;
    document.getElementById("loginPage").classList.add("hidden");
    document.getElementById("dashboard").classList.remove("hidden");
    document.getElementById("bottomNav").classList.remove("hidden");
    renderPlans();
    updateWithdrawUsername();
  }
};
</script></body>
</html>
