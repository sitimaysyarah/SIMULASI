<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Latihan Hirarki File & Folder (Tree)</title>
    <style>
        * {
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background-color: #f4f7f6;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 20px;
        }

        h2 {
            color: #333;
            margin-bottom: 5px;
        }

        p {
            color: #666;
            margin-bottom: 20px;
        }

        .container {
            display: flex;
            gap: 40px;
            background: white;
            padding: 30px;
            border-radius: 12px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
            max-width: 800px;
            width: 100%;
        }

        .section {
            flex: 1;
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        .section h3 {
            margin-bottom: 15px;
            color: #444;
        }

        /* Area Pilihan (Acak) */
        #pool {
            width: 100%;
            min-height: 350px;
            border: 2px dashed #bbb;
            border-radius: 8px;
            padding: 15px;
            display: flex;
            flex-direction: column;
            gap: 15px;
            align-items: center;
            background-color: #fafafa;
        }

        /* Area Menyusun Hirarki */
        .tree-container {
            width: 100%;
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        .dropzone {
            width: 220px;
            height: 60px;
            border: 2px dashed #999;
            border-radius: 10px;
            display: flex;
            justify-content: center;
            align-items: center;
            background-color: #f9f9f9;
            transition: all 0.3s ease;
        }

        .dropzone.hovered {
            background-color: #e3f2fd;
            border-color: #2196f3;
        }

        /* Panah Penghubung */
        .arrow {
            font-size: 24px;
            color: #555;
            margin: 5px 0;
            font-weight: bold;
        }

        /* Desain Item Node */
        .item {
            width: 200px;
            height: 50px;
            border-radius: 8px;
            display: flex;
            align-items: center;
            padding: 0 15px;
            font-weight: 600;
            font-size: 15px;
            cursor: grab;
            user-select: none;
            box-shadow: 0 2px 5px rgba(0,0,0,0.15);
        }

        .item:active {
            cursor: grabbing;
        }

        /* Warna berdasarkan jenis */
        .drive-c {
            background-color: #dbeafe;
            border: 2px solid #2563eb;
            color: #1e40af;
        }

        .folder {
            background-color: #fef3c7;
            border: 2px solid #d97706;
            color: #92400e;
        }

        .file {
            background-color: #ede9fe;
            border: 2px solid #7c3aed;
            color: #5b21b6;
        }

        .icon {
            margin-right: 10px;
            font-size: 18px;
        }

        /* Tombol & Pesan */
        .btn-check {
            margin-top: 20px;
            padding: 10px 25px;
            background-color: #2563eb;
            color: white;
            border: none;
            border-radius: 6px;
            font-size: 16px;
            font-weight: bold;
            cursor: pointer;
            transition: background 0.2s;
        }

        .btn-check:hover {
            background-color: #1d4ed8;
        }

        #result-message {
            margin-top: 15px;
            font-weight: bold;
            font-size: 16px;
        }
    </style>
</head>
<body>

    <h2>Latihan Hirarki Struktur Tree</h2>
    <p>Geser (drag) elemen dari kotak sebelah kiri ke hirarki sebelah kanan sesuai urutan tingkatan yang benar!</p>

    <div class="container">
        <!-- Pilihan Elemen Acak -->
        <div class="section">
            <h3>Pilihan Elemen</h3>
            <div id="pool" ondrop="drop(event)" ondragover="allowDrop(event)">
                <div class="item folder" draggable="true" ondragstart="drag(event)" id="node-tugas">
                    <span class="icon">📁</span> Tugas Sekolah
                </div>
                <div class="item drive-c" draggable="true" ondragstart="drag(event)" id="node-drive">
                    <span class="icon">💻</span> Drive C
                </div>
                <div class="item file" draggable="true" ondragstart="drag(event)" id="node-file">
                    <span class="icon">📄</span> Informatika.docx
                </div>
                <div class="item folder" draggable="true" ondragstart="drag(event)" id="node-dokumen">
                    <span class="icon">📁</span> Dokumen
                </div>
            </div>
        </div>

        <!-- Target Hirarki Tree -->
        <div class="section">
            <h3>Hirarki (Tree)</h3>
            <div class="tree-container">
                <!-- Level 1 -->
                <div class="dropzone" id="level-1" ondrop="drop(event)" ondragover="allowDrop(event)"></div>
                
                <div class="arrow">↓</div>
                
                <!-- Level 2 -->
                <div class="dropzone" id="level-2" ondrop="drop(event)" ondragover="allowDrop(event)"></div>
                
                <div class="arrow">↓</div>
                
                <!-- Level 3 -->
                <div class="dropzone" id="level-3" ondrop="drop(event)" ondragover="allowDrop(event)"></div>
                
                <div class="arrow">↓</div>
                
                <!-- Level 4 -->
                <div class="dropzone" id="level-4" ondrop="drop(event)" ondragover="allowDrop(event)"></div>
            </div>
        </div>
    </div>

    <button class="btn-check" onclick="checkAnswer()">Periksa Jawaban</button>
    <div id="result-message"></div>

    <script>
        function allowDrop(ev) {
            ev.preventDefault();
        }

        function drag(ev) {
            ev.dataTransfer.setData("text", ev.target.id);
        }

        function drop(ev) {
            ev.preventDefault();
            var data = ev.dataTransfer.getData("text");
            var draggedElement = document.getElementById(data);
            
            // Jika meletakkan di dropzone dan dropzone masih kosong (atau di area pool)
            if (ev.target.classList.contains('dropzone')) {
                if (ev.target.children.length === 0) {
                    ev.target.appendChild(draggedElement);
                }
            } else if (ev.target.id === 'pool') {
                ev.target.appendChild(draggedElement);
            }
        }

        function checkAnswer() {
            // Urutan id yang benar
            const correctOrder = [
                "node-drive",   // Level 1: Drive C
                "node-dokumen", // Level 2: Dokumen
                "node-tugas",   // Level 3: Tugas Sekolah
                "node-file"     // Level 4: Informatika.docx
            ];

            const levels = ["level-1", "level-2", "level-3", "level-4"];
            let isCorrect = true;

            for (let i = 0; i < levels.length; i++) {
                const zone = document.getElementById(levels[i]);
                const child = zone.children[0];

                if (!child || child.id !== correctOrder[i]) {
                    isCorrect = false;
                    break;
                }
            }

            const message = document.getElementById("result-message");
            if (isCorrect) {
                message.style.color = "#16a34a";
                message.innerText = "🎉 Jawaban Benar! Hirarki Tree disusun dengan sempurna.";
            } else {
                message.style.color = "#dc2626";
                message.innerText = "❌ Masih ada yang salah/belum lengkap. Silakan coba lagi!";
            }
        }
    </script>
</body>
</html>
