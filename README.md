<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>CE Campus Admission System</title>

<script src="https://checkout.razorpay.com/v1/checkout.js"></script>
<script src="https://cdn.jsdelivr.net/npm/emailjs-com@3/dist/email.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

<style>

body{
margin:0;
font-family:'Segoe UI',sans-serif;
background:linear-gradient(135deg,#141e30,#243b55);
}

.header{
background:linear-gradient(90deg,#00c9ff,#92fe9d);
padding:15px;
text-align:center;
font-size:22px;
font-weight:bold;
display:flex;
justify-content:center;
align-items:center;
gap:10px;
}

.header img{
height:50px;
}

.container{
max-width:650px;
margin:30px auto;
background:#fff;
padding:25px;
border-radius:15px;
box-shadow:0 15px 40px rgba(0,0,0,0.4);
}

label{
font-weight:bold;
display:block;
margin-top:10px;
}

.star{color:red;}

input,select{
width:100%;
padding:12px;
margin-top:5px;
border-radius:8px;
border:1px solid #ccc;
}

button{
width:100%;
padding:14px;
margin-top:12px;
border:none;
border-radius:10px;
font-size:15px;
font-weight:bold;
cursor:pointer;
background:linear-gradient(90deg,#00c9ff,#92fe9d);
transition:0.3s;
}

button:hover{transform:scale(1.02);}

.slip{
margin-top:20px;
padding:20px;
border:2px dashed #00c9ff;
background:#f5ffff;
border-radius:10px;
text-align:center;
}

.success{
text-align:center;
color:green;
font-weight:bold;
font-size:16px;
margin-top:10px;
}

.hidden{display:none;}

.design{
position:fixed;
right:10px;
bottom:10px;
background:#00c9ff;
padding:8px;
border-radius:8px;
font-size:12px;
font-weight:bold;
}

</style>
</head>

<body>

<div class="header">
<img src="Ce.JPEG">
🎓 CATALYST EDUCATIONAL CAMPUS - ADMISSION PORTAL
</div>

<div class="container">

<form id="form">

<label>Student Name <span class="star">*</span></label>
<input id="name" required>

<label>Father Name <span class="star">*</span></label>
<input id="father" required>

<label>Mobile <span class="star">*</span></label>
<input id="mobile" required>

<label>Address <span class="star">*</span></label>
<input id="address" required>

<label>Aadhar <span class="star">*</span></label>
<input id="aadhar" maxlength="14" required>

<label>Class <span class="star">*</span></label>
<select id="class" required>
<option value="">Select</option>
<option>06</option>
<option>07</option>
<option>08</option>
<option>09</option>
<option>10</option>
<option>11</option>
<option>12</option>
</select>

<div id="streamBox" class="hidden">
<label>Stream <span class="star">*</span></label>
<select id="stream">
<option value="">Select</option>
<option>Science</option>
<option>Arts</option>
<option>Commerce</option>
</select>
</div>

<div id="subjectBox" class="hidden">
<label>Subject <span class="star">*</span></label>
<select id="subject">
<option value="">Select</option>
<option>Mathematics</option>
<option>Biology</option>
<option>Physics</option>
<option>Chemistry</option>
<option>History</option>
<option>Geography</option>
<option>Economics</option>
</select>
</div>

<label>Photo <span class="star">*</span></label>
<input type="file" id="photo" required>

<button type="button" onclick="payNow()">💳 Pay Registration Fee</button>
<button type="button" onclick="checkPayment()">🔄 Refresh Payment Status</button>

</p>

<button type="submit">🚀 REGISTER STUDENT</button>

</form>

<p id="msg" class="success"></p>
<div id="slip"></div>

</div>

<div class="design">Design by M Rahman</div>

<script>
emailjs.init("q7WRi2qk3AUR725UG");

let paymentDone = localStorage.getItem("paymentDone")==="true";
let paymentId = localStorage.getItem("paymentId")||"";

function updateUI(){
document.getElementById("payStatus").innerText =
paymentDone ? "✅ Paid: "+paymentId : "❌ Payment Pending";
}
updateUI();

document.getElementById("class").addEventListener("change",function(){
let c=this.value;
if(c==="11"||c==="12"){
document.getElementById("streamBox").style.display="block";
}else{
document.getElementById("streamBox").style.display="none";
document.getElementById("subjectBox").style.display="none";
}
});

document.getElementById("stream").addEventListener("change",function(){
document.getElementById("subjectBox").style.display=this.value?"block":"none";
});

document.getElementById("aadhar").addEventListener("input",function(e){
let v=e.target.value.replace(/\D/g,"").substring(0,12);
let f=v;
if(v.length>4) f=v.substring(0,4)+"-"+v.substring(4);
if(v.length>8) f=v.substring(0,4)+"-"+v.substring(4,8)+"-"+v.substring(8);
e.target.value=f;
});

function payNow(){
var options={
key:"rzp_test_SeELoMqEhitjTE",
amount:0,
currency:"INR",
name:"CE Campus",
description:"Registration Fee",
handler:function(response){
paymentDone=true;
paymentId=response.razorpay_payment_id;
localStorage.setItem("paymentDone","true");
localStorage.setItem("paymentId",paymentId);
updateUI();
alert("Payment Successful ✔");
}
};
new Razorpay(options).open();
}

function checkPayment(){
paymentDone = localStorage.getItem("paymentDone")==="true";
paymentId = localStorage.getItem("paymentId")||"";
updateUI();
alert(paymentDone ? "Payment Verified ✔" : "Payment Not Done ❌");
}

document.getElementById("form").addEventListener("submit",function(e){
e.preventDefault();

if(!paymentDone){
alert("❌ Complete payment first");
return;
}

let file=document.getElementById("photo").files[0];
let reader=new FileReader();

reader.onload=function(e){

let photo=e.target.result;

let data={
name:document.getElementById("name").value,
class:document.getElementById("class").value,
stream:document.getElementById("stream").value,
subject:document.getElementById("subject").value,
date:new Date().toLocaleString()
};

alert("🎉 Welcome to CE Campus\nStudent: "+data.name);

/* SLIP */
document.getElementById("slip").innerHTML=`
<div class="slip">

<img src="Ce.JPEG" style="height:60px;"><br>
<h2 style="color:#00c9ff;">CE CAMPUS SLIP</h2>

<p><b>Name:</b> ${data.name}</p>
<p><b>Class:</b> ${data.class}</p>

<img src="${photo}" style="width:120px;height:120px;border-radius:10px;border:2px solid #00c9ff;">

<br><br>
<button onclick="window.print()">🖨️ PRINT</button>

</div>`;

/* PDF */
const {jsPDF}=window.jspdf;
let doc=new jsPDF();

let logo=new Image();
logo.src="Ce.JPEG";

logo.onload=function(){

doc.addImage(logo,"JPEG",10,5,30,30);

doc.setFontSize(20);
doc.text("CE CAMPUS",80,20);

doc.rect(10,40,190,200);

doc.text("Name: "+data.name,15,60);
doc.text("Class: "+data.class,15,70);

doc.addImage(photo,"JPEG",140,50,50,50);

doc.save(data.name+"_Certificate.pdf");

};

};

reader.readAsDataURL(file);
});
</script>

</body>
</html>
