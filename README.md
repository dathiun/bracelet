
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
        body {
            margin: 0;
            font-family: 'Arial', sans-serif;
            background: #ffffff;
            display: flex;
            flex-direction: column;
        }
        header {
            text-align: center;
            padding: 20px;
            background-color: white;
        }
        .main-container {
            display: flex;
            flex-direction: row;
            justify-content: space-between;
            align-items: center;
            flex: 1;
        }
        .main {
            flex: 1;
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        .bracelet-container {
            position: relative;
            width: 400px;
            height: 400px;
            margin-top: 20px;
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
            background: #f1c40f;
            border: none;
            padding: 10px 15px;
            margin: 5px;
            font-size: 14px;
            cursor: pointer;
        }
        .sidebar {
            width: 240px;
            background: #f1c40f;
            border-radius: 0 20px 20px 0;
            padding: 20px 10px;
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 10px;
            height: auto;
            margin-right: 20px;
        }
        .stone {
            width: 50px;
            height: 50px;
            border-radius: 50%;
            background-size: cover;
            background-position: center;
            border: 2px solid #fff;
            cursor: pointer;
        }
        .preview {
            position: fixed;
            bottom: 15px;
            left: 15px;
            width: 80px;
            height: 80px;
            border-radius: 10px;
            border: 2px solid #333;
            background-size: cover;
            background-position: center;
            display: none;
        }
    </style>
</head>
<body>
    <header>
        <img src="https://i.imgur.com/kdFqlF7.png" alt="DATHIUN Store" style="max-width: 200px; height: auto;">
        <h1 style="margin: 10px 0 5px; font-size: 24px; color: #222;">اصنع سوارك بنفسك</h1>
        <p style="margin: 0; font-size: 14px; color: #555;">اختر الحجر واضغط على المكان المناسب في السوار</p>
    </header>

    <div class="main-container">
        <div class="main">
            <div class="bracelet-container" id="bracelet"></div>
            <div class="controls">
                <button onclick="resetBracelet()">إعادة التصميم</button>
                <button onclick="saveBracelet()">تحميل الصورة</button>
            </div>
        </div>

        <div class="sidebar" id="stonePanel"></div>
    </div>

    <div class="preview" id="previewBox"></div>

    <script src="https://html2canvas.hertzen.com/dist/html2canvas.min.js"></script>
    <script>
        const stones = [
            'https://i.imgur.com/V4oFzfl.jpg',
            'https://i.imgur.com/I8B8nLx.jpg',
            'https://i.imgur.com/ZlE6Snk.jpg',
            'https://i.imgur.com/FOU9kK3.jpg',
            'https://i.imgur.com/9AlRjsn.jpg',
            'https://i.imgur.com/l9m95mL.jpg',
            'https://i.imgur.com/0pKk8zk.jpg',
            'https://i.imgur.com/uyht9Dr.jpg',
            'https://i.imgur.com/HoPAwzu.jpg',
            'https://i.imgur.com/nAy4Sce.jpg'
        ];

        const stonePanel = document.getElementById("stonePanel");
        const previewBox = document.getElementById("previewBox");
        let selectedStone = "";

        stones.forEach(stoneUrl => {
            const stone = document.createElement('div');
            stone.className = 'stone';
            stone.style.backgroundImage = `url('${stoneUrl}')`;

            stone.addEventListener('click', () => {
                selectedStone = stoneUrl;
            });

            stone.addEventListener('mouseover', () => {
                previewBox.style.backgroundImage = `url('${stoneUrl}')`;
                previewBox.style.display = "block";
            });

            stone.addEventListener('mouseout', () => {
                previewBox.style.display = "none";
            });

            stonePanel.appendChild(stone);
        });

        const bracelet = document.getElementById("bracelet");
        const totalSlots = 24;

        for (let i = 0; i < totalSlots; i++) {
            const angle = (i / totalSlots) * 2 * Math.PI;
            const radius = 160;
            const x = 200 + radius * Math.cos(angle);
            const y = 200 + radius * Math.sin(angle);

            const slot = document.createElement('div');
            slot.className = 'slot';
            slot.setAttribute('data-index', i);
            slot.style.left = `${x}px`;
            slot.style.top = `${y}px`;

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
        }

        function resetBracelet() {
            document.querySelectorAll('.slot').forEach(slot => {
                slot.style.backgroundImage = '';
            });
        }

        function saveBracelet() {
            html2canvas(document.body).then(canvas => {
                const link = document.createElement("a");
                link.download = "bracelet_design.png";
                link.href = canvas.toDataURL();
                link.click();
            });
        }
    </script>
</body>
</html>
