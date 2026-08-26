<!DOCTYPE html>
<html lang="ur" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>JIGAR Uploader Pro</title>
<style>
body{background:#111;color:#fff;text-align:center;padding:20px;font-family:Arial}
h1{color:#00ff88}
.btn{background:#00ff88;color:#000;border:none;padding:15px 30px;font-size:18px;font-weight:bold;border-radius:8px;margin:10px;cursor:pointer}
.btn:disabled{background:#555;cursor:not-allowed}
input{padding:10px;background:#333;color:#fff;border-radius:5px;border:1px solid #555}
#gallery{display:flex;flex-wrap:wrap;gap:10px;justify-content:center;margin-top:20px}
#gallery img{width:150px;height:150px;object-fit:cover;border:2px solid #00ff88;border-radius:8px}
#links{width:90%;height:150px;background:#000;color:#0f0;border:2px solid #0f0;padding:10px;font-family:monospace;margin-top:15px;font-size:14px;text-align:left}
</style>
</head>
<body>
<h1>📸 JIGAR Uploader Pro</h1>
<p>تصویریں سلیکٹ کریں → CREATE LINKS دبائیں</p>

<input type="file" id="f" multiple accept="image/*"><br>
<button class="btn" id="btn" onclick="u()">CREATE LINKS</button>
<button class="btn" onclick="c()">COPY ALL LINKS</button>

<p id="s" style="color:#00ff88;font-weight:bold;margin-top:15px"></p>
<div id="gallery"></div>

<textarea id="links" readonly placeholder="لنک یہاں آئیں گے..."></textarea>
<script>
if(!f.files.length)return alert("پہلے فائل سلیکٹ کریں");
const ID="a1b2c3d4e5f6g7";  // 👈 یہ نئی لائن لگائیں
if(ID==="PASTE_YOUR_CLIENT_ID_HERE")return a
if(!f.files.length)return alert("پہلے فائل سلیکٹ کریں");
if(ID==="PASTE_YOUR_CLIENT_ID_HERE")return alert("پہلے Client ID لگائیں");
btn.disabled=true;
s.innerText="اپلوڈ ہو رہا ہے... 0/"+f.files.length;
gallery.innerHTML="";
let L="";
let count=0;
for(let x of f.files){
let d=new FormData();
d.append("image",x);
let r=await fetch("https://api.imgur.com/3/image",{method:"POST",headers:{Authorization:"Client-ID "+ID},body:d});
let j=await r.json();
if(j.success){
L+=j.data.link+"\n";
count++;
s.innerText="اپلوڈ ہو رہا ہے... "+count+"/"+f.files.length;
gallery.innerHTML+=`<img src="${j.data.link}">`;
}
links.value=L;
s.innerText="✅ مکمل! "+count+" لنک بن گئے";
btn.disabled=false;
}

function c(){
links.select();
document.execCommand("copy");
alert("لنک کاپی ہو گئے");
}
</script>
</body>
</html>
