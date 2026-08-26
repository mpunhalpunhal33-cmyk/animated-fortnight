<!DOCTYPE html>
<html lang="ur" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>JIGAR Uploader Pro</title>
<style>
body { font-family: 'Segoe UI', Tahoma; background: #1a1a1a; color: white; padding: 20px; text-align: center; }
.container { max-width: 900px; margin: auto; background: #2d2d2d; padding: 30px; border-radius: 15px; }
h1 { color: #00ff88; }
input[type="file"] { margin: 20px 0; padding: 10px; background: #444; color: white; border-radius: 5px; border: 1px solid #555; }
button { background: #00ff88; color: black; border: none; padding: 15px 40px; font-size: 18px; font-weight: bold; border-radius: 8px; cursor: pointer; margin: 10px; }
button:hover { background: #00cc70; }
button:disabled { background: #555; cursor: not-allowed; }
#gallery { display: flex; flex-wrap: wrap; gap: 15px; margin-top: 30px; justify-content: center; }
#gallery div { position: relative; }
#gallery img { width: 160px; height: 160px; object-fit: cover; border: 3px solid #00ff88; border-radius: 8px; }
.link-box { margin-top: 30px; text-align: right; }
.link-box textarea { width: 100%; height: 200px; padding: 15px; background: #1a1a1a; color: #00ff88; border: 2px solid #00ff88; border-radius: 8px; font-family: monospace; font-size: 14px; }
.status { margin-top: 15px; color: #00ff88; font-weight: bold; }
</style>
</head>
<body>
<div class="container">
<h1>📸 JIGAR Uploader Pro</h1>
<p>تصویریں سلیکٹ کریں → CREATE LINKS دبائیں → ڈائریکٹ CDN لنک حاصل کریں</p>

<input type="file" id="fileInput" multiple accept="image/*">
<br>
<button id="uploadBtn" onclick="uploadImages()">CREATE LINKS</button>
<button onclick="copyLinks()">COPY ALL LINKS</button>

<div class="status" id="status"></div>
<div id="gallery"></div>

<div class="link-box">
<h3>✅ آپ کے CDN لنک:</h3>
<textarea id="links" readonly placeholder="لنک یہاں آئیں گے..."></textarea>
</div>

</div>

<script>
// یہاں اپنا Imgur Client ID لگائیں
const CLIENT_ID = "YOUR_IMGUR_CLIENT_ID_HERE"; 

async function uploadImages() {
  const files = document.getElementById('fileInput').files;
  const status = document.getElementById('status');
  const btn = document.getElementById('uploadBtn');
  
  if(files.length === 0) { alert("پہلے تصویریں سلیکٹ کریں"); return; }
  if(CLIENT_ID === "YOUR_IMGUR_CLIENT_ID_HERE") { 
    alert("پہلے اپنا Client ID لگائیں"); 
    return; 
  }
  
  btn.disabled = true;
  document.getElementById('gallery').innerHTML = "";
  status.innerText = "اپلوڈ ہو رہا ہے... 0/" + files.length;
  let allLinks = "";
  let count = 0;
  
  for(let file of files) {
    let formData = new FormData();
    formData.append("image", file);
    
    try {
      let res = await fetch("https://api.imgur.com/3/image", {
        method: "POST",
        headers: { Authorization: "Client-ID " + CLIENT_ID },
        body: formData
      });
      
      let data = await res.json();
      if(data.success) {
        allLinks += data.data.link + "\n";
        count++;
        status.innerText = "اپلوڈ ہو رہا ہے... " + count + "/" + files.length;
        
        let div = document.createElement("div");
        div.innerHTML = `<img src="${data.data.link}"><br><small>${file.name}</small>`;
        document.getElementById('gallery').appendChild(div);
      }
    } catch(e) {}
  }
  document.getElementById('links').value = allLinks;
  status.innerText = "✅ مکمل! " + count + " لنک بن گئے";
  btn.disabled = false;
}

function copyLinks() {
  const links = document.getElementById('links');
  links.select();
  document.execCommand('copy');
  alert("لنک کاپی ہو گئے!");
}
</script>
</body>
</html>
