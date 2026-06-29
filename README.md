# index.html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8" />
  <title>AI Studio</title>
  <style>
    body { font-family: sans-serif; background:#111; color:#fff; margin:0; }
    .box { max-width:600px; margin:auto; padding:10px; }
    #chat { height:70vh; overflow:auto; border:1px solid #333; padding:10px; }
    input, select, button { width:100%; padding:10px; margin-top:5px; }
    .msg { margin:10px 0; }
    .user { color:#4af; }
    .ai { color:#afa; }
  </style>
</head>
<body>
<div class="box">
  <h2>AI Studio</h2>

  <select id="model">
    <option value="grok">Grok</option>
    <option value="gemini">Gemini</option>
    <option value="qwen">Qwen</option>
  </select>

  <div id="chat"></div>

  <input id="text" placeholder="Write prompt..." />
  <button onclick="send()">Send</button>
</div>

<script>
async function send() {
  let text = document.getElementById("text").value;
  let model = document.getElementById("model").value;

  add("user", text);

  let res = await fetch("https://YOUR-WORKER-URL/api", {
    method: "POST",
    headers: {"Content-Type":"application/json"},
    body: JSON.stringify({ text, model })
  });

  let data = await res.json();
  add("ai", data.result);

  document.getElementById("text").value = "";
}

function add(type, text){
  let div = document.createElement("div");
  div.className = "msg " + type;
  div.innerText = type + ": " + text;
  document.getElementById("chat").appendChild(div);
}
</script>
</body>
</html>