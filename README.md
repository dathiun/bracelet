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
    .stone-selection {
      display: flex;
      flex-direction: column;
      align-items: flex-start;
      gap: 10px;
      margin-bottom: 20px;
    }
    .stone-selection-row {
      display: flex;
      gap: 10px;
    }
    .stone-dropdown {
      padding: 5px;
      font-size: 14px;
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
      margin-top: 20px;
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
    <div class="stone-selection" id="stoneSelection">
      <div class="stone-selection-row">
        <select class="stone-dropdown"></select>
      </div>
      <button onclick="addStoneDropdown()">+ إضافة حجر آخر</button>
      <button onclick="generatePatternedBracelet()">ولّد التصميم الذكي</button>
    </div>

    <div class="bracelet-container" id="bracelet"></div>
    <div class="controls">
      <button onclick="resetBracelet()">إعادة التصميم</button>
    </div>
  </div>

  <div class="sidebar" id="stonePanel"></div>

  <script>
    const stones = [
  'https://i.imgur.com/doUTIAd.png',
  'https://i.imgur.com/rjRfPu8.png',
  'https://i.imgur.com/rK01MAV.png',
  'https://i.imgur.com/DGZ8tEY.png',
  'https://i.imgur.com/u8OSpaa.png',
  'https://i.imgur.com/5brUFSs.png',
  'https://i.imgur.com/9fHyBAJ.png',
  'https://i.imgur.com/MFlTbHw.png',
  'https://i.imgur.com/zhX1hz6.png',
  'https://i.imgur.com/6zFJUgn.png',
  'https://i.imgur.com/hpALoFh.png',
  'https://i.imgur.com/ZgGLKde.png',
  'https://i.imgur.com/ixxO9XM.png',
  'https://i.imgur.com/fUjLdCJ.png',
  'https://i.imgur.com/b35xnRX.png',
  'https://i.imgur.com/qyq8sbk.png',
  'https://i.imgur.com/rPiW3j8.png',
  'https://i.imgur.com/nfZW3le.png',
  'https://i.imgur.com/mwG77GL.png',
  'https://i.imgur.com/BN9Bg7q.png',
  'https://i.imgur.com/vt7G56f.png',
  'https://i.imgur.com/qTh2bwC.png'
];

    const bracelet = document.getElementById("bracelet");
    const stonePanel = document.getElementById("stonePanel");
    const stoneSelection = document.getElementById("stoneSelection");
    let selectedStone = "";
    const slots = [];
    const totalSlots = 24;

    function createDropdown() {
      const dropdown = document.createElement("select");
      dropdown.className = "stone-dropdown";
      stones.forEach(url => {
        const option = document.createElement("option");
        option.value = url;
        option.textContent = url.split("/").pop();
        dropdown.appendChild(option);
      });
      return dropdown;
    }

    function addStoneDropdown() {
      const row = document.createElement("div");
      row.className = "stone-selection-row";
      row.appendChild(createDropdown());
      stoneSelection.insertBefore(row, stoneSelection.children[stoneSelection.children.length - 2]);
    }

    function resetBracelet() {
      slots.forEach(slot => slot.style.backgroundImage = '');
    }

    function generatePatternedBracelet() {
      resetBracelet();
      const dropdowns = document.querySelectorAll('.stone-dropdown');
      const selected = Array.from(dropdowns).map(d => d.value);
      if (selected.length < 2) {
        alert("اختر حجرين على الأقل لتوليد النمط الذكي.");
        return;
      }
      const A = selected[0];
      const B = selected[1];

      const pattern = [A, B, B, A];
      for (let i = 0; i < totalSlots; i++) {
        if (i < 4) {
          slots[i].style.backgroundImage = `url('${pattern[i]}')`;
        } else {
          // نكمل باقي السوار بحجر A أو B (نختار A هنا للبساطة)
          slots[i].style.backgroundImage = `url('${A}')`;
        }
      }
    }

    for (let i = 0; i < totalSlots; i++) {
      const angle = (i / totalSlots) * 2 * Math.PI;
      const radius = 160;
      const x = 200 + radius * Math.cos(angle);
      const y = 200 + radius * Math.sin(angle);

      const slot = document.createElement('div');
      slot.className = 'slot';
      slot.style.left = `${x}px`;
      slot.style.top = `${y}px`;
      slot.setAttribute('data-index', i);

      slot.addEventListener('click', () => {
        if (selectedStone) {
          const current = slot.style.backgroundImage;
          if (current.includes(selectedStone)) {
            slot.style.backgroundImage = "";
          } else {
            slot.style.backgroundImage = `url('${selectedStone}')`;
          }
        }
      });

      bracelet.appendChild(slot);
      slots.push(slot);
    }

    stones.forEach(stoneUrl => {
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
      stone.addEventListener('click', () => selectedStone = stoneUrl);
      stonePanel.appendChild(stone);
    });
  </script>
</body>
</html>
