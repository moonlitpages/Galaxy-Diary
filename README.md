<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Galaxy Diary 💫</title>
  <style>
    body {
      font-family: 'Comic Sans MS', cursive, sans-serif;
      background: radial-gradient(circle at top, #1a1a40, #000000 80%);
      margin: 0;
      padding: 0;
      color: #f0e6ff;
      overflow: hidden;
    }

    /* 🌟 Star field */
    .stars {
      position: fixed;
      width: 100%;
      height: 100%;
      top: 0;
      left: 0;
      z-index: 0;
      pointer-events: none;
      overflow: hidden;
    }
    .star {
      position: absolute;
      background: white;
      border-radius: 50%;
      opacity: 0.8;
      animation: twinkle 2s infinite alternate;
    }
    @keyframes twinkle {
      from { opacity: 0.3; transform: scale(0.8); }
      to { opacity: 1; transform: scale(1.2); }
    }

    /* 🌙 Moon */
    .moon {
      position: fixed;
      top: 60px;
      right: 80px;
      width: 80px;
      height: 80px;
      background: radial-gradient(circle at 30% 30%, #fff, #ddd 70%);
      border-radius: 50%;
      box-shadow: 0 0 40px rgba(255,255,255,0.6);
      animation: floatMoon 6s ease-in-out infinite alternate;
      z-index: 0;
    }
    @keyframes floatMoon {
      from { transform: translateY(0); }
      to { transform: translateY(-15px); }
    }

    /* 🪐 Planets */
    .planet {
      position: fixed;
      border-radius: 50%;
      opacity: 0.9;
      z-index: 0;
      animation: orbit linear infinite;
    }
    .planet.pink {
      width: 40px; height: 40px;
      background: radial-gradient(circle, #ff99cc, #cc6699);
      top: 200px; left: 15%;
      animation-duration: 25s;
    }
    .planet.blue {
      width: 60px; height: 60px;
      background: radial-gradient(circle, #99ccff, #3366cc);
      top: 400px; left: 70%;
      animation-duration: 40s;
    }
    @keyframes orbit {
      from { transform: rotate(0deg) translateX(50px) rotate(0deg); }
      to { transform: rotate(360deg) translateX(50px) rotate(-360deg); }
    }

    /* 📦 Container */
    .container {
      position: relative;
      z-index: 1;
      max-width: 600px;
      margin: 40px auto;
      background: rgba(255,255,255,0.1);
      border-radius: 20px;
      padding: 20px;
      box-shadow: 0px 8px 20px rgba(255,255,255,0.2);
      text-align: center;
      backdrop-filter: blur(6px);
    }
    h1 {
      color: #f8dfff;
      animation: glow 3s infinite alternate;
    }
    @keyframes glow {
      from { text-shadow: 0 0 10px #ff80ff, 0 0 20px #ffccff; }
      to { text-shadow: 0 0 20px #a6c1ee, 0 0 40px #ffffff; }
    }
    h2 { color: #d1c4e9; }
    textarea {
      width: 100%;
      height: 100px;
      border: 2px solid #ba68c8;
      border-radius: 15px;
      padding: 10px;
      resize: none;
      font-size: 16px;
      background: #fce4ec;
    }
    button {
      margin-top: 10px;
      background: #ba68c8;
      border: none;
      padding: 10px 20px;
      border-radius: 15px;
      color: white;
      font-size: 16px;
      cursor: pointer;
      transition: all 0.3s;
    }
    button:hover {
      background: #8e44ad;
      box-shadow: 0 0 15px #f48fb1, 0 0 30px #ba68c8;
    }
    .message {
      background: rgba(255,255,255,0.1);
      border: 1px solid #e1bee7;
      border-radius: 15px;
      padding: 10px;
      margin: 10px 0;
      text-align: left;
      animation: fadeIn 1s ease-in-out;
    }
    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(20px); }
      to { opacity: 1; transform: translateY(0); }
    }
    #error { color: #ff8080; margin-top: 5px; }
  </style>
</head>
<body>
  <!-- Galaxy background -->
  <div class="stars"></div>
  <div class="moon"></div>
  <div class="planet pink"></div>
  <div class="planet blue"></div>

  <div class="container">
    <h1>🌌 My Personal Diary 💫</h1>
    <form id="messageForm">
      <textarea id="messageInput" placeholder="Write anything in your mind... 💭"></textarea>
      <button type="submit">Send 💌</button>
    </form>
    <div id="error"></div>
    <h2>Recent Entries ✨</h2>
    <div id="messages"></div>
  </div>

  <script>
    const badWords = ["badword1","badword2","curse"]; // simple filter

    function loadMessages() {
      const messages = JSON.parse(localStorage.getItem("messages") || "[]");
      const messagesDiv = document.getElementById("messages");
      messagesDiv.innerHTML = "";
      messages.slice(-20).reverse().forEach(msg => {
        const p = document.createElement("p");
        p.textContent = msg.text;
        p.className = "message";
        messagesDiv.appendChild(p);
      });
    }

    document.getElementById("messageForm").addEventListener("submit", (e) => {
      e.preventDefault();
      const text = document.getElementById("messageInput").value.trim();
      if (text.length < 3) {
        document.getElementById("error").textContent = "Message too short! 🌱";
        return;
      }
      if (badWords.some(bad => text.toLowerCase().includes(bad))) {
        document.getElementById("error").textContent = "Message contains inappropriate words 🚫";
        return;
      }
      const messages = JSON.parse(localStorage.getItem("messages") || "[]");
      messages.push({ text });
      localStorage.setItem("messages", JSON.stringify(messages));
      document.getElementById("messageInput").value = "";
      document.getElementById("error").textContent = "";
      loadMessages();
    });

    loadMessages();

    // 🌟 Create random stars
    function createStars() {
      const starsContainer = document.querySelector('.stars');
      for (let i = 0; i < 100; i++) {
        const star = document.createElement('div');
        star.classList.add('star');
        const size = Math.random() * 3 + 1; // 1px - 4px
        star.style.width = size + "px";
        star.style.height = size + "px";
        star.style.top = Math.random() * 100 + "%";
        star.style.left = Math.random() * 100 + "%";
        star.style.animationDuration = (Math.random() * 3 + 2) + "s";
        starsContainer.appendChild(star);
      }
    }
    createStars();
  </script>
</body>
</html>
