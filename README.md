<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE — Neon Premium</title>

<script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-app.js"></script>
<script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-auth.js"></script>
<script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-firestore.js"></script>
<script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-storage.js"></script>

<style>
body{background:#0d1117;color:white;font-family:sans-serif;padding:10px}
.box{border:1px solid cyan;padding:10px;margin:10px;border-radius:8px}
.btn{background:cyan;color:black;padding:8px;border:none;border-radius:6px}
</style>
</head>

<body>

<h2>💰 Balance: <span id="balance">0</span> PKR</h2>
<button onclick="logout()" class="btn">Logout</button>

<div class="box">
<h3>Deposit</h3>
<input id="amount" placeholder="Amount"><br>
<input id="tid" placeholder="TID"><br>
<input type="file" id="proof"><br>
<button onclick="submitDeposit()" class="btn">Submit</button>
</div>

<div class="box">
<h3>History</h3>
<div id="history"></div>
</div>

<div class="box">
<h3>Withdraw</h3>
<input id="withdrawAmount" placeholder="Amount"><br>
<input id="account" placeholder="Account"><br>
<button onclick="withdraw()" class="btn">Withdraw</button>
</div>

<!-- ADMIN PANEL -->
<div id="adminPanel" style="display:none;position:fixed;top:0;left:0;width:100%;height:100%;background:black;overflow:auto;padding:20px">
<h2>👑 ADMIN PANEL</h2>
<button onclick="closeAdmin()">Close</button>

<div id="adminDeposits"></div>
<div id="adminWithdraws"></div>
<div id="allUsers"></div>
</div>

<script>

// Firebase
const firebaseConfig = {
  apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk",
  authDomain: "dark-web-9.firebaseapp.com",
  projectId: "dark-web-9",
  storageBucket: "dark-web-9.firebasestorage.app",
  messagingSenderId: "564328425161",
  appId: "1:564328425161:web:eb109ab77356dafe7f4f18"
};

firebase.initializeApp(firebaseConfig);
const db = firebase.firestore();

// Logout
function logout(){
firebase.auth().signOut().then(()=>location.reload());
}

// Deposit
function submitDeposit(){
let file=document.getElementById("proof").files[0];
let user=firebase.auth().currentUser;

let ref=firebase.storage().ref("proofs/"+Date.now()+"_"+file.name);

ref.put(file).then(()=>{
ref.getDownloadURL().then(url=>{
db.collection("deposits").add({
user:user.uid,
amount:document.getElementById("amount").value,
tid:document.getElementById("tid").value,
proof:url,
status:"pending"
});
alert("Submitted");
});
});
}

// Balance + History
firebase.auth().onAuthStateChanged(user=>{
if(user){

db.collection("users").doc(user.uid)
.onSnapshot(doc=>{
document.getElementById("balance").innerText=doc.data()?.balance||0;
});

db.collection("deposits").where("user","==",user.uid)
.onSnapshot(snap=>{
let html="";
snap.forEach(doc=>{
let d=doc.data();
html+=`<div class="box">${d.amount} PKR - ${d.status}</div>`;
});
document.getElementById("history").innerHTML=html;
});

}
});

// Withdraw
function withdraw(){
let user=firebase.auth().currentUser;
db.collection("withdraws").add({
user:user.uid,
amount:document.getElementById("withdrawAmount").value,
account:document.getElementById("account").value,
status:"pending"
});
}

// SECRET ADMIN
let tap=0;
document.body.addEventListener("click",()=>{
tap++;
if(tap>=4){
let key=prompt("Admin Key");
if(key==="verbose786"){
document.getElementById("adminPanel").style.display="block";
}
tap=0;
}
});

function closeAdmin(){
document.getElementById("adminPanel").style.display="none";
}

// Admin Deposits
db.collection("deposits").onSnapshot(snap=>{
let html="";
snap.forEach(doc=>{
let d=doc.data();
html+=`
<div class="box">
${d.amount} PKR<br>
${d.tid}<br>
<img src="${d.proof}" width="80"><br>
${d.status}
<button onclick="approveDeposit('${doc.id}','${d.user}',${d.amount})">Approve</button>
</div>`;
});
document.getElementById("adminDeposits").innerHTML=html;
});

// Approve Deposit
function approveDeposit(id,uid,amount){
db.collection("deposits").doc(id).update({status:"approved"});
db.collection("users").doc(uid).get().then(doc=>{
let bal=doc.data()?.balance||0;
db.collection("users").doc(uid).update({balance:bal+Number(amount)});
});
}

// Withdraw Admin
db.collection("withdraws").onSnapshot(snap=>{
let html="";
snap.forEach(doc=>{
let d=doc.data();
html+=`
<div class="box">
${d.amount} PKR - ${d.account}
<button onclick="approveWithdraw('${doc.id}','${d.user}',${d.amount})">Approve</button>
</div>`;
});
document.getElementById("adminWithdraws").innerHTML=html;
});

// Approve Withdraw
function approveWithdraw(id,uid,amount){
db.collection("withdraws").doc(id).update({status:"approved"});
db.collection("users").doc(uid).get().then(doc=>{
let bal=doc.data()?.balance||0;
db.collection("users").doc(uid).update({balance:bal-Number(amount)});
});
}

// Users
db.collection("users").onSnapshot(snap=>{
let html="";
snap.forEach(doc=>{
let d=doc.data();
html+=`<div class="box">${doc.id} - ${d.balance||0}</div>`;
});
document.getElementById("allUsers").innerHTML=html;
});

</script>

</body>
</html>
