<!DOCTYPE html>
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
display:flex;
align-items:center;
justify-content:center;
gap:10px;
font-size:22px;
font-weight:bold;
}

.header img{height:50px;border-radius:10px;}

.container{
max-width:700px;
margin:30px auto;
background:#fff;
padding:25px;
border-radius:15px;
box-shadow:0 15px 40px rgba(0,0,0,0.4);
}

label{font-weight:bold;margin-top:10px;display:block;}

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

.slip{
margin-top:20px;
padding:20px;
border:2px dashed #00c9ff;
background:#f5ffff;
border-radius:10px;
text-align:center;
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
<img src="C.jpeg">
🎓 CE CAMPUS - ADMISSION PORTAL
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
<option>06</option><option>07</option><option>08</option>
<option>09</option><option>10</option><option>11</option><option>12</option>
</select>

<div id="streamBox" style="display:none;">
<label>Stream *</label>
<select id="stream">
<option value="">Select</option>
<option>Science</option>
<option>Arts</option>
<option>Commerce</option>
</select>
</div>

<div id="subjectBox" style="display:none;">
<label>Subject *</label>
<select id="subject"></select>
</div>

<label>Photo *</label>
<input type="file" id="photo" required>

<button type="button" onclick="payNow()">💳 Pay ₹500</button>
<p id="payStatus">❌ Payment Pending</p>

<button type="submit">🚀 REGISTER STUDENT</button>

</form>

<div id="paymentSlip"></div>
<div id="slip"></div>

</div>

<div class="design">Web Design by Marghubur Rahman</div>

<script>

const subjects={
Science:["Physics","Chemistry","Maths","Biology"],
Arts:["History","Geography","Political Science","Economics"],
Commerce:["Accounts","Business","Economics"]
};

/* CLASS */
class.onchange=function(){
streamBox.style.display=(this.value==="11"||this.value==="12")?"block":"none";
};

/* STREAM */
stream.onchange=function(){
let s=this.value;
subject.innerHTML="<option>Select</option>";
if(subjects[s]){
subjects[s].forEach(x=>{
let o=document.createElement("option");
o.text=x;
subject.add(o);
});
subjectBox.style.display="block";
}
};

/* AADHAR */
aadhar.oninput=function(e){
let v=e.target.value.replace(/\D/g,"").substring(0,12);
e.target.value=v.replace(/(\d{4})(\d{4})(\d{0,4})/,"$1-$2-$3");
};

/* PAYMENT */
let paymentDone=false, paymentId="";

function payNow(){
new Razorpay({
key:"rzp_live_SeTZG1tAB8E8uH",
amount:50000,
currency:"INR",
name:"CE Campus",
description:"Registration Fee",
handler:function(res){
paymentDone=true;
paymentId=res.razorpay_payment_id;

payStatus.innerText="✅ Paid: "+paymentId;

/* PAYMENT SLIP */
paymentSlip.innerHTML=`
<div class="slip">
<img src="C.jpeg" height="60"><br>
<h2>PAYMENT RECEIPT</h2>
<p><b>Amount:</b> ₹500</p>
<p><b>Payment ID:</b> ${paymentId}</p>
<p><b>Date:</b> ${new Date().toLocaleString()}</p>
<button onclick="window.print()">🖨️ PRINT PAYMENT SLIP</button>
</div>`;
}
}).open();
}

/* SUBMIT */
form.onsubmit=function(e){
e.preventDefault();

if(!paymentDone){alert("Pay first");return;}

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

/* REGISTRATION SLIP */
slip.innerHTML=`
<div class="slip">
<img src="C.jpeg" height="60"><br>
<h2>REGISTRATION SLIP</h2>
${Object.entries(data).map(([k,v])=>`<p><b>${k}:</b> ${v}</p>`).join("")}
<img src="${img}" width="120">
<br><br>
<button onclick="window.print()">🖨️ PRINT REGISTRATION SLIP</button>
</div>`;

/* PDF */
const {jsPDF}=window.jspdf;
let doc=new jsPDF();

doc.setFillColor(0,201,255);
doc.rect(0,0,210,30,"F");

doc.setTextColor(255,255,255);
doc.setFontSize(18);
doc.text("CE CAMPUS ADMISSION",50,20);

doc.setTextColor(0,0,0);

let y=50;
Object.entries(data).forEach(([k,v])=>{
doc.text(k+": "+v,20,y);
y+=10;
});

doc.addImage(img,"JPEG",140,50,50,50);
doc.rect(10,40,190,240);

doc.save(data.Name+"_Admission.pdf");

};

reader.readAsDataURL(photo.files[0]);
};

</script>

</body>
</html>
