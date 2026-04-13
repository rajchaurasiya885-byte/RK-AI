# RK-AI
Ai website 
<!DOCTYPE html>
<html>
<head>
  <title>Raj AI Pro</title>
  <style>
    body {
      font-family: Arial;
      background: linear-gradient(135deg, #0f172a, #1e293b);
      color: white;
      margin: 0;
    }

    header {
      background: #38bdf8;
      padding: 15px;
      text-align: center;
      font-size: 24px;
      color: black;
    }

    #chat {
      height: 75vh;
      overflow-y: auto;
      padding: 15px;
    }

    .msg {
      max-width: 70%;
      padding: 12px;
      border-radius: 12px;
      margin: 8px;
      font-size: 18px;
    }

    .user {
      background: #facc15;
      color: black;
      margin-left: auto;
    }

    .ai {
      background: #4ade80;
      color: black;
    }

    .input-area {
      display: flex;
      padding: 10px;
      background: #020617;
    }

    input {
      flex: 1;
      padding: 12px;
      border-radius: 10px;
      border: none;
      font-size: 18px;
    }

    button {
      margin-left: 8px;
      padding: 12px;
      border: none;
      border-radius: 10px;
      background: #38bdf8;
      cursor: pointer;
    }
  </style>
</head>
<body>

<header>🤖 Raj AI Pro</header>

<div id="chat"></div>

<div class="input-area">
  <input id="question" placeholder="Ask anything...">
  <button onclick="askAI()">Send</button>
  <button onclick="startVoice()">🎤</button>
</div>

<script>
// 🔊 Voice
function speak(text) {
  let speech = new SpeechSynthesisUtterance(text);
  speech.lang = "hi-IN";
  window.speechSynthesis.speak(speech);
}

// 💬 Add message
function addMessage(text, type) {
  let chat = document.getElementById("chat");
  let div = document.createElement("div");
  div.className = "msg " + type;
  div.innerText = text;
  chat.appendChild(div);
  chat.scrollTop = chat.scrollHeight;
}

// 🤖 REAL AI
async function askAI() {
  let input = document.getElementById("question");
  let q = input.value;
  if (!q) return;

  addMessage(q, "user");
  input.value = "";

  let res = await fetch("https://api.openai.com/v1/chat/completions", {
    method: "POST",
    headers: {
      "Authorization": "Bearer YOUR_API_KEY_HERE",
      "Content-Type": "application/json"
    },
    body: JSON.stringify({
      model: "gpt-4o-mini",
      messages: [{ role: "user", content: q }]
    })
  });

  let data = await res.json();
  let reply = data.choices[0].message.content;

  addMessage(reply, "ai");
  speak(reply);
}

// 🎤 Voice input
function startVoice() {
  const rec = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
  rec.lang = "hi-IN";
  rec.start();

  rec.onresult = function(e) {
    let text = e.results[0][0].transcript;
    document.getElementById("question").value = text;
    askAI();
  };
}
</script>

</body>
</html>
