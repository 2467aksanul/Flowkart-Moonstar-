<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Instagram Order Form</title>
<style>
body{margin-top:120px;font-family:Arial,sans-serif;background:#f7f7f7;}
.scrolling-banner{position:fixed;top:60px;left:0;width:100%;background:#000;color:#fff;font-weight:bold;font-size:16px;white-space:nowrap;overflow:hidden;z-index:9999;padding:10px 0;border-top:2px solid #ffeb3b;border-bottom:2px solid #ffeb3b;}
.scrolling-banner span{display:inline-block;padding-left:100%;animation:scroll-left 20s linear infinite;}
@keyframes scroll-left{0%{transform:translateX(0)}100%{transform:translateX(-100%)}}
.container{max-width:700px;margin:20px auto;padding:16px;background:#fff;border-radius:10px;box-shadow:0 4px 20px rgba(0,0,0,.06);}
.container h2{text-align:center;color:#333;}
.form-row{margin:10px 0;}
label{display:block;margin-bottom:6px;font-weight:600;}
input, select{width:100%;padding:10px;border:1px solid #ccc;border-radius:6px;}
#details{background:#f3f3f3;border-radius:8px;margin:10px 0 15px;padding:10px;font-weight:600;}
.btn{display:inline-block;padding:12px 20px;border:0;border-radius:8px;color:#fff;cursor:pointer;font-size:16px;}
.btn-primary{background:#25d366;}
.btn-upi{background:#007bff;}
.btn:disabled{opacity:.6;cursor:not-allowed;}
.floating-call{position:fixed;left:20px;bottom:20px;z-index:9999;background:#28a745;color:#fff;padding:12px 16px;border-radius:50px;box-shadow:0 4px 8px rgba(0,0,0,.3);text-decoration:none;}
#popup{position:fixed;left:50%;bottom:16px;transform:translateX(-50%);background:#007bff;color:#fff;padding:6px 12px;border-radius:6px;box-shadow:0 1px 5px rgba(0,0,0,.15);display:none;z-index:9999;font-size:13px;font-family:Arial,sans-serif;white-space:nowrap;}
#countdown-timer{position:relative;width:100%;background:#fff4f4;padding:10px;text-align:center;font-size:18px;font-weight:bold;color:red;border-bottom:2px dashed #ffcccc;border-radius:8px;}
footer{background:#f8f9fa;padding:20px;text-align:center;color:#666;margin-top:20px;border-radius:8px;}
</style>
</head>
<body>

<div class="scrolling-banner"><span>💥 Use code AK47 for 10% Discount! 🔥 | 🚀 Flowkart Moonstar | ⏳ Limited Time Offer!</span></div>

<div class="container">
  <h2>📸 Instagram Order Form</h2>
  <div class="form-row">
    <label>📌 Select Service:</label>
    <select id="service" onchange="updateDetails()" required>
      <option value="followers">Instagram Followers</option>
      <option value="likes">Instagram Likes</option>
      <option value="views">Instagram Views</option>
      <option value="reel">Instagram Reel Views</option>
      <option value="comments">Instagram Comments</option>
    </select>
  </div>

  <div id="details"></div>

  <div class="form-row"><label>👤 Your Name:</label><input id="name" type="text" placeholder="Enter your name" required></div>
  <div class="form-row"><label>🔗 Link:</label><input id="link" type="text" placeholder="Paste Instagram link" required></div>
  <div class="form-row"><label>🔢 Quantity:</label><input id="qty" type="number" min="1" placeholder="Enter quantity" required></div>
  <div class="form-row"><label>🏷️ Coupon Code (Optional):</label><input id="coupon" type="text" placeholder="AK47"></div>
  <div class="form-row"><label>🧾 UTR / Transaction ID:</label><input id="utr" type="text" placeholder="Enter UTR / txn ID" required></div>

  <div class="form-row" style="display:flex; gap:10px; flex-wrap:wrap;">
    <button class="btn btn-upi" onclick="payNow()">Pay Now via UPI</button>
    <button class="btn btn-primary" onclick="sendOrder()">Submit Order</button>
  </div>
</div>

<a class="floating-call" href="tel:+913369028837">📞 Call Now</a>
<audio id="popup-sound" preload="auto" src="https://www.myinstants.com/media/sounds/pop-up-kort.mp3"></audio>
<div id="popup"><span id="popup-text"></span></div>
<div id="countdown-timer">Special Offer Ends In: <span id="timer">10:00</span></div>
<footer>Trusted & Secure • Fast Support 24×7</footer>

<script>
// ===== Pricing & Details =====
function updateDetails(){
  const service=document.getElementById('service').value;
  let priceText=(service==='followers')?'₹0.50 per follower':'₹0.30 per like/view';
  document.getElementById('details').innerHTML=`<p>💰 <b>Price:</b> ${priceText}</p>`;
}
updateDetails();

function calculateTotal(){
  const service=document.getElementById('service').value;
  const qty=parseInt(document.getElementById('qty').value.trim());
  if(!qty || isNaN(qty)) return 0;
  let price=(service==='followers')?0.50:0.30;
  let total=price*qty;
  const coupon=document.getElementById('coupon').value.trim();
  if(coupon.toUpperCase()==='AK47') total=total*0.9;
  return Number(total.toFixed(2));
}

// ===== Cashfree / UPI Payment =====
function payNow(){
  const total=calculateTotal();
  if(total<=0){ alert('Enter valid quantity'); return; }
  const upiID=window.CASHFREE_UPI_ID || '9954646296-3@ybl';
  const name='Instagram Order';
  window.location.href=`upi://pay?pa=${upiID}&pn=${encodeURIComponent(name)}&am=${total}&cu=INR`;
}

// ===== FavSMM API Integration =====
async function sendOrder(){
  const service=document.getElementById('service').value;
  const name=document.getElementById('name').value.trim();
  const link=document.getElementById('link').value.trim();
  const qty=parseInt(document.getElementById('qty').value.trim());
  const coupon=document.getElementById('coupon').value.trim();
  const utr=document.getElementById('utr').value.trim();
  if(!name || !link || !qty || !utr){ alert('Fill all required fields'); return; }

  const total=calculateTotal();
  const favsmmKey=window.FAVSMM_API_KEY || 'IBLYI27V4LCYHYX7';
  const favsmmURL='https://favoritesmm.com/api/v2';
  const params={api_key:favsmmKey, action:'add', service:service, link:link, quantity:qty};

  try{
    const resp=await fetch(favsmmURL,{method:'POST',headers:{'Content-Type':'application/json'},body:JSON.stringify(params)});
    const data=await resp.json();
    console.log(data);
    alert('✅ Order submitted! Check FavSMM panel.');
  }catch(e){console.error(e); alert('❌ Error sending order to FavSMM'); }

  // ===== Wisebook Placeholder =====
  try{
    const wisebookKey=window.WISEBOOK_API_KEY || '';
    if(wisebookKey){
      // Example API call
      console.log('Wisebook order sent (mock).');
    }
  }catch(e){console.error('Wisebook error:',e);}
}

// ===== Popup notifications =====
const names=["Ravi from Kolkata","Sneha from Assam","Anil from Jharkhand"];
const actions=["just buy 5k Followers","just got 10k Views","just ordered 1k Likes"];
function showPopup(){
  const popup=document.getElementById('popup');
  const popupText=document.getElementById('popup-text');
  const sound=document.getElementById('popup-sound');
  const name=names[Math.floor(Math.random()*names.length)];
  const action=actions[Math.floor(Math.random()*actions.length)];
  popupText.textContent=`${name} ${action}`;
  popup.style.display='inline-block';
  try{sound.currentTime=0;sound.play();}catch(e){}
  setTimeout(()=>{popup.style.display='none';},2500);
}
setInterval(showPopup,3000);

// ===== Countdown =====
(function(){
  let duration=10*60;
  const display=document.getElementById('timer');
  function tick(){
    const m=Math.floor(duration/60);
    const s=duration%60;
    display.textContent=`${m<10?'0':''}${m}:${s<10?'0':''}${s}`;
    duration--;
    if(duration<0){clearInterval(interval); display.textContent='EXPIRED';}
  }
  tick();
  const interval=setInterval(tick,1000);
})();
</script>
</body>
</html>
