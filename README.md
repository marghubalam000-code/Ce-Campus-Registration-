<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Catalyst Educational Campus</title>

<script src="https://cdn.jsdelivr.net/npm/emailjs-com@3/dist/email.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

<style>
body{
    font-family:Arial;
    margin:0;
    background:linear-gradient(135deg,#0f2027,#203a43,#2c5364);
}

.header{
    padding:18px;
    text-align:center;
    background:linear-gradient(90deg,#00c9ff,#92fe9d);
    font-weight:bold;
    font-size:20px;
}

.container{
    max-width:600px;
    margin:25px auto;
    background:#fff;
    padding:25px;
    border-radius:12px;
}

input,select{
    width:100%;
    padding:12px;
    margin:8px 0;
    border-radius:8px;
    border:1px solid #ccc;
}

button{
    width:100%;
    padding:12px;
    background:#00c9ff;
    border:none;
    border-radius:8px;
    font-weight:bold;
    cursor:pointer;
}

button:hover{background:#00b3e6;}

.slip{
    margin-top:15px;
    padding:15px;
    border:2px dashed #00c9ff;
    background:#f4ffff;
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
}

.hidden{display:none;}

.success{
    text-align:center;
    color:green;
    font-weight:bold;
}
</style>
</head>

<body>

<div class="header">CATALYST EDUCATIONAL CAMPUS - REGISTRATION</div>

<div class="container">

<form id="form">

<input id="name" placeholder="Student Name" required>
<input id="father" placeholder="Father Name" required>
<input id="mobile" placeholder="Mobile" required>
<input id="address" placeholder="Address" required>

<input id="aadhar" placeholder="Aadhar (12 digits)" maxlength="12" required>

<select id="class" required>
<option value="">Select Class</option>
<option>Class 10</option>
<option>Class 11</option>
<option>Class 12</option>
</select>

<select id="stream" required>
<option value="">Select Stream</option>
<option value="Science">Science</option>
<option value="Arts">Arts</option>
<option value="Commerce">Commerce</option>
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
</select>
</div>

<input type="file" id="photo" required>

<button type="submit">REGISTER STUDENT</button>

</form>

<p id="msg" class="success"></p>
<div id="slip"></div>

</div>

<div class="design">Design by M Rahman</div>

<script>
emailjs.init("q7WRi2qk3AUR725UG");

/* STREAM CHANGE */
document.getElementById("stream").addEventListener("change",function(){
let s=this.value;
document.getElementById("scienceBox").style.display=(s==="Science")?"block":"none";
document.getElementById("artsBox").style.display=(s==="Arts")?"block":"none";
});

/* FORM SUBMIT - FIXED */
document.getElementById("form").addEventListener("submit",function(e){
e.preventDefault();

let file=document.getElementById("photo").files[0];
if(!file){alert("Upload photo");return;}

let aadhar=document.getElementById("aadhar").value.trim();
if(!/^\d{12}$/.test(aadhar)){
alert("Invalid Aadhar (12 digits only)");
return;
}

let stream=document.getElementById("stream").value;
let subject="";

if(stream==="Science"){
subject=document.getElementById("scienceSubject").value;
}
if(stream==="Arts"){
subject=document.getElementById("artsSubject").value;
}
if(stream==="Commerce") subject="Commerce";

if(!subject){alert("Select subject");return;}

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

/* SLIP FIXED */
document.getElementById("slip").innerHTML=`
<div class="slip">
<img src="${photo}" style="width:120px;height:120px;border-radius:10px;"><br><br>
<b>Name:</b> ${data.name}<br>
<b>Class:</b> ${data.class}<br>
<b>Stream:</b> ${data.stream}<br>
<b>Subject:</b> ${data.subject}<br>
<b>Date:</b> ${data.date}<br>
</div>`;

/* PDF FIXED */
const {jsPDF}=window.jspdf;
let doc=new jsPDF();

doc.setFontSize(18);
doc.text("CATALYST EDUCATIONAL CAMPUS",20,20);

doc.setFontSize(12);
doc.text("REGISTRATION CERTIFICATE",60,30);

doc.rect(10,40,190,140);

doc.text("Name: "+data.name,15,60);
doc.text("Father: "+data.father,15,70);
doc.text("Class: "+data.class,15,80);
doc.text("Stream: "+data.stream,15,90);
doc.text("Subject: "+data.subject,15,100);
doc.text("Mobile: "+data.mobile,15,110);
doc.text("Date: "+data.date,15,120);

doc.addImage(photo,"JPEG",140,50,50,50);

doc.save(data.name+"_certificate.pdf");

/* SUCCESS */
document.getElementById("msg").innerText="Registration Successful 🎉";

};

reader.readAsDataURL(file);
});
</script>

</body>
</html>
