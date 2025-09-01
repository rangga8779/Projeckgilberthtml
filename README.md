<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>GILBERT HTML</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: Arial, sans-serif;
    }

    body, html {
      height: 100%;
      overflow: hidden;
      background: black;
      color: white;
    }

    /* === Galaxy background === */
    canvas#stars {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: -1;
    }

    /* === Judul === */
    h1 {
      text-align: center;
      margin-top: 60px;
      font-size: 3rem;
      background: linear-gradient(90deg, #00d0ff, #ff00f7);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      animation: glow 2s infinite alternate;
    }

    @keyframes glow {
      from { text-shadow: 0 0 10px #00d0ff; }
      to { text-shadow: 0 0 20px #ff00f7; }
    }

    /* === Container tombol === */
    .controls {
      margin-top: 40px;
      display: flex;
      justify-content: center;
      gap: 20px;
    }

    button, label {
      padding: 12px 20px;
      font-size: 1rem;
      border: none;
      border-radius: 10px;
      cursor: pointer;
      background: linear-gradient(135deg, #00d0ff, #ff00f7);
      color: white;
      font-weight: bold;
      box-shadow: 0 0 10px rgba(255,255,255,0.3);
      transition: transform 0.3s ease, box-shadow 0.3s ease;
      animation: float 3s ease-in-out infinite;
    }

    button:hover, label:hover {
      transform: scale(1.1);
      box-shadow: 0 0 20px rgba(255,255,255,0.8);
    }

    @keyframes float {
      0%, 100% { transform: translateY(0); }
      50% { transform: translateY(-8px); }
    }

    iframe {
      width: 90%;
      height: 400px;
      margin: 40px auto;
      display: block;
      border: 2px solid #fff;
      border-radius: 12px;
      box-shadow: 0 0 20px rgba(255,255,255,0.3);
    }
  </style>
</head>
<body>
  <canvas id="stars"></canvas>
  <h1>🚀 GILBERT HTML 🌌</h1>

  <div class="controls">
    <label for="fileUpload">📂 Upload HTML</label>
    <input type="file" id="fileUpload" accept=".html" hidden>
    <button onclick="deployAlert()">🚀 Deploy</button>
  </div>

  <iframe id="preview"></iframe>

  <script>
    // === Stars animation ===
    const canvas = document.getElementById("stars");
    const ctx = canvas.getContext("2d");
    let stars = [];

    function resizeCanvas() {
      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight;
    }
    window.addEventListener("resize", resizeCanvas);
    resizeCanvas();

    function createStars() {
      stars = [];
      for (let i = 0; i < 150; i++) {
        stars.push({
          x: Math.random() * canvas.width,
          y: Math.random() * canvas.height,
          radius: Math.random() * 2,
          speed: Math.random() * 0.5 + 0.2
        });
      }
    }

    function drawStars() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      ctx.fillStyle = "white";
      stars.forEach(star => {
        ctx.beginPath();
        ctx.arc(star.x, star.y, star.radius, 0, Math.PI * 2);
        ctx.fill();
        star.y += star.speed;
        if (star.y > canvas.height) {
          star.y = 0;
          star.x = Math.random() * canvas.width;
        }
      });
      requestAnimationFrame(drawStars);
    }

    createStars();
    drawStars();

    // === File Upload ===
    const fileInput = document.getElementById("fileUpload");
    const preview = document.getElementById("preview");

    fileInput.addEventListener("change", () => {
      const file = fileInput.files[0];
      if (file && file.type === "text/html") {
        const reader = new FileReader();
        reader.onload = e => {
          const blob = new Blob([e.target.result], { type: "text/html" });
          const url = URL.createObjectURL(blob);
          preview.src = url;
        };
        reader.readAsText(file);
      }
    });

    // === Deploy button ===
    function deployAlert() {
      alert("Untuk deploy sungguhan: upload project ini ke Vercel 🚀");
    }
  </script>
</body>
</html>
