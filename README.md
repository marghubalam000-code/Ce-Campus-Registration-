<Web Design By Marghub07>
<html>
<head>
<meta charset="UTF-8">
<title>Catalyst Educational Campus Admission System</title>

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
display:flex;
align-items:center;
justify-content:center;
gap:10px;
font-size:22px;
font-weight:bold;
color:#000;
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

label{font-weight:bold;margin-top:10px;display:block;}
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
font-weight:bold;
cursor:pointer;
background:linear-gradient(90deg,#00c9ff,#92fe9d);
}

.status{text-align:center;font-weight:bold;margin-top:10px;}

.slip{
margin-top:20px;
padding:20px;
border:2px dashed #00c9ff;
background:#f5ffff;
border-radius:10px;
}

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
<option>06</option><option>07</option><option>08</option>
<option>09</option><option>10</option><option>11</option><option>12</option>
</select>

<div id="streamBox" style="display:none;">
<label>Stream <span class="star">*</span></label>
<select id="stream">
<option value="">Select</option>
<option>Science</option>
<option>Arts</option>
<option>Commerce</option>
</select>
</div>

<div id="subjectBox" style="display:none;">
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

<button type="button" onclick="payNow()">💳 Pay ₹500</button>
<p id="payStatus" class="status">❌ Payment Pending</p>

<button type="submit">🚀 REGISTER STUDENT</button>

</form>

<div id="slip"></div>

</div>

<div class="design">Web Design by Marghubur Rahman🌹🌹</div>

<script>

/* PAYMENT */
let paymentDone=false;

function payNow(){
new Razorpay({
key:"rzp_live_SeTZG1tAB8E8uH",
amount:01,
currency:"INR",
name:"CE Campus",
description:"Registration Fee",
handler:function(res){
paymentDone=true;
document.getElementById("payStatus").innerText="✅ Paid: "+res.razorpay_payment_id;
alert("Payment Successful ✔");
}
}).open();
}

/* CLASS LOGIC */
class.onchange=function(){
streamBox.style.display=(this.value==="11"||this.value==="12")?"block":"none";
subjectBox.style.display="none";
};

/* STREAM */
stream.onchange=function(){
subjectBox.style.display=this.value?"block":"none";
};

/* AADHAR FORMAT */
aadhar.oninput=function(e){
let v=e.target.value.replace(/\D/g,"").substring(0,12);
e.target.value=v.replace(/(\d{4})(\d{4})(\d{0,4})/,"$1-$2-$3");
};

/* SUBMIT */
form.onsubmit=function(e){
e.preventDefault();

if(!paymentDone){alert("❌ Please complete payment first");return;}

let file=photo.files[0];
if(!file){alert("Upload photo");return;}

let aadharVal=aadhar.value.replace(/-/g,"");
if(!/^\d{12}$/.test(aadharVal)){
alert("Invalid Aadhar");
return;
}

let reader=new FileReader();

reader.onload=function(e){

let img=e.target.result;

let data={
Name:name.value,
Father:father.value,
Mobile:mobile.value,
Address:address.value,
Class:class.value,
Stream:stream.value,
Subject:subject.value,
Date:new Date().toLocaleString()
};

alert("🎉 Welcome to CE Campus\n"+data.Name);

/* SLIP */
slip.innerHTML=`
<div class="slip">
<img src="Ce.JPEG" height="60"><br>
<h2 style="color:#00c9ff;">REGISTRATION SLIP</h2>
${Object.entries(data).map(([k,v])=>`<p><b>${k}:</b> ${v}</p>`).join("")}
<img src="${img}" width="120" style="border-radius:10px;">
<br><br>
<button onclick="window.print()">🖨️ PRINT</button>
</div>`;

/* PDF */
const {jsPDF}=window.jspdf;
let doc=new jsPDF();

/* Header */
doc.setFillColor(0,201,255);
doc.rect(0,0,210,30,"F");

doc.setTextColor(255,255,255);
doc.setFontSize(18);
doc.text("CATALYST EDUCATIONAL CAMPUS ADMISSION",50,20);

/* Body */
doc.setTextColor(0,0,0);
doc.setFontSize(12);

let y=50;
Object.entries(data).forEach(([k,v])=>{
doc.text(`${k}: ${v}`,20,y);
y+=10;
});

/* Photo */
doc.addImage(img,"JPEG",140,50,50,50);

/* Border */
doc.rect(10,40,190,240);

/* Save */
doc.save(data.Name+"_Admission.pdf");

};

reader.readAsDataURL(file);
};

</script>

</body>
</html>
