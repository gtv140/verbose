<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>VERBOSE – Earn Daily</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
body{margin:0;padding:0;font-family:Arial;background:#f2f2f2;}
header{background:#111;color:#fff;padding:15px;text-align:center;font-size:22px;font-weight:bold;}
.container{padding:15px;}
.box{background:#fff;padding:15px;margin-bottom:12px;border-radius:8px;box-shadow:0 0 5px rgba(0,0,0,0.1);}
.plan{border:1px solid #ddd;padding:12px;border-radius:8px;margin-bottom:10px;}
button{background:#111;color:#fff;padding:10px;width:100%;border:none;border-radius:6px;margin-top:10px;font-size:16px;}
.copy-btn{background:#008CFF;margin-top:5px;}
input,select{width:100%;padding:10px;margin-top:8px;border-radius:6px;border:1px solid #ccc;}
.fixed-nav{position:fixed;bottom:0;left:0;width:100%;background:#fff;padding:10px;display:flex;justify-content:space-around;border-top:1px solid #ccc;}
.fixed-nav div{text-align:center;font-size:14px;}
#logoutBtn{color:red;font-weight:bold;}
.alert{background:#ffebc7;padding:10px;border-radius:6px;border-left:4px solid orange;margin-bottom:10px;font-size:14px;}
</style>

<script>
let balance = 0;

// COPY NUMBER FUNCTION
function copyNumber(num){
  navigator.clipboard.writeText(num);
  alert("Number Copied: " + num);
}

// BUY PLAN FUNCTION
function buyPlan(amount){
  document.getElementById("depositAmount").value = amount;
  document.getElementById("depositSection").scrollIntoView();
}

// SUBMIT DEPOSIT FUNCTION
function submitDeposit(){
  let amount = Number(document.getElementById("depositAmount").value);
  let txid = document.getElementById("txid").value;
  let proof = document.getElementById("proof").value;

  if(amount <= 0 || txid=="" || proof==""){
    alert("Please fill all deposit fields.");
    return;
  }

  alert("Deposit Submitted. Admin will approve soon.");
  balance += amount;
  document.getElementById("balance").innerText = balance;
}

// WITHDRAW FUNCTION
function withdraw(){
  let w = Number(prompt("Enter withdrawal amount:"));

  if(w > balance){
    alert("Insufficient Balance.");
  } else {
    balance -= w;
    document.getElementById("balance").innerText = balance;
    alert("Withdrawal Request Submitted. Admin will approve.");
  }
}

// PREVENT AUTO LOGOUT ON REFRESH
window.addEventListener("beforeunload", () => {
  localStorage.setItem("vb_balance", balance);
});
window.onload = () => {
  if(localStorage.getItem("vb_balance")){
    balance = Number(localStorage.getItem("vb_balance"));
    document.getElementById("balance").innerText = balance;
  }
};
</script>
</head>

<body>

<header>VERBOSE</header>

<div class="container">

<div class="alert">⚠️ Deposit or withdrawal issue? Contact Administration immediately.</div>

<div class="box">
<h3>Your Balance: PKR <span id="balance">0</span></h3>
<button onclick="withdraw()">Withdraw</button>
</div>

<div class="box">
<h3>Special Offers (24 Hours)</h3>

<div class="plan">
<b>Plan 1:</b> PKR 200<br>
Days: 20 • Daily Profit Auto Add<br>
<button onclick="buyPlan(200)">Buy Now</button>
</div>

<div class="plan">
<b>Plan 2:</b> PKR 500<br>
Days: 30<br>
<button onclick="buyPlan(500)">Buy Now</button>
</div>

<div class="plan">
<b>Plan 3:</b> PKR 1000<br>
Days: 40<br>
<button onclick="buyPlan(1000)">Buy Now</button>
</div>

<div class="plan">
<b>Plan 4:</b> PKR 2000<br>
Days: 50<br>
<button onclick="buyPlan(2000)">Buy Now</button>
</div>

<div class="plan">
<b>Plan 5:</b> PKR 3000<br>
Days: 70<br>
<button onclick="buyPlan(3000)">Buy Now</button>
</div>
</div>

<div class="box">
<h3>Normal Plans</h3>

<div class="plan"><b>Plan A:</b> 5000 PKR • 30 Days <button onclick="buyPlan(5000)">Buy</button></div>
<div class="plan"><b>Plan B:</b> 10000 PKR • 45 Days <button onclick="buyPlan(10000)">Buy</button></div>
<div class="plan"><b>Plan C:</b> 20000 PKR • 60 Days <button onclick="buyPlan(20000)">Buy</button></div>
<div class="plan"><b>Plan D:</b> 30000 PKR • 80 Days <button onclick="buyPlan(30000)">Buy</button></div>

</div>

<div class="box" id="depositSection">
<h3>Deposit</h3>

<label>JazzCash (Copy Number)</label>
<button class="copy-btn" onclick="copyNumber('03705519562')">Copy 03705519562</button>

<label>Easypaisa (Copy Number)</label>
<button class="copy-btn" onclick="copyNumber('03379827882')">Copy 03379827882</button>

<input type="number" id="depositAmount" placeholder="Amount">
<input type="text" id="txid" placeholder="Transaction ID">
<input type="text" id="proof" placeholder="Upload Proof Link">

<button onclick="submitDeposit()">Submit Deposit</button>
</div>

<div class="box">
<h3>Administration Support</h3>
<p><b>Company:</b> VERBOSE – Branch of ROCK EARN (Million+ Active Users)</p>
<p><b>WhatsApp:</b> <a href="https://wa.me/923705519562">03705519562</a></p>
<p><b>Email:</b> <a href="mailto:rock.earn92@gmail.com">rock.earn92@gmail.com</a></p>
<p>24/7 Support Available • Your earnings are fully secure.</p>
</div>

</div>

<div class="fixed-nav">
<div onclick="location.reload()">🔄<br>Refresh</div>
<div id="logoutBtn">🚪<br>Logout</div>
</div>

</body>
</html>
