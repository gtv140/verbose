<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE Neon</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.2/css/all.min.css">
<style>
:root {
  --bg-dark:#02020a;
  --bg-gradient:linear-gradient(135deg,#01010f,#0d0d2a);
  --neon-cyan:#00f7ff;
  --neon-pink:#ff5cff;
  --text-light:#e6fbff;
  --muted:rgba(230,247,251,0.5);
}
*{box-sizing:border-box;}
body{
  margin:0;
  font-family:Arial,sans-serif;
  background:var(--bg-gradient);
  color:var(--text-light);
  overflow-x:hidden;
  animation:bgAnimate 20s infinite alternate;
}
@keyframes bgAnimate{
  0%{background:linear-gradient(135deg,#01010f,#0d0d2a);}
  50%{background:linear-gradient(135deg,#000000,#0c0c1f);}
  100%{background:linear-gradient(135deg,#020212,#0d0d2a);}
}
.hidden{display:none;}
header{
  padding:20px;
  text-align:center;
  font-weight:900;
  color:var(--neon-cyan);
  text-shadow:0 0 10px var(--neon-cyan),0 0 20px var(--neon-pink);
  font-size:26px;
  animation:neonHeader 2s infinite alternate;
}
@keyframes neonHeader{
  0%{text-shadow:0 0 5px var(--neon-cyan),0 0 10px var(--neon-pink);}
  100%{text-shadow:0 0 20px var(--neon-cyan),0 0 30px var(--neon-pink);}
}
.wrap{max-width:480px;margin:12px auto;padding:12px;}
.card{
  background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(0,0,0,0.05));
  padding:12px;
  border-radius:12px;
  margin-bottom:12px;
  border:1px solid rgba(0,255,240,0.1);
  box-shadow:0 0 10px rgba(0,255,240,0.2);
  transition:0.3s;
}
.card:hover{
  transform:translateY(-3px);
  box-shadow:0 0 20px rgba(0,255,240,0.5);
}
input,select,button{
  width:100%;
  padding:10px;
  margin-top:6px;
  border-radius:8px;
  border:1px solid rgba(255,255,255,0.05);
  background:transparent;
  color:#dff;
  font-size:14px;
}
input:focus,select:focus{outline:none;border-color:var(--neon-cyan);box-shadow:0 0 5px var(--neon-cyan);}
.btn{
  background:linear-gradient(90deg,var(--neon-cyan),var(--neon-pink));
  border:none;
  color:#001;
  font-weight:800;
  cursor:pointer;
  padding:10px;
  border-radius:10px;
  text-shadow:0 0 5px #fff;
  transition:0.3s;
}
.btn:hover{
  transform:scale(1.05);
  box-shadow:0 0 15px var(--neon-cyan),0 0 20px var(--neon-pink);
}
.nav{
  position:fixed;
  bottom:0;
  left:0;
  right:0;
  display:flex;
  justify-content:space-around;
  padding:10px;
  background:rgba(0,0,0,0.7);
  border-top:1px solid rgba(0,255,240,0.1);
}
.nav div{
  color:var(--neon-cyan);
  text-align:center;
  font-size:13px;
  cursor:pointer;
}
.countdown{font-weight:800;color:var(--neon-cyan);margin-top:6px;text-shadow:0 0 5px var(--neon-cyan);}
.muted{color:var(--muted);font-size:13px;}
.plan{display:flex;justify-content:space-between;gap:10px;padding:10px;border-radius:10px;border:1px solid rgba(0,255,240,0.1);margin-bottom:8px;background:linear-gradient(180deg,rgba(255,255,255,0.01),rgba(0,0,0,0.05));transition:0.3s;}
.plan:hover{box-shadow:0 0 15px var(--neon-cyan);}
.plan .meta{flex:1;}
.plan .actions{width:110px;text-align:right;}
.user-box{display:flex;justify-content:space-between;align-items:center;margin-bottom:12px;}
.user-box .badge{background:rgba(0,255,240,0.06);padding:4px 8px;border-radius:999px;color:var(--neon-cyan);}
.icon{margin-right:6px;}
.admin-box{display:flex;justify-content:space-around;margin-top:12px;}
.admin-box div{flex:1;text-align:center;padding:10px;background:rgba(0,255,240,0.06);border-radius:10px;margin:2px;cursor:pointer;transition:0.3s;}
.admin-box div:hover{box-shadow:0 0 10px var(--neon-cyan);}
.admin-box div i{font-size:20px;margin-bottom:4px;display:block;color:var(--neon-cyan);}
.coming-soon{opacity:0.5;font-style:italic;}
.alert-note{background:rgba(255,0,128,0.1);color:var(--neon-cyan);padding:10px;margin-bottom:12px;border-radius:8px;font-weight:600;text-align:center;text-shadow:0 0 5px var(--neon-cyan);}
.referral-box{display:flex;justify-content:space-between;align-items:center;margin-bottom:12px;}
.referral-box input{flex:1;}
</style>
</head>
<body>
<header><i class="fas fa-bolt icon"></i>VERBOSE<i class="fas fa-bolt icon"></i></header>
<div class="wrap">

<!-- LOGIN / DASHBOARD / PLANS / DEPOSIT / WITHDRAW same as before -->
<!-- Tumhare previous HTML structure yaha copy karo, sirf CSS aur theme animate kiya gaya hai -->

</div>

<div class="nav">
<div onclick="nav('dashboardCard')"><i class="fas fa-home icon"></i>Home</div>
<div onclick="nav('plansCard')"><i class="fas fa-box icon"></i>Plans</div>
<div onclick="nav('depositCard')"><i class="fas fa-wallet icon"></i>Deposit</div>
<div onclick="nav('withdrawCard')"><i class="fas fa-hand-holding-usd icon"></i>Withdraw</div>
</div>

<script>
// Tumhara previous JS code same rahega (Auth, Deposit, Withdraw, Plans, Offers)
</script>
</body>
</html>
