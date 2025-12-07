<!-- PLANS -->
<div id="plans" class="page hidden">
  <h2>Plans</h2>
  <div id="plansList"></div>
</div>

<!-- DEPOSIT -->
<div id="deposit" class="page hidden">
  <h2>Deposit</h2>
  <label>Method</label>
  <select id="depositMethod" onchange="updateDepositNumber()">
    <option value="jazzcash">JazzCash</option>
    <option value="easypaisa">EasyPaisa</option>
  </select>
  <div style="display:flex;gap:8px;align-items:center;margin-top:10px">
    <input id="depositNumber" readonly style="flex:1" />
    <button onclick="copyDepositNumber()" style="width:110px">Copy</button>
  </div>
  <label style="margin-top:12px">Amount</label>
  <input id="depositAmount" readonly />
  <label>Transaction ID</label>
  <input id="depositTxId" placeholder="TX ID" />
  <label>Upload Proof</label>
  <input type="file" id="depositProof" />
  <button onclick="submitDeposit()">Submit Deposit</button>
  <p class="small" style="margin-top:8px">After submitting, share proof with admin on WhatsApp or Email for verification.</p>
</div>

<!-- WITHDRAWAL -->
<div id="withdrawal" class="page hidden">
  <h2>Withdrawal</h2>
  <label>Method</label>
  <select id="withdrawMethod">
    <option value="jazzcash">JazzCash</option>
    <option value="easypaisa">EasyPaisa</option>
    <option value="bank">Bank</option>
  </select>
  <input id="withdrawUsername" readonly placeholder="Username" />
  <input id="withdrawAccount" placeholder="Account Number (manual)" />
  <input id="withdrawAmount" placeholder="Amount" />
  <button onclick="submitWithdraw()">Request Withdrawal</button>
  <p class="small" style="margin-top:8px">Requests are reviewed by admin. Keep your proof ready.</p>
</div>

<!-- SUPPORT -->
<div id="support" class="page hidden">
  <div class="support-box">
    <h2>Contact Administration</h2>
    <div class="support-grid">
      <div class="support-item">
        <div class="icon">💬</div>
        <div>
          <p><strong>WhatsApp Support</strong></p>
          <p class="small">Fastest support — join our group or message the admin directly.</p>
          <p style="margin-top:6px"><a href="https://chat.whatsapp.com/Kmaiv3VdSo09rio4qcRTRM" target="_blank">Join WhatsApp Group</a></p>
        </div>
      </div>
      <div class="support-item">
        <div class="icon">📧</div>
        <div>
          <p><strong>Email Support</strong></p>
          <p class="small">For formal queries and proofs, email our support team.</p>
          <p style="margin-top:6px"><a href="mailto:rock.earn92@gmail.com">rock.earn92@gmail.com</a></p>
        </div>
      </div>
    </div>
    <p class="support-note">Important: Always use the official support channels above to verify deposits/withdrawals. Avoid sharing sensitive information publicly.</p>
    <p style="color:var(--neon);font-weight:800;text-align:center;margin-top:10px">VERBOSE — Secure. Transparent. Professional.</p>
  </div>
</div>

<!-- NAVIGATION -->
<div id="bottomNav" class="nav hidden">
  <div onclick="showPage('dashboard')"><span class="ico">🏠</span>Home</div>
  <div onclick="showPage('plans')"><span class="ico">📦</span>Plans</div>
  <div onclick="showPage('deposit')"><span class="ico">💰</span>Deposit</div>
  <div onclick="showPage('withdrawal')"><span class="ico">💵</span>Withdraw</div>
  <div onclick="showPage('support')"><span class="ico">📞</span>Support</div>
</div>

<script>
// USERS & PLANS
let currentUser = localStorage.getItem('verbose_user')||null;
let balance = parseFloat(localStorage.getItem('verbose_balance'))||0;
let dailyProfit = parseFloat(localStorage.getItem('verbose_daily'))||0;
let userPlans = JSON.parse(localStorage.getItem('verbose_userPlans')||'[]');
let referralCode = localStorage.getItem('verbose_referral')||'';
let plansData = [];
for(let i=1;i<=30;i++){
  let invest,days,multiplier,coming=false;
  if(i<=7){ invest=Math.round(200 + (i-1)*(3000-200)/6); multiplier=3; days=20 + Math.floor((i-1)*(50-20)/6);}
  else if(i<=25){ invest=Math.round(200 + (i-1)*(30000-200)/24); multiplier=2.5; days=20 + Math.floor((i-1)*(70-20)/24);}
  else{ invest=Math.round(200 + (i-1)*(30000-200)/24); multiplier=2.5; days=20 + Math.floor((i-1)*(70-20)/24); coming=true;}
  plansData.push({id:i,name:`Plan ${i}`,invest,days,total:Math.round(invest*multiplier),multiplier,offer:i<=7,coming});
}

// AUTH
function login(){const option=document.getElementById('userOption').value;const u=document.getElementById('user').value.trim();const p=document.getElementById('pass').value.trim();if(!u||!p){alert("Enter username & password");return;}currentUser=u;localStorage.setItem('verbose_user',currentUser);referralCode=referralCode||Math.random().toString(36).substring(2,10);localStorage.setItem('verbose_referral',referralCode);if(option==='signup'){localStorage.setItem('verbose_balance','0');localStorage.setItem('verbose_daily','0');userPlans=[];localStorage.setItem('verbose_userPlans',JSON.stringify(userPlans));}balance=parseFloat(localStorage.getItem('verbose_balance'))||0;dailyProfit=parseFloat(localStorage.getItem('verbose_daily'))||0;document.getElementById('dashUser').innerText=currentUser;document.getElementById('dashBalance').innerText=balance;document.getElementById('dashDaily').innerText=dailyProfit;document.getElementById('dashSince').innerText=new Date().toLocaleDateString();document.getElementById('refLink').value=`https://gtv140.github.io/verbose/?ref=${referralCode}`;document.getElementById('loginPage').classList.add('hidden');document.getElementById('dashboard').classList.remove('hidden');document.getElementById('bottomNav').classList.remove('hidden');renderPlans();}

