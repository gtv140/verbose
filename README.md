<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>VERBOSE</title>
<style>
:root{--neon:#00f7ff;--accent:#ff5cff;--dark:#070707}
*{box-sizing:border-box}
body{margin:0;font-family:Inter,Arial,sans-serif;background:var(--dark);color:#fff;overflow-x:hidden}
header{text-align:center;padding:24px 12px;font-size:28px;font-weight:800;letter-spacing:2px;background:linear-gradient(90deg,var(--neon),var(--accent));-webkit-background-clip:text;-webkit-text-fill-color:transparent}
.login-box,.page{max-width:430px;margin:18px auto;background:linear-gradient(180deg,rgba(255,255,255,0.02),rgba(255,255,255,0.01));padding:18px;border-radius:12px;border:1px solid rgba(0,255,240,0.06)}
input,button,select{width:100%;padding:10px;margin-top:10px;border-radius:8px;border:1px solid rgba(0,255,240,0.08);background:transparent;color:#e6f7fb;outline:none;font-size:14px}
button{background:linear-gradient(90deg,var(--neon),var(--accent));border:none;color:#001;font-weight:700;cursor:pointer;padding:11px;border-radius:10px;transition:0.15s}
button:hover{transform:translateY(-2px);box-shadow:0 10px 30px rgba(0,0,0,0.5)}
.nav{position:fixed;bottom:0;left:0;right:0;background:linear-gradient(180deg,rgba(255,255,255,0.02),rgba(255,255,255,0.01));display:flex;justify-content:space-around;padding:12px 6px;border-top:1px solid rgba(0,255,240,0.04);font-size:14px;gap:6px}
.nav div{text-align:center;cursor:pointer;width:64px}
.nav div .ico{font-size:20px;display:block;margin-bottom:4px}
.hidden{display:none}
.plan-box{border:1px solid rgba(0,255,240,0.06);padding:12px;margin:10px 0;border-radius:10px;background:linear-gradient(180deg,rgba(255,255,255,0.01),rgba(0,0,0,0.04));display:flex;gap:12px;align-items:center;justify-content:space-between}
.plan-box .meta b{display:block;margin-bottom:6px;font-size:15px}
.countdown{font-weight:700;color:var(--neon)}
.user-box{display:flex;justify-content:space-between;padding:12px;background:rgba(0,255,240,0.03);border-radius:12px;margin-bottom:12px}
.deposit-box,input[type=file]{margin-top:10px;width:100%}
.deposit-box label{display:block;margin-top:8px}
@media(max-width:480px){.login-box,.page{margin:12px;padding:14px}.nav div{width:48px}header{font-size:22px}}
</style>
</head>
<body>
<header>VERBOSE</header>

<!-- LOGIN -->
<div id="loginPage" class="login-box">
<h2>Login / Signup</h2>
<select id="userOption"><option value="login">Login</option><option value="signup">Signup</option></select>
<input id="user" placeholder="Username" />
<input id="pass" placeholder="Password" type="password" />
<button onclick="login()">Submit</button>
<p class="small">Tip: Use same browser/device to keep account saved.</p>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
<div class="user-box">
<div>
<div id="dashUser">—</div>
<div class="small">Member since: <span id="dashSince">—</span></div>
</div>
<div>
<div>Balance: Rs <span id="dashBalance">0</span></div>
<div>Daily: Rs <span id="dashDaily">0</span></div>
</div>
</div>

<h2>Plans</h2>
<div id="plansList"></div>

<div class="deposit-box hidden" id="depositBox">
<h3>Deposit</h3>
<label>Method</label>
<select id="depositMethod" onchange="updateDepositNumber()">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<label>Deposit Number</label>
<input id="depositNumber" readonly/>
<button onclick="copyDepositNumber()">Copy Number</button>
<label>Amount</label>
<input id="depositAmount" readonly/>
<label>Transaction ID</label>
<input id="depositTxId" placeholder="TX ID"/>
<label>Upload Proof</label>
<input type="file" id="depositProof"/>
<button onclick="submitDeposit()">Submit Deposit</button>
</div>

<div style="margin-top:12px">
<button onclick="logout()">Logout</button>
</div>

<div class="small" style="margin-top:12px;color:rgba(230,247,251,0.8)">
VERBOSE Company — Secure, Transparent & Professional.<br>
Contact Admin: WhatsApp 03705519562 | Email: rock.earn92@gmail.com<br>
Company Registration: #123456789 | Serving Since 2022 | Trusted by 10k+ Users
</div>
</div>

<div class="nav hidden" id="bottomNav">
<div onclick="showPage('dashboard')"><span class="ico">🏠</span>Home</div>
</div>

<script>
let currentUser = localStorage.getItem('verbose_user')||null;
let balance=parseFloat(localStorage.getItem('verbose_balance'))||0;
let dailyProfit=parseFloat(localStorage.getItem('verbose_daily'))||0;
let plansData=[];

// Special 3x plans (200-3000)
for(let i=1;i<=7;i++){
  let invest=200*i*2; let multiplier=3; let days=5+i;
  plansData.push({id:i,name:`Special Plan ${i}`,invest,total:Math.round(invest*multiplier),multiplier,daily:Math.round(invest*(multiplier-1)/days),days,countdown:Date.now()+24*3600*1000,offer:true});
}
// Normal plans 3000-30000
for(let i=8;i<=30;i++){
  let invest=3000+(i-8)*1000; let multiplier=2.5; let days=10+i;
  plansData.push({id:i,name:`Plan ${i}`,invest,total:Math.round(invest*multiplier),multiplier,daily:Math.round(invest*(multiplier-1)/days),days,countdown:Date.now()+24*3600*1000,offer:false});
}

function login(){
  const u=document.getElementById('user').value.trim();
  if(!u){alert("Enter username");return;}
  currentUser=u;
  localStorage.setItem('verbose_user',u);
  localStorage.setItem('verbose_balance','0');
  localStorage.setItem('verbose_daily','0');
  document.getElementById('dashUser').innerText=u;
  document.getElementById('dashBalance').innerText='0';
  document.getElementById('dashDaily').innerText='0';
  document.getElementById('dashSince').innerText=new Date().toLocaleDateString();
  document.getElementById('loginPage').classList.add('hidden');
  document.getElementById('dashboard').classList.remove('hidden');
  document.getElementById('bottomNav').classList.remove('hidden');
  renderPlans();
}

function logout(){
  currentUser=null;
  localStorage.removeItem('verbose_user');
  document.getElementById('loginPage').classList.remove('hidden');
  document.getElementById('dashboard').classList.add('hidden');
  document.getElementById('bottomNav').classList.add('hidden');
  document.getElementById('user').value='';
  document.getElementById('pass').value='';
}

function showPage(id){
  document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));
  document.getElementById(id).classList.remove('hidden');
}

function renderPlans(){
  const container=document.getElementById('plansList'); container.innerHTML='';
  plansData.forEach(p=>{
    let div=document.createElement('div'); div.className='plan-box';
    div.innerHTML=`<div class="meta"><b>${p.name}</b>
    <div class="small">Invest: Rs ${p.invest} | Total: Rs ${p.total} | Daily: Rs ${p.daily} | Days: ${p.days} ${p.offer?'<span style="color:var(--neon)">Special 24h!</span>':''}</div>
    <div class="small countdown" id="timer${p.id}"></div>
    </div>
    <button onclick="buyPlan(${p.id})">Buy Now</button>`;
    container.appendChild(div);
    startCountdown(`timer${p.id}`,p.countdown);
  });
}

function startCountdown(id,endTime){
  const el=document.getElementById(id);
  const interval=setInterval(()=>{
    let now=Date.now(); let diff=endTime-now;
    if(diff<=0){el.innerText='Offer ended';clearInterval(interval);return;}
    let h=Math.floor(diff/3600000); let m=Math.floor(diff%3600000/60000); let s=Math.floor(diff%60000/1000);
    el.innerText=`${h}h ${m}m ${s}s`;
  },1000);
}

function buyPlan(id){
  const plan=plansData.find(p=>p.id===id);
  document.getElementById('depositBox').classList.remove('hidden');
  document.getElementById('depositAmount').value=plan.invest;
  updateDepositNumber();
  alert(`Plan Selected: ${plan.name}\nDeposit Amount: Rs ${plan.invest}`);
}

function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}

function copyDepositNumber(){const num=document.getElementById('depositNumber');num.select();document.execCommand('copy');alert("Deposit number copied!");}

function submitDeposit(){
  let amount=parseFloat(document.getElementById('depositAmount').value);
  let tx=document.getElementById('depositTxId').value.trim();
  let proof=document.getElementById('depositProof').files[0];
  if(!tx || !proof){alert("Transaction ID & proof required"); return;}
  alert(`Deposit submitted!\nAmount: Rs ${amount}\nAdmin will verify soon.`);
}
</script>
</body>
</html>
