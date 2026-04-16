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
padding:18px;
text-align:center;
font-size:22px;
font-weight:bold;
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
🎓 CE CAMPUS - ADMISSION PORTAL
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

<!-- PAYMENT -->
<button type="button" onclick="payNow()">💳 Pay Registration Fee</button>
<button type="button" onclick="checkPayment()">🔄 Refresh Payment Status</button>

<p id="payStatus">❌ Payment Pending</p>

<button type="submit">🚀 REGISTER STUDENT</button>

</form>

<p id="msg" class="success"></p>
<div id="slip"></div>

</div>

<div class="design">Design by M Rahman</div>

<script>
emailjs.init("q7WRi2qk3AUR725UG");

/* PAYMENT STORAGE */
let paymentDone = localStorage.getItem("paymentDone")==="true";
let paymentId = localStorage.getItem("paymentId")||"";

function updateUI(){
document.getElementById("payStatus").innerText =
paymentDone ? "✅ Paid: "+paymentId : "❌ Payment Pending";
}
updateUI();

/* CLASS LOGIC */
document.getElementById("class").addEventListener("change",function(){
let c=this.value;
if(c==="11"||c==="12"){
document.getElementById("streamBox").style.display="block";
}else{
document.getElementById("streamBox").style.display="none";
document.getElementById("subjectBox").style.display="none";
}
});

/* STREAM */
document.getElementById("stream").addEventListener("change",function(){
document.getElementById("subjectBox").style.display=this.value?"block":"none";
});

/* AADHAR FORMAT */
document.getElementById("aadhar").addEventListener("input",function(e){
let v=e.target.value.replace(/\D/g,"").substring(0,12);
let f=v;
if(v.length>4) f=v.substring(0,4)+"-"+v.substring(4);
if(v.length>8) f=v.substring(0,4)+"-"+v.substring(4,8)+"-"+v.substring(8);
e.target.value=f;
});

/* PAY NOW */
function payNow(){
var options={
key:"rzp_test_SeDZVp4F9WDRt9",
amount:50000,
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
},
theme:{color:"#00c9ff"}
};

new Razorpay(options).open();
}

/* REFRESH */
function checkPayment(){
paymentDone = localStorage.getItem("paymentDone")==="true";
paymentId = localStorage.getItem("paymentId")||"";
updateUI();

alert(paymentDone ? "Payment Verified ✔" : "Payment Not Done ❌");
}

/* SUBMIT */
document.getElementById("form").addEventListener("submit",function(e){
e.preventDefault();

if(!paymentDone){
alert("❌ Please complete payment first");
return;
}

let file=document.getElementById("photo").files[0];
if(!file){alert("Upload photo");return;}

let aadhar=document.getElementById("aadhar").value.replace(/-/g,"");
if(!/^\d{12}$/.test(aadhar)){
alert("Invalid Aadhar");
return;
}

let cls=document.getElementById("class").value;
let stream=document.getElementById("stream").value;
let subject=document.getElementById("subject").value;

if((cls==="11"||cls==="12") && !stream){alert("Select stream");return;}
if((cls==="11"||cls==="12") && !subject){alert("Select subject");return;}

let reader=new FileReader();

reader.onload=function(e){

let photo=e.target.result;

let data={
name:document.getElementById("name").value,
father:document.getElementById("father").value,
mobile:document.getElementById("mobile").value,
address:document.getElementById("address").value,
class:cls,
stream:stream,
subject:subject,
date:new Date().toLocaleString()
};

/* WELCOME */
alert("🎉 Welcome to CE Campus\nStudent: "+data.name);

/* SLIP */
document.getElementById("slip").innerHTML=`
<div class="slip">

<h2 style="text-align:center;color:#00c9ff;">CE CAMPUS SLIP</h2>

<p><b>Name:</b> ${data.name}</p>
<p><b>Class:</b> ${data.class}</p>
<p><b>Stream:</b> ${data.stream}</p>
<p><b>Subject:</b> ${data.subject}</p>
<p><b>Date:</b> ${data.date}</p>

<img src="${photo}" style="width:120px;height:120px;border-radius:10px;border:2px solid #00c9ff;">

<br><br>
<button onclick="window.print()">🖨️ PRINT SLIP</button>

</div>`;

/* PDF */
const {jsPDF}=window.jspdf;
let doc=new jsPDF();

doc.setFillColor(0,201,255);
doc.rect(0,0,220,40,"F");

doc.setFontSize(20);
doc.text("CE CAMPUS",85,20);

doc.setFontSize(14);
doc.text("ADMISSION CERTIFICATE",60,30);

doc.rect(10,50,190,200);

doc.text("Name: "+data.name,15,70);
doc.text("Class: "+data.class,15,80);
doc.text("Stream: "+data.stream,15,90);
doc.text("Subject: "+data.subject,15,100);
doc.text("Mobile: "+data.mobile,15,110);
doc.text("Date: "+data.date,15,120);

doc.addImage(photo,"JPEG",140,60,50,50);

doc.save(data.name+"_CE_Certificate.pdf");

document.getElementById("msg").innerText="✔ Registration Successful";

};

reader.readAsDataURL(file);
});
</script>

</body>
</html>
