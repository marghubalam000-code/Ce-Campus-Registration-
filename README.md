<! Design By Marghubur Rahman>
<html>
<head>
<meta charset="UTF-8">
<title>Catalyst Educational Campus</title>

<script src="https://cdn.jsdelivr.net/npm/emailjs-com@3/dist/email.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

<style>

/* ================= BODY ================= */
body{
    margin:0;
    font-family: 'Segoe UI', sans-serif;
    background: linear-gradient(135deg,#0f2027,#203a43,#2c5364);
}

/* ================= HEADER ================= */
.header{
    background: linear-gradient(90deg,#00c9ff,#92fe9d);
    padding:20px;
    text-align:center;
    color:#003b2f;
    font-size:22px;
    font-weight:bold;
    letter-spacing:1px;
}

/* ================= CONTAINER ================= */
.container{
    max-width:600px;
    margin:30px auto;
    background:#fff;
    padding:25px;
    border-radius:15px;
    box-shadow:0 15px 35px rgba(0,0,0,0.3);
}

/* ================= INPUT ================= */
input,select{
    width:100%;
    padding:12px;
    margin:10px 0;
    border:1px solid #ddd;
    border-radius:8px;
    font-size:14px;
    outline:none;
}

input:focus,select:focus{
    border-color:#00c9ff;
    box-shadow:0 0 5px #00c9ff;
}

/* ================= BUTTON ================= */
button{
    width:100%;
    padding:14px;
    background: linear-gradient(90deg,#00c9ff,#92fe9d);
    border:none;
    border-radius:8px;
    font-size:16px;
    font-weight:bold;
    cursor:pointer;
    margin-top:10px;
    transition:0.3s;
}

button:hover{
    transform:scale(1.02);
}

/* ================= SLIP ================= */
.slip{
    margin-top:20px;
    padding:15px;
    border-radius:10px;
    border:2px dashed #00c9ff;
    background:#f8ffff;
    text-align:center;
}

/* ================= DESIGN BY ================= */
.design-by{
    position:fixed;
    right:10px;
    bottom:10px;
    background:#00c9ff;
    color:#000;
    padding:8px 12px;
    border-radius:8px;
    font-size:13px;
    font-weight:bold;
}

/* ================= SUCCESS ================= */
.success{
    text-align:center;
    color:green;
    font-weight:bold;
}

.hidden{display:none;}

</style>
</head>

<body>

<div class="header">
🎓 CATALYST EDUCATIONAL CAMPUS - ADMISSION PORTAL
</div>

<div class="container">

<form id="form">

<input id="name" placeholder="Student Name" required>
<input id="father" placeholder="Father Name" required>
<input id="mobile" placeholder="Mobile Number" required>
<input id="address" placeholder="Full Address" required>

<input id="aadhar" placeholder="Aadhar (XXXX-XXXX-XXXX)" maxlength="14" required>

<select id="class" required>
<option value="">Select Class</option>
<option>Class 10</option>
<option>Class 11</option>
<option>Class 12</option>
</select>

<select id="stream" onchange="showSubjects()" required>
<option value="">Select Stream</option>
<option value="Science">Science</option>
<option value="Commerce">Commerce</option>
<option value="Arts">Arts</option>
</select>

<div id="scienceBox" class="hidden">
<select id="scienceSubject">
<option value="">Select Subject</option>
<option>Mathematics</option>
<option>Biology</option>
</select>
</div>

<div id="artsBox" class="hidden">
<select id="artsSubject">
<option value="">Select Subject</option>
<option>History</option>
<option>Geography</option>
<option>Political Science</option>
<option>Economics</option>
</select>
</div>

<input type="file" id="photo" required>

<button type="submit">REGISTER STUDENT</button>

</form>

<p id="msg" class="success"></p>
<div id="slip"></div>

</div>

<div class="design-by">Design by Rahman💞</div>

<script>
emailjs.init("q7WRi2qk3AUR725UG");

/* ================= SUBJECT ================= */
function showSubjects(){
    let s=document.getElementById("stream").value;
    document.getElementById("scienceBox").style.display=(s==="Science")?"block":"none";
    document.getElementById("artsBox").style.display=(s==="Arts")?"block":"none";
}

/* ================= AADHAR FORMAT ================= */
document.getElementById("aadhar").addEventListener("input",function(e){
    let v=e.target.value.replace(/\D/g,"").substring(0,12);
    let f=v;
    if(v.length>4) f=v.substring(0,4)+"-"+v.substring(4);
    if(v.length>8) f=v.substring(0,4)+"-"+v.substring(4,8)+"-"+v.substring(8);
    e.target.value=f;
});

/* ================= SUBMIT ================= */
document.getElementById("form").addEventListener("submit",function(e){
e.preventDefault();

let stream=document.getElementById("stream").value;
let subject="";

if(stream==="Science"){
subject=document.getElementById("scienceSubject").value;
if(!subject){alert("Select Subject");return;}
}
if(stream==="Arts"){
subject=document.getElementById("artsSubject").value;
if(!subject){alert("Select Subject");return;}
}
if(stream==="Commerce") subject="Commerce";

let file=document.getElementById("photo").files[0];
if(!file){alert("Upload photo");return;}

let aadhar=document.getElementById("aadhar").value.replace(/-/g,"");
if(aadhar.length!==12){
alert("Invalid Aadhar");
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
stream:stream,
subject:subject,
date:new Date().toLocaleString()
};

/* EMAIL */
emailjs.send("service_bnw6tan","template_ye9opt9",{
name:data.name,
father:data.father,
mobile:data.mobile,
address:data.address,
aadhar:aadhar,
class:data.class,
stream:data.stream,
subject:data.subject,
date:data.date
});

/* WHATSAPP */
let msg=`New Admission:
Name:${data.name}
Father:${data.father}
Mobile:${data.mobile}
Class:${data.class}
Stream:${data.stream}
Subject:${data.subject}`;

window.sent("https://wa.me/919263960341?text="+encodeURIComponent(msg));

/* SLIP */
document.getElementById("slip").innerHTML=`
<div class="slip">
<img src="${photo}" style="width:120px;height:120px;border-radius:10px;"><br><br>

<b>Name:</b> ${data.name}<br>
<b>Class:</b> ${data.class}<br>
<b>Stream:</b> ${data.stream}<br>
<b>Subject:</b> ${data.subject}<br>
<b>Date:</b> ${data.date}<br>

</div>`;

/* PDF */
const {jsPDF}=window.jspdf;
let doc=new jsPDF();

doc.setFillColor(0,201,255);
doc.rect(0,0,220,40,"F");

doc.setFontSize(20);
doc.setTextColor(0,0,0);
doc.text("CATALYST EDUCATIONAL CAMPUS",25,25);

doc.setFontSize(12);
doc.text("ADMISSION CERTIFICATE",65,35);

doc.rect(10,50,190,200);

doc.text("Name:"+data.name,15,70);
doc.text("Father:"+data.father,15,80);
doc.text("Class:"+data.class,15,90);
doc.text("Stream:"+data.stream,15,100);
doc.text("Subject:"+data.subject,15,110);
doc.text("Mobile:"+data.mobile,15,120);
doc.text("Date:"+data.date,15,130);

doc.addImage(photo,"JPEG",140,60,50,50);

doc.save(data.name+"_certificate.pdf");

document.getElementById("msg").innerText="Registration Successful";

};

reader.readAsDataURL(file);
});
</script>

</body>
</html>
