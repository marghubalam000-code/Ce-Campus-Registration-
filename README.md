<Marghub07_dz>
<html>
<head>
<meta charset="UTF-8">
<title>CE Campus Admission System</title>

<script src="https://checkout.razorpay.com/v1/checkout.js"></script>
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

.header img{height:50px;}

.container{
max-width:650px;
margin:30px auto;
background:#fff;
padding:25px;
border-radius:15px;
box-shadow:0 15px 40px rgba(0,0,0,0.4);
}

label{font-weight:bold;display:block;margin-top:10px;}
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
}

.status{
text-align:center;
font-weight:bold;
margin-top:10px;
}

.slip{
margin-top:20px;
padding:20px;
border:2px dashed #00c9ff;
background:#f5ffff;
border-radius:10px;
text-align:center;
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

<label>Student Name *</label>
<input id="name" required>

<label>Father Name *</label>
<input id="father" required>

<label>Mobile *</label>
<input id="mobile" required>

<label>Address *</label>
<input id="address" required>

<label>Aadhar *</label>
<input id="aadhar" maxlength="14" required>

<label>Class *</label>
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
<label>Stream *</label>
<select id="stream">
<option value="">Select</option>
<option>Science</option>
<option>Arts</option>
<option>Commerce</option>
</select>
</div>

<div id="subjectBox" class="hidden">
<label>Subject *</label>
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

<label>Photo *</label>
<input type="file" id="photo" required>

<button type="button" onclick="payNow()">💳 Pay ₹500 Registration Fee</button>
<p id="payStatus" class="status">❌ Payment Pending</p>

<button type="submit">🚀 REGISTER STUDENT</button>

</form>

<div id="slip"></div>

</div>

<div class="design">Web Design by Marghubur Rahman💞💞</div>

<script>

/* PAYMENT */
let paymentDone=false;

function payNow(){
var options={
key:"rzp_live_SeTZG1tAB8E8uH",
amount:500,
currency:"INR",
name:"CE Campus",
description:"Registration Fee",
handler:function(response){
paymentDone=true;
document.getElementById("payStatus").innerText="✅ Payment Successful: "+response.razorpay_payment_id;
alert("Payment Successful ✔");
}
};
new Razorpay(options).open();
}

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

document.getElementById("stream").addEventListener("change",function(){
document.getElementById("subjectBox").style.display=this.value?"block":"none";
});

/* AADHAR */
document.getElementById("aadhar").addEventListener("input",function(e){
let v=e.target.value.replace(/\D/g,"").substring(0,12);
let f=v;
if(v.length>4) f=v.substring(0,4)+"-"+v.substring(4);
if(v.length>8) f=v.substring(0,4)+"-"+v.substring(4,8)+"-"+v.substring(8);
e.target.value=f;
});

/* SUBMIT */
document.getElementById("form").addEventListener("submit",function(e){
e.preventDefault();

if(!paymentDone){
alert("❌ Please complete payment first");
return;
}

let file=document.getElementById("photo").files[0];
if(!file){alert("Upload photo");return;}

let reader=new FileReader();

reader.onload=function(e){

let photo=e.target.result;

let data={
name:document.getElementById("name").value,
father:document.getElementById("father").value,
mobile:document.getElementById("mobile").value,
address:document.getElementById("address").value,
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
<h2 style="color:#00c9ff;">CE CAMPUS REGISTRATION SLIP</h2>
<p><b>Name:</b> ${data.name}</p>
<p><b>Class:</b> ${data.class}</p>
<img src="${photo}" style="width:120px;height:120px;border-radius:10px;">
<br><br>
<button onclick="window.print()">🖨️ PRINT</button>
</div>`;

/* PDF */
const {jsPDF}=window.jspdf;
let doc=new jsPDF();

doc.setFillColor(0,201,255);
doc.rect(0,0,220,40,"F");

doc.setFontSize(20);
doc.setTextColor(255,255,255);
doc.text("CE CAMPUS",80,20);

doc.setTextColor(0,0,0);
doc.text("Name: "+data.name,20,70);

doc.addImage(photo,"JPEG",140,60,50,50);

doc.save(data.name+"_Registration Form.pdf");

};

reader.readAsDataURL(file);
});
</script>

</body>
</html>
