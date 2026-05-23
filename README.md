# Cicacc
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Teman Curhat</title>
    <style>
        /* Menggunakan font yang kasual dan nyaman dibaca */
        @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap');

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Poppins', sans-serif;
        }

        body {
            background-color: #FDFBF7; /* Warna utama Cream lembut */
            color: #1A1A1A; /* Hitam pekat untuk teks */
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            padding: 20px;
        }

        .card {
            background-color: #FFFFFF; /* Putih untuk area utama */
            border: 3px solid #1A1A1A; /* Garis pembatas hitam tegas */
            border-radius: 20px;
            padding: 40px 30px;
            max-width: 450px;
            width: 100%;
            text-align: center;
            box-shadow: 8px 8px 0px #1A1A1A; /* Efek bayangan ala pop-art/retro modern */
            transition: all 0.3s ease;
        }

        /* Styling untuk stiker imut */
        .sticker-container {
            font-size: 4rem;
            margin-bottom: 20px;
            animation: bounce 2s infinite;
        }

        @keyframes bounce {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-10px); }
        }

        h1 {
            font-size: 1.6rem;
            margin-bottom: 25px;
            line-height: 1.4;
        }

        /* Area tombol pilihan */
        .btn-group {
            display: flex;
            gap: 15px;
            justify-content: center;
            margin-top: 20px;
        }

        button {
            padding: 12px 28px;
            font-size: 1rem;
            font-weight: 600;
            border: 2px solid #1A1A1A;
            border-radius: 12px;
            cursor: pointer;
            transition: transform 0.1s, box-shadow 0.1s;
        }

        .btn-oke {
            background-color: #1A1A1A;
            color: #FDFBF7;
            box-shadow: 4px 4px 0px #C5B39A; /* Shadow cream gelap */
        }

        .btn-tidak {
            background-color: #FFFFFF;
            color: #1A1A1A;
            box-shadow: 4px 4px 0px #1A1A1A;
        }

        button:hover {
            transform: translate(-2px, -2px);
        }

        button:active {
            transform: translate(2px, 2px);
            box-shadow: none;
        }

        /* Kotak input curhat (disembunyikan di awal) */
        .input-section {
            display: none;
            margin-top: 25px;
            animation: fadeIn 0.5s ease;
        }

        textarea {
            width: 100%;
            height: 120px;
            padding: 15px;
            border: 2px solid #1A1A1A;
            border-radius: 12px;
            resize: none;
            font-size: 0.95rem;
            background-color: #FFFDF9;
            margin-bottom: 15px;
        }

        textarea:focus {
            outline: none;
            background-color: #FDFBF7;
        }

        /* Area hasil/output teks motivasi */
        .result-section {
            display: none;
            margin-top: 20px;
            animation: fadeIn 0.5s ease;
        }

        .motivation-text {
            font-size: 1.1rem;
            font-style: italic;
            font-weight: 600;
            line-height: 1.5;
            margin-top: 15px;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
    </style>
</head>
<body>

    <div class="card">
        <!-- Stiker Utama -->
        <div id="sticker" class="sticker-container">🧸</div>
        
        <!-- Teks Pertanyaan -->
        <h1 id="question">Halo! Hatimu lagi mendung ya? Mau cerita atau curhat sesuatu ke aku?</h1>

        <!-- Tombol Utama -->
        <div id="main-buttons" class="btn-group">
            <button class="btn-oke" onclick="pilihOke()">Oke</button>
            <button class="btn-tidak" onclick="pilihTidak()">Tidak</button>
        </div>

        <!-- Bagian Input Curhat (Muncul jika klik Oke) -->
        <div id="input-area" class="input-section">
            <textarea id="curhat-text" placeholder="Tumpahkan semua ceritamu di sini... Aku mendengarkan."></textarea>
            <button class="btn-oke" onclick="kirimCurhat()">Kirim Cerita</button>
        </div>

        <!-- Bagian Hasil Akhir (Kata Motivasi & Emoji) -->
        <div id="result-area" class="result-section">
            <div id="final-emoji" style="font-size: 4rem; margin-bottom: 10px;"></div>
            <p id="final-text" class="motivation-text"></p>
            <button class="btn-tidak" style="margin-top: 25px;" onclick="resetMulai()">Mulai Lagi</button>
        </div>
    </div>

    <script>
        // Kumpulan kata motivasi acak untuk yang curhat
        const motivasi = [
            "Terima kasih sudah bertahan sejauh ini ya. Kamu hebat banget, badai pasti berlalu!",
            "Nggak apa-apa kalau hari ini lelah. Istirahat sejenak, besok kita coba lagi pelan-pelan.",
            "Ingat, kamu itu berharga. Masalah ini cuma satu bab kecil dari buku kehidupanmu yang indah.",
            "Kamu sudah melakukan yang terbaik. Tarik napas dalam-dalam, segalanya akan baik-baik saja."
        ];

        const stickerElement = document.getElementById('sticker');
        const questionElement = document.getElementById('question');
        const mainButtons = document.getElementById('main-buttons');
        const inputArea = document.getElementById('input-area');
        const resultArea = document.getElementById('result-area');
        const finalEmoji = document.getElementById('final-emoji');
        const finalText = document.getElementById('final-text');

        function pilihOke() {
            stickerElement.innerHTML = "🐱"; // Ganti stiker kucing siap mendengarkan
            questionElement.innerHTML = "Aku siap dengerin. Tulis apa aja yang bikin kamu ganjel di hati.";
            mainButtons.style.display = "none";
            inputArea.style.display = "block";
        }

        function kirimCurhat() {
            const teksInput = document.getElementById('curhat-text').value;
            
            // Validasi jika kotak teks kosong
            if(teksInput.trim() === "") {
                alert("Tulis sesuatu dulu ya sebelum mengirim...");
                return;
            }

            inputArea.style.display = "none";
            questionElement.innerHTML = "Terima kasih sudah mau berbagi cerita.";
            
            // Mengambil kata motivasi secara acak
            const kataAcak = motivasi[Math.floor(Math.random() * motivasi.length)];
            
            finalEmoji.innerHTML = "💖✨🐱"; // Emoji lucu setelah curhat
            finalText.innerHTML = kataAcak;
            resultArea.style.display = "block";
        }

        function pilihTidak() {
            mainButtons.style.display = "none";
            questionElement.innerHTML = "Nggak apa-apa kalau belum siap cerita.";
            
            // Efek emoji mengangguk (disimulasikan dengan paduan emoji) dan memberi semangat
            finalEmoji.innerHTML = "👋☺️ <span style='animation: bounce 0.5s infinite; display:inline-block;'>👍</span>";
            finalText.innerHTML = "Aku selalu ada di sini kok kalau kamu butuh. Tetap semangat ya hari ini, kamu pasti bisa melalui semuanya!";
            resultArea.style.display = "block";
        }

        function resetMulai() {
            // Mengembalikan ke tampilan awal website
            document.getElementById('curhat-text').value = "";
            stickerElement.innerHTML = "🧸";
            questionElement.innerHTML = "Halo! Hatimu lagi mendung ya? Mau cerita atau curhat sesuatu ke aku?";
            mainButtons.style.display = "flex";
            inputArea.style.display = "none";
            resultArea.style.display = "none";
        }
    </script>
</body>
</html>