// LOGOUT
function logout(){currentUser=null;localStorage.removeItem('verbose_user');document.getElementById('loginPage').classList.remove('hidden');document.getElementById('dashboard').classList.add('hidden');document.getElementById('bottomNav').classList.add('hidden');document.getElementById('user').value='';document.getElementById('pass').value='';}

// PAGES
function showPage(id){document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));document.getElementById(id).classList.remove('hidden');}

// COPY
function copyDepositNumber(){const num=document.getElementById('depositNumber');num.select();document.execCommand('copy');alert("Deposit number copied!");}
function copyReferral(){const ref=document.getElementById('refLink');ref.select();document.execCommand('copy');alert("Referral link copied!");}

// RENDER PLANS
function renderPlans(){const list=document.getElementById('plansList');list.innerHTML='';plansData.forEach(p=>{const div=document.createElement('div');div.className='plan-box';const daily=Math.round(p.total/p.days);div.innerHTML=`<div class="meta"><b>${p.name}</b><div class="small">${p.offer?'Special 24h Offer':(p.coming?'Coming Soon':'Standard Plan')}</div><div class="small" style="margin-top:8px">Invest: Rs ${p.invest} · Days: ${p.days}</div><div class="small">Total Profit: Rs ${p.total} · Daily: Rs ${daily}</div></div><div class="actions"><div id="countdownWrap${p.id}" style="margin-bottom:6px"><span class="countdown" id="countdown${p.id}"></span></div>${p.coming?'<div class="small" style="color:rgba(255,255,255,0.6)">COMING SOON</div>':`<button onclick="buyPlan(${p.id})">Buy Now</button>`}</div>`;list.appendChild(div);if(p.offer && !p.coming) startCountdown(p.id,24*3600);});}

// COUNTDOWN
let countdownIntervals={};
function startCountdown(id,seconds){
    const display=document.getElementById(`countdown${id}`);
    if(!display) return;
    let remaining=seconds;
    countdownIntervals[id]=setInterval(()=>{
        if(remaining<=0){display.innerText="Offer ended";clearInterval(countdownIntervals[id]);return;}
        let h=Math.floor(remaining/3600);
        let m=Math.floor((remaining%3600)/60);
        let s=remaining%60;
        display.innerText=`Offer ends in ${h}h ${m}m ${s}s`;
        remaining--;
    },1000);
}

// BUY PLAN
function buyPlan(id){
    const plan=plansData.find(p=>p.id===id);
    if(!plan || plan.coming){alert("This plan is coming soon!");return;}
    if(balance<plan.invest){alert(`Insufficient balance! Required Rs ${plan.invest}`);return;}
    balance-=plan.invest;
    dailyProfit+=Math.round(plan.total/plan.days);
    userPlans.push({id:plan.id,name:plan.name,invest:plan.invest,daily:Math.round(plan.total/plan.days),daysLeft:plan.days});
    localStorage.setItem('verbose_balance',balance);
    localStorage.setItem('verbose_daily',dailyProfit);
    localStorage.setItem('verbose_userPlans',JSON.stringify(userPlans));
    alert(`${plan.name} purchased successfully!`);
    document.getElementById('dashBalance').innerText=balance;
    document.getElementById('dashDaily').innerText=dailyProfit;
}

// UPDATE DEPOSIT NUMBER
function updateDepositNumber(){
    const method=document.getElementById('depositMethod').value;
    if(method==='jazzcash') document.getElementById('depositNumber').value='03705519562';
    else if(method==='easypaisa') document.getElementById('depositNumber').value='03379827882';
}

// SUBMIT DEPOSIT
function submitDeposit(){
    const amount=document.getElementById('depositAmount').value || "0";
    const tx=document.getElementById('depositTxId').value.trim();
    if(!tx){alert("Enter transaction ID!"); return;}
    alert(`Deposit of Rs ${amount} submitted. Admin will verify shortly.`);
    document.getElementById('depositTxId').value='';
    document.getElementById('depositProof').value='';
}

// SUBMIT WITHDRAW
function submitWithdraw(){
    const method=document.getElementById('withdrawMethod').value;
    const acc=document.getElementById('withdrawAccount').value.trim();
    const amt=parseFloat(document.getElementById('withdrawAmount').value);
    if(!acc || !amt){alert("Enter account and amount!"); return;}
    if(amt>balance){alert("Insufficient balance!"); return;}
    balance-=amt;
    dailyProfit-=Math.round(amt/10);
    localStorage.setItem('verbose_balance',balance);
    localStorage.setItem('verbose_daily',dailyProfit);
    document.getElementById('dashBalance').innerText=balance;
    document.getElementById('dashDaily').innerText=dailyProfit;
    alert(`Withdrawal request of Rs ${amt} submitted to admin.`);
    document.getElementById('withdrawAccount').value='';
    document.getElementById('withdrawAmount').value='';
}

// INIT
document.addEventListener('DOMContentLoaded',()=>{
    if(currentUser){login();}
    updateDepositNumber();
});
</script>
</body>
</html>
