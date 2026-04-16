<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>CE Campus Registration</title>

<script src="https://cdn.jsdelivr.net/npm/emailjs-com@3/dist/email.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

<style>

/* BACKGROUND */
body{
margin:0;
font-family:'Segoe UI',sans-serif;
background:linear-gradient(135deg,#141e30,#243b55);
}

/* HEADER */
.header{
background:linear-gradient(90deg,#00c9ff,#92fe9d);
padding:18px;
text-align:center;
font-size:22px;
font-weight:bold;
}

/* CARD */
.container{
max-width:650px;
margin:30px auto;
background:#fff;
padding:25px;
border-radius:15px;
box-shadow:0 15px 40px rgba(0,0,0,0.4);
}

/* LABEL */
label{
font-weight:bold;
display:block;
margin-top:10px;
}

/* STAR */
.star{color:red;}

/* INPUT */
input,select{
width:100%;
padding:12px;
margin-top:5px;
border-radius:8px;
border:1px solid #ccc;
}

/* BUTTON */
button{
width:100%;
padding:14px;
margin-top:15px;
border:none;
border-radius:10px;
font-size:16px;
font-weight:bold;
cursor:pointer;
background:linear-gradient(90deg,#00c9ff,#92fe9d);
transition:0.3s;
}

button:hover{transform:scale(1.02);}

/* SLIP */
.slip{
margin-top:20px;
padding:20px;
border:2px dashed #00c9ff;
background:#f5ffff;
border-radius:10px;
}

/* PRINT AREA */
#printArea{
width:210mm;
padding:20px;
background:white;
border:2px solid #00c9ff;
}

/* DESIGN */
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

.success{
text-align:center;
color:green;
font-weight:bold;
font-size:16px;
margin-top:10px;
}

.hidden{display:none;}

</style>
</head>

<body>

<div class="header">
🎓STUDENT REGISTRATION PORTAL FOR CATALYST EDUCATIONAL CAMPUS
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

<label>Aadhar Number <span class="star">*</span></label>
<input id="aadhar Number" maxlength="12" required>

<label>Class <span class="star">*</span></label>
<select id="class" required>
<option value="">Select</option>
<option>Class 06</option><not option>
<option>Class 07</option>
<option>Class 08</option>
<option>Class 09</option>
<option>Class 10</option>
<option>Class 11</option>
<option>Class 12</option>
</select>

<label>Stream <span class="star">*</span></label>
<select id="stream" required>
<option value="">Select</option>
<option>Science</option>
<option>Arts</option>
<option>Commerce</option>
</select>

<label>Photo <span class="star">*</span></label>
<input type="file" id="photo" required>

<button type="submit">REGISTER STUDENT</button>

<p id="msg" class="success"></p>

<div id="slip"></div>

</div>

<div class="design">Design by Marghubur Rahman👋👋</div>

<script>
emailjs.init("q7WRi2qk3AUR725UG");

document.getElementById("form").addEventListener("submit",function(e){
e.preventDefault();

let file=document.getElementById("photo").files[0];
if(!file){alert("Upload Photo");return;}

let aadhar=document.getElementById("aadhar").value.trim();
if(!/^\d{12}$/.test(aadhar)){
alert("Invalid Aadhar (12 digits only)");
return;
}

let reader=new FileReader();

reader.onload=function(event){

let photo=event.target.result;

let data={
name:document.getElementById("name").value,
father:document.getElementById("father").value,
mobile:document.getElementById("mobile").value,
address:document.getElementById("address").value,
class:document.getElementById("class").value,
stream:document.getElementById("stream").value,
date:new Date().toLocaleString()
};

/* EMAIL */
emailjs.send("service_bnw6tan","template_ye9opt9",{
...data,
aadhar:aadhar
});

/* SUCCESS POPUP */
alert("🎉 Welcome to CE Campus For Registration\n\n "Name:+data.name);

/* PRINTABLE SLIP */
document.getElementById("slip").innerHTML=`
<div id="printArea">

<h2 style="text-align:center;color:#00c9ff;">CE CAMPUS REGISTRATION SLIP</h2>

<p><b>Student Name:</b> ${data.name}</p>
<p><b>Father Name:</b> ${data.father}</p>
<p><b>Mobile:</b> ${data.mobile}</p>
<p><b>Address:</b> ${data.address}</p>
<p><b>Aadhar:</b> ${aadhar}</p>
<p><b>Class:</b> ${data.class}</p>
<p><b>Stream:</b> ${data.stream}</p>
<p><b>Date:</b> ${data.date}</p>

<img src="${photo}" style="width:120px;height:120px;border-radius:10px;border:2px solid #00c9ff;">

<br><br>

<button onclick="window.print()">🖨️ PRINT SLIP</button>

</div>`;

/* PROFESSIONAL PDF */
const {jsPDF}=window.jspdf;
let doc=new jsPDF();

doc.setFillColor(0,201,255);
doc.rect(0,0,220,40,"F");

doc.setFontSize(25);
doc.text("CATALYST EDUCATIONAL CAMPUS",85,20);

doc.setFontSize(18);
doc.text("REGISTRATION CERTIFICATE",60,30);

doc.rect(10,50,190,200);

doc.setFontSize(12);
doc.text("Name: "+data.name,15,70);
doc.text("Father: "+data.father,15,80);
doc.text("Mobile: "+data.mobile,15,90);
doc.text("Class: "+data.class,15,100);
doc.text("Stream: "+data.stream,15,110);
doc.text("Address: "+data.address,15,120);
doc.text("Aadhar: "+aadhar,15,130);
doc.text("Date: "+data.date,15,140);

doc.addImage(photo,"JPEG",140,60,50,50);

doc.setTextColor(0,150,100);
doc.text("✓ Successfully Registered",50,180);

doc.save(data.name+"_CE_Registration form.pdf");

/* MESSAGE */
document.getElementById("msg").innerText=
"✔ Registration Successful - Welcome to CE Campus";

};

reader.readAsDataURL(file);
});
</script>

</body>
</html>
