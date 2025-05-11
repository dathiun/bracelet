<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <title>DATHIUN AI Bracelet Generator</title>
  <style>
    body {
      margin: 0;
      font-family: 'Arial', sans-serif;
      background: #f4f4f4;
      display: flex;
      flex-direction: row;
      justify-content: space-between;
      align-items: flex-start;
      height: 100vh;
      padding: 20px;
    }
    .main {
      flex: 1;
      display: flex;
      flex-direction: column;
      align-items: center;
    }
    .selection-bar {
      display: flex;
      gap: 10px;
      margin-bottom: 10px;
    }
    .selection-slot {
      width: 50px;
      height: 50px;
      border-radius: 50%;
      border: 2px dashed #aaa;
      background-size: cover;
      background-position: center;
      cursor: pointer;
    }
    .bracelet-container {
      position: relative;
      width: 400px;
      height: 400px;
    }
    .slot {
      width: 40px;
      height: 40px;
      border-radius: 50%;
      background: #fff;
      border: 2px solid #555;
      position: absolute;
      transform: translate(-50%, -50%);
      background-size: cover;
      background-position: center;
      cursor: pointer;
    }
    .controls {
      margin-top: 10px;
    }
    .controls button {
      background: #2ecc71;
      border: none;
      padding: 10px 15px;
      margin: 5px;
      font-size: 14px;
      cursor: pointer;
      color: white;
      border-radius: 6px;
    }
    .sidebar {
      width: 240px;
      background: #fff;
      border-left: 2px solid #ccc;
      padding: 20px 10px;
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 10px;
      height: auto;
      overflow-y: auto;
    }
  </style>
</head>
<body>
  <div class="main">
    <div class="selection-bar" id="selectionBar"></div>
    <div class="controls">
      <button onclick="generatePatternedBracelet()">ولّد التصميم الذكي</button>
      <button onclick="resetBracelet()">إعادة التصميم</button>
    </div>
    <div class="bracelet-container" id="bracelet"></div>
  </div>

  <div class="sidebar" id="stonePanel"></div>

  <script>
    const stoneImages = [
      'https://i.imgur.com/doUTIAd.png','https://i.imgur.com/rjRfPu8.png','https://i.imgur.com/rK01MAV.png',
      'https://i.imgur.com/DGZ8tEY.png','https://i.imgur.com/u8OSpaa.png','https://i.imgur.com/5brUFSs.png',
      'https://i.imgur.com/9fHyBAJ.png','https://i.imgur.com/MFlTbHw.png','https://i.imgur.com/zhX1hz6.png',
      'https://i.imgur.com/6zFJUgn.png','https://i.imgur.com/hpALoFh.png','https://i.imgur.com/ZgGLKde.png',
      'https://i.imgur.com/ixxO9XM.png','https://i.imgur.com/fUjLdCJ.png','https://i.imgur.com/b35xnRX.png',
      'https://i.imgur.com/qyq8sbk.png','https://i.imgur.com/rPiW3j8.png','https://i.imgur.com/nfZW3le.png',
      'https://i.imgur.com/mwG77GL.png','https://i.imgur.com/BN9Bg7q.png','https://i.imgur.com/vt7G56f.png',
      'https://i.imgur.com/qTh2bwC.png'
    ];

    const patterns = {
      "A-B": [0, 1],
      "A-B-B-A": [0, 1, 1, 0],
      "A-B-C": [0, 1, 2],
      "متماثل": "mirror",
      "عشوائي": "random"
    };

    const bracelet = document.getElementById("bracelet");
    const stonePanel = document.getElementById("stonePanel");
    const selectionBar = document.getElementById("selectionBar");
    let currentStone = "";
    const braceletSlots = [];
    const selectedSlots = [];
    const totalBraceletSlots = 24;

    for (let i = 0; i < 6; i++) {
      const selector = document.createElement("div");
      selector.className = "selection-slot";
      selector.dataset.index = i;
      selector.addEventListener("click", () => {
        if (currentStone) {
          if (selector.dataset.value === currentStone) {
            selector.style.backgroundImage = "";
            selector.dataset.value = "";
          } else {
            selector.style.backgroundImage = `url('${currentStone}')`;
            selector.dataset.value = currentStone;
          }
        }
      });
      selectionBar.appendChild(selector);
      selectedSlots.push(selector);
    }

    function generatePatternedBracelet() {
      resetBracelet();
      const selectedStones = selectedSlots.map(s => s.dataset.value).filter(Boolean);
      if (selectedStones.length === 0) {
        alert("اختر حجرًا واحدًا على الأقل لتوليد السوار.");
        return;
      }

      const basePattern = patterns["A-B"]; // استخدم نمط قابل للتغيير لاحقًا
      for (let i = 0; i < totalBraceletSlots; i++) {
        const patternIndex = basePattern[i % basePattern.length];
        const stone = selectedStones[patternIndex % selectedStones.length];
        braceletSlots[i].style.backgroundImage = `url('${stone}')`;
      }
    }

    function resetBracelet() {
      braceletSlots.forEach(slot => slot.style.backgroundImage = '');
    }

    for (let i = 0; i < totalBraceletSlots; i++) {
      const angle = (i / totalBraceletSlots) * 2 * Math.PI;
      const radius = 160;
      const x = 200 + radius * Math.cos(angle);
      const y = 200 + radius * Math.sin(angle);

      const slot = document.createElement('div');
      slot.className = 'slot';
      slot.style.left = `${x}px`;
      slot.style.top = `${y}px`;
      slot.setAttribute('data-index', i);

      slot.addEventListener('click', () => {
        if (currentStone) {
          const current = slot.style.backgroundImage;
          if (current.includes(currentStone)) {
            slot.style.backgroundImage = "";
          } else {
            slot.style.backgroundImage = `url('${currentStone}')`;
          }
        }
      });

      bracelet.appendChild(slot);
      braceletSlots.push(slot);
    }

    stoneImages.forEach(stoneUrl => {
      const stone = document.createElement('div');
      stone.className = 'stone';
      stone.style.width = '50px';
      stone.style.height = '50px';
      stone.style.borderRadius = '50%';
      stone.style.backgroundSize = 'cover';
      stone.style.backgroundPosition = 'center';
      stone.style.border = '2px solid #fff';
      stone.style.cursor = 'pointer';
      stone.style.backgroundImage = `url('${stoneUrl}')`;
      stone.addEventListener('click', () => currentStone = stoneUrl);
      stonePanel.appendChild(stone);
    });
  </script>
</body>
</html>
