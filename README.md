<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Umum Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [


  { "en": "Apa Satuan Internasional Untuk Tegangan Listrik?", "id": "Volt." },
  { "en": "Apa Simbol Besaran Untuk Arus Listrik?", "id": "I." },
  { "en": "Apa Satuan Internasional Untuk Daya Listrik?", "id": "Watt." },
  { "en": "Alat Apa Yang Digunakan Mengukur Tegangan?", "id": "Voltmeter." },
  { "en": "Apa Satuan Internasional Untuk Hambatan Listrik?", "id": "Ohm." },
  { "en": "Apa Kepanjangan DC (Direct Current)?", "id": "Arus Searah." },
  { "en": "Apa Kepanjangan AC (Alternating Current)?", "id": "Arus Bolak Balik." },
  { "en": "Rumus Hukum Ohm Untuk Mencari Tegangan?", "id": "I x R." },
  { "en": "Apa Nama Alat Pengukur Arus Listrik?", "id": "Amperemeter." },
  { "en": "Apa Satuan Energi Dalam Sistem SI?", "id": "Joule." },
  { "en": "Apa Istilah Lain Untuk Beda Potensial?", "id": "Tegangan Listrik." },
  { "en": "Apa Fungsi Utama Dari Resistor?", "id": "Menghambat Arus Listrik." },
  { "en": "Apa Satuan Dari Muatan Listrik?", "id": "Coulomb." },
  { "en": "Apa Simbol Satuan Untuk Ohm?", "id": "Omega." },
  { "en": "Berapa Frekuensi Listrik PLN Di Indonesia?", "id": "50 Hz." },
  { "en": "Apa Definisi Dari Arus Listrik?", "id": "Aliran Muatan Listrik." },
  { "en": "Apa Kepanjangan GGL (Gaya Gerak Listrik)?", "id": "Gaya Gerak Listrik." },
  { "en": "Apa Rumus Dasar Daya Listrik DC (Direct Current)?", "id": "V x I." },
  { "en": "Apa Nama Bahan Penghantar Listrik Baik?", "id": "Konduktor." },
  { "en": "Apa Nama Bahan Yang Tidak Menghantarkan Listrik?", "id": "Isolator." },
  { "en": "Apa Satuan Dari Frekuensi Listrik?", "id": "Hertz." },
  { "en": "Apa Kepanjangan V (Volt)?", "id": "Volt." },
  { "en": "Apa Kepanjangan A (Ampere)?", "id": "Ampere." },
  { "en": "Apa Itu Arus Nominal?", "id": "Arus Operasi Normal." },
  { "en": "Apa Yang Mengalir Dalam Kabel Listrik?", "id": "Elektron." },
  { "en": "Apa Fungsi Dari Sekering Atau Fuse?", "id": "Pengaman Arus Lebih." },
  { "en": "Apa Kepanjangan MCB (Miniature Circuit Breaker)?", "id": "Pemutus Sirkuit Miniatur." },
  { "en": "Alat Untuk Mengukur Hambatan Disebut Apa?", "id": "Ohmmeter." },
  { "en": "Apa Warna Kabel Netral Standar PUIL?", "id": "Biru." },
  { "en": "Apa Warna Kabel Grounding Standar PUIL?", "id": "Kuning Strip Hijau." },
  { "en": "Baterai Menghasilkan Jenis Arus Apa?", "id": "Arus Searah." },
  { "en": "Generator Menghasilkan Energi Apa?", "id": "Energi Listrik." },
  { "en": "Motor Listrik Mengubah Energi Listrik Menjadi?", "id": "Energi Mekanik." },
  { "en": "Apa Kepanjangan LED (Light Emitting Diode)?", "id": "Dioda Pemancar Cahaya." },
  { "en": "Apa Fungsi Kapasitor Dalam Rangkaian DC (Direct Current)?", "id": "Menyimpan Muatan Listrik." },
  { "en": "Apa Satuan Dari Kapasitansi Kapasitor?", "id": "Farad." },
  { "en": "Apa Satuan Dari Induktansi Induktor?", "id": "Henry." },
  { "en": "Apa Itu Rangkaian Seri?", "id": "Rangkaian Tak Bercabang." },
  { "en": "Apa Itu Rangkaian Paralel?", "id": "Rangkaian Bercabang." },
  { "en": "Pada Rangkaian Seri, Besaran Apa Yang Sama?", "id": "Arus Listrik." },
  { "en": "Pada Rangkaian Paralel, Besaran Apa Yang Sama?", "id": "Tegangan Listrik." },
  { "en": "Apa Yang Terjadi Jika Arus Berlebihan?", "id": "Panas Atau Kebakaran." },
  { "en": "Apa Itu Hubung Singkat Atau Korsleting?", "id": "Hubungan Tahanan 0." },
  { "en": "Apa Kepanjangan KWh (Kilo Watt Hour)?", "id": "Kilo Watt Jam." },
  { "en": "KWh (Kilo Watt Hour) Meter Mengukur Apa?", "id": "Energi Listrik Terpakai." },
  { "en": "Apa Kepanjangan PVC (Polyvinyl Chloride) Pada Kabel?", "id": "Polivinil Klorida." },
  { "en": "Tegangan Tinggi Diukur Dalam Satuan Apa?", "id": "kV." },
  { "en": "Daya Semu Memiliki Satuan Apa?", "id": "VA." },
  { "en": "Apa Satuan Daya Reaktif?", "id": "VAR." },
  { "en": "Apa Rumus Mencari Arus Jika Diketahui Daya?", "id": "P / V." },
  { "en": "Logam Apa Yang Paling Baik Menghantar Listrik?", "id": "Perak." },
  { "en": "Logam Apa Yang Umum Dipakai Kabel Listrik?", "id": "Tembaga." },
  { "en": "Apa Itu Konduktansi Listrik?", "id": "Kemampuan Menghantarkan Arus." },
  { "en": "Apa Satuan Dari Konduktansi?", "id": "Siemens." },
  { "en": "Kebalikan Dari Resistansi Adalah?", "id": "Konduktansi." },
  { "en": "Apa Itu Resistivitas Bahan?", "id": "Hambatan Jenis Bahan." },
  { "en": "Apa Satuan Dari Resistivitas?", "id": "Ohm Meter." },
  { "en": "Hukum Kirchhoff Satu Membahas Tentang Apa?", "id": "Arus Pada Percabangan." },
  { "en": "Hukum Kirchhoff Dua Membahas Tentang Apa?", "id": "Tegangan Pada Loop." },
  { "en": "Apa Itu Multimeter?", "id": "Alat Ukur Serbaguna." },
  { "en": "Lampu Pijar Mengubah Energi Listrik Menjadi?", "id": "Cahaya Dan Panas." },
  { "en": "Apa Yang Dimaksud Dengan Beban Resistif?", "id": "Beban Murni Hambatan." },
  { "en": "Apa Contoh Beban Resistif?", "id": "Setrika Atau Pemanas." },
  { "en": "Apa Itu Semikonduktor?", "id": "Antara Konduktor Isolator." },
  { "en": "Silikon Termasuk Jenis Bahan Apa?", "id": "Semikonduktor." },
  { "en": "Apa Fungsi Transformator Atau Trafo?", "id": "Mengubah Level Tegangan." },
  { "en": "Trafo Step Up Berfungsi Untuk Apa?", "id": "Menaikkan Tegangan." },
  { "en": "Trafo Step Down Berfungsi Untuk Apa?", "id": "Menurunkan Tegangan." },
  { "en": "Apa Itu Anoda?", "id": "Kutub Positif." },
  { "en": "Apa Itu Katoda?", "id": "Kutub Negatif." },
  { "en": "Arus Mengalir Dari Potensial Mana?", "id": "Tinggi Ke Rendah." },
  { "en": "Elektron Mengalir Dari Potensial Mana?", "id": "Rendah Ke Tinggi." },
  { "en": "Apa Kepanjangan HP (Horse Power)?", "id": "Tenaga Kuda." },
  { "en": "Satu Horse Power Setara Berapa Watt?", "id": "746 Watt." },
  { "en": "Apa Yang Dimaksud Dengan Isolasi Kabel?", "id": "Pelindung Kawat Konduktor." },
  { "en": "Apa Bahaya Sentuhan Langsung Listrik?", "id": "Sengatan Listrik." },
  { "en": "Apa Fungsi Grounding Pada Instalasi Rumah?", "id": "Membuang Arus Bocor." },
  { "en": "Berapa Tegangan Standar Rumah Tangga Indonesia?", "id": "220 V." },
  { "en": "Apa Yang Menyebabkan Kabel Menjadi Panas?", "id": "Arus Melebihi Kapasitas." },
  { "en": "Apa Itu Drop Voltage Atau Susut Tegangan?", "id": "Penurunan Nilai Tegangan." },
  { "en": "Apa Penyebab Utama Drop Voltage?", "id": "Hambatan Pada Penghantar." },
  { "en": "Apa Kepanjangan UPS (Uninterruptible Power Supply)?", "id": "Suplai Daya Bebas Gangguan." },
  { "en": "Apa Fungsi Utama UPS (Uninterruptible Power Supply)?", "id": "Cadangan Listrik Sementara." },
  { "en": "Baterai Aki Menggunakan Cairan Apa?", "id": "Asam Sulfat." },
  { "en": "Apa Kutub Yang Lebih Panjang Pada Simbol Baterai?", "id": "Kutub Positif." },
  { "en": "Apa Kutub Yang Lebih Pendek Pada Simbol Baterai?", "id": "Kutub Negatif." },
  { "en": "Apa Itu Listrik Statis?", "id": "Muatan Listrik Diam." },
  { "en": "Apa Itu Listrik Dinamis?", "id": "Muatan Listrik Bergerak." },
  { "en": "Petir Adalah Contoh Fenomena Apa?", "id": "Pelepasan Listrik Statis." },
  { "en": "Kapasitor Mylar Termasuk Jenis Komponen Apa?", "id": "Komponen Pasif." },
  { "en": "Transistor Termasuk Jenis Komponen Apa?", "id": "Komponen Aktif." },
  { "en": "IC (Integrated Circuit) Termasuk Komponen Apa?", "id": "Komponen Aktif." },
  { "en": "Apa Fungsi Dioda Penyearah?", "id": "Mengubah AC Jadi DC." },
  { "en": "Apa Itu Tegangan Puncak Ke Puncak?", "id": "Tegangan Peak To Peak." },
  { "en": "Apa Itu Periode Gelombang?", "id": "Waktu 1 Gelombang." },
  { "en": "Hubungan Frekuensi Dan Periode Adalah?", "id": "Berbanding Terbalik." },
  { "en": "Apa Bunyi Hukum Kekekalan Energi?", "id": "Energi Tidak Bisa Musnah." },
  { "en": "Energi Kinetik Adalah Energi Apa?", "id": "Energi Gerak." },
  { "en": "Energi Potensial Listrik Bergantung Pada Apa?", "id": "Posisi Muatan Listrik." },
  { "en": "Apa Satuan Daya Dalam Skala Besar?", "id": "Megawatt." },
  { "en": "Apa Fungsi Dari Sakelar Atau Switch?", "id": "Memutus Dan Menyambung Arus." },
  { "en": "Apa Itu Rangkaian Terbuka Atau Open Circuit?", "id": "Arus Tidak Dapat Mengalir." },
  { "en": "Apa Satuan Internasional Induksi Magnetik?", "id": "Tesla." },
  { "en": "Apa Inti Dari Sebuah Elektromagnet?", "id": "Kumparan Kawat Dan Besi." },
  { "en": "Apa Fungsi Dielektrik Pada Kapasitor?", "id": "Penyekat Antara 2 Pelat." },
  { "en": "Resistor Variabel Sering Disebut Juga Apa?", "id": "Potensiometer." },
  { "en": "Apa Kepanjangan LDR (Light Dependent Resistor)?", "id": "Resistor Peka Terhadap Cahaya." },
  { "en": "Apa Fungsi Relay Dalam Rangkaian Listrik?", "id": "Sakelar Yang Dioperasikan Listrik." },
  { "en": "Apa Itu Impedansi Dalam Rangkaian AC?", "id": "Total Hambatan Arus AC." },
  { "en": "Apa Simbol Matematika Untuk Impedansi?", "id": "Z." },
  { "en": "Apa Satuan Dari Fluks Magnetik?", "id": "Weber." },
  { "en": "Apa Definisi Efisiensi Daya Listrik?", "id": "Perbandingan Output Dan Input." },
  { "en": "Apa Kepanjangan ELCB (Earth Leakage Circuit Breaker)?", "id": "Pemutus Sirkuit Kebocoran Bumi." },
  { "en": "Berapa Tegangan Baterai AA (Double A) Standar?", "id": "1,5 Volt." },
  { "en": "Apa Itu Elemen Volta?", "id": "Baterai Cair Sederhana." },
  { "en": "Aki Mobil Termasuk Jenis Baterai Apa?", "id": "Baterai Sekunder." },
  { "en": "Apa Itu Reaktansi Induktif?", "id": "Hambatan Induktor Arus AC." },
  { "en": "Apa Itu Reaktansi Kapasitif?", "id": "Hambatan Kapasitor Arus AC." },
  { "en": "Pada Kapasitor Arus Mendahului Tegangan Sebesar?", "id": "90 Derajat." },
  { "en": "Pada Induktor Tegangan Mendahului Arus Sebesar?", "id": "90 Derajat." },
  { "en": "Apa Bentuk Gelombang Listrik PLN Indonesia?", "id": "Gelombang Sinusoidal." },
  { "en": "Apa Itu Amplitudo Gelombang?", "id": "Simpangan Terjauh Gelombang." },
  { "en": "Apa Nama Alat Ukur Daya Listrik?", "id": "Wattmeter." },
  { "en": "Apa Nama Alat Ukur Frekuensi Listrik?", "id": "Frekuensimeter." },
  { "en": "Alat Untuk Melihat Bentuk Gelombang Listrik?", "id": "Osiloskop." },
  { "en": "Apa Fungsi Utama Sekering Lebur?", "id": "Putus Saat Arus Berlebih." },
  { "en": "Apa Bahan Filamen Pada Lampu Pijar?", "id": "Logam Tungsten." },
  { "en": "Apa Sifat Utama Superkonduktor?", "id": "Hambatan Listrik Hampir 0." },
  { "en": "Apa Pengaruh Suhu Naik Pada Resistor Logam?", "id": "Nilai Hambatan Naik." },
  { "en": "Apa Kepanjangan NTC (Negative Temperature Coefficient)?", "id": "Koefisien Suhu Negatif." },
  { "en": "Apa Kepanjangan PTC (Positive Temperature Coefficient)?", "id": "Koefisien Suhu Positif." },
  { "en": "Komponen Termistor Digunakan Sebagai Sensor Apa?", "id": "Sensor Suhu." },
  { "en": "Apa Itu Sel Surya Atau Solar Cell?", "id": "Pengubah Cahaya Jadi Listrik." },
  { "en": "Panel Surya Menghasilkan Jenis Arus Apa?", "id": "Arus Searah." },
  { "en": "Alat Inverter Berfungsi Mengubah Apa?", "id": "DC Menjadi AC." },
  { "en": "Alat Rectifier Berfungsi Mengubah Apa?", "id": "AC Menjadi DC." },
  { "en": "Apa Fungsi Adaptor Listrik?", "id": "Penurun Dan Penyearah Tegangan." },
  { "en": "Apa Komponen Utama Dalam Chip Komputer?", "id": "Transistor." },
  { "en": "Transistor Umumnya Memiliki Berapa Kaki?", "id": "3 Kaki." },
  { "en": "Sebutkan Kaki Transistor BJT (Bipolar Junction Transistor)?", "id": "Basis Kolektor Emitor." },
  { "en": "Sebutkan Kaki Transistor FET (Field Effect Transistor)?", "id": "Gate Drain Source." },
  { "en": "Apa Kepanjangan MOSFET (Metal Oxide Semiconductor)?", "id": "Transistor Efek Medan Oksida Logam." },
  { "en": "Apa Fungsi Utama Komponen MOSFET?", "id": "Sakelar Kecepatan Tinggi." },
  { "en": "Hukum Ohm Berlaku Pada Kondisi Suhu?", "id": "Suhu Konstan." },
  { "en": "Jembatan Wheatstone Digunakan Mengukur Apa?", "id": "Hambatan Yang Tidak Diketahui." },
  { "en": "Apa Itu Node Dalam Rangkaian Listrik?", "id": "Titik Pertemuan Komponen." },
  { "en": "Apa Itu Loop Dalam Rangkaian Listrik?", "id": "Lintasan Tertutup Arus." },
  { "en": "Teorema Thevenin Menyederhanakan Rangkaian Menjadi?", "id": "1 Tegangan 1 Hambatan." },
  { "en": "Teorema Norton Menyederhanakan Rangkaian Menjadi?", "id": "1 Arus 1 Hambatan." },
  { "en": "Gaya Tarik Magnet Terjadi Antara?", "id": "Kutub Berbeda." },
  { "en": "Gaya Tolak Magnet Terjadi Antara?", "id": "Kutub Sejenis." },
  { "en": "Arah Garis Gaya Magnet Keluar Dari?", "id": "Kutub Utara." },
  { "en": "Arah Garis Gaya Magnet Masuk Ke?", "id": "Kutub Selatan." },
  { "en": "Apa Kepanjangan EMF (Electromotive Force)?", "id": "Gaya Gerak Listrik." },
  { "en": "Hukum Faraday Membahas Tentang Fenomena Apa?", "id": "Induksi Elektromagnetik." },
  { "en": "Dinamo Sepeda Menggunakan Prinsip Apa?", "id": "Induksi Elektromagnetik." },
  { "en": "Apa Itu Eddy Current Atau Arus Pusar?", "id": "Arus Liar Pada Inti." },
  { "en": "Mengapa Inti Trafo Dibuat Berlapis Lapis?", "id": "Mengurangi Arus Pusar." },
  { "en": "Apa Satuan Fluks Cahaya Lampu?", "id": "Lumen." },
  { "en": "Apa Satuan Intensitas Cahaya?", "id": "Candela." },
  { "en": "Efisiensi Lampu LED (Light Emitting Diode) Adalah?", "id": "Sangat Tinggi." },
  { "en": "Apa Itu Beban Induktif?", "id": "Beban Mengandung Kumparan." },
  { "en": "Apa Contoh Peralatan Beban Induktif?", "id": "Motor Listrik Dan Trafo." },
  { "en": "Apa Itu Beban Kapasitif?", "id": "Beban Mengandung Kapasitor." },
  { "en": "Apa Fungsi Kapasitor Bank Pada Industri?", "id": "Memperbaiki Faktor Daya." },
  { "en": "Nilai Faktor Daya Ideal Adalah?", "id": "1." },
  { "en": "Istilah Lain Untuk Faktor Daya Adalah?", "id": "Cosinus Phi." },
  { "en": "Berapa Nilai Muatan Satu Elektron?", "id": "1,6 Coulomb." },
  { "en": "Arus Listrik Mengalir Karena Adanya?", "id": "Beda Potensial." },
  { "en": "Apa Itu Bahan Feromagnetik?", "id": "Kuat Ditarik Magnet." },
  { "en": "Apa Contoh Bahan Feromagnetik?", "id": "Besi Dan Nikel." },
  { "en": "Apa Itu Bahan Diamagnetik?", "id": "Ditolak Lemah Oleh Magnet." },
  { "en": "Apa Contoh Bahan Isolator Cair?", "id": "Minyak Trafo." },
  { "en": "Udara Kering Termasuk Konduktor Atau Isolator?", "id": "Isolator." },
  { "en": "Kapan Udara Menjadi Konduktor Listrik?", "id": "Saat Tegangan Tembus." },
  { "en": "Apa Itu Tegangan Breakdown Atau Tembus?", "id": "Batas Isolasi Rusak." },
  { "en": "Apa Kepanjangan SOP (Standard Operating Procedure)?", "id": "Prosedur Operasi Standar." },
  { "en": "Apa Warna Kabel Fasa R Standar Lama?", "id": "Merah." },
  { "en": "Apa Warna Kabel Fasa S Standar Lama?", "id": "Kuning." },
  { "en": "Apa Warna Kabel Fasa T Standar Lama?", "id": "Hitam." },
  { "en": "Apa Warna Kabel Fasa Standar Baru 2011?", "id": "Hitam Coklat Abu." },
  { "en": "Apa Ciri Fisik Multimeter Analog?", "id": "Menggunakan Jarum Penunjuk." },
  { "en": "Apa Ciri Fisik Multimeter Digital?", "id": "Menggunakan Layar Angka." },
  { "en": "Mengapa Mengukur Arus Harus Seri?", "id": "Hambatan Amperemeter Sangat Kecil." },
  { "en": "Mengapa Mengukur Tegangan Harus Paralel?", "id": "Hambatan Voltmeter Sangat Besar." },
  { "en": "Apa Akibat Jika Voltmeter Dipasang Seri?", "id": "Rangkaian Terputus." },
  { "en": "Apa Fungsi Alat Solder?", "id": "Menyambung Komponen Logam." },
  { "en": "Kawat Timah Solder Disebut Juga Apa?", "id": "Tenol." },
  { "en": "Apa Fungsi Pasta Solder Atau Flux?", "id": "Memudahkan Timah Menempel." },
  { "en": "Apa Kepanjangan PCB (Printed Circuit Board)?", "id": "Papan Rangkaian Cetak." },
  { "en": "Apa Fungsi Breadboard Atau Project Board?", "id": "Merakit Tanpa Solder." },
  { "en": "Kabel Jumper Digunakan Untuk Apa?", "id": "Menghubungkan Titik Rangkaian." },
  { "en": "Apa Itu Skematik Diagram?", "id": "Gambar Simbol Rangkaian." },
  { "en": "Apa Itu Layout PCB (Printed Circuit Board)?", "id": "Tata Letak Jalur Tembaga." },
  { "en": "Satuan KVA (Kilo Volt Ampere) Untuk Daya?", "id": "Daya Semu." },
  { "en": "Satuan KW (Kilo Watt) Untuk Daya Apa?", "id": "Daya Nyata." },
  { "en": "Satuan KVAR (Kilo Volt Ampere Reactive) Untuk?", "id": "Daya Reaktif." },
  { "en": "Segitiga Daya Menggambarkan Hubungan Apa?", "id": "Nyata Semu Dan Reaktif." },
  { "en": "Apa Komponen Rumus Daya Tiga Fasa?", "id": "Akar 3 VI Cosphi." },
  { "en": "Apa Basis Bilangan Biner?", "id": "2." },
  { "en": "Apa Basis Bilangan Desimal?", "id": "10." },
  { "en": "Apa Basis Bilangan Heksadesimal?", "id": "16." },
  { "en": "Gerbang Logika Inverter Disebut Juga?", "id": "Gerbang Not." },
  { "en": "Output Gerbang And Bernilai Satu Jika?", "id": "Semua Input 1." },
  { "en": "Output Gerbang Or Bernilai Satu Jika?", "id": "Salah Satu Input 1." },
  { "en": "Apa Kepanjangan IC (Integrated Circuit)?", "id": "Sirkuit Terpadu." },
  { "en": "Apa Itu Bit Dalam Sistem Digital?", "id": "Binary Digit." },
  { "en": "Satu Byte Terdiri Dari Berapa Bit?", "id": "8 Bit." },
  { "en": "Apa Kepanjangan CPU (Central Processing Unit)?", "id": "Unit Pemrosesan Pusat." },
  { "en": "Bagian Motor Listrik Yang Diam Disebut?", "id": "Stator." },
  { "en": "Bagian Motor Listrik Yang Berputar Disebut?", "id": "Rotor." },
  { "en": "Apa Fungsi Komutator Pada Motor DC?", "id": "Pembalik Arah Arus." },
  { "en": "Sikat Arang Pada Motor Disebut Apa?", "id": "Carbon Brush." },
  { "en": "Motor Induksi Bekerja Berdasarkan Prinsip?", "id": "Induksi Elektromagnetik." },
  { "en": "Apa Satuan Kecepatan Putar Motor?", "id": "RPM." },
  { "en": "Apa Itu Torsi Pada Motor Listrik?", "id": "Gaya Putar." },
  { "en": "Alternator Adalah Nama Lain Dari?", "id": "Generator AC." },
  { "en": "Dioda Zener Berfungsi Sebagai Apa?", "id": "Penstabil Tegangan." },
  { "en": "Apa Fungsi Sekering Jenis Thermal?", "id": "Putus Saat Panas Berlebih." },
  { "en": "Apa Fungsi Varistor Dalam Rangkaian?", "id": "Pelindung Lonjakan Tegangan." },
  { "en": "Apa Itu Komponen Optocoupler?", "id": "Pengisolasi Sinyal Cahaya." },
  { "en": "Relay Solid State Tidak Memiliki Apa?", "id": "Kontak Gerak Mekanik." },
  { "en": "Apa Fungsi Heatsink Pada Transistor?", "id": "Membuang Panas." },
  { "en": "Bahan Heatsink Biasanya Terbuat Dari Apa?", "id": "Aluminium." },
  { "en": "Apa Kepanjangan SUTET Di Indonesia?", "id": "Saluran Udara Tegangan Ekstra Tinggi." },
  { "en": "Berapa Tegangan Transmisi SUTET?", "id": "500 kV." },
  { "en": "Apa Fungsi Gardu Induk?", "id": "Mengatur Distribusi Daya." },
  { "en": "Apa Itu Trafo Distribusi?", "id": "Trafo Tiang Listrik." },
  { "en": "Tegangan Menengah PLN Biasanya Berapa?", "id": "20 kV." },
  { "en": "Apa Fungsi Isolator Keramik Di Tiang?", "id": "Menahan Kabel Bertegangan." },
  { "en": "Tang Ampere Digunakan Untuk Mengukur Apa?", "id": "Arus Tanpa Memutus Kabel." },
  { "en": "Megger Test Digunakan Untuk Mengukur Apa?", "id": "Tahanan Isolasi." },
  { "en": "Apa Itu Kalibrasi Alat Ukur?", "id": "Penyesuaian Akurasi Alat." },
  { "en": "Apa Fungsi Test Pen?", "id": "Mendeteksi Fasa Listrik." },
  { "en": "Test Pen Menyala Pada Kabel Apa?", "id": "Kabel Fasa." },
  { "en": "Apa Kepanjangan AM (Amplitude Modulation)?", "id": "Modulasi Amplitudo." },
  { "en": "Apa Kepanjangan FM (Frequency Modulation)?", "id": "Modulasi Frekuensi." },
  { "en": "Apa Itu Sinyal Analog?", "id": "Sinyal Kontinyu." },
  { "en": "Apa Itu Sinyal Digital?", "id": "Sinyal Diskrit." },
  { "en": "Apa Kepanjangan PWM (Pulse Width Modulation)?", "id": "Modulasi Lebar Pulsa." },
  { "en": "PWM (Pulse Width Modulation) Sering Digunakan Untuk?", "id": "Mengatur Kecepatan Motor." },
  { "en": "Apa Kepanjangan ADC (Analog To Digital)?", "id": "Pengubah Analog Ke Digital." },
  { "en": "Apa Kepanjangan DAC (Digital To Analog)?", "id": "Pengubah Digital Ke Analog." },
  { "en": "Rangkaian Jembatan Wheatstone Seimbang Jika?", "id": "Perkalian Silang Sama." },
  { "en": "Apa Itu Teorema Superposisi?", "id": "Analisis Banyak Sumber." },
  { "en": "Sumber Tegangan Ideal Memiliki Hambatan Dalam?", "id": "0 Ohm." },
  { "en": "Sumber Arus Ideal Memiliki Hambatan Dalam?", "id": "Tak Hingga." },
  { "en": "Apa Itu Elektrostatika?", "id": "Ilmu Muatan Diam." },
  { "en": "Apa Itu Elektrodinamika?", "id": "Ilmu Muatan Bergerak." },
  { "en": "Satuan Intensitas Medan Listrik Adalah?", "id": "Volt Per Meter." },
  { "en": "Satuan Kerapatan Fluks Magnet Adalah?", "id": "Tesla." },
  { "en": "Apa Itu Permitivitas Ruang Hampa?", "id": "Konstanta Dielektrik Vakum." },
  { "en": "Apa Itu Permeabilitas Ruang Hampa?", "id": "Konstanta Magnetik Vakum." },
  { "en": "Apa Warna Gelang Resistor Satu Kiloohm?", "id": "Coklat Hitam Merah Emas." },
  { "en": "Gelang Emas Pada Resistor Artinya Apa?", "id": "Toleransi 5 Persen." },
  { "en": "Gelang Perak Pada Resistor Artinya Apa?", "id": "Toleransi 10 Persen." },
  { "en": "Resistor Seratus Ohm Warnanya Apa?", "id": "Coklat Hitam Coklat." },
  { "en": "Apa Itu Potensiometer Logaritmik?", "id": "Perubahan Hambatan Melengkung." },
  { "en": "Apa Itu Potensiometer Linear?", "id": "Perubahan Hambatan Lurus." },
  { "en": "Apa Fungsi Trimpot (Trimmer Potentiometer)?", "id": "Disetel Dengan Obeng." },
  { "en": "Kapasitor Elektrolit Memiliki Kutub Apa?", "id": "Positif Dan Negatif." },
  { "en": "Kapasitor Keramik Memiliki Kutub Apa?", "id": "Non Polar." },
  { "en": "Satuan Pikofarad Setara Berapa Farad?", "id": "10 Pangkat Minus 12." },
  { "en": "Satuan Mikrofarad Setara Berapa Farad?", "id": "10 Pangkat Minus 6." },
  { "en": "Satuan Nanofarad Setara Berapa Farad?", "id": "10 Pangkat Minus 9." },
  { "en": "Apa Kepanjangan Vpp (Voltage Peak To Peak)?", "id": "Tegangan Puncak Ke Puncak." },
  { "en": "Apa Kepanjangan Vrms (Root Mean Square)?", "id": "Tegangan Efektif." },
  { "en": "Hubungan Vmax Dan Vrms Adalah?", "id": "Vmax Adalah Vrms Akar 2." },
  { "en": "Tegangan Dua Ratus Dua Puluh Volt PLN Adalah?", "id": "Tegangan Efektif Vrms." },
  { "en": "Berapa Tegangan Puncak Listrik Rumah?", "id": "Sekitar 311 Volt." },
  { "en": "Apa Itu Harmonisa Pada Listrik?", "id": "Gangguan Gelombang Frekuensi." },
  { "en": "Apa Dampak Buruk Harmonisa?", "id": "Peralatan Cepat Panas." },
  { "en": "Apa Itu Transien Listrik?", "id": "Lonjakan Tegangan Sesaat." },
  { "en": "Apa Itu Sag Voltage?", "id": "Tegangan Turun Sesaat." },
  { "en": "Apa Itu Swell Voltage?", "id": "Tegangan Naik Sesaat." },
  { "en": "Apa Itu Blackout Listrik?", "id": "Padam Total." },
  { "en": "Alat Penangkal Petir Disebut Juga?", "id": "Lightning Arrester." },
  { "en": "Di Mana Arrester Biasanya Dipasang?", "id": "Dekat Gardu Atau Trafo." },
  { "en": "Apa Fungsi Kabel Coaxial?", "id": "Transmisi Frekuensi Tinggi." },
  { "en": "Apa Impedansi Kabel Antena TV Standar?", "id": "75 Ohm." },
  { "en": "Apa Impedansi Kabel Audio Profesional?", "id": "600 Ohm." },
  { "en": "Apa Itu Serat Optik Atau Fiber Optic?", "id": "Transmisi Cahaya Kaca." },
  { "en": "Kecepatan Cahaya Adalah Berapa?", "id": "300 Juta Meter Detik." },
  { "en": "Sinyal Listrik Merambat Secepat Apa?", "id": "Mendekati Kecepatan Cahaya." },
  { "en": "Apa Itu Efek Kulit Atau Skin Effect?", "id": "Arus Di Permukaan Kabel." },
  { "en": "Skin Effect Terjadi Pada Frekuensi?", "id": "Frekuensi Tinggi." },
  { "en": "Apa Itu Superkonduktivitas?", "id": "Hambatan Nol Suhu Rendah." },
  { "en": "Apa Itu Kriogenik Dalam Elektro?", "id": "Pendinginan Suhu Ekstrem." },
  { "en": "Apa Fungsi Termokopel?", "id": "Sensor Suhu Tegangan." },
  { "en": "Prinsip Kerja Termokopel Adalah?", "id": "Efek Seebeck." },
  { "en": "Apa Itu Efek Piezoelektrik?", "id": "Tekanan Menjadi Listrik." },
  { "en": "Korek Api Gas Menggunakan Prinsip?", "id": "Piezoelektrik." },
  { "en": "Apa Itu Motor Stepper?", "id": "Motor Gerak Per Langkah." },
  { "en": "Motor Servo Dikendalikan Oleh Apa?", "id": "Lebar Pulsa PWM." },
  { "en": "Apa Fungsi Mikrokontroler?", "id": "Pengendali Sistem Digital." },
  { "en": "Arduino Menggunakan Mikrokontroler Merk Apa?", "id": "Atmel AVR." },
  { "en": "Apa Bahasa Pemrograman Dasar Arduino?", "id": "C Plus Plus." },
  { "en": "Apa Itu IoT (Internet Of Things)?", "id": "Benda Terhubung Internet." },
  { "en": "Apa Kepanjangan SCR (Silicon Controlled Rectifier)?", "id": "Penyearah Terkendali Silikon." },
  { "en": "SCR (Silicon Controlled Rectifier) Memiliki Berapa Kaki Terminal?", "id": "3 Kaki." },
  { "en": "Sebutkan Nama Kaki Pada SCR (Silicon Controlled Rectifier)?", "id": "Anoda Katoda Gate." },
  { "en": "Apa Fungsi Utama Komponen TRIAC?", "id": "Sakelar Arus Bolak Balik." },
  { "en": "Apa Fungsi Utama Komponen DIAC?", "id": "Pemicu Gate Pada TRIAC." },
  { "en": "Apa Kepanjangan VSD (Variable Speed Drive)?", "id": "Penggerak Kecepatan Variabel." },
  { "en": "VSD (Variable Speed Drive) Digunakan Untuk Mengatur Apa?", "id": "Kecepatan Motor Induksi." },
  { "en": "Apa Kepanjangan PLC (Programmable Logic Controller)?", "id": "Pengendali Logika Terprogram." },
  { "en": "Bahasa Pemrograman PLC Paling Umum Adalah?", "id": "Ladder Diagram." },
  { "en": "Apa Kepanjangan HMI (Human Machine Interface)?", "id": "Antarmuka Manusia Dan Mesin." },
  { "en": "Apa Kepanjangan SCADA (Supervisory Control And Data Acquisition)?", "id": "Pengawasan Dan Akuisisi Data." },
  { "en": "Apa Itu Sistem Kendali Loop Terbuka?", "id": "Tanpa Umpan Balik." },
  { "en": "Apa Itu Sistem Kendali Loop Tertutup?", "id": "Menggunakan Umpan Balik." },
  { "en": "Sensor Apa Yang Mengukur Jarak Dengan Suara?", "id": "Sensor Ultrasonik." },
  { "en": "Sensor Apa Yang Mendeteksi Gerakan Manusia?", "id": "Sensor PIR." },
  { "en": "Apa Kepanjangan PIR (Passive Infrared Receiver)?", "id": "Penerima Inframerah Pasif." },
  { "en": "Apa Fungsi Limit Switch Pada Mesin?", "id": "Pembatas Gerakan Mekanik." },
  { "en": "Apa Itu Solenoid Valve?", "id": "Katup Kendali Listrik." },
  { "en": "Apa Fungsi Proximity Sensor?", "id": "Deteksi Objek Tanpa Sentuh." },
  { "en": "Proximity Induktif Mendeteksi Bahan Apa?", "id": "Bahan Logam." },
  { "en": "Proximity Kapasitif Mendeteksi Bahan Apa?", "id": "Logam Dan Non Logam." },
  { "en": "Apa Satuan Kapasitas Baterai Kecil?", "id": "mAh." },
  { "en": "Apa Satuan Kapasitas Baterai Besar Atau Aki?", "id": "Ah." },
  { "en": "Apa Kepanjangan BMS (Battery Management System)?", "id": "Sistem Manajemen Baterai." },
  { "en": "Apa Fungsi BMS (Battery Management System)?", "id": "Melindungi Dan Menyeimbangkan Baterai." },
  { "en": "Baterai LiPo (Lithium Polymer) Berbentuk Apa?", "id": "Kantung Fleksibel." },
  { "en": "Berapa Tegangan Nominal Sel Lithium Ion?", "id": "3,7 Volt." },
  { "en": "Apa Itu Efek Memori Pada Baterai?", "id": "Kapasitas Berkurang Jika Diisi." },
  { "en": "Jenis Baterai Apa Yang Memiliki Efek Memori?", "id": "Nikel Kadmium." },
  { "en": "Apa Bahaya Baterai Lithium Jika Bocor?", "id": "Api Dan Ledakan." },
  { "en": "Apa Kepanjangan UPS (Uninterruptible Power Supply) Online?", "id": "Tanpa Jeda Waktu Pindah." },
  { "en": "Apa Itu Genset (Generator Set)?", "id": "Mesin Pembangkit Listrik." },
  { "en": "Apa Fungsi AVR (Automatic Voltage Regulator) Pada Genset?", "id": "Menstabilkan Tegangan Output." },
  { "en": "Apa Itu Exciter Pada Generator?", "id": "Pembangkit Medan Magnet." },
  { "en": "Apa Fungsi Governor Pada Mesin Genset?", "id": "Mengatur Kecepatan Putaran." },
  { "en": "Apa Itu Sinkronisasi Generator?", "id": "Paralel 2 Generator." },
  { "en": "Syarat Sinkronisasi Adalah Tegangan Dan?", "id": "Frekuensi Dan Fasa Sama." },
  { "en": "Apa Itu Busbar Dalam Panel Listrik?", "id": "Batang Tembaga Penghantar." },
  { "en": "Apa Warna Busbar Fasa R Standar Baru?", "id": "Merah." },
  { "en": "Apa Warna Busbar Fasa S Standar Baru?", "id": "Kuning." },
  { "en": "Apa Warna Busbar Fasa T Standar Baru?", "id": "Hitam." },
  { "en": "Catatan: Standar Warna Busbar Berbeda Dengan Kabel.", "id": "Pahami Standar Lokal." },
  { "en": "Apa Fungsi CT (Current Transformer)?", "id": "Menurunkan Arus Pengukuran." },
  { "en": "Apa Fungsi PT (Potential Transformer)?", "id": "Menurunkan Tegangan Pengukuran." },
  { "en": "Rasio CT 100/5 Artinya Apa?", "id": "100 Ampere Jadi 5." },
  { "en": "Apa Itu Panel LVMDP (Low Voltage Main Distribution Panel)?", "id": "Panel Distribusi Utama Tegangan Rendah." },
  { "en": "Apa Itu Panel SDP (Sub Distribution Panel)?", "id": "Panel Distribusi Cabang." },
  { "en": "Apa Fungsi Kapasitor Bank Di Panel?", "id": "Perbaikan Faktor Daya." },
  { "en": "Apa Itu Harmonisa Ke Tiga?", "id": "Frekuensi 150." },
  { "en": "Apa Bahaya Arus Netral Tinggi?", "id": "Kabel Netral Panas." },
  { "en": "Apa Itu Grounding Rod?", "id": "Batang Tembaga Tanam." },
  { "en": "Berapa Nilai Tahanan Grounding Yang Baik?", "id": "Di Bawah 5 Ohm." },
  { "en": "Alat Apa Untuk Mengukur Tahanan Grounding?", "id": "Earth Tester." },
  { "en": "Apa Itu Tegangan Langkah?", "id": "Beda Potensial Antara Kaki." },
  { "en": "Apa Itu Tegangan Sentuh?", "id": "Beda Potensial Saat Menyentuh." },
  { "en": "Apa Fungsi Perisai Atau Shielding Kabel?", "id": "Mencegah Gangguan Elektromagnetik." },
  { "en": "Apa Itu Kabel UTP (Unshielded Twisted Pair)?", "id": "Kabel Jaringan Tanpa Pelindung." },
  { "en": "Apa Itu Kabel STP (Shielded Twisted Pair)?", "id": "Kabel Jaringan Dengan Pelindung." },
  { "en": "Konektor RJ45 Digunakan Untuk Kabel Apa?", "id": "Kabel Ethernet LAN." },
  { "en": "Apa Fungsi Fiber Optic Splicer?", "id": "Menyambung Kabel Optik." },
  { "en": "Apa Itu OTDR (Optical Time Domain Reflectometer)?", "id": "Alat Ukur Fiber Optik." },
  { "en": "Apa Itu Redaman Pada Serat Optik?", "id": "Penurunan Kekuatan Sinyal." },
  { "en": "Apa Itu Hukum Coulomb?", "id": "Gaya Antar Muatan Listrik." },
  { "en": "Gaya Coulomb Berbanding Terbalik Dengan?", "id": "Kuadrat Jarak." },
  { "en": "Apa Itu Medan Listrik?", "id": "Daerah Pengaruh Muatan." },
  { "en": "Arah Medan Listrik Positif Adalah?", "id": "Keluar Menjauhi Muatan." },
  { "en": "Arah Medan Listrik Negatif Adalah?", "id": "Masuk Menuju Muatan." },
  { "en": "Apa Itu Kapasitor Tantalum?", "id": "Kapasitor Stabil Ukuran Kecil." },
  { "en": "Apa Kelemahan Kapasitor Elektrolit?", "id": "Tidak Tahan Suhu Tinggi." },
  { "en": "Apa Itu Supercapacitor?", "id": "Kapasitas Penyimpanan Sangat Besar." },
  { "en": "Supercapacitor Sering Digunakan Sebagai Pengganti?", "id": "Baterai Jangka Pendek." },
  { "en": "Apa Fungsi Induktor Toroid?", "id": "Efisiensi Medan Magnet Tinggi." },
  { "en": "Apa Itu Inti Ferit?", "id": "Inti Magnetik Serbuk Besi." },
  { "en": "Ferit Bead Digunakan Untuk Apa?", "id": "Meredam Noise Frekuensi Tinggi." },
  { "en": "Apa Itu EMI (Electromagnetic Interference)?", "id": "Gangguan Elektromagnetik." },
  { "en": "Apa Itu EMC (Electromagnetic Compatibility)?", "id": "Keserasian Elektromagnetik." },
  { "en": "Apa Itu Sangkar Faraday?", "id": "Ruang Kedap Medan Listrik." },
  { "en": "Sinyal HP Hilang Di Lift Karena?", "id": "Efek Sangkar Faraday." },
  { "en": "Apa Itu Osilator?", "id": "Pembangkit Sinyal Gelombang." },
  { "en": "Osilator Kristal Menggunakan Bahan Apa?", "id": "Batu Kuarsa." },
  { "en": "Apa Keunggulan Osilator Kristal?", "id": "Frekuensi Sangat Stabil." },
  { "en": "Apa Fungsi RTC (Real Time Clock)?", "id": "Penghitung Waktu Nyata." },
  { "en": "Apa Itu Seven Segment Display?", "id": "Layar Angka 7 Bagian." },
  { "en": "Jenis Seven Segment Ada Apa Saja?", "id": "Common Anode Dan Cathode." },
  { "en": "Apa Itu LCD (Liquid Crystal Display)?", "id": "Layar Kristal Cair." },
  { "en": "Apa Itu OLED (Organic Light Emitting Diode)?", "id": "Layar Organik Cahaya Sendiri." },
  { "en": "Apa Kelebihan Layar OLED?", "id": "Hitam Lebih Pekat." },
  { "en": "Apa Itu PCB (Printed Circuit Board) Single Layer?", "id": "1 Lapis Tembaga." },
  { "en": "Apa Itu PCB (Printed Circuit Board) Double Layer?", "id": "2 Lapis Atas Bawah." },
  { "en": "Apa Fungsi Via Pada PCB?", "id": "Menghubungkan Lapisan Atas Bawah." },
  { "en": "Apa Itu Solder Mask?", "id": "Pelapis Anti Solder Hijau." },
  { "en": "Apa Itu Silkscreen Pada PCB?", "id": "Tulisan Atau Gambar Putih." },
  { "en": "Apa Proses Etching PCB?", "id": "Melarutkan Tembaga Tidak Terpakai." },
  { "en": "Bahan Kimia Untuk Etching PCB Adalah?", "id": "Ferri Klorida." },
  { "en": "Apa Itu Multimeter True RMS?", "id": "Akurasi Tinggi Gelombang Cacat." },
  { "en": "Kapan Wajib Pakai True RMS?", "id": "Mengukur Beban Non Linear." },
  { "en": "Apa Itu Clamp Meter?", "id": "Tang Ampere." },
  { "en": "Mengapa Tang Ampere DC Lebih Mahal?", "id": "Menggunakan Sensor Hall Effect." },
  { "en": "Sensor Hall Effect Mendeteksi Apa?", "id": "Medan Magnet." },
  { "en": "Apa Arti Angka Pertama Pada Kode IP Rating?", "id": "Perlindungan Terhadap Benda Padat." },
  { "en": "Apa Arti Angka Kedua Pada Kode IP Rating?", "id": "Perlindungan Terhadap Cairan." },
  { "en": "IP68 (Ingress Protection) Menandakan Alat Tahan Apa?", "id": "Debu Dan Rendaman Air." },
  { "en": "IP20 (Ingress Protection) Biasanya Digunakan Di Mana?", "id": "Dalam Ruangan Kering." },
  { "en": "Apa Warna Isolasi Kabel NYA Sesuai Standar?", "id": "Merah Kuning Hitam Biru." },
  { "en": "Kabel NYA Memiliki Berapa Lapis Isolasi?", "id": "1 Lapis PVC." },
  { "en": "Apa Warna Selubung Luar Kabel NYM?", "id": "Putih." },
  { "en": "Kabel NYM Cocok Digunakan Untuk Instalasi Apa?", "id": "Instalasi Tetap Dalam Ruangan." },
  { "en": "Apa Warna Selubung Luar Kabel NYY?", "id": "Hitam." },
  { "en": "Keunggulan Utama Kabel NYY Adalah?", "id": "Tahan Ditanam Dalam Tanah." },
  { "en": "Apa Kepanjangan USB (Universal Serial Bus)?", "id": "Bus Serial Universal." },
  { "en": "Berapa Tegangan Standar Port USB Komputer?", "id": "5 Volt." },
  { "en": "Apa Kepanjangan MCCB (Molded Case Circuit Breaker)?", "id": "Pemutus Sirkuit Kotak Cetakan." },
  { "en": "Apa Perbedaan Utama MCB Dan MCCB?", "id": "Kapasitas Arus Lebih Besar." },
  { "en": "Apa Kepanjangan ACB (Air Circuit Breaker)?", "id": "Pemutus Sirkuit Udara." },
  { "en": "ACB (Air Circuit Breaker) Digunakan Pada Arus Berapa?", "id": "Di Atas 1000 Ampere." },
  { "en": "Apa Itu Kontak NO (Normally Open)?", "id": "Terbuka Saat Normal." },
  { "en": "Apa Itu Kontak NC (Normally Closed)?", "id": "Tertutup Saat Normal." },
  { "en": "Tombol Emergency Stop Menggunakan Kontak Jenis Apa?", "id": "Normally Closed." },
  { "en": "Apa Fungsi Thermal Overload Relay?", "id": "Proteksi Motor Beban Lebih." },
  { "en": "Apa Kepanjangan DOL (Direct On Line) Starter?", "id": "Pengasutan Langsung." },
  { "en": "Apa Kelemahan Starters Jenis DOL (Direct On Line)?", "id": "Arus Start Sangat Tinggi." },
  { "en": "Rangkaian Star Delta Berfungsi Untuk Apa?", "id": "Mengurangi Arus Start Motor." },
  { "en": "Pada Hubungan Star Tegangan Tiap Fasa Adalah?", "id": "220 Volt." },
  { "en": "Pada Hubungan Delta Tegangan Tiap Fasa Adalah?", "id": "380 Volt." },
  { "en": "Apa Kepanjangan I2C (Inter Integrated Circuit)?", "id": "Sirkuit Antar Terpadu." },
  { "en": "Komunikasi I2C Menggunakan Berapa Kabel Data?", "id": "2 Kabel." },
  { "en": "Apa Nama Pin Data Pada Protokol I2C?", "id": "SDA Dan SCL." },
  { "en": "Apa Kepanjangan SPI (Serial Peripheral Interface)?", "id": "Antarmuka Periferal Serial." },
  { "en": "Komunikasi Serial UART Membutuhkan Pin Apa?", "id": "TX Dan RX." },
  { "en": "Apa Kepanjangan TX Pada Komunikasi Serial?", "id": "Transmit." },
  { "en": "Apa Kepanjangan RX Pada Komunikasi Serial?", "id": "Receive." },
  { "en": "Standar Komunikasi RS232 Biasa Digunakan Pada?", "id": "Port Serial Komputer Lama." },
  { "en": "Kelebihan RS485 Dibanding RS232 Adalah?", "id": "Jarak Jauh Dan Multi Titik." },
  { "en": "Apa Kepanjangan MPPT (Maximum Power Point Tracking)?", "id": "Pelacakan Titik Daya Maksimum." },
  { "en": "MPPT Digunakan Pada Sistem Apa?", "id": "Pembangkit Listrik Tenaga Surya." },
  { "en": "Apa Fungsi Inverter Grid Tie?", "id": "Sinkronisasi Dengan Jaringan PLN." },
  { "en": "Apa Itu Sistem PLTS Off Grid?", "id": "Tidak Terhubung PLN." },
  { "en": "Apa Itu Sistem PLTS On Grid?", "id": "Terhubung Jaringan PLN." },
  { "en": "Apa Satuan Intensitas Cahaya Matahari?", "id": "Watt Per Meter Persegi." },
  { "en": "Apa Itu Efek Photovoltaic?", "id": "Cahaya Menjadi Tegangan Listrik." },
  { "en": "Multimeter Kategori CAT III Aman Untuk?", "id": "Instalasi Gedung Dan Panel." },
  { "en": "Multimeter Kategori CAT IV Aman Untuk?", "id": "Gardu Dan Kabel Luar." },
  { "en": "Apa Bahaya Mengukur Tegangan Dengan Mode Ampere?", "id": "Alat Meledak Dan Rusak." },
  { "en": "Probe Osiloskop x10 Berfungsi Untuk Apa?", "id": "Meningkatkan Impedansi Input." },
  { "en": "Apa Itu Cold Solder Joint?", "id": "Solderan Matang Tidak Sempurna." },
  { "en": "Apa Ciri Fisik Solderan Yang Buruk?", "id": "Kusam Dan Tidak Mengkilap." },
  { "en": "Suhu Solder Untuk Timah Biasa Adalah?", "id": "Sekitar 350 Derajat Celcius." },
  { "en": "Apa Fungsi Flux Rosin Pada Timah?", "id": "Membersihkan Oksidasi Logam." },
  { "en": "Apa Itu Desoldering Pump Atau Atraktor?", "id": "Penyedot Timah Cair." },
  { "en": "Apa Itu Solder Wick Atau Pita Solder?", "id": "Pita Tembaga Penyerap Timah." },
  { "en": "Resistor SMD (Surface Mount Device) Adalah?", "id": "Resistor Tempel Permukaan." },
  { "en": "Kode Resistor SMD 103 Artinya Berapa?", "id": "10000 Ohm Atau 10k." },
  { "en": "Kode Resistor SMD 472 Artinya Berapa?", "id": "4700 Ohm Atau 4k7." },
  { "en": "Apa Ukuran SMD 0805 Dalam Inci?", "id": "0,08 Kali 0,05 Inci." },
  { "en": "Apa Itu Reflow Soldering?", "id": "Menyolder Dengan Oven Pemanas." },
  { "en": "Apa Itu BGA (Ball Grid Array)?", "id": "Kaki Chip Berbentuk Bola." },
  { "en": "Gelombang Radio Adalah Gelombang Apa?", "id": "Gelombang Elektromagnetik." },
  { "en": "Frekuensi WiFi Standar Adalah?", "id": "2,4 GHz Dan 5 GHz." },
  { "en": "Apa Kepanjangan RFID (Radio Frequency Identification)?", "id": "Identifikasi Frekuensi Radio." },
  { "en": "Kartu E-Toll Menggunakan Teknologi Apa?", "id": "RFID NFC." },
  { "en": "Apa Kepanjangan NFC (Near Field Communication)?", "id": "Komunikasi Medan Dekat." },
  { "en": "Bluetooth Bekerja Pada Frekuensi Berapa?", "id": "2,4 Giga Hertz." },
  { "en": "Apa Itu Lora (Long Range) Communication?", "id": "Komunikasi Jarak Jauh Daya Rendah." },
  { "en": "Antena Yagi Digunakan Untuk Apa?", "id": "Mengarahkan Sinyal Radio." },
  { "en": "Apa Itu Gain Antena?", "id": "Penguatan Sinyal Antena." },
  { "en": "Satuan Gain Antena Adalah?", "id": "dBi Atau dBd." },
  { "en": "Apa Kepanjangan VSWR (Voltage Standing Wave Ratio)?", "id": "Rasio Gelombang Berdiri Tegangan." },
  { "en": "Nilai VSWR Sempurna Adalah?", "id": "1 Banding 1." },
  { "en": "Apa Akibat Nilai VSWR Tinggi?", "id": "Daya Balik Merusak Pemancar." },
  { "en": "Kabel Serat Optik Single Mode Intinya?", "id": "Sangat Kecil." },
  { "en": "Kabel Serat Optik Multi Mode Intinya?", "id": "Lebih Besar." },
  { "en": "Sinar Laser Digunakan Pada Fiber Optik Apa?", "id": "Single Mode." },
  { "en": "Sinar LED Digunakan Pada Fiber Optik Apa?", "id": "Multi Mode." },
  { "en": "Apa Kepanjangan ODP (Optical Distribution Point)?", "id": "Titik Distribusi Optik." },
  { "en": "Di Mana Letak ODP Biasanya?", "id": "Tiang Kabel Telkom." },
  { "en": "Apa Fungsi Media Converter?", "id": "Ubah Sinyal Optik Ke LAN." },
  { "en": "Apa Itu Protokol TCP/IP?", "id": "Standar Komunikasi Internet." },
  { "en": "Alamat IP Address V4 Terdiri Dari?", "id": "32 Bit Angka." },
  { "en": "Apa Fungsi Switch Hub?", "id": "Menghubungkan Perangkat Jaringan." },
  { "en": "Apa Fungsi Router?", "id": "Mengatur Lalu Lintas Jaringan." },
  { "en": "Apa Kepanjangan PoE (Power Over Ethernet)?", "id": "Daya Melalui Kabel Ethernet." },
  { "en": "Kamera CCTV IP Biasanya Menggunakan Daya?", "id": "Power Over Ethernet." },
  { "en": "Berapa Tegangan Maksimal PoE Standar?", "id": "Sekitar 48 Volt." },
  { "en": "Apa Itu Pin GPIO Pada Mikrokontroler?", "id": "Pin Input Output Umum." },
  { "en": "Pull Up Resistor Berfungsi Untuk?", "id": "Menjaga Logika High." },
  { "en": "Pull Down Resistor Berfungsi Untuk?", "id": "Menjaga Logika Low." },
  { "en": "Apa Itu Debounce Pada Tombol?", "id": "Menghilangkan Sinyal Pantul Tombol." },
  { "en": "Apa Kepanjangan IDE (Integrated Development Environment)?", "id": "Lingkungan Pengembangan Terpadu." },
  { "en": "Arduino IDE Digunakan Untuk Apa?", "id": "Menulis Kode Program Arduino." },
  { "en": "Apa Itu Bootloader Pada Mikrokontroler?", "id": "Program Awal Saat Nyala." },
  { "en": "ESP8266 Adalah Modul Apa?", "id": "Modul WiFi Mikrokontroler." },
  { "en": "ESP32 Memiliki Fitur Apa?", "id": "WiFi Dan Bluetooth." },
  { "en": "Raspberry Pi Adalah Jenis Komputer Apa?", "id": "Komputer Papan Tunggal." },
  { "en": "Sistem Operasi Raspberry Pi Biasanya?", "id": "Linux." },
  { "en": "Apa Itu Relay Module?", "id": "Modul Sakelar Kendali Logika." },
  { "en": "Mengapa Relay Perlu Dioda Flyback?", "id": "Mencegah Arus Balik Induksi." },
  { "en": "Apa Itu H-Bridge Driver?", "id": "Pengendali Arah Putaran Motor." },
  { "en": "IC L298N Sering Digunakan Sebagai?", "id": "Driver Motor DC." },
  { "en": "Apa Fungsi Sensor Load Cell?", "id": "Menimbang Berat." },
  { "en": "Sensor Strain Gauge Mengukur Apa?", "id": "Regangan Atau Deformasi." },
  { "en": "Apa Fungsi Sensor Hall Effect?", "id": "Deteksi Medan Magnet." },
  { "en": "Sensor LM35 Digunakan Untuk Mengukur?", "id": "Suhu." },
  { "en": "Apa Fungsi Akselerometer (Accelerometer)?", "id": "Mengukur Percepatan Dan Kemiringan." },
  { "en": "Apa Fungsi Giroskop (Gyroscope)?", "id": "Mengukur Kecepatan Sudut Rotasi." },
  { "en": "Apa Fungsi Microphone Pada Rangkaian?", "id": "Ubah Suara Jadi Listrik." },
  { "en": "Apa Fungsi Speaker Pada Rangkaian?", "id": "Ubah Listrik Jadi Suara." },
  { "en": "Apa Itu Buzzer Elektronika?", "id": "Penghasil Suara Beep." },
  { "en": "Buzzer Aktif Memerlukan Apa Untuk Bunyi?", "id": "Hanya Tegangan DC." },
  { "en": "Buzzer Pasif Memerlukan Apa Untuk Bunyi?", "id": "Sinyal Osilasi PWM." },
  { "en": "Apa Fungsi Photodiode?", "id": "Sensor Cahaya." },
  { "en": "Apa Bedanya Photodiode Dan LDR (Light Dependent Resistor)?", "id": "Photodiode Lebih Cepat Responnya." },
  { "en": "Apa Fungsi Phototransistor?", "id": "Sakelar Cahaya Sensitif." },
  { "en": "Komunikasi Simplex Artinya Apa?", "id": "Satu Arah Saja." },
  { "en": "Contoh Komunikasi Simplex Adalah?", "id": "Radio Dan Televisi." },
  { "en": "Komunikasi Half Duplex Artinya Apa?", "id": "Dua Arah Bergantian." },
  { "en": "Contoh Komunikasi Half Duplex Adalah?", "id": "Walkie Talkie HT." },
  { "en": "Komunikasi Full Duplex Artinya Apa?", "id": "Dua Arah Bersamaan." },
  { "en": "Contoh Komunikasi Full Duplex Adalah?", "id": "Telepon Handphone." },
  { "en": "Apa Kepanjangan GSM (Global System for Mobile)?", "id": "Sistem Global Seluler." },
  { "en": "Apa Kepanjangan LTE (Long Term Evolution)?", "id": "Standar Jaringan 4G." },
  { "en": "Apa Kepanjangan GPS (Global Positioning System)?", "id": "Sistem Pemosisi Global." },
  { "en": "GPS (Global Positioning System) Menggunakan Sinyal Apa?", "id": "Satelit." },
  { "en": "Apa Itu Bandwidth Dalam Internet?", "id": "Lebar Pita Data." },
  { "en": "Satuan Kecepatan Internet Biasanya Apa?", "id": "Mbps." },
  { "en": "Apa Kepanjangan Mbps?", "id": "Megabit Per Second." },
  { "en": "1 Byte Setara Berapa Bit?", "id": "8 Bit." },
  { "en": "Apa Itu In Bow Dus?", "id": "Kotak Sakelar Tanam Tembok." },
  { "en": "Apa Itu Out Bow Dus?", "id": "Kotak Sakelar Tempel Tembok." },
  { "en": "Apa Fungsi T Dus (Junction Box)?", "id": "Kotak Percabangan Kabel." },
  { "en": "Apa Fungsi Pipa Conduit?", "id": "Pelindung Kabel Instalasi." },
  { "en": "Apa Itu Klem Kabel?", "id": "Penjepit Kabel Di Dinding." },
  { "en": "Alat Tang Kupas Kabel Disebut?", "id": "Wire Stripper." },
  { "en": "Alat Tang Press Skun Disebut?", "id": "Crimping Tool." },
  { "en": "Apa Fungsi Skun Kabel (Cable Lug)?", "id": "Konektor Ujung Kabel." },
  { "en": "Apa Itu Heat Shrink Tube?", "id": "Selongsong Bakar Pelindung Sambungan." },
  { "en": "Apa Fungsi Cable Ties (Kabel Ties)?", "id": "Pengikat Rapih Kabel." },
  { "en": "Ukuran Penampang Kabel Dinyatakan Dalam?", "id": "mm Persegi." },
  { "en": "Semakin Besar Penampang Kabel Maka Hambatan?", "id": "Semakin Kecil." },
  { "en": "Semakin Panjang Kabel Maka Hambatan?", "id": "Semakin Besar." },
  { "en": "Apa Itu AWG (American Wire Gauge)?", "id": "Standar Ukuran Kabel Amerika." },
  { "en": "Semakin Kecil Angka AWG Maka Kabel?", "id": "Semakin Besar." },
  { "en": "Apa Itu Fuse Blade?", "id": "Sekering Tancap Otomotif." },
  { "en": "Apa Fungsi Relay Klakson?", "id": "Meringankan Beban Sakelar Klakson." },
  { "en": "Apa Kepanjangan ECU (Electronic Control Unit)?", "id": "Unit Kontrol Elektronik Mobil." },
  { "en": "Apa Itu OBD (On Board Diagnostics)?", "id": "Port Diagnosa Kerusakan Mobil." },
  { "en": "Berapa Tegangan Aki Mobil Standar?", "id": "12 Volt." },
  { "en": "Berapa Tegangan Aki Truk Besar?", "id": "24 Volt." },
  { "en": "Apa Itu Dinamo Ampere (Alternator)?", "id": "Pengisi Aki Mobil." },
  { "en": "Apa Itu Dinamo Starter?", "id": "Motor Pemutar Mesin Awal." },
  { "en": "Apa Fungsi Busi (Spark Plug)?", "id": "Pemercik Api Pembakaran." },
  { "en": "Busi Membutuhkan Tegangan Berapa?", "id": "Ribuan Volt." },
  { "en": "Koil Pengapian Berfungsi Sebagai?", "id": "Trafo Step Up Tinggi." },
  { "en": "Apa Itu EV (Electric Vehicle)?", "id": "Kendaraan Listrik." },
  { "en": "Apa Komponen Utama Mobil Listrik?", "id": "Baterai Dan Motor Traksi." },
  { "en": "Apa Itu Regenerative Braking?", "id": "Pengereman Menghasilkan Listrik." },
  { "en": "Satuan Kapasitas Baterai Mobil Listrik?", "id": "kWh." },
  { "en": "Apa Itu Hybrid Vehicle?", "id": "Gabungan Bensin Dan Listrik." },
  { "en": "Apa Itu Soft Starter Pada Motor?", "id": "Pengasutan Halus Motor AC." },
  { "en": "Soft Starter Mengurangi Lonjakan Apa?", "id": "Lonjakan Arus Awal." },
  { "en": "Apa Itu Kontaktor Magnet?", "id": "Sakelar Daya Elektromagnetik." },
  { "en": "Kontaktor Memiliki Kontak Utama Dan?", "id": "Kontak Bantu." },
  { "en": "Kode Kontak A1 Dan A2 Adalah?", "id": "Terminal Koil Kontaktor." },
  { "en": "Kode Kontak 13 Dan 14 Biasanya?", "id": "Kontak Bantu NO." },
  { "en": "Apa Fungsi Push Button Switch?", "id": "Tombol Tekan Sesaat." },
  { "en": "Apa Itu Pilot Lamp?", "id": "Lampu Indikator Panel." },
  { "en": "Warna Pilot Lamp Untuk Fasa R?", "id": "Merah." },
  { "en": "Warna Pilot Lamp Untuk Fasa S?", "id": "Kuning." },
  { "en": "Warna Pilot Lamp Untuk Fasa T?", "id": "Hijau." },
  { "en": "Apa Itu Selector Switch?", "id": "Sakelar Pemilih Mode." },
  { "en": "Apa Itu Busbar Sisir?", "id": "Penghantar MCB Berjejer." },
  { "en": "Apa Fungsi Isolasi Busbar?", "id": "Mencegah Loncat Api." },
  { "en": "Apa Itu Creepage Distance?", "id": "Jarak Rambat Permukaan Isolator." },
  { "en": "Apa Itu Clearance Distance?", "id": "Jarak Bebas Udara." },
  { "en": "Apa Itu Short Circuit Current (Isc)?", "id": "Arus Hubung Singkat." },
  { "en": "Apa Itu Open Circuit Voltage (Voc)?", "id": "Tegangan Rangkaian Terbuka." },
  { "en": "Rangkaian Seri Resistor Berfungsi Sebagai?", "id": "Pembagi Tegangan." },
  { "en": "Rangkaian Paralel Resistor Berfungsi Sebagai?", "id": "Pembagi Arus." },
  { "en": "Rumus Arus Total Resistor Paralel?", "id": "Jumlah Arus Tiap Cabang." },
  { "en": "Rumus Tegangan Total Resistor Seri?", "id": "Jumlah Tegangan Tiap Resistor." },
  { "en": "Hambatan Total Resistor Paralel Selalu?", "id": "Lebih Kecil Dari Terkecil." },
  { "en": "Hambatan Total Resistor Seri Selalu?", "id": "Lebih Besar Dari Terbesar." },
  { "en": "Apa Itu Joule Heating?", "id": "Panas Akibat Arus Listrik." },
  { "en": "Elemen Pemanas Dibuat Dari Kawat?", "id": "Niklin (Nichrome)." },
  { "en": "Apa Itu Biaya Beban Listrik?", "id": "Biaya Abonemen Tetap." },
  { "en": "Apa Itu Token Listrik Prabayar?", "id": "Kode Isi Ulang KWh." },
  { "en": "Kode Token Terdiri Dari Berapa Angka?", "id": "20 Digit Angka." },
  { "en": "Apa Fungsi Kapasitor Coupling?", "id": "Meneruskan AC Menahan DC." },
  { "en": "Apa Fungsi Kapasitor Decoupling?", "id": "Meredam Noise Power Supply." },
  { "en": "Apa Itu Ground Loop?", "id": "Masalah Beda Potensial Ground." },
  { "en": "Ground Loop Menyebabkan Apa Pada Audio?", "id": "Suara Dengung (Humming)." },
  { "en": "Bagaimana Cara Mengatasi Ground Loop?", "id": "Satu Titik Grounding Pusat." },
  { "en": "Apa Itu Ferroresonansi?", "id": "Osilasi Non Linear Inti Besi." },
  { "en": "Trafo Isolasi Memiliki Rasio Lilitan?", "id": "1 Banding 1." },
  { "en": "Apa Fungsi Utama Trafo Isolasi?", "id": "Keamanan Dari Sengatan Listrik." },
  { "en": "Apa Itu Galvanic Isolation?", "id": "Pemisahan Aliran Arus Langsung." },
  { "en": "Optocoupler Menggunakan Prinsip Apa?", "id": "Isolasi Galvanik Optik." },
  { "en": "Apa Itu Thermostat?", "id": "Sakelar Suhu Otomatis." },
  { "en": "Setrika Listrik Menggunakan Komponen Apa?", "id": "Thermostat Bimetal." },
  { "en": "Apa Sifat Output Gerbang Logika Buffer?", "id": "Sama Dengan Input." },
  { "en": "Gerbang NAND Disebut Juga Sebagai?", "id": "Gerbang Universal." },
  { "en": "Output Gerbang XOR Bernilai 1 Jika?", "id": "Input Berbeda Logika." },
  { "en": "Output Gerbang XNOR Bernilai 1 Jika?", "id": "Input Sama Logika." },
  { "en": "Apa Fungsi Flip Flop Pada Digital?", "id": "Menyimpan 1 Bit Data." },
  { "en": "Apa Itu Clock Pada Rangkaian Digital?", "id": "Detak Penyelaras Waktu." },
  { "en": "Sinyal Clock Biasanya Berbentuk Gelombang?", "id": "Gelombang Kotak." },
  { "en": "Apa Kepanjangan RAM (Random Access Memory)?", "id": "Memori Akses Acak." },
  { "en": "Sifat Volatile Pada RAM Artinya?", "id": "Data Hilang Tanpa Listrik." },
  { "en": "Apa Kepanjangan ROM (Read Only Memory)?", "id": "Memori Hanya Baca." },
  { "en": "Sifat Non Volatile Pada ROM Artinya?", "id": "Data Tetap Ada Tanpa Listrik." },
  { "en": "Apa Kepanjangan EEPROM?", "id": "ROM Dapat Dihapus Listrik." },
  { "en": "Flash Memory Digunakan Pada Perangkat Apa?", "id": "USB Flashdisk Dan SSD." },
  { "en": "Apa Itu Register Pada Prosesor?", "id": "Memori Internal Kecepatan Tinggi." },
  { "en": "Apa Itu ALU (Arithmetic Logic Unit)?", "id": "Unit Operasi Hitung Logika." },
  { "en": "Bilangan Biner 1010 Setara Desimal?", "id": "10." },
  { "en": "Bilangan Biner 1111 Setara Desimal?", "id": "15." },
  { "en": "Bilangan Heksadesimal F Setara Desimal?", "id": "15." },
  { "en": "Apa Itu LSB (Least Significant Bit)?", "id": "Bit Nilai Terkecil." },
  { "en": "Apa Itu MSB (Most Significant Bit)?", "id": "Bit Nilai Terbesar." },
  { "en": "Jika 2 Baterai 12V Diseri Tegangan Jadi?", "id": "24 Volt." },
  { "en": "Jika 2 Baterai 12V Diseri Kapasitas Jadi?", "id": "Tetap Sama." },
  { "en": "Jika 2 Baterai 12V Diparalel Tegangan Jadi?", "id": "Tetap 12 Volt." },
  { "en": "Jika 2 Baterai 12V Diparalel Kapasitas Jadi?", "id": "Bertambah 2 Kali Lipat." },
  { "en": "Rangkaian Seri Baterai Meningkatkan Apa?", "id": "Tegangan Total." },
  { "en": "Rangkaian Paralel Baterai Meningkatkan Apa?", "id": "Kapasitas Arus Total." },
  { "en": "Apa Warna Standar Helm Safety Listrik?", "id": "Kuning Atau Putih." },
  { "en": "Kebakaran Akibat Listrik Termasuk Kelas Apa?", "id": "Kelas C." },
  { "en": "Alat Pemadam Api Untuk Listrik Adalah?", "id": "CO2 Atau Powder." },
  { "en": "Mengapa Air Dilarang Untuk Memadamkan Listrik?", "id": "Air Menghantarkan Listrik." },
  { "en": "Apa Fungsi Sepatu Safety Isolator?", "id": "Mencegah Aliran Ke Tanah." },
  { "en": "Berapa Batas Arus Bocor Yang Mematikan?", "id": "Sekitar 30 Mili Ampere." },
  { "en": "Apa Itu LOTO (Lock Out Tag Out)?", "id": "Prosedur Penguncian Sumber Energi." },
  { "en": "Kapan LOTO Wajib Diterapkan?", "id": "Saat Perbaikan Mesin." },
  { "en": "Apa Fungsi Matras Karet Di Panel?", "id": "Isolasi Pijakan Kaki." },
  { "en": "Apa Itu Kesalahan Paralaks Saat Mengukur?", "id": "Kesalahan Sudut Pandang Mata." },
  { "en": "Agar Akurat Mata Harus Bagaimana Saat Mengukur?", "id": "Tegak Lurus Jarum." },
  { "en": "Cermin Pada Skala Multimeter Berfungsi Untuk?", "id": "Mencegah Kesalahan Paralaks." },
  { "en": "Apa Itu Zero Adjustment Pada Multimeter?", "id": "Kalibrasi Jarum Ke Nol." },
  { "en": "Kapan Harus Melakukan Zero Adjustment Ohm?", "id": "Setiap Pindah Selektor Ohm." },
  { "en": "Apa Itu Motor BLDC (Brushless DC)?", "id": "Motor DC Tanpa Sikat." },
  { "en": "Apa Kelebihan Motor BLDC Dibanding Brushed?", "id": "Lebih Awet Dan Efisien." },
  { "en": "Kipas Angin PC Menggunakan Jenis Motor?", "id": "Motor BLDC." },
  { "en": "Drone Biasanya Menggunakan Jenis Motor Apa?", "id": "Motor BLDC." },
  { "en": "ESC (Electronic Speed Controller) Digunakan Untuk?", "id": "Mengatur Motor BLDC." },
  { "en": "Apa Itu Motor Universal?", "id": "Bisa Pakai AC Atau DC." },
  { "en": "Contoh Alat Dengan Motor Universal?", "id": "Bor Listrik Dan Blender." },
  { "en": "Apa Ciri Fisik Motor Universal?", "id": "Punya Sikat Arang (Brush)." },
  { "en": "Apa Itu Slip Ring Pada Generator?", "id": "Cincin Gesek Rotor." },
  { "en": "Apa Bedanya Slip Ring Dan Komutator?", "id": "Slip Ring Cincin Utuh." },
  { "en": "Generator AC Menggunakan Slip Ring Atau Komutator?", "id": "Slip Ring." },
  { "en": "Generator DC Menggunakan Slip Ring Atau Komutator?", "id": "Komutator Belah." },
  { "en": "Apa Fungsi Kapasitor Start Pada Motor?", "id": "Membantu Putaran Awal." },
  { "en": "Apa Fungsi Kapasitor Run Pada Motor?", "id": "Memperhalus Putaran Motor." },
  { "en": "Apa Tanda Kapasitor Motor Rusak?", "id": "Motor Mendengung Tidak Berputar." },
  { "en": "Apa Itu Duty Cycle Pada PWM?", "id": "Persentase Waktu Sinyal High." },
  { "en": "Duty Cycle 50 Persen Artinya?", "id": "Setengah On Setengah Off." },
  { "en": "Duty Cycle 100 Persen Artinya?", "id": "Sinyal On Terus Menerus." },
  { "en": "Rumus Tegangan Rata Rata PWM Adalah?", "id": "Vmax Kali Duty Cycle." },
  { "en": "Apa Itu Buck Converter?", "id": "Penurun Tegangan DC DC." },
  { "en": "Apa Itu Boost Converter?", "id": "Penaik Tegangan DC DC." },
  { "en": "Apa Itu Buck Boost Converter?", "id": "Bisa Naik Dan Turun." },
  { "en": "Regulator Linear 7805 Menghasilkan Output?", "id": "Positif 5 Volt." },
  { "en": "Regulator Linear 7812 Menghasilkan Output?", "id": "Positif 12 Volt." },
  { "en": "Regulator Linear 7905 Menghasilkan Output?", "id": "Negatif 5 Volt." },
  { "en": "Apa Kelemahan Regulator Linear?", "id": "Boros Daya Jadi Panas." },
  { "en": "Efisiensi Switching Regulator (SMPS) Biasanya?", "id": "Di Atas 80 Persen." },
  { "en": "Apa Fungsi Trafo Ferrite Pada SMPS?", "id": "Transfer Daya Frekuensi Tinggi." },
  { "en": "Apa Itu Opto Triac?", "id": "Isolator Pemicu Triac." },
  { "en": "Zero Crossing Detector Berfungsi Untuk?", "id": "Deteksi Titik Nol AC." },
  { "en": "Mengapa Switching Pada Zero Crossing Penting?", "id": "Mengurangi Noise Dan Panas." },
  { "en": "Dimmer Lampu AC Menggunakan Komponen?", "id": "TRIAC Dan DIAC." },
  { "en": "Apa Itu Ballast Pada Lampu Neon?", "id": "Pembatas Arus Gas." },
  { "en": "Lampu TL (Tube Luminescent) Berisi Gas?", "id": "Gas Merkuri Rendah." },
  { "en": "Lapisan Fosfor Pada Lampu Berfungsi?", "id": "Mengubah UV Jadi Cahaya." },
  { "en": "Starter Lampu TL Berfungsi Sebagai?", "id": "Sakelar Otomatis Pemanas Filamen." },
  { "en": "Lampu LED Tidak Memerlukan Apa?", "id": "Ballast Dan Starter." },
  { "en": "Driver LED Berfungsi Sebagai?", "id": "Sumber Arus Konstan." },
  { "en": "Apa Satuan Efikasi Cahaya?", "id": "Lumen Per Watt." },
  { "en": "Warna Cahaya Warm White Suhunya?", "id": "Sekitar 3000 Kelvin." },
  { "en": "Warna Cahaya Cool Day Light Suhunya?", "id": "Sekitar 6500 Kelvin." },
  { "en": "Apa Itu Indeks Bias Cahaya?", "id": "Pembelokan Cahaya." },
  { "en": "Kabel Tembaga Murni Bersifat?", "id": "Non Magnetik." },
  { "en": "Kabel Campuran Besi Bersifat?", "id": "Ditarik Magnet." },
  { "en": "Apa Itu CCA (Copper Clad Aluminum)?", "id": "Aluminium Berlapis Tembaga." },
  { "en": "Kelemahan Kabel CCA Adalah?", "id": "Mudah Patah Dan Panas." },
  { "en": "Cara Membedakan Tembaga Murni Dan CCA?", "id": "Kerok Lapisan Kabel." },
  { "en": "Warna Dalam Kabel CCA Adalah?", "id": "Putih Perak." },
  { "en": "Warna Dalam Kabel Tembaga Murni Adalah?", "id": "Merah Tembaga." },
  { "en": "Apa Itu Through Hole Technology (THT)?", "id": "Komponen Kaki Tembus PCB." },
  { "en": "Apa Itu Surface Mount Technology (SMT)?", "id": "Komponen Tempel Permukaan." },
  { "en": "Komponen SMT Lebih Hemat Apa?", "id": "Tempat Dan Biaya." },
  { "en": "Resistor Kapur (Cement Resistor) Digunakan Untuk?", "id": "Daya Besar." },
  { "en": "Apa Itu Wirewound Resistor?", "id": "Resistor Lilitan Kawat." },
  { "en": "Kapasitor Keramik 104 Nilainya?", "id": "100 Nano Farad." },
  { "en": "Kapasitor Keramik 102 Nilainya?", "id": "1 Nano Farad." },
  { "en": "Apa Fungsi Flux Pada Pasta Solder?", "id": "Mencegah Oksidasi." },
  { "en": "Apa Fungsi Amplifier Kelas A?", "id": "Penguat Sinyal Paling Linier." },
  { "en": "Apa Kelemahan Utama Amplifier Kelas A?", "id": "Efisiensi Rendah Cepat Panas." },
  { "en": "Amplifier Kelas B Memiliki Masalah Apa?", "id": "Cacat Persilangan Crossover." },
  { "en": "Amplifier Kelas AB Menggabungkan Sifat Apa?", "id": "Kelas A Dan Kelas B." },
  { "en": "Amplifier Kelas D Sering Disebut Apa?", "id": "Amplifier Switching Digital." },
  { "en": "Apa Keunggulan Utama Amplifier Kelas D?", "id": "Efisiensi Sangat Tinggi." },
  { "en": "Apa Fungsi Dioda Schottky?", "id": "Penyearah Kecepatan Tinggi." },
  { "en": "Apa Keunggulan Dioda Schottky?", "id": "Tegangan Jatuh Sangat Rendah." },
  { "en": "Berapa Tegangan Jatuh Dioda Silikon?", "id": "0,7 Volt." },
  { "en": "Berapa Tegangan Jatuh Dioda Germanium?", "id": "0,3 Volt." },
  { "en": "Apa Fungsi Dioda Varactor?", "id": "Pengganti Kapasitor Variabel." },
  { "en": "Dioda Varactor Bekerja Pada Kondisi Apa?", "id": "Bias Mundur." },
  { "en": "Apa Fungsi Dioda Tunnel?", "id": "Osilator Frekuensi Sangat Tinggi." },
  { "en": "Apa Itu Reverse Bias Pada Dioda?", "id": "Kutub Positif Ke Katoda." },
  { "en": "Apa Itu Forward Bias Pada Dioda?", "id": "Kutub Positif Ke Anoda." },
  { "en": "Apa Fungsi Bridge Diode (Kiprok)?", "id": "Penyearah Gelombang Penuh." },
  { "en": "Bridge Diode Berisi Berapa Dioda?", "id": "4 Dioda." },
  { "en": "Apa Itu Ripple Tegangan?", "id": "Sisa Gelombang AC Pada DC." },
  { "en": "Bagaimana Cara Mengurangi Ripple Tegangan?", "id": "Menambah Nilai Kapasitor Filter." },
  { "en": "Sumbu X Pada Layar Osiloskop Menunjukkan?", "id": "Waktu." },
  { "en": "Sumbu Y Pada Layar Osiloskop Menunjukkan?", "id": "Tegangan." },
  { "en": "Apa Fungsi Knop Volt Div?", "id": "Skala Tegangan Per Kotak." },
  { "en": "Apa Fungsi Knop Time Div?", "id": "Skala Waktu Per Kotak." },
  { "en": "Apa Fungsi Trigger Pada Osiloskop?", "id": "Menstabilkan Tampilan Gelombang." },
  { "en": "Apa Itu Probe Kalibrasi Osiloskop?", "id": "Sumber Sinyal Kotak Standar." },
  { "en": "Gelombang Kotak Mengandung Banyak Apa?", "id": "Harmonisa Ganjil." },
  { "en": "Gelombang Sinus Murni Terdiri Dari?", "id": "1 Frekuensi Fundamental." },
  { "en": "Apa Satuan Kebisingan Suara Audio?", "id": "Desibel." },
  { "en": "Berapa Rentang Frekuensi Audio Manusia?", "id": "20 Hz Sampai 20 kHz." },
  { "en": "Apa Itu Frekuensi Infrasonik?", "id": "Di Bawah 20 Hz." },
  { "en": "Apa Itu Frekuensi Ultrasonik?", "id": "Di Atas 20 kHz." },
  { "en": "Apa Fungsi Crossover Pasif Speaker?", "id": "Memisahkan Frekuensi Suara." },
  { "en": "Tweeter Adalah Speaker Untuk Frekuensi?", "id": "Frekuensi Tinggi Treble." },
  { "en": "Woofer Adalah Speaker Untuk Frekuensi?", "id": "Frekuensi Rendah Bass." },
  { "en": "Subwoofer Khusus Menghasilkan Frekuensi?", "id": "Frekuensi Sangat Rendah." },
  { "en": "Apa Itu Feedback Pada Microphone?", "id": "Suara Mencuit Masuk Kembali." },
  { "en": "Apa Komponen Utama PLTA?", "id": "Turbin Air Dan Generator." },
  { "en": "Apa Fungsi Penstock Pada PLTA?", "id": "Pipa Pesat Pengarah Air." },
  { "en": "Turbin Pelton Cocok Untuk Kondisi?", "id": "Debit Kecil Tekanan Tinggi." },
  { "en": "Turbin Kaplan Cocok Untuk Kondisi?", "id": "Debit Besar Tekanan Rendah." },
  { "en": "Apa Fungsi Gearbox Pada Kincir Angin?", "id": "Meningkatkan Putaran Generator." },
  { "en": "Apa Itu PLTU (Pembangkit Listrik Tenaga Uap)?", "id": "Pembangkit Bahan Bakar Batubara." },
  { "en": "PLTU Menggunakan Prinsip Siklus Apa?", "id": "Siklus Rankine." },
  { "en": "Apa Fungsi Boiler Pada PLTU?", "id": "Memanaskan Air Menjadi Uap." },
  { "en": "Apa Fungsi Kondensor Pada PLTU?", "id": "Mendinginkan Uap Menjadi Air." },
  { "en": "Apa Itu Geothermal?", "id": "Energi Panas Bumi." },
  { "en": "Apa Itu Biomassa?", "id": "Energi Dari Bahan Organik." },
  { "en": "Apa Elektrolit Pada Aki Basah?", "id": "Asam Sulfat H2SO4." },
  { "en": "Berapa Tegangan Per Sel Aki Timbal?", "id": "2,1 Volt." },
  { "en": "Aki 12 Volt Terdiri Dari Berapa Sel?", "id": "6 Sel Seri." },
  { "en": "Apa Itu Deep Cycle Battery?", "id": "Tahan Pengosongan Hingga Habis." },
  { "en": "Apa Itu CCA (Cold Cranking Amps)?", "id": "Arus Start Saat Dingin." },
  { "en": "Apa Tanda Aki Mobil Rusak?", "id": "Tegangan Drop Saat Starter." },
  { "en": "Air Apa Untuk Menambah Cairan Aki?", "id": "Air Destilasi Murni." },
  { "en": "Mengapa Tidak Boleh Pakai Air Zuur?", "id": "Kadar Asam Jadi Tinggi." },
  { "en": "Apa Itu ESD (Electrostatic Discharge)?", "id": "Pelepasan Listrik Statis Tiba Tiba." },
  { "en": "Komponen Apa Paling Sensitif Terhadap ESD?", "id": "IC MOS Dan Prosesor." },
  { "en": "Apa Fungsi Gelang Antistatis?", "id": "Membuang Listrik Statis Tubuh." },
  { "en": "Apa Warna Kantong Antistatis Shielding?", "id": "Perak Mengkilap." },
  { "en": "Apa Warna Kantong Antistatis Dissipative?", "id": "Merah Muda Pink." },
  { "en": "Apa Itu Arus Inrush?", "id": "Lonjakan Arus Saat Dinyalakan." },
  { "en": "Komponen NTC Berfungsi Meredam Apa?", "id": "Arus Inrush." },
  { "en": "Apa Itu Thermal Runaway?", "id": "Panas Menghasilkan Tambahan Panas." },
  { "en": "Apa Fungsi Thermal Paste?", "id": "Penghantar Panas Ke Heatsink." },
  { "en": "Kaca Termasuk Konduktor Atau Isolator?", "id": "Isolator." },
  { "en": "Air Murni Termasuk Konduktor Atau Isolator?", "id": "Isolator." },
  { "en": "Air Garam Termasuk Konduktor Atau Isolator?", "id": "Konduktor." },
  { "en": "Karet Termasuk Konduktor Atau Isolator?", "id": "Isolator." },
  { "en": "Karbon Termasuk Konduktor Atau Isolator?", "id": "Konduktor." },
  { "en": "Apa Itu Efek Hall?", "id": "Beda Potensial Akibat Magnet." },
  { "en": "Sensor Efek Hall Digunakan Untuk?", "id": "Mengukur Arus Dan Putaran." },
  { "en": "Apa Itu Crosstalk Pada Kabel?", "id": "Gangguan Sinyal Kabel Sebelah." },
  { "en": "Kabel Twisted Pair Dipilin Untuk Apa?", "id": "Mengurangi Crosstalk." },
  { "en": "Konektor BNC Digunakan Untuk Kabel Apa?", "id": "Kabel Coaxial." },
  { "en": "Konektor RCA Biasanya Berwarna Apa?", "id": "Merah Putih Kuning." },
  { "en": "Konektor XLR Biasanya Digunakan Untuk?", "id": "Microphone Profesional." },
  { "en": "Jack Audio 3,5mm Disebut Juga?", "id": "Mini Jack." },
  { "en": "Jack Audio 6,5mm Disebut Juga?", "id": "Jack Akai." },
  { "en": "Apa Bedanya Jack Mono Dan Stereo?", "id": "Jumlah Gelang Hitam." },
  { "en": "Jack Stereo Memiliki Berapa Gelang Hitam?", "id": "2 Gelang." },
  { "en": "Apa Itu Solder Bridge?", "id": "Timah Menyambung 2 Kaki." },
  { "en": "Apa Itu Tombstoning Pada Komponen SMD?", "id": "Komponen Terangkat Berdiri." },
  { "en": "Apa Penyebab Tombstoning?", "id": "Panas Solder Tidak Merata." },
  { "en": "Apa Itu Multimeter Auto Range?", "id": "Memilih Skala Ukur Otomatis." },
  { "en": "Apa Fungsi Tombol Hold Di Multimeter?", "id": "Menahan Angka Di Layar." },
  { "en": "Apa Fungsi Tombol Min Max?", "id": "Merekam Nilai Terendah Tertinggi." },
  { "en": "Apa Itu Duty Cycle Pada Las?", "id": "Siklus Kerja Mesin Las." },
  { "en": "Trafo Las Inverter Menggunakan Sistem?", "id": "Switching Frekuensi Tinggi." },
  { "en": "Apa Kelebihan Trafo Las Inverter?", "id": "Ringan Dan Hemat Daya." },
  { "en": "Kawat Las Disebut Juga Apa?", "id": "Elektroda." },
  { "en": "Apa Fungsi Flux Pada Kawat Las?", "id": "Melindungi Logam Cair." },
  { "en": "Las Listrik Menggunakan Arus Besar Dan?", "id": "Tegangan Rendah." },
  { "en": "Bahaya Sinar Las Bagi Mata Adalah?", "id": "Radiasi Ultraviolet." },
  { "en": "Apa Nama Alat Pelindung Wajah Las?", "id": "Kedok Las." },
  { "en": "Apa Itu MCB Karakteristik C?", "id": "Untuk Beban Induktif Motor." },
  { "en": "Apa Itu MCB Karakteristik CL?", "id": "Untuk Beban Lampu Rumah." },
  { "en": "Apa Itu Breaking Capacity MCB?", "id": "Kapasitas Putus Arus Hubung Singkat." },
  { "en": "Satuan Breaking Capacity Biasanya?", "id": "Kilo Ampere." },
  { "en": "Apa Fungsi Watchdog Timer Pada Mikrokontroler?", "id": "Reset Sistem Saat Macet." },
  { "en": "Apa Itu Interrupt Pada Mikrokontroler?", "id": "Gangguan Prioritas Eksekusi Program." },
  { "en": "Apa Fungsi Crystal Oscillator 16MHz?", "id": "Sumber Detak Sistem." },
  { "en": "Apa Itu Brown Out Reset?", "id": "Reset Saat Tegangan Turun." },
  { "en": "Apa Itu Pin Reset Pada IC?", "id": "Mengulang Program Dari Awal." },
  { "en": "Apa Itu Rangkaian Resonansi RLC?", "id": "Reaktansi Induktif Sama Kapasitif." },
  { "en": "Pada Saat Resonansi Impedansi Menjadi?", "id": "Minimum Atau Maksimum." },
  { "en": "Apa Itu Frekuensi Cut Off?", "id": "Batas Frekuensi Sinyal." },
  { "en": "Filter Low Pass Meloloskan Frekuensi?", "id": "Frekuensi Rendah." },
  { "en": "Filter High Pass Meloloskan Frekuensi?", "id": "Frekuensi Tinggi." },
  { "en": "Filter Band Pass Meloloskan Frekuensi?", "id": "Rentang Frekuensi Tertentu." },
  { "en": "Apa Penyebab Motor Listrik Berdengung Keras?", "id": "Hilang 1 Fasa." },
  { "en": "Apa Itu Cogging Torque Pada Motor?", "id": "Gerakan Tersendat Saat Pelan." },
  { "en": "Apa Fungsi Kapasitor Bypass?", "id": "Membuang Noise Ke Ground." },
  { "en": "Apa Itu Common Emitter Amplifier?", "id": "Penguat Tegangan Paling Umum." },
  { "en": "Apa Itu Common Collector Amplifier?", "id": "Penguat Arus Penyangga." },
  { "en": "Nama Lain Common Collector Adalah?", "id": "Emitter Follower." },
  { "en": "Apa Itu Schmitt Trigger?", "id": "Komparator Dengan Histeresis." },
  { "en": "Apa Fungsi Schmitt Trigger?", "id": "Membersihkan Sinyal Digital Noise." },
  { "en": "Bahan Semikonduktor Tipe N Kelebihan Apa?", "id": "Elektron." },
  { "en": "Bahan Semikonduktor Tipe P Kelebihan Apa?", "id": "Hole." },
  { "en": "Doping Adalah Proses Menambahkan Apa?", "id": "Zat Pengotor." },
  { "en": "Apa Itu Junction PN?", "id": "Pertemuan Tipe P N." },
  { "en": "Apa Itu Depletion Layer?", "id": "Daerah Kosong Muatan." },
  { "en": "Apa Itu Modulasi ASK (Amplitude Shift Keying)?", "id": "Modulasi Digital Amplitudo." },
  { "en": "Apa Itu Modulasi FSK (Frequency Shift Keying)?", "id": "Modulasi Digital Frekuensi." },
  { "en": "Apa Fungsi LCR Meter?", "id": "Mengukur Induktansi Kapasitansi Resistansi." },
  { "en": "Apa Itu Logic Probe?", "id": "Alat Cek Logika Digital." },
  { "en": "Logic Probe Menyala Merah Menandakan?", "id": "Logika Tinggi 1." },
  { "en": "Logic Probe Menyala Hijau Menandakan?", "id": "Logika Rendah 0." },
  { "en": "Apa Warna Gelang Resistor 2200 Ohm?", "id": "Merah Merah Merah." },
  { "en": "Apa Warna Gelang Resistor 1000 Ohm?", "id": "Coklat Hitam Merah." },
  { "en": "Apa Warna Gelang Resistor 330 Ohm?", "id": "Oranye Oranye Coklat." },
  { "en": "Apa Warna Gelang Resistor 47000 Ohm?", "id": "Kuning Ungu Oranye." },
  { "en": "Tegangan Nominal Baterai NiMH Adalah?", "id": "1,2 Volt." },
  { "en": "Tegangan Penuh Baterai 18650 Adalah?", "id": "4,2 Volt." },
  { "en": "Tegangan Penyimpanan Baterai Lithium Adalah?", "id": "3,8 Volt." },
  { "en": "Apa Itu Breadboard Power Supply?", "id": "Modul Daya Papan Percobaan." },
  { "en": "Berapa Kaki IC Timer 555?", "id": "8 Kaki." },
  { "en": "Apa Fungsi IC Timer 555?", "id": "Pewaktu Dan Osilator." },
  { "en": "Mode Astable Pada 555 Menghasilkan?", "id": "Gelombang Kotak Kontinyu." },
  { "en": "Mode Monostable Pada 555 Menghasilkan?", "id": "Satu Pulsa Saja." },
  { "en": "Apa Kepanjangan MOSFET N Channel?", "id": "Saluran Negatif." },
  { "en": "Apa Kepanjangan MOSFET P Channel?", "id": "Saluran Positif." },
  { "en": "MOSFET N Channel Aktif Jika Gate?", "id": "Diberi Tegangan Positif." },
  { "en": "MOSFET P Channel Aktif Jika Gate?", "id": "Diberi Tegangan Negatif Ground." },
  { "en": "Apa Itu Saturation Mode Transistor?", "id": "Sakelar Kondisi On Penuh." },
  { "en": "Apa Itu Cut Off Mode Transistor?", "id": "Sakelar Kondisi Off Penuh." },
  { "en": "Apa Itu Active Mode Transistor?", "id": "Berfungsi Sebagai Penguat." },
  { "en": "Apa Fungsi Dioda Free Wheeling?", "id": "Mencegah Lonjakan Tegangan Balik." },
  { "en": "Di Mana Dioda Free Wheeling Dipasang?", "id": "Paralel Dengan Beban Induktif." },
  { "en": "Apa Itu Snubber Circuit?", "id": "Peredam Lonjakan Tegangan." },
  { "en": "Rangkaian Snubber Terdiri Dari?", "id": "Resistor Dan Kapasitor." },
  { "en": "Apa Itu Galvanometer?", "id": "Alat Ukur Arus Kecil." },
  { "en": "Apa Itu Shunt Resistor?", "id": "Resistor Paralel Ampere Meter." },
  { "en": "Fungsi Shunt Resistor Adalah?", "id": "Membagi Arus Besar." },
  { "en": "Apa Itu Multiplier Resistor?", "id": "Resistor Seri Volt Meter." },
  { "en": "Apa Itu Current Limiting Resistor?", "id": "Resistor Pembatas Arus." },
  { "en": "Rumus Resistor Untuk LED Adalah?", "id": "Vs Kurang Vled Bagi I." },
  { "en": "Arus Standar LED Indikator Adalah?", "id": "20 Mili Ampere." },
  { "en": "Berapa Tegangan Kerja LED Merah?", "id": "Sekitar 2 Volt." },
  { "en": "Berapa Tegangan Kerja LED Biru?", "id": "Sekitar 3 Volt." },
  { "en": "Berapa Tegangan Kerja LED Putih?", "id": "Sekitar 3 Volt." },
  { "en": "Apa Itu Common Anode RGB LED?", "id": "Kaki Positif Gabung." },
  { "en": "Apa Itu Common Cathode RGB LED?", "id": "Kaki Negatif Gabung." },
  { "en": "Apa Itu Relay NC (Normally Closed)?", "id": "Tersambung Saat Mati." },
  { "en": "Apa Itu Relay NO (Normally Open)?", "id": "Terputus Saat Mati." },
  { "en": "Apa Itu SSR (Solid State Relay)?", "id": "Relay Tanpa Gerak Mekanik." },
  { "en": "Apa Kelebihan Solid State Relay?", "id": "Cepat Dan Tahan Lama." },
  { "en": "Apa Kekurangan Solid State Relay?", "id": "Menghasilkan Panas." },
  { "en": "Apa Itu Thermistor NTC 10k?", "id": "10 Kiloohm Pada 25 Derajat." },
  { "en": "Apa Itu Encoder Rotary?", "id": "Sensor Putaran Digital." },
  { "en": "Apa Fungsi Encoder Pada Motor?", "id": "Umpan Balik Posisi." },
  { "en": "Apa Itu PWM Duty Cycle 0%?", "id": "Tegangan 0 Volt." },
  { "en": "Apa Itu PWM Duty Cycle 25%?", "id": "Seperempat Tegangan Maksimal." },
  { "en": "Apa Itu PWM Duty Cycle 75%?", "id": "Tiga Perempat Tegangan Maksimal." },
  { "en": "Frekuensi Listrik Pesawat Terbang Adalah?", "id": "400 Hertz." },
  { "en": "Mengapa Pesawat Menggunakan 400Hz?", "id": "Trafo Lebih Ringan." },
  { "en": "Apa Itu Kabel NYMHY?", "id": "Kabel Serabut Putih Fleksibel." },
  { "en": "Apa Itu Kabel NYAF?", "id": "Kabel Serabut Tunggal." },
  { "en": "Kabel NYAF Biasa Digunakan Di?", "id": "Panel Listrik." },
  { "en": "Apa Itu Busduct?", "id": "Rel Penghantar Daya Besar." },
  { "en": "Busduct Digunakan Sebagai Pengganti?", "id": "Kabel Ukuran Besar." },
  { "en": "Apa Fungsi ATS (Automatic Transfer Switch)?", "id": "Pindah Daya Otomatis Genset." },
  { "en": "ATS Biasanya Dipasangkan Dengan?", "id": "AMF Automatic Main Failure." },
  { "en": "Apa Fungsi AMF (Automatic Main Failure)?", "id": "Menyalakan Genset Saat Padam." },
  { "en": "Apa Itu Tegangan Eksitasi?", "id": "Tegangan Penguat Medan Magnet." },
  { "en": "Apa Itu AVR Genset Rusak?", "id": "Tegangan Output Tidak Stabil." },
  { "en": "Apa Itu Over Speed Pada Genset?", "id": "Putaran Mesin Terlalu Cepat." },
  { "en": "Apa Itu Under Voltage?", "id": "Tegangan Di Bawah Standar." },
  { "en": "Apa Itu Over Voltage?", "id": "Tegangan Di Atas Standar." },
  { "en": "Apa Fungsi Lightning Counter?", "id": "Menghitung Sambaran Petir." },
  { "en": "Apa Itu Head Terminal Penangkal Petir?", "id": "Ujung Runcing Penerima Petir." },
  { "en": "Jenis Penangkal Petir Elektrostatis Adalah?", "id": "Radius Perlindungan Luas." },
  { "en": "Jenis Penangkal Petir Konvensional Adalah?", "id": "Tombak Tembaga Runcing." },
  { "en": "Rumus Menghitung Kecepatan Sinkron Motor AC?", "id": "120 Kali Frekuensi Bagi Kutub." },
  { "en": "Apa Satuan Kecepatan Sinkron Motor?", "id": "RPM." },
  { "en": "Apa Itu Slip Pada Motor Induksi?", "id": "Selisih Kecepatan Sinkron Dan Rotor." },
  { "en": "Jika Kecepatan Rotor Sama Dengan Sinkron Maka?", "id": "Torsi Adalah 0." },
  { "en": "Motor 4 Kutub 50Hz Kecepatannya Berapa?", "id": "1500 RPM." },
  { "en": "Motor 2 Kutub 50Hz Kecepatannya Berapa?", "id": "3000 RPM." },
  { "en": "Apa Itu Nameplate Motor?", "id": "Pelat Data Spesifikasi Motor." },
  { "en": "Apa Arti Kode IP55 Pada Motor?", "id": "Tahan Debu Dan Semprotan Air." },
  { "en": "Kelas Isolasi F Pada Motor Tahan Suhu?", "id": "155 Derajat Celcius." },
  { "en": "Kelas Isolasi H Pada Motor Tahan Suhu?", "id": "180 Derajat Celcius." },
  { "en": "Apa Itu Efisiensi Motor Listrik?", "id": "Daya Mekanik Bagi Daya Listrik." },
  { "en": "Apa Itu Rugi Tembaga Pada Trafo?", "id": "Panas Akibat Hambatan Kawat." },
  { "en": "Apa Itu Rugi Inti Pada Trafo?", "id": "Histeresis Dan Arus Pusar." },
  { "en": "Kapan Efisiensi Trafo Bernilai Maksimum?", "id": "Rugi Tembaga Sama Dengan Inti." },
  { "en": "Apa Fungsi Minyak Trafo?", "id": "Pendingin Dan Isolator." },
  { "en": "Apa Itu Relay Buchholz?", "id": "Deteksi Gas Dalam Trafo." },
  { "en": "Relay Buchholz Dipasang Di Mana?", "id": "Antara Tangki Dan Konservator." },
  { "en": "Apa Fungsi Silica Gel Pada Trafo?", "id": "Menyerap Kelembaban Udara." },
  { "en": "Warna Silica Gel Kering Yang Baik?", "id": "Biru Atau Oranye." },
  { "en": "Jika Silica Gel Berwarna Merah Muda Artinya?", "id": "Sudah Jenuh Air." },
  { "en": "Apa Itu Tap Changer Pada Trafo?", "id": "Pengatur Rasio Belitan Tegangan." },
  { "en": "Apa Itu OLTC (On Load Tap Changer)?", "id": "Pindah Tap Saat Berbeban." },
  { "en": "Apa Itu Tegangan Induksi Diri?", "id": "GGL Lawan Akibat Perubahan Arus." },
  { "en": "Satuan Koefisien Induksi Diri Adalah?", "id": "Henry." },
  { "en": "Energi Yang Tersimpan Di Induktor Berupa?", "id": "Medan Magnet." },
  { "en": "Energi Yang Tersimpan Di Kapasitor Berupa?", "id": "Medan Listrik." },
  { "en": "Rumus Energi Induktor Adalah?", "id": "Setengah L I Kuadrat." },
  { "en": "Rumus Energi Kapasitor Adalah?", "id": "Setengah C V Kuadrat." },
  { "en": "Konstanta Waktu Rangkaian RC Adalah?", "id": "R Kali C." },
  { "en": "Konstanta Waktu Rangkaian RL Adalah?", "id": "L Bagi R." },
  { "en": "Dalam 1 Konstanta Waktu Tegangan Mencapai?", "id": "63 Persen." },
  { "en": "Kapasitor Penuh Setelah Berapa Konstanta Waktu?", "id": "5 Tau." },
  { "en": "Apa Itu Reluktansi Magnet?", "id": "Hambatan Aliran Fluks Magnet." },
  { "en": "Satuan Reluktansi Adalah?", "id": "Ampere Turns Per Weber." },
  { "en": "Kebalikan Dari Reluktansi Adalah?", "id": "Permeansi." },
  { "en": "Apa Itu Gaya Gerak Magnet (GGM)?", "id": "Penyebab Timbulnya Fluks Magnet." },
  { "en": "Rumus Gaya Gerak Magnet Adalah?", "id": "Jumlah Lilitan Kali Arus." },
  { "en": "Satuan Kuat Medan Magnet H Adalah?", "id": "Ampere Per Meter." },
  { "en": "Apa Itu Kurva Histeresis?", "id": "Kurva B H Bahan Magnet." },
  { "en": "Apa Itu Retentivitas Magnet?", "id": "Sifat Menyimpan Magnet Sisa." },
  { "en": "Apa Itu Koersivitas Magnet?", "id": "Gaya Untuk Menghilangkan Magnet." },
  { "en": "Magnet Permanen Membutuhkan Retentivitas?", "id": "Tinggi." },
  { "en": "Inti Trafo Membutuhkan Histeresis?", "id": "Rendah Atau Sempit." },
  { "en": "Hukum Biot Savart Menjelaskan Tentang?", "id": "Medan Magnet Di Sekitar Arus." },
  { "en": "Hukum Ampere Menjelaskan Hubungan?", "id": "Arus Dan Medan Magnet." },
  { "en": "Apa Itu Aturan Tangan Kanan Fleming?", "id": "Arah Arus Induksi Generator." },
  { "en": "Apa Itu Aturan Tangan Kiri Fleming?", "id": "Arah Gaya Motor Listrik." },
  { "en": "Ibu Jari Pada Aturan Fleming Menunjukkan?", "id": "Arah Gerakan Atau Gaya." },
  { "en": "Jari Telunjuk Pada Aturan Fleming Menunjukkan?", "id": "Arah Medan Magnet." },
  { "en": "Jari Tengah Pada Aturan Fleming Menunjukkan?", "id": "Arah Arus Listrik." },
  { "en": "Apa Itu Listrik 1 Fasa?", "id": "2 Kabel Fasa Dan Netral." },
  { "en": "Apa Itu Listrik 3 Fasa?", "id": "3 Kabel Fasa R S T." },
  { "en": "Beda Sudut Fasa Pada Listrik 3 Fasa?", "id": "120 Derajat." },
  { "en": "Apa Keuntungan Sistem 3 Fasa?", "id": "Daya Lebih Besar Kabel Hemat." },
  { "en": "Daya 3 Fasa Adalah Berapa Kali 1 Fasa?", "id": "3 Kali Lipat." },
  { "en": "Tegangan Line To Line 3 Fasa Adalah?", "id": "380 Volt." },
  { "en": "Tegangan Line To Neutral 3 Fasa Adalah?", "id": "220 Volt." },
  { "en": "Hubungan Tegangan Line Dan Phase Adalah?", "id": "Akar 3 Kali V Phase." },
  { "en": "Apa Itu Sambungan Bintang (Star/Y)?", "id": "Memiliki Titik Netral." },
  { "en": "Apa Itu Sambungan Segitiga (Delta)?", "id": "Tanpa Titik Netral." },
  { "en": "Pada Hubungan Star Arus Line Adalah?", "id": "Sama Dengan Arus Fasa." },
  { "en": "Pada Hubungan Delta Tegangan Line Adalah?", "id": "Sama Dengan Tegangan Fasa." },
  { "en": "Apa Itu Urutan Fasa (Phase Sequence)?", "id": "Arah Putaran R S T." },
  { "en": "Alat Cek Urutan Fasa Disebut?", "id": "Phase Sequence Indicator." },
  { "en": "Jika Urutan Fasa Terbalik Motor Akan?", "id": "Berputar Terbalik." },
  { "en": "Apa Itu Beban Seimbang 3 Fasa?", "id": "Arus Tiap Fasa Sama." },
  { "en": "Pada Beban Seimbang Arus Netral Adalah?", "id": "0 Ampere." },
  { "en": "Apa Itu Harmonisa Triplen?", "id": "Kelipatan 3 Frekuensi Dasar." },
  { "en": "Harmonisa Triplen Terakumulasi Di Kabel?", "id": "Kabel Netral." },
  { "en": "Apa Fungsi Kapasitor Coupling Audio?", "id": "Memblokir Tegangan DC." },
  { "en": "Apa Itu Gain Bandwidth Product?", "id": "Kualitas Op Amp Frekuensi Tinggi." },
  { "en": "Apa Itu Slew Rate Op Amp?", "id": "Kecepatan Perubahan Tegangan Output." },
  { "en": "Op Amp Ideal Memiliki Impedansi Input?", "id": "Tak Terhingga." },
  { "en": "Op Amp Ideal Memiliki Impedansi Output?", "id": "0 Ohm." },
  { "en": "Rangkaian Inverting Op Amp Membalikkan Apa?", "id": "Fasa Sinyal Input." },
  { "en": "Rumus Penguatan Inverting Op Amp?", "id": "Minus Rf Bagi Rin." },
  { "en": "Rumus Penguatan Non Inverting Op Amp?", "id": "1 Tambah Rf Bagi Rin." },
  { "en": "Apa Itu Voltage Follower?", "id": "Penguatan Sama Dengan 1." },
  { "en": "Voltage Follower Berfungsi Sebagai?", "id": "Buffer Impedansi." },
  { "en": "Apa Itu Komparator Tegangan?", "id": "Membandingkan 2 Nilai Tegangan." },
  { "en": "Output Komparator Bernilai High Jika?", "id": "V Positif Lebih Dari V Negatif." },
  { "en": "Apa Itu Histeresis Pada Komparator?", "id": "Celah Tegangan Ambang Batas." },
  { "en": "Apa Fungsi IC Regulator 7805?", "id": "Menstabilkan Tegangan 5 Volt." },
  { "en": "Apa Fungsi IC Regulator 7812?", "id": "Menstabilkan Tegangan 12 Volt." },
  { "en": "Kaki Input IC 7805 Nomor Berapa?", "id": "Kaki 1." },
  { "en": "Kaki Ground IC 7805 Nomor Berapa?", "id": "Kaki 2." },
  { "en": "Kaki Output IC 7805 Nomor Berapa?", "id": "Kaki 3." },
  { "en": "Tegangan Input Minimal 7805 Adalah?", "id": "7 Volt." },
  { "en": "Apa Itu Dropout Voltage Regulator?", "id": "Selisih Input Output Minimal." },
  { "en": "Apa Kepanjangan LDO Regulator?", "id": "Low Dropout Regulator." },
  { "en": "LDO Cocok Untuk Aplikasi Apa?", "id": "Baterai Tegangan Rendah." },
  { "en": "Apa Itu Thermal Shutdown?", "id": "Mati Otomatis Saat Panas." },
  { "en": "Apa Itu Short Circuit Protection?", "id": "Proteksi Hubung Singkat." },
  { "en": "Apa Fungsi Fuse Pada Multimeter?", "id": "Melindungi Alat Ukur." },
  { "en": "Apa Itu Kategori Pengukuran CAT I?", "id": "Elektronik Daya Rendah." },
  { "en": "Apa Itu Kategori Pengukuran CAT II?", "id": "Peralatan Rumah Tangga." },
  { "en": "Apa Itu Bandwidth Osiloskop?", "id": "Batas Frekuensi Ukur Akurat." },
  { "en": "Apa Itu Sampling Rate?", "id": "Kecepatan Ambil Data Sinyal." },
  { "en": "Satuan Sampling Rate Adalah?", "id": "Sample Per Second." },
  { "en": "Apa Itu Kontrol PID?", "id": "Proporsional Integral Derivatif." },
  { "en": "Apa Fungsi Bagian Integral Pada PID?", "id": "Menghilangkan Error Steady State." },
  { "en": "Apa Fungsi Bagian Derivatif Pada PID?", "id": "Merespon Perubahan Error Cepat." },
  { "en": "Sensor Suhu RTD Terbuat Dari Apa?", "id": "Logam Platinum Murni." },
  { "en": "Apa Bedanya RTD Dan Thermocouple?", "id": "RTD Akurat Thermocouple Luas." },
  { "en": "Apa Itu PT100?", "id": "RTD 100 Ohm Pada 0 Derajat." },
  { "en": "Apa Itu Efek Corona Pada Transmisi?", "id": "Pendaran Cahaya Kabel Tegangan Tinggi." },
  { "en": "Gas SF6 Digunakan Untuk Apa?", "id": "Isolasi Dan Pemadam Busur Api." },
  { "en": "Apa Sifat Fisik Gas SF6?", "id": "Tidak Berwarna Dan Tidak Berbau." },
  { "en": "Apa Itu VCB (Vacuum Circuit Breaker)?", "id": "Pemutus Sirkuit Ruang Hampa." },
  { "en": "Apa Itu OCB (Oil Circuit Breaker)?", "id": "Pemutus Sirkuit Media Minyak." },
  { "en": "Sistem Grounding TN-S Adalah?", "id": "Netral Dan Ground Terpisah." },
  { "en": "Sistem Grounding TN-C Adalah?", "id": "Netral Dan Ground Digabung." },
  { "en": "Apa Itu Isolator Pin Post?", "id": "Penyangga Kabel Tegangan Menengah." },
  { "en": "Apa Itu Isolator Suspension Atau Gantung?", "id": "Penyangga Kabel Tegangan Tinggi." },
  { "en": "Apa Itu Bundle Conductor?", "id": "Beberapa Kabel Per Fasa." },
  { "en": "Tujuan Bundle Conductor Adalah?", "id": "Mengurangi Efek Corona." },
  { "en": "Apa Itu Andongan Atau Sag Kabel?", "id": "Lengkungan Kabel Antara Tiang." },
  { "en": "Kabel ACSR Terbuat Dari Apa?", "id": "Aluminium Berinti Baja." },
  { "en": "Apa Kepanjangan ACSR?", "id": "Aluminium Conductor Steel Reinforced." },
  { "en": "Apa Itu Spacer Pada SUTET?", "id": "Pemisah Kabel Bundle." },
  { "en": "Apa Fungsi Damper Pada Kabel?", "id": "Meredam Getaran Angin." },
  { "en": "Bola Merah Di SUTET Berfungsi Untuk?", "id": "Tanda Peringatan Pesawat." },
  { "en": "Apa Itu Gardu GIS?", "id": "Gas Insulated Switchgear." },
  { "en": "Keunggulan Gardu GIS Adalah?", "id": "Ukuran Lebih Kecil." },
  { "en": "Apa Itu Trafo Arus (CT) Kelas P?", "id": "Untuk Proteksi Relay." },
  { "en": "Apa Itu Trafo Arus (CT) Kelas 0.5?", "id": "Untuk Metering Atau Pengukuran." },
  { "en": "Apa Bahaya Sekunder CT Terbuka?", "id": "Tegangan Tinggi Meledak." },
  { "en": "Sekunder CT Harus Selalu?", "id": "Terhubung Beban Atau Short." },
  { "en": "Apa Itu Burden Pada Trafo Arus?", "id": "Beban Impedansi Sekunder CT." },
  { "en": "Apa Itu Relay Jarak (Distance Relay)?", "id": "Proteksi Transmisi Berdasarkan Impedansi." },
  { "en": "Apa Itu Relay Diferensial?", "id": "Membandingkan Arus Masuk Keluar." },
  { "en": "Relay Diferensial Melindungi Apa?", "id": "Trafo Utama Dan Generator." },
  { "en": "Apa Itu OCR (Over Current Relay)?", "id": "Relay Arus Lebih." },
  { "en": "Apa Itu GFR (Ground Fault Relay)?", "id": "Relay Gangguan Tanah." },
  { "en": "Apa Itu UVR (Under Voltage Relay)?", "id": "Relay Tegangan Kurang." },
  { "en": "Apa Itu Recloser Pada Jaringan?", "id": "Pemutus Penutup Otomatis." },
  { "en": "Apa Fungsi Recloser?", "id": "Mengatasi Gangguan Sesaat." },
  { "en": "Apa Itu LBS (Load Break Switch)?", "id": "Sakelar Pemutus Berbeban." },
  { "en": "Bedanya LBS Dan Disconnector (PMS)?", "id": "PMS Tidak Boleh Berbeban." },
  { "en": "Apa Fungsi PMS (Pemisah)?", "id": "Memastikan Isolasi Fisik Aman." },
  { "en": "Kapan PMS Boleh Dibuka?", "id": "Saat Tidak Ada Arus." },
  { "en": "Apa Itu Scada Master Station?", "id": "Pusat Kontrol Pemantauan." },
  { "en": "Apa Itu RTU (Remote Terminal Unit)?", "id": "Unit Kontrol Jarak Jauh." },
  { "en": "RTU Berkomunikasi Dengan Apa?", "id": "Master Station SCADA." },
  { "en": "Protokol Modbus Menggunakan Media Apa?", "id": "Serial RS485 Atau Ethernet." },
  { "en": "Apa Itu Modbus RTU?", "id": "Data Biner Serial." },
  { "en": "Apa Itu Modbus TCP/IP?", "id": "Data Via Jaringan Ethernet." },
  { "en": "Kabel Fiber Optik OPGW Adalah?", "id": "Kabel Ground Berisi Optik." },
  { "en": "Posisi Kabel OPGW Di Tower?", "id": "Paling Atas Tower." },
  { "en": "Apa Itu Partial Discharge?", "id": "Peluhatan Listrik Tidak Sempurna." },
  { "en": "Alat Deteksi Partial Discharge Menggunakan?", "id": "Sensor Ultrasonik Atau TEV." },
  { "en": "Apa Itu Thermovision Atau Thermal Camera?", "id": "Kamera Deteksi Panas." },
  { "en": "Titik Panas Pada Sambungan Menandakan?", "id": "Koneksi Kendor Atau Kotor." },
  { "en": "Apa Itu Baterai VRLA?", "id": "Valve Regulated Lead Acid." },
  { "en": "Baterai VRLA Sering Disebut?", "id": "Aki Kering Bebas Perawatan." },
  { "en": "Baterai LiFePO4 Memiliki Tegangan Sel?", "id": "3,2 Volt." },
  { "en": "Siklus Hidup LiFePO4 Dibanding Lead Acid?", "id": "Jauh Lebih Panjang." },
  { "en": "Apa Itu DoD (Depth Of Discharge)?", "id": "Kedalaman Pengosongan Baterai." },
  { "en": "DoD 80 Persen Artinya?", "id": "Sisa Kapasitas 20 Persen." },
  { "en": "Apa Itu C Rate Baterai?", "id": "Kecepatan Pengisian Pengosongan." },
  { "en": "Baterai 100Ah Discharge 1C Artinya?", "id": "Arus 100 Ampere." },
  { "en": "Apa Itu Floating Charge?", "id": "Menjaga Baterai Tetap Penuh." },
  { "en": "Tegangan Floating Aki 12V Adalah?", "id": "13,5 Sampai 13,8 Volt." },
  { "en": "Apa Itu Equalizing Charge?", "id": "Menyamakan Tegangan Sel Baterai." },
  { "en": "Apa Itu Inverter Pure Sine Wave?", "id": "Gelombang Sinus Murni." },
  { "en": "Apa Itu Inverter Modified Sine Wave?", "id": "Gelombang Kotak Modifikasi." },
  { "en": "Peralatan Induktif Wajib Pakai Inverter?", "id": "Pure Sine Wave." },
  { "en": "Apa Itu Hybrid Inverter Solar?", "id": "Bisa Baterai Dan PLN." },
  { "en": "Apa Itu Grid Tie Inverter?", "id": "Mengirim Listrik Ke PLN." },
  { "en": "Apa Itu Anti Islanding Pada Inverter?", "id": "Mati Saat PLN Padam." },
  { "en": "Tujuannya Anti Islanding Adalah?", "id": "Keamanan Teknisi PLN." },
  { "en": "Panel Surya Monocrystalline Ciri Fisiknya?", "id": "Warna Hitam Sudut Terpotong." },
  { "en": "Panel Surya Polycrystalline Ciri Fisiknya?", "id": "Warna Biru Bercak Kotak." },
  { "en": "Efisiensi Mono Vs Poly Bagus Mana?", "id": "Monocrystalline Lebih Tinggi." },
  { "en": "Apa Itu Junction Box Panel Surya?", "id": "Kotak Terminal Di Belakang." },
  { "en": "Dioda Bypass Di Panel Surya Berfungsi?", "id": "Mengatasi Efek Bayangan Shading." },
  { "en": "Apa Itu MC4 Connector?", "id": "Konektor Standar Panel Surya." },
  { "en": "Kabel Solar Panel Harus Tahan Apa?", "id": "Sinar UV Matahari." },
  { "en": "Apa Itu Azimuth Angle Panel Surya?", "id": "Arah Mata Angin Panel." },
  { "en": "Apa Itu Tilt Angle Panel Surya?", "id": "Sudut Kemiringan Panel." },
  { "en": "Di Indonesia Panel Menghadap Ke Mana?", "id": "Utara Atau Selatan." },
  { "en": "Apa Itu Pyranometer?", "id": "Alat Ukur Radiasi Matahari." },
  { "en": "Apa Itu Solenoid Valve NC?", "id": "Tertutup Saat Tidak Ada Listrik." },
  { "en": "Apa Itu Solenoid Valve NO?", "id": "Terbuka Saat Tidak Ada Listrik." },
  { "en": "Tekanan Angin Pneumatik Standar Adalah?", "id": "6 Bar." },
  { "en": "Apa Itu Aktuator Pneumatik?", "id": "Silinder Penggerak Angin." },
  { "en": "Apa Fungsi Kompresor Angin?", "id": "Menghasilkan Udara Bertekanan." },
  { "en": "Apa Fungsi Air Dryer Kompresor?", "id": "Memisahkan Air Dari Udara." },
  { "en": "Apa Itu FRL Unit Pneumatik?", "id": "Filter Regulator Lubricator." },
  { "en": "Apa Fungsi Lubricator Pneumatik?", "id": "Memberi Oli Ke Sistem." },
  { "en": "Sistem Hidrolik Menggunakan Fluida Apa?", "id": "Oli Hidrolik Khusus." },
  { "en": "Tekanan Hidrolik Dibanding Pneumatik Adalah?", "id": "Jauh Lebih Tinggi." },
  { "en": "Apa Itu Check Valve?", "id": "Katup 1 Arah." },
  { "en": "Sensor NPN Outputnya Apa?", "id": "Sinyal Negatif Atau Ground." },
  { "en": "Sensor PNP Outputnya Apa?", "id": "Sinyal Positif." },
  { "en": "Apa Itu PLC Sinking Input?", "id": "Menerima Sinyal Negatif." },
  { "en": "Apa Itu PLC Sourcing Input?", "id": "Menerima Sinyal Positif." },
  { "en": "Standar Tegangan Input PLC Industri?", "id": "24 Volt DC." },
  { "en": "Apa Itu THD (Total Harmonic Distortion)?", "id": "Total Distorsi Harmonisa Gelombang." },
  { "en": "Batas Standar THD Tegangan Adalah?", "id": "Maksimal 5 Persen." },
  { "en": "Sumber Utama Harmonisa Di Industri Adalah?", "id": "Beban Non Linear VSD." },
  { "en": "Filter Harmonisa Pasif Terdiri Dari?", "id": "Induktor Dan Kapasitor." },
  { "en": "Filter Harmonisa Aktif Menggunakan Apa?", "id": "Inverter Pembangkit Arus Kompensasi." },
  { "en": "Apa Itu Flicker Lampu?", "id": "Kedipan Cahaya Akibat Tegangan Fluktuatif." },
  { "en": "Apa Penyebab Utama Flicker?", "id": "Beban Busur Api Las." },
  { "en": "Apa Itu K Factor Pada Trafo?", "id": "Ketahanan Terhadap Panas Harmonisa." },
  { "en": "Trafo K-13 Digunakan Untuk Beban?", "id": "Beban Non Linear Tinggi." },
  { "en": "Apa Itu Derating Pada Kabel?", "id": "Penurunan Kapasitas Hantar Arus." },
  { "en": "Faktor Derating Kabel Dipengaruhi Oleh?", "id": "Suhu Dan Jumlah Kabel." },
  { "en": "Apa Itu KHA (Kuat Hantar Arus)?", "id": "Arus Maksimal Kabel Aman." },
  { "en": "KHA Kabel Di Udara Dibanding Tanah?", "id": "Di Tanah Lebih Besar." },
  { "en": "Apa Itu Kabel NYFGbY?", "id": "Kabel Tanah Perisai Baja Pipih." },
  { "en": "Apa Itu Kabel NYRgbY?", "id": "Kabel Tanah Perisai Kawat Baja." },
  { "en": "Armour Kabel Berfungsi Melindungi Dari?", "id": "Tekanan Mekanis Fisik." },
  { "en": "Apa Itu Filling Ratio Pipa Conduit?", "id": "Persentase Isi Pipa Maksimal." },
  { "en": "Maksimal Isi Pipa Kabel Adalah?", "id": "40 Persen Penampang Pipa." },
  { "en": "Tujuannya Pembatasan Isi Pipa Adalah?", "id": "Sirkulasi Panas Dan Penarikan." },
  { "en": "Apa Itu Sakelar Tukar (Hotel)?", "id": "Sakelar 2 Arah." },
  { "en": "Fungsi Sakelar Tukar Adalah?", "id": "Kontrol 1 Lampu 2 Tempat." },
  { "en": "Sakelar Silang (Cross Switch) Digunakan Untuk?", "id": "Kontrol 1 Lampu 3 Tempat." },
  { "en": "Apa Itu Relay Impulse (Impulse Relay)?", "id": "Sakelar Tekan Sekali On Off." },
  { "en": "Relay Impulse Menghemat Penggunaan Apa?", "id": "Kabel Sakelar Banyak Titik." },
  { "en": "Apa Itu Dimmer TRIAC?", "id": "Pengatur Terang Lampu AC." },
  { "en": "Dimmer TRIAC Bekerja Dengan Cara?", "id": "Memotong Gelombang Sinus." },
  { "en": "Apa Itu Leading Power Factor?", "id": "Arus Mendahului Tegangan." },
  { "en": "Apa Itu Lagging Power Factor?", "id": "Arus Tertinggal Dari Tegangan." },
  { "en": "Beban Kapasitif Menyebabkan Faktor Daya?", "id": "Leading Atau Mendahului." },
  { "en": "Beban Induktif Menyebabkan Faktor Daya?", "id": "Lagging Atau Tertinggal." },
  { "en": "Denda PLN Diberikan Jika Cos Phi?", "id": "Kurang Dari 0,85." },
  { "en": "Apa Itu kWh Meter Prabayar?", "id": "Meteran Sistem Token." },
  { "en": "Apa Itu kWh Meter Pascabayar?", "id": "Meteran Tagihan Bulanan." },
  { "en": "Apa Itu Tamper Pada kWh Meter?", "id": "Indikasi Utak Atik Meteran." },
  { "en": "Lampu Tamper Menyala Jika?", "id": "Ada Kebocoran Atau Bypass." },
  { "en": "Apa Itu AMR (Automatic Meter Reading)?", "id": "Pembacaan Meter Jarak Jauh." },
  { "en": "Apa Itu MCB Cincin Biru (CL)?", "id": "Milik PLN Batas Daya." },
  { "en": "Apa Itu Segel Timah PLN?", "id": "Tanda Legalitas Alat Ukur." },
  { "en": "Memutus Segel PLN Adalah Tindakan?", "id": "Ilegal Pelanggaran Listrik." },
  { "en": "Apa Itu P2TL?", "id": "Penertiban Pemakaian Tenaga Listrik." },
  { "en": "Jenis Pelanggaran P2TL P1 Adalah?", "id": "Mempengaruhi Batas Daya." },
  { "en": "Jenis Pelanggaran P2TL P2 Adalah?", "id": "Mempengaruhi Pengukuran Energi." },
  { "en": "Jenis Pelanggaran P2TL P3 Adalah?", "id": "Mempengaruhi Daya Dan Energi." },
  { "en": "Apa Itu VFD (Variable Frequency Drive)?", "id": "Pengendali Kecepatan Motor AC." },
  { "en": "VFD Mengubah Frekuensi Dan Apa?", "id": "Tegangan Output." },
  { "en": "Apa Itu Carrier Frequency VFD?", "id": "Frekuensi Switching PWM Inverter." },
  { "en": "Frekuensi Carrier Tinggi Menyebabkan?", "id": "Motor Senyap Tapi Panas." },
  { "en": "Apa Itu Braking Resistor VFD?", "id": "Membuang Energi Pengereman Motor." },
  { "en": "Kapan Braking Resistor Diperlukan?", "id": "Beban Inersia Tinggi Berhenti Cepat." },
  { "en": "Apa Itu DC Injection Braking?", "id": "Menyuntik DC Agar Motor Berhenti." },
  { "en": "Apa Itu Ramp Up Time?", "id": "Waktu Akselerasi Kecepatan Penuh." },
  { "en": "Apa Itu Ramp Down Time?", "id": "Waktu Deselerasi Hingga Berhenti." },
  { "en": "VFD Memerlukan Kabel Jenis Apa?", "id": "Kabel Shielded Atau Screened." },
  { "en": "Apa Masalah Umum Bearing Motor VFD?", "id": "Arus Poros Merusak Bearing." },
  { "en": "Cara Mencegah Kerusakan Bearing VFD?", "id": "Pasang Grounding Brush Poros." },
  { "en": "Apa Itu Motor Squirrel Cage (Sangkar Tupai)?", "id": "Rotor Batang Hubung Singkat." },
  { "en": "Apa Itu Motor Wound Rotor (Belitan)?", "id": "Rotor Memiliki Kumparan Kawat." },
  { "en": "Motor Wound Rotor Menggunakan Apa?", "id": "Slip Ring Dan Sikat." },
  { "en": "Keunggulan Motor Wound Rotor Adalah?", "id": "Torsi Start Tinggi Terkendali." },
  { "en": "Apa Itu Kontrol PID Temperature?", "id": "Menjaga Suhu Tetap Stabil." },
  { "en": "Apa Itu Overshoot Pada Kontrol?", "id": "Nilai Melebihi Setpoint Sesaat." },
  { "en": "Apa Itu Steady State Error?", "id": "Selisih Nilai Akhir Setpoint." },
  { "en": "Sensor Termokopel Tipe K Rentang Suhunya?", "id": "Minus 200 Sampai 1250 Celcius." },
  { "en": "Warna Kabel Termokopel Tipe K Adalah?", "id": "Kuning Dan Merah." },
  { "en": "Sensor Termokopel Tipe J Rentang Suhunya?", "id": "0 Sampai 750 Celcius." },
  { "en": "Apa Itu Transmitter 4-20mA?", "id": "Pengirim Sinyal Arus Standar." },
  { "en": "Mengapa Pakai Arus 4-20mA Bukan Tegangan?", "id": "Tahan Noise Jarak Jauh." },
  { "en": "Jika Kabel Putus Sinyal 4-20mA Menjadi?", "id": "0 mA Terdeteksi Error." },
  { "en": "Apa Itu Hart Protocol?", "id": "Komunikasi Digital Tumpangan Analog." },
  { "en": "Berapa Resistansi Beban Hart Protocol?", "id": "Minimal 250 Ohm." },
  { "en": "Apa Itu Transistor Darlington?", "id": "Dua Transistor Sambungan Ganda." },
  { "en": "Keunggulan Pasangan Darlington Adalah?", "id": "Penguatan Arus Sangat Tinggi." },
  { "en": "Tegangan Jatuh Basis Emitor Darlington?", "id": "Sekitar 1,4 Volt." },
  { "en": "Apa Itu Transistor IGBT?", "id": "Gabungan MOSFET Dan BJT." },
  { "en": "Kepanjangan IGBT Adalah?", "id": "Insulated Gate Bipolar Transistor." },
  { "en": "IGBT Digunakan Pada Alat Apa?", "id": "Inverter Dan Mesin Las." },
  { "en": "Apa Itu Thermal Noise?", "id": "Desis Akibat Suhu Komponen." },
  { "en": "Apa Itu Shot Noise?", "id": "Desis Akibat Aliran Elektron." },
  { "en": "SNR Adalah Singkatan Dari?", "id": "Signal To Noise Ratio." },
  { "en": "Nilai SNR Yang Baik Adalah?", "id": "Semakin Tinggi Semakin Baik." },
  { "en": "Apa Itu Impedansi Input Osiloskop?", "id": "Biasanya 1 Megaohm." },
  { "en": "Apa Itu Mode X-Y Osiloskop?", "id": "Menampilkan Grafik Lissajous." },
  { "en": "Grafik Lissajous Digunakan Untuk?", "id": "Mengukur Beda Fasa Frekuensi." },
  { "en": "Apa Itu FFT (Fast Fourier Transform)?", "id": "Analisis Spektrum Frekuensi Sinyal." },
  { "en": "Fungsi FFT Pada Osiloskop?", "id": "Melihat Komponen Harmonisa Sinyal." },
  { "en": "Apa Itu Grounding ESE?", "id": "Early Streamer Emission Lightning." },
  { "en": "Apa Itu Step Up Chopper?", "id": "Konverter DC DC Boost." },
  { "en": "Apa Itu Step Down Chopper?", "id": "Konverter DC DC Buck." },
  { "en": "Duty Cycle Adalah Perbandingan?", "id": "Waktu On Bagi Periode." },
  { "en": "Apa Itu Komutasi Pada SCR?", "id": "Proses Mematikan SCR." },
  { "en": "Komutasi Alami Terjadi Pada Sumber?", "id": "Sumber Listrik AC." },
  { "en": "Komutasi Paksa Diperlukan Pada Sumber?", "id": "Sumber Listrik DC." },
  { "en": "Apa Itu Cycloconverter?", "id": "Pengubah Frekuensi AC AC." },
  { "en": "Aplikasi Cycloconverter Adalah?", "id": "Motor Besar Kecepatan Rendah." },
  { "en": "Apa Itu Matrix Converter?", "id": "Konverter AC AC Langsung." },
  { "en": "Kelebihan Matrix Converter?", "id": "Tanpa Kapasitor DC Link." },
  { "en": "Apa Itu Regenerative Drive?", "id": "Mengembalikan Energi Ke Jala." },
  { "en": "Lift Gedung Menggunakan Drive Jenis?", "id": "Regenerative Drive." },
  { "en": "Apa Kepanjangan BIL (Basic Insulation Level)?", "id": "Tingkat Isolasi Dasar." },
  { "en": "Apa Fungsi Uji Tan Delta Pada Trafo?", "id": "Mengetahui Kualitas Isolasi." },
  { "en": "Nilai Tan Delta Yang Baik Adalah?", "id": "Mendekati Angka 0." },
  { "en": "Apa Itu Vector Group Trafo?", "id": "Hubungan Fasa Primer Sekunder." },
  { "en": "Kode Dyn5 Pada Trafo Artinya?", "id": "Delta Star Netral Geser 150." },
  { "en": "Apa Itu Inrush Current Trafo?", "id": "Arus Kejut Saat Trafo Dinyalakan." },
  { "en": "Arus Inrush Bisa Mencapai Berapa Kali Nominal?", "id": "8 Sampai 12 Kali." },
  { "en": "Apa Itu Polarity Test Trafo?", "id": "Menentukan Kutub Lilitan Trafo." },
  { "en": "Apa Itu Efek Ferranti Pada Transmisi?", "id": "Tegangan Ujung Terima Lebih Tinggi." },
  { "en": "Efek Ferranti Terjadi Pada Kondisi Apa?", "id": "Beban Ringan Atau Kosong." },
  { "en": "Apa Fungsi Reaktor Shunt?", "id": "Menurunkan Tegangan Lebih." },
  { "en": "Apa Fungsi Kapasitor Seri Pada Transmisi?", "id": "Mengurangi Jatuh Tegangan Saluran." },
  { "en": "Apa Itu Transposisi Kabel Transmisi?", "id": "Pertukaran Posisi Fasa Kabel." },
  { "en": "Tujuan Transposisi Kabel Adalah?", "id": "Menyeimbangkan Impedansi Saluran." },
  { "en": "Apa Itu Busbar Ganda (Double Busbar)?", "id": "Dua Rel Daya Fleksibel." },
  { "en": "Apa Kelebihan Sistem Double Busbar?", "id": "Perbaikan Tanpa Padam Total." },
  { "en": "Apa Itu Interlock System?", "id": "Mencegah Kesalahan Operasi Urutan." },
  { "en": "Apa Itu Ferroresonance Pada Trafo Tegangan?", "id": "Osilasi Tegangan Berbahaya." },
  { "en": "Apa Itu Arester Kelas Distribusi?", "id": "Melindungi Trafo Tiang." },
  { "en": "Apa Itu Arester Kelas Station?", "id": "Melindungi Gardu Induk." },
  { "en": "Komponen Utama Arester Adalah?", "id": "Varistor Oksida Logam ZnO." },
  { "en": "Apa Sifat Varistor ZnO?", "id": "Non Linear Resistor." },
  { "en": "Pada Tegangan Normal Tahanan Arester Adalah?", "id": "Sangat Tinggi Isolator." },
  { "en": "Saat Ada Petir Tahanan Arester Menjadi?", "id": "Sangat Rendah Konduktor." },
  { "en": "Apa Itu Counterpoise Grounding?", "id": "Kawat Tanah Paralel Transmisi." },
  { "en": "Fungsi Counterpoise Adalah?", "id": "Menurunkan Tahanan Kaki Menara." },
  { "en": "Apa Itu Span Kawat?", "id": "Jarak Antar Dua Tiang." },
  { "en": "Jarak Bebas SUTM 20kV Adalah?", "id": "Minimal 2,5 Meter." },
  { "en": "Jarak Bebas SUTET 500kV Adalah?", "id": "Minimal 8,5 Meter." },
  { "en": "Apa Itu Right Of Way (ROW)?", "id": "Ruang Bebas Jalur Transmisi." },
  { "en": "Pohon Di Bawah SUTET Harus?", "id": "Ditebang Atau Dipangkas." },
  { "en": "Apa Itu Load Shedding?", "id": "Pelepasan Beban Bergilir." },
  { "en": "Kapan Load Shedding Dilakukan?", "id": "Daya Pembangkit Kurang." },
  { "en": "Relay UFR (Under Frequency Relay) Bekerja Saat?", "id": "Frekuensi Turun Drastis." },
  { "en": "Penurunan Frekuensi Disebabkan Oleh?", "id": "Beban Melebihi Pembangkitan." },
  { "en": "Kenaikan Frekuensi Disebabkan Oleh?", "id": "Pembangkitan Melebihi Beban." },
  { "en": "Apa Itu Spinning Reserve?", "id": "Cadangan Daya Berputar." },
  { "en": "Apa Itu Black Start?", "id": "Start Awal Tanpa Listrik Luar." },
  { "en": "Pembangkit Black Start Biasanya Jenis?", "id": "PLTA Atau PLTG." },
  { "en": "Apa Itu Island Operation?", "id": "Beroperasi Terpisah Dari Grid." },
  { "en": "Apa Itu AVR (Automatic Voltage Regulator)?", "id": "Pengatur Tegangan Generator Otomatis." },
  { "en": "AVR Mengatur Tegangan Dengan Mengubah?", "id": "Arus Eksitasi Rotor." },
  { "en": "Apa Itu Exciter Brushless?", "id": "Penguat Medan Tanpa Sikat." },
  { "en": "Komponen Penyearah Pada Exciter Brushless Adalah?", "id": "Dioda Putar Rotating Diode." },
  { "en": "Apa Itu PMG (Permanent Magnet Generator)?", "id": "Sumber Daya Pilot Exciter." },
  { "en": "Apa Fungsi PMG Pada Generator Besar?", "id": "Sumber Listrik AVR Stabil." },
  { "en": "Apa Itu Droop Speed Control?", "id": "Pengaturan Kecepatan Berbeban." },
  { "en": "Generator Paralel Harus Memiliki?", "id": "Karakteristik Droop Yang Sama." },
  { "en": "Apa Itu Hunting Pada Generator?", "id": "Osilasi Putaran Rotor." },
  { "en": "Damper Winding Berfungsi Untuk?", "id": "Meredam Efek Hunting." },
  { "en": "Letak Damper Winding Ada Di Mana?", "id": "Wajah Kutub Rotor." },
  { "en": "Apa Itu Armature Reaction?", "id": "Pengaruh Medan Stator Ke Rotor." },
  { "en": "Apa Itu Power System Stabilizer (PSS)?", "id": "Alat Peredam Osilasi Daya." },
  { "en": "Apa Itu Faktor Kebersamaan (Coincidence Factor)?", "id": "Rasio Beban Puncak Maksimum." },
  { "en": "Apa Itu Faktor Beban (Load Factor)?", "id": "Rasio Rata Rata Ke Puncak." },
  { "en": "Apa Itu Demand Factor?", "id": "Rasio Maksimum Ke Terpasang." },
  { "en": "Apa Itu Base Load Power Plant?", "id": "Pembangkit Beban Dasar." },
  { "en": "Contoh Pembangkit Base Load Adalah?", "id": "PLTU Batubara." },
  { "en": "Apa Itu Peaker Power Plant?", "id": "Pembangkit Beban Puncak." },
  { "en": "Contoh Pembangkit Peaker Adalah?", "id": "PLTG Gas." },
  { "en": "Waktu Start PLTG Adalah?", "id": "Sangat Cepat." },
  { "en": "Waktu Start PLTU Adalah?", "id": "Sangat Lama Berjam Jam." },
  { "en": "Apa Itu Thermography Panel?", "id": "Foto Suhu Panel Listrik." },
  { "en": "Hotspot Pada Kabel Disebabkan Oleh?", "id": "Baut Kendor Atau Korosi." },
  { "en": "Apa Itu Vibrometer?", "id": "Alat Ukur Getaran Motor." },
  { "en": "Satuan Level Getaran Adalah?", "id": "mm Per Detik." },
  { "en": "Getaran Tinggi Pada Motor Menandakan?", "id": "Unbalance Atau Misalignment." },
  { "en": "Apa Itu Misalignment Poros?", "id": "Poros Tidak Segaris Lurus." },
  { "en": "Alat Untuk Alignment Poros Adalah?", "id": "Dial Indicator Atau Laser." },
  { "en": "Apa Itu Unbalance Rotor?", "id": "Berat Rotor Tidak Seimbang." },
  { "en": "Apa Itu Soft Foot Pada Motor?", "id": "Kaki Motor Tidak Rata." },
  { "en": "Apa Itu Greasing Bearing?", "id": "Pelumasan Bantalan Motor." },
  { "en": "Akibat Kelebihan Grease Adalah?", "id": "Suhu Bearing Naik." },
  { "en": "Akibat Kekurangan Grease Adalah?", "id": "Bearing Aus Dan Bunyi." },
  { "en": "Apa Itu Shielded Bearing?", "id": "Bearing Tutup Pelat Logam." },
  { "en": "Apa Itu Sealed Bearing?", "id": "Bearing Tutup Karet Rapat." },
  { "en": "Apa Kode Bearing C3 Artinya?", "id": "Celah Internal Lebih Longgar." },
  { "en": "Kapan Bearing C3 Digunakan?", "id": "Suhu Operasi Tinggi." },
  { "en": "Apa Itu Insulation Class B?", "id": "Tahan 130 Derajat Celcius." },
  { "en": "Apa Itu Service Factor Motor?", "id": "Kemampuan Beban Lebih Sesaat." },
  { "en": "Service Factor 1.15 Artinya?", "id": "Bisa Lebih 15 Persen." },
  { "en": "Apa Itu Frame Size Motor?", "id": "Ukuran Dimensi Standar Motor." },
  { "en": "Jarak Tinggi Poros Ke Kaki Disebut?", "id": "Shaft Height." },
  { "en": "Apa Itu Flange Mounting Motor?", "id": "Pemasangan Muka Tanpa Kaki." },
  { "en": "Apa Itu Foot Mounting Motor?", "id": "Pemasangan Dengan Kaki." },
  { "en": "Kode Mounting B3 Adalah?", "id": "Motor Kaki Horizontal." },
  { "en": "Kode Mounting B5 Adalah?", "id": "Motor Flange Tanpa Kaki." },
  { "en": "Apa Itu Duty Cycle S1 Motor?", "id": "Kerja Terus Menerus." },
  { "en": "Apa Itu Duty Cycle S2 Motor?", "id": "Kerja Waktu Pendek." },
  { "en": "Motor Crane Biasanya Duty Cycle?", "id": "S3 Intermittent Periodic." },
  { "en": "Apa Itu Heater Motor (Space Heater)?", "id": "Pemanas Anti Embun." },
  { "en": "Kapan Space Heater Dinyalakan?", "id": "Saat Motor Mati." },
  { "en": "Tujuan Space Heater Adalah?", "id": "Menjaga Tahanan Isolasi." },
  { "en": "Tahanan Isolasi Minimal Motor 380V?", "id": "Minimal 0,38 Mega Ohm." },
  { "en": "Rumus Praktis Minimal Isolasi Adalah?", "id": "1000 Ohm Per Volt." },
  { "en": "Apa Itu IPW (Indeks Polarisasi)?", "id": "Rasio Isolasi 10 Dan 1 Menit." },
  { "en": "Nilai IP (Indeks Polarisasi) Yang Baik?", "id": "Di Atas Angka 2." },
  { "en": "Apa Itu Hi Pot Test?", "id": "Uji Tahan Tegangan Tinggi." },
  { "en": "Tegangan Uji Hi Pot Biasanya?", "id": "2 Kali Tegangan Tambah 1000." },
  { "en": "Hi Pot Test Bersifat?", "id": "Destructive Test." },
  { "en": "Apa Itu Gerber File Pada PCB?", "id": "File Data Cetak PCB." },
  { "en": "Apa Fungsi DRC (Design Rule Check)?", "id": "Memeriksa Kesalahan Desain PCB." },
  { "en": "Apa Itu V-Cut Pada PCB?", "id": "Garis Potong Patah PCB." },
  { "en": "Apa Itu Mouse Bites Pada PCB?", "id": "Lubang Kecil Pemisah PCB." },
  { "en": "Apa Itu Stencil PCB?", "id": "Cetakan Pasta Solder SMD." },
  { "en": "Mesin Pick And Place Berfungsi Untuk?", "id": "Memasang Komponen SMD Otomatis." },
  { "en": "Apa Itu Reflow Oven?", "id": "Pemanas Solder Pasta SMD." },
  { "en": "Apa Itu Wave Soldering?", "id": "Solder Gelombang Komponen THT." },
  { "en": "Apa Itu Flux Remover?", "id": "Cairan Pembersih Sisa Solder." },
  { "en": "Lapisan Conformal Coating Berfungsi Untuk?", "id": "Melindungi PCB Dari Lembab." },
  { "en": "Sensor DHT11 Mengukur Apa Saja?", "id": "Suhu Dan Kelembaban Udara." },
  { "en": "Sensor DHT22 Lebih Unggul Dalam?", "id": "Akurasi Dan Rentang Ukur." },
  { "en": "Sensor DS18B20 Adalah Sensor Apa?", "id": "Sensor Suhu Tahan Air." },
  { "en": "Sensor MQ-2 Mendeteksi Gas Apa?", "id": "Asap Dan Gas LPG." },
  { "en": "Sensor MQ-7 Mendeteksi Gas Apa?", "id": "Gas Karbon Monoksida." },
  { "en": "Sensor HC-SR04 Menggunakan Gelombang Apa?", "id": "Gelombang Ultrasonik." },
  { "en": "Prinsip Kerja Sensor HC-SR04 Adalah?", "id": "Pantulan Gelombang Suara." },
  { "en": "Modul HX711 Digunakan Bersama Sensor?", "id": "Load Cell Timbangan." },
  { "en": "Apa Fungsi Modul RTC DS3231?", "id": "Jam Digital Akurasi Tinggi." },
  { "en": "Modul Relay 5V Aktif Low Artinya?", "id": "Nyala Saat Sinyal 0V." },
  { "en": "Modul Relay 5V Aktif High Artinya?", "id": "Nyala Saat Sinyal 5V." },
  { "en": "Resolusi ADC Arduino Uno Adalah?", "id": "10 Bit." },
  { "en": "Nilai Maksimal Pembacaan Analog Arduino?", "id": "1023." },
  { "en": "Tegangan Referensi Default ADC Arduino?", "id": "5 Volt." },
  { "en": "Resolusi PWM Arduino Uno Adalah?", "id": "8 Bit." },
  { "en": "Nilai Maksimal Output PWM Arduino?", "id": "255." },
  { "en": "Apa Fungsi Perintah DigitalWrite?", "id": "Mengeluarkan Logika High Low." },
  { "en": "Apa Fungsi Perintah DigitalRead?", "id": "Membaca Logika Input Pin." },
  { "en": "Apa Fungsi Perintah AnalogRead?", "id": "Membaca Tegangan Pin Analog." },
  { "en": "Apa Fungsi Perintah AnalogWrite?", "id": "Mengeluarkan Sinyal PWM." },
  { "en": "Apa Fungsi Serial Monitor Arduino?", "id": "Menampilkan Data Ke Komputer." },
  { "en": "Kecepatan Serial Komunikasi Disebut?", "id": "Baud Rate." },
  { "en": "Baud Rate Standar Arduino Adalah?", "id": "9600 bps." },
  { "en": "Apa Itu SPI (Serial Peripheral Interface)?", "id": "Komunikasi Serial Sinkron Cepat." },
  { "en": "Jalur Data SPI Terdiri Dari?", "id": "MISO MOSI SCK SS." },
  { "en": "Apa Kepanjangan MOSI?", "id": "Master Out Slave In." },
  { "en": "Apa Kepanjangan MISO?", "id": "Master In Slave Out." },
  { "en": "Apa Kepanjangan SCK Pada SPI?", "id": "Serial Clock." },
  { "en": "Apa Kepanjangan SS Pada SPI?", "id": "Slave Select." },
  { "en": "Apa Itu Antena Dipole?", "id": "Antena Dua Kutub Lurus." },
  { "en": "Pola Radiasi Antena Dipole Adalah?", "id": "Menyebar Samping Angka 8." },
  { "en": "Apa Itu Antena Omni Directional?", "id": "Memancar Ke Segala Arah." },
  { "en": "Pola Radiasi Antena Omni Adalah?", "id": "Lingkaran 360 Derajat." },
  { "en": "Apa Itu Antena Yagi?", "id": "Antena Pengarah Tulang Ikan." },
  { "en": "Apa Itu Antena Grid Parabolik?", "id": "Antena Jaring Jarak Jauh." },
  { "en": "Apa Fungsi Balun Pada Antena?", "id": "Penyesuai Impedansi Seimbang." },
  { "en": "Konektor SMA Male Memiliki?", "id": "Jarum Di Tengah." },
  { "en": "Konektor SMA Female Memiliki?", "id": "Lubang Di Tengah." },
  { "en": "Impedansi Konektor BNC Adalah?", "id": "50 Ohm Atau 75 Ohm." },
  { "en": "Frekuensi Radio VHF Berkisar?", "id": "30 Sampai 300 MHz." },
  { "en": "Frekuensi Radio UHF Berkisar?", "id": "300 MHz Sampai 3 GHz." },
  { "en": "Apa Itu Fading Sinyal Radio?", "id": "Pelemahan Sinyal Naik Turun." },
  { "en": "Apa Itu Line Of Sight (LOS)?", "id": "Jalur Pandang Tanpa Halangan." },
  { "en": "Apa Itu Zona Fresnel?", "id": "Area Ellips Jalur Sinyal." },
  { "en": "Hukum Kirchhoff Tegangan (KVL) Digunakan Pada?", "id": "Analisis Mesh Loop." },
  { "en": "Hukum Kirchhoff Arus (KCL) Digunakan Pada?", "id": "Analisis Nodal Simpul." },
  { "en": "Jembatan Wheatstone Seimbang Jika Galvanometer?", "id": "Menunjuk Angka 0." },
  { "en": "Teorema Transfer Daya Maksimum Terjadi Saat?", "id": "R Beban Sama Dengan R Sumber." },
  { "en": "Rumus Daya Maksimum Transfer Adalah?", "id": "V Kuadrat Bagi 4R." },
  { "en": "Rangkaian Integrator Op Amp Menggunakan?", "id": "Kapasitor Di Umpan Balik." },
  { "en": "Rangkaian Diferensiator Op Amp Menggunakan?", "id": "Kapasitor Di Input." },
  { "en": "Apa Itu CMRR Op Amp?", "id": "Rasio Penolakan Mode Bersama." },
  { "en": "Nilai CMRR Yang Bagus Adalah?", "id": "Sangat Tinggi." },
  { "en": "Apa Itu Virtual Ground Op Amp?", "id": "Titik Ground Semu." },
  { "en": "Timah Solder 60/40 Artinya?", "id": "60 Timah 40 Timbal." },
  { "en": "Timah Solder Lead Free Artinya?", "id": "Bebas Timbal." },
  { "en": "Titik Leleh Timah Lead Free Adalah?", "id": "Lebih Tinggi Dari Biasa." },
  { "en": "Bahaya Uap Solder Adalah?", "id": "Mengandung Logam Berat Beracun." },
  { "en": "Apa Fungsi Exhaust Fan Solder?", "id": "Menghisap Asap Solder." },
  { "en": "Apa Itu Solder Station?", "id": "Solder Dengan Pengatur Suhu." },
  { "en": "Apa Itu Hot Air Gun Solder?", "id": "Pemanas Udara Untuk SMD." },
  { "en": "Solder Uap Biasa Digunakan Untuk?", "id": "Melepas IC Kaki Banyak." },
  { "en": "Pinset Antistatis Terbuat Dari?", "id": "Besi Atau Plastik ESD." },
  { "en": "Obeng Trimpot Biasanya Berbahan?", "id": "Plastik Atau Keramik." },
  { "en": "Mengapa Obeng Trimpot Bukan Logam?", "id": "Agar Tidak Mengganggu Induksi." },
  { "en": "Apa Itu Tang Potong Micro?", "id": "Pemotong Kaki Komponen Kecil." },
  { "en": "Apa Itu Kaca Pembesar Lampu Servis?", "id": "Melihat Jalur PCB Kecil." },
  { "en": "Mikroskop Digital Digunakan Untuk?", "id": "Inspeksi Solderan SMD." },
  { "en": "Apa Fungsi Dummy Load?", "id": "Beban Tiruan Pengujian." },
  { "en": "Dummy Load Antena Mengubah Daya Menjadi?", "id": "Panas." },
  { "en": "Apa Itu Audio Generator?", "id": "Pembangkit Sinyal Suara." },
  { "en": "Apa Itu Function Generator?", "id": "Pembangkit Gelombang Sinus Kotak." },
  { "en": "Apa Itu Frequency Counter?", "id": "Penghitung Frekuensi Sinyal." },
  { "en": "Apa Itu Spectrum Analyzer?", "id": "Analisis Kekuatan Sinyal Frekuensi." },
  { "en": "Spectrum Analyzer Menampilkan Grafik?", "id": "Amplitudo Terhadap Frekuensi." },
  { "en": "Osiloskop Menampilkan Grafik?", "id": "Tegangan Terhadap Waktu." },
  { "en": "Apa Itu Logic Analyzer?", "id": "Menganalisis Banyak Sinyal Digital." },
  { "en": "Apa Itu Protokol CAN Bus?", "id": "Komunikasi Data Otomotif Mobil." },
  { "en": "Kabel CAN Bus Terdiri Dari?", "id": "CAN High Dan CAN Low." },
  { "en": "Resistor Terminasi CAN Bus Bernilai?", "id": "120 Ohm." },
  { "en": "Apa Itu ECU Mapping?", "id": "Mengatur Ulang Program Mesin." },
  { "en": "Apa Itu Piggyback ECU?", "id": "Manipulasi Sinyal Ke ECU." },
  { "en": "Sensor MAP Mengukur Apa?", "id": "Tekanan Udara Manifold." },
  { "en": "Sensor MAF Mengukur Apa?", "id": "Massa Aliran Udara." },
  { "en": "Sensor TPS (Throttle Position Sensor) Mengukur?", "id": "Bukaan Gas." },
  { "en": "Sensor O2 (Oxygen Sensor) Mengukur?", "id": "Sisa Oksigen Gas Buang." },
  { "en": "Sensor Knocking Mendeteksi Apa?", "id": "Getaran Ledakan Mesin." },
  { "en": "Apa Itu Injektor Bahan Bakar?", "id": "Semprotan Bensin Elektrik." },
  { "en": "Injektor Dikendalikan Oleh Apa?", "id": "Pulsa Listrik ECU." },
  { "en": "Apa Kepanjangan CRI (Color Rendering Index)?", "id": "Indeks Kesesuaian Warna Cahaya." },
  { "en": "Nilai CRI Sempurna Matahari Adalah?", "id": "100." },
  { "en": "Lampu Jalan Biasanya Menggunakan Jenis?", "id": "Lampu Sodium Tekanan Tinggi." },
  { "en": "Ciri Khas Cahaya Lampu Sodium Adalah?", "id": "Warna Kuning Pekat." },
  { "en": "Apa Itu Lux Meter?", "id": "Alat Ukur Intensitas Cahaya." },
  { "en": "Apa Perbedaan Lumen Dan Lux?", "id": "Lumen Sumber Lux Diterima." },
  { "en": "Standar Pencahayaan Ruang Kantor Adalah?", "id": "300 Sampai 500 Lux." },
  { "en": "Apa Itu Emergency Lamp?", "id": "Lampu Darurat Saat Padam." },
  { "en": "Baterai Nicd Memiliki Kelemahan Apa?", "id": "Efek Memori Yang Kuat." },
  { "en": "Baterai Nimh Lebih Ramah Lingkungan Karena?", "id": "Tidak Mengandung Kadmium Beracun." },
  { "en": "Apa Itu Self Discharge Baterai?", "id": "Daya Berkurang Saat Disimpan." },
  { "en": "Baterai Eneloop Terkenal Karena?", "id": "Low Self Discharge." },
  { "en": "Apa Itu Fast Charging?", "id": "Pengisian Arus Besar Cepat." },
  { "en": "Kabel Data Fast Charging Harus?", "id": "Mampu Mengalirkan Arus Besar." },
  { "en": "Apa Itu Protokol Quick Charge?", "id": "Teknologi Pengisian Cepat Qualcomm." },
  { "en": "Apa Itu USB Power Delivery (PD)?", "id": "Standar Daya USB Tipe C." },
  { "en": "Tegangan Maksimal USB PD Adalah?", "id": "20 Volt Atau 48 Volt." },
  { "en": "Daya Maksimal USB PD Standar Awal?", "id": "100 Watt." },
  { "en": "Apa Fungsi Ferrite Core Di Kabel USB?", "id": "Meredam Gangguan Sinyal." },
  { "en": "Apa Itu Kabel Shielded?", "id": "Kabel Dengan Lapisan Pelindung." },
  { "en": "Apa Itu Profibus Dalam Industri?", "id": "Standar Komunikasi Fieldbus Siemens." },
  { "en": "Kabel Profibus DP Berwarna Apa?", "id": "Ungu." },
  { "en": "Konektor Profibus Biasanya Menggunakan?", "id": "DB9." },
  { "en": "Apa Itu Profinet?", "id": "Standar Ethernet Industri Siemens." },
  { "en": "Kabel Profinet Berwarna Apa?", "id": "Hijau." },
  { "en": "Konektor Profinet Menggunakan Jenis?", "id": "RJ45." },
  { "en": "Apa Kelebihan Ethernet Industri?", "id": "Kecepatan Data Tinggi." },
  { "en": "Apa Itu Switch Unmanaged?", "id": "Switch Tanpa Konfigurasi." },
  { "en": "Apa Itu Switch Managed?", "id": "Switch Bisa Dikonfigurasi." },
  { "en": "Apa Itu VLAN (Virtual LAN)?", "id": "Jaringan Virtual Terpisah." },
  { "en": "Apa Itu Redundansi Jaringan?", "id": "Jalur Cadangan Jika Putus." },
  { "en": "Protokol STP (Spanning Tree Protocol) Berfungsi?", "id": "Mencegah Looping Jaringan." },
  { "en": "Apa Itu Mac Address?", "id": "Alamat Fisik Perangkat Jaringan." },
  { "en": "Berapa Bit Panjang Mac Address?", "id": "48 Bit." },
  { "en": "Apa Itu Subnet Mask?", "id": "Pemisah Network Dan Host." },
  { "en": "Subnet Mask 255.255.255.0 Prefixnya?", "id": "Garing 24." },
  { "en": "Apa Itu Gateway Dalam Jaringan?", "id": "Gerbang Keluar Jaringan Lokal." },
  { "en": "Apa Itu DNS (Domain Name System)?", "id": "Penerjemah Nama Ke IP." },
  { "en": "Apa Itu Stall Torque Motor?", "id": "Torsi Saat Poros Ditahan." },
  { "en": "Saat Stall Current Arus Menjadi?", "id": "Sangat Tinggi Maksimal." },
  { "en": "Apa Itu No Load Speed?", "id": "Kecepatan Tanpa Beban." },
  { "en": "Apa Itu Rated Speed Motor?", "id": "Kecepatan Pada Beban Penuh." },
  { "en": "Rated Speed Selalu Lebih Rendah Dari?", "id": "Kecepatan Sinkron." },
  { "en": "Apa Itu Breakdown Torque?", "id": "Torsi Maksimum Sebelum Berhenti." },
  { "en": "Apa Itu Locked Rotor Current?", "id": "Arus Saat Rotor Terkunci." },
  { "en": "Berapa Kali Arus Locked Rotor?", "id": "6 Sampai 7 Kali Nominal." },
  { "en": "Apa Fungsi Thermal Protector Motor?", "id": "Memutus Saat Gulungan Panas." },
  { "en": "Sensor Vibration Probe Mengukur?", "id": "Amplitudo Getaran Mesin." },
  { "en": "Satuan Getaran Displacement Adalah?", "id": "Mikron." },
  { "en": "Satuan Getaran Velocity Adalah?", "id": "Milimeter Per Detik." },
  { "en": "Satuan Getaran Acceleration Adalah?", "id": "G Force." },
  { "en": "Apa Itu FFT Spectrum Getaran?", "id": "Grafik Frekuensi Getaran." },
  { "en": "Getaran Frekuensi 1x RPM Menandakan?", "id": "Masalah Unbalance." },
  { "en": "Getaran Frekuensi 2x RPM Menandakan?", "id": "Masalah Misalignment." },
  { "en": "Getaran Frekuensi Tinggi Menandakan?", "id": "Kerusakan Bearing Atau Gear." },
  { "en": "Apa Itu Stroboscope?", "id": "Lampu Kilat Pengukur Putaran." },
  { "en": "Stroboscope Membuat Benda Berputar Terlihat?", "id": "Diam Atau Berhenti." },
  { "en": "Apa Itu Tachometer Kontak?", "id": "Mengukur Putaran Dengan Sentuhan." },
  { "en": "Apa Itu Tachometer Laser?", "id": "Mengukur Putaran Tanpa Sentuh." },
  { "en": "Tachometer Laser Membutuhkan Apa?", "id": "Stiker Reflektor Pantul." },
  { "en": "Apa Itu Megger Test 1 Menit?", "id": "Uji Tahanan Isolasi Standar." },
  { "en": "Apa Itu DAR (Dielectric Absorption Ratio)?", "id": "Rasio Isolasi 60 Dan 30 Detik." },
  { "en": "Nilai DAR Yang Baik Adalah?", "id": "Di Atas 1,25." },
  { "en": "Apa Itu Step Voltage Test?", "id": "Uji Isolasi Tegangan Bertahap." },
  { "en": "Jika Arus Bocor Naik Cepat Artinya?", "id": "Isolasi Rusak Atau Kotor." },
  { "en": "Apa Itu Tan Delta Test?", "id": "Mengukur Faktor Daya Isolasi." },
  { "en": "Kenaikan Nilai Tan Delta Menandakan?", "id": "Penuaan Isolasi Atau Air." },
  { "en": "Apa Itu SFRA (Sweep Frequency Response Analysis)?", "id": "Mendeteksi Pergeseran Gulungan Trafo." },
  { "en": "SFRA Membandingkan Grafik Dengan?", "id": "Data Awal Pabrik." },
  { "en": "Apa Itu TTR (Transformer Turns Ratio)?", "id": "Alat Ukur Rasio Belitan." },
  { "en": "Hasil TTR Berbeda Jauh Menandakan?", "id": "Hubung Singkat Antar Lilitan." },
  { "en": "Toleransi Perbedaan Rasio TTR Adalah?", "id": "0,5 Persen." },
  { "en": "Apa Itu Winding Resistance Test?", "id": "Uji Tahanan Gulungan DC." },
  { "en": "Apa Fungsi Uji Tahanan Kontak?", "id": "Cek Koneksi Sambungan Breaker." },
  { "en": "Satuan Tahanan Kontak Breaker Adalah?", "id": "Mikro Ohm." },
  { "en": "Alat Ukur Tahanan Kontak Disebut?", "id": "Micro Ohm Meter." },
  { "en": "Micro Ohm Meter Menggunakan Arus?", "id": "Arus DC Besar." },
  { "en": "Minimal Arus Uji Tahanan Kontak?", "id": "100 Ampere DC." },
  { "en": "Apa Itu CB Analyzer?", "id": "Analisis Waktu Buka Tutup Breaker." },
  { "en": "Apa Itu Close Time Breaker?", "id": "Waktu Kontak Menutup." },
  { "en": "Apa Itu Trip Time Breaker?", "id": "Waktu Kontak Membuka." },
  { "en": "Apa Itu Keserempakan Kontak?", "id": "Selisih Waktu Antar Fasa." },
  { "en": "Batas Toleransi Keserempakan Biasanya?", "id": "Di Bawah 10 Milidetik." },
  { "en": "Apa Itu Charging Spring Motor?", "id": "Motor Pengisi Pegas Breaker." },
  { "en": "Apa Tanda Pegas Breaker Penuh?", "id": "Indikator Charged Kuning." },
  { "en": "Apa Fungsi Coil Shunt Trip?", "id": "Membuka Breaker Secara Elektrik." },
  { "en": "Apa Fungsi Coil Closing?", "id": "Menutup Breaker Secara Elektrik." },
  { "en": "Apa Fungsi Coil Under Voltage?", "id": "Trip Jika Tegangan Hilang." },
  { "en": "Apa Itu Rack In Breaker?", "id": "Memasukkan Breaker Ke Posisi Kerja." },
  { "en": "Apa Itu Rack Out Breaker?", "id": "Mengeluarkan Breaker Ke Posisi Test." },
  { "en": "Posisi Test Pada Breaker Artinya?", "id": "Kontrol Hidup Daya Mati." },
  { "en": "Posisi Service Pada Breaker Artinya?", "id": "Terhubung Penuh Ke Busbar." },
  { "en": "Apa Itu Arc Chute?", "id": "Peredam Busur Api Breaker." },
  { "en": "Arc Chute Berisi Pelat Apa?", "id": "Pelat Logam Pembelah Api." },
  { "en": "Apa Itu Main Contact Breaker?", "id": "Kontak Utama Arus Besar." },
  { "en": "Apa Itu Arcing Contact?", "id": "Kontak Tumbal Loncat Api." },
  { "en": "Arcing Contact Terbuat Dari?", "id": "Tungsten Atau Tembaga Keras." },
  { "en": "Apa Itu EVSE (Electric Vehicle Supply Equipment)?", "id": "Peralatan Suplai Kendaraan Listrik." },
  { "en": "Apa Fungsi OBC (On Board Charger) Mobil?", "id": "Mengubah AC Menjadi DC Baterai." },
  { "en": "Konektor Charging Tipe 1 Standar Mana?", "id": "Amerika Dan Jepang." },
  { "en": "Konektor Charging Tipe 2 Standar Mana?", "id": "Eropa Dan Indonesia." },
  { "en": "Apa Itu Konektor CCS (Combined Charging System)?", "id": "Kombinasi AC Dan DC Fast." },
  { "en": "Apa Itu Konektor CHAdeMO?", "id": "Standar DC Charging Jepang." },
  { "en": "Apa Itu Wallbox Charger?", "id": "Pengisi Daya Terpasang Di Dinding." },
  { "en": "Charging Mode 2 Menggunakan Kabel Apa?", "id": "Kabel Bawaan Dengan Control Box." },
  { "en": "Charging Mode 3 Menggunakan Apa?", "id": "Stasiun Pengisian Khusus AC." },
  { "en": "Charging Mode 4 Menggunakan Apa?", "id": "DC Fast Charging Station." },
  { "en": "Apa Itu SOC (State Of Charge)?", "id": "Persentase Sisa Baterai." },
  { "en": "Apa Itu SOH (State Of Health)?", "id": "Kondisi Kesehatan Baterai." },
  { "en": "Baterai EV Menggunakan Pendingin Apa?", "id": "Cairan Coolant Atau Udara." },
  { "en": "Apa Tegangan Baterai Mobil Listrik Umumnya?", "id": "400 Volt Sampai 800 Volt." },
  { "en": "Apa Itu LVDT (Linear Variable Differential Transformer)?", "id": "Sensor Posisi Gerak Linear." },
  { "en": "Prinsip Kerja LVDT Berdasarkan Apa?", "id": "Induksi Elektromagnetik Inti Bergerak." },
  { "en": "Apa Fungsi Flow Meter Magnetik?", "id": "Mengukur Aliran Cairan Konduktif." },
  { "en": "Flow Meter Magnetik Menggunakan Hukum?", "id": "Hukum Faraday." },
  { "en": "Apa Fungsi Flow Meter Ultrasonic?", "id": "Mengukur Aliran Dengan Gelombang Suara." },
  { "en": "Apa Itu Vortex Flow Meter?", "id": "Mengukur Pusaran Aliran Fluida." },
  { "en": "Sensor Tekanan Bourdon Tube Berbentuk?", "id": "Pipa Melengkung Seperti C." },
  { "en": "Apa Itu I/P Converter?", "id": "Pengubah Arus Ke Tekanan Angin." },
  { "en": "Sinyal Input I/P Converter Biasanya?", "id": "4 Sampai 20 mA." },
  { "en": "Sinyal Output I/P Converter Biasanya?", "id": "3 Sampai 15 PSI." },
  { "en": "Apa Itu Control Valve (Katup Kendali)?", "id": "Elemen Akhir Pengatur Aliran." },
  { "en": "Apa Itu Valve Positioner?", "id": "Pengatur Bukaan Katup Presisi." },
  { "en": "Jenis Katup Butterfly Valve Bentuknya?", "id": "Piringan Berputar." },
  { "en": "Jenis Katup Ball Valve Bentuknya?", "id": "Bola Berlubang." },
  { "en": "Jenis Katup Globe Valve Digunakan Untuk?", "id": "Mengatur Debit Aliran Halus." },
  { "en": "Apa Itu Fail Safe Open?", "id": "Terbuka Saat Listrik/Angin Mati." },
  { "en": "Apa Itu Fail Safe Close?", "id": "Tertutup Saat Listrik/Angin Mati." },
  { "en": "Apa Itu Protokol Zigbee?", "id": "Komunikasi Nirkabel Hemat Daya." },
  { "en": "Zigbee Bekerja Pada Frekuensi?", "id": "2,4 GHz." },
  { "en": "Topologi Jaringan Zigbee Adalah?", "id": "Topologi Mesh." },
  { "en": "Apa Itu Protokol Z Wave?", "id": "Komunikasi Rumah Pintar Gelombang Rendah." },
  { "en": "Apa Itu Protokol MQTT?", "id": "Komunikasi IoT Ringan Berbasis Topik." },
  { "en": "Perangkat Pengirim Data MQTT Disebut?", "id": "Publisher." },
  { "en": "Perangkat Penerima Data MQTT Disebut?", "id": "Subscriber." },
  { "en": "Server Pengatur Pesan MQTT Disebut?", "id": "Broker." },
  { "en": "Apa Itu Protokol KNX?", "id": "Standar Otomasi Gedung Kabel." },
  { "en": "Apa Itu DALI (Digital Addressable Lighting Interface)?", "id": "Protokol Kontrol Lampu Digital." },
  { "en": "Satu Jalur DALI Maksimal Berapa Lampu?", "id": "64 Perangkat." },
  { "en": "Tegangan Bus DALI Adalah?", "id": "Sekitar 16 Volt DC." },
  { "en": "Apa Itu Flyback Converter?", "id": "Topologi Power Supply Terisolasi." },
  { "en": "Komponen Utama Flyback Converter Adalah?", "id": "Trafo Flyback Dan MOSFET." },
  { "en": "Apa Itu Push Pull Converter?", "id": "Konverter Dengan Trafo Center Tap." },
  { "en": "Apa Itu H Bridge Driver?", "id": "Jembatan 4 Sakelar Pengendali Motor." },
  { "en": "Fungsi H Bridge Adalah?", "id": "Membalik Arah Putaran Motor DC." },
  { "en": "Apa Itu Dead Time Pada Inverter?", "id": "Jeda Waktu Antar Sakelar." },
  { "en": "Tujuan Dead Time Adalah?", "id": "Mencegah Hubung Singkat Atas Bawah." },
  { "en": "Apa Itu PWM Sinusoidal (SPWM)?", "id": "PWM Membentuk Gelombang Sinus." },
  { "en": "Apa Itu Filter LC Pada Inverter?", "id": "Menghaluskan PWM Menjadi Sinus." },
  { "en": "Apa Itu Galvanic Corrosion?", "id": "Korosi Dua Logam Berbeda." },
  { "en": "Cara Mencegah Galvanic Corrosion Adalah?", "id": "Hindari Kontak Langsung Logam Beda." },
  { "en": "Apa Itu IP Rating IP65?", "id": "Tahan Debu Dan Semprotan Air." },
  { "en": "Apa Itu IP Rating IP67?", "id": "Tahan Rendaman Air Sementara." },
  { "en": "Apa Itu IP Rating IP68?", "id": "Tahan Rendaman Air Terus Menerus." },
  { "en": "Apa Itu Explosion Proof (Ex)?", "id": "Tahan Ledakan Di Area Gas." },
  { "en": "Peralatan Ex Wajib Di Area Mana?", "id": "Area Kilang Minyak Dan Gas." },
  { "en": "Apa Itu Zona 0 Pada Area Berbahaya?", "id": "Gas Meledak Selalu Ada." },
  { "en": "Apa Itu Zona 1 Pada Area Berbahaya?", "id": "Gas Meledak Kadang Ada." },
  { "en": "Apa Itu Zona 2 Pada Area Berbahaya?", "id": "Gas Meledak Jarang Ada." },
  { "en": "Apa Itu Intrinsically Safe Barrier?", "id": "Pembatas Energi Ke Area Bahaya." },
  { "en": "Apa Itu Kabel XLPE?", "id": "Cross Linked Polyethylene." },
  { "en": "Kelebihan Isolasi XLPE Dibanding PVC?", "id": "Tahan Suhu Lebih Tinggi." },
  { "en": "Suhu Maksimal Operasi Kabel XLPE?", "id": "90 Derajat Celcius." },
  { "en": "Suhu Maksimal Operasi Kabel PVC?", "id": "70 Derajat Celcius." },
  { "en": "Apa Itu Bending Radius Kabel?", "id": "Radius Tekukan Kabel Minimal." },
  { "en": "Tekukan Kabel Terlalu Tajam Menyebabkan?", "id": "Isolasi Pecah Atau Putus." },
  { "en": "Apa Itu Cable Gland?", "id": "Pengikat Kabel Masuk Panel." },
  { "en": "Fungsi Cable Gland Adalah?", "id": "Menahan Kabel Dan Segel Air." },
  { "en": "Apa Itu Busbar Support?", "id": "Isolator Penyangga Busbar." },
  { "en": "Apa Itu Fish Tape (Kawat Pancing)?", "id": "Alat Tarik Kabel Dalam Pipa." },
  { "en": "Apa Itu Conduit Bender?", "id": "Alat Tekuk Pipa Besi." },
  { "en": "Apa Itu Load Center?", "id": "Panel Breaker Di Rumah." },
  { "en": "Apa Itu Main Lug Only (MLO)?", "id": "Panel Tanpa Breaker Utama." },
  { "en": "Apa Itu Main Breaker (MB)?", "id": "Panel Dengan Breaker Utama." },
  { "en": "Apa Itu Power Factor Correction (PFC)?", "id": "Perbaikan Faktor Daya." },
  { "en": "Apa Itu Active PFC Pada PSU Komputer?", "id": "Rangkaian Elektronik Koreksi Daya." },
  { "en": "Apa Itu Passive PFC?", "id": "Menggunakan Induktor Besar." },
  { "en": "PSU Tanpa PFC Memiliki Power Factor?", "id": "Rendah Sekitar 0,6." },
  { "en": "Apa Itu Efisiensi 80 Plus Bronze?", "id": "Efisiensi Di Atas 82 Persen." },
  { "en": "Apa Itu Efisiensi 80 Plus Gold?", "id": "Efisiensi Di Atas 87 Persen." },
  { "en": "Apa Itu Efisiensi 80 Plus Platinum?", "id": "Efisiensi Di Atas 90 Persen." },
  { "en": "Apa Itu Rail 12V Pada PSU?", "id": "Jalur Tegangan Utama Komputer." },
  { "en": "Single Rail Vs Multi Rail Bagus Mana?", "id": "Tergantung Kebutuhan Proteksi Arus." },
  { "en": "Apa Itu Modular PSU?", "id": "Kabel Bisa Dilepas Pasang." },
  { "en": "Apa Itu UPS Line Interactive?", "id": "Ada AVR Stabilizer Di Dalamnya." },
  { "en": "Apa Itu UPS Offline (Standby)?", "id": "Pindah Baterai Saat Mati Saja." },
  { "en": "Transfer Time UPS Offline Adalah?", "id": "Sekitar 6 Sampai 10 ms." },
  { "en": "Transfer Time UPS Online Adalah?", "id": "0 ms Tanpa Jeda." },
  { "en": "Baterai UPS Biasanya Tipe Apa?", "id": "Lead Acid VRLA 12V." },
  { "en": "Apa Itu Cold Start Pada UPS?", "id": "Nyalakan UPS Tanpa Colok PLN." },
  { "en": "Apa Itu Bypass Mode Pada UPS?", "id": "Listrik Langsung Tanpa Baterai." },
  { "en": "Apa Itu Piezo Buzzer?", "id": "Penghasil Suara Kristal Piezo." },
  { "en": "Piezo Buzzer Bekerja Dengan Efek?", "id": "Getaran Keramik Akibat Listrik." },
  { "en": "Apa Itu Speaker Impedansi 4 Ohm?", "id": "Hambatan Coil Speaker." },
  { "en": "Speaker 4 Ohm Vs 8 Ohm Berat Mana?", "id": "4 Ohm Lebih Berat Ke Amplifier." },
  { "en": "Apa Itu Bahan FR4 Pada PCB?", "id": "Bahan Fiber Kaca Standar PCB." },
  { "en": "Apa Itu Blind Via Pada PCB?", "id": "Lubang Via Tidak Tembus." },
  { "en": "Apa Itu Buried Via Pada PCB?", "id": "Lubang Via Di Dalam Lapisan." },
  { "en": "Apa Itu Single Sided PCB?", "id": "PCB Satu Sisi Tembaga." },
  { "en": "Apa Itu Double Sided PCB?", "id": "PCB Dua Sisi Tembaga." },
  { "en": "Apa Itu Multi Layer PCB?", "id": "PCB Lebih Dari 2 Lapis." },
  { "en": "Apa Fungsi Ground Plane Pada PCB?", "id": "Meredam Noise Dan Referensi Ground." },
  { "en": "Apa Itu Trace Width PCB?", "id": "Lebar Jalur Tembaga." },
  { "en": "Lebar Jalur PCB Menentukan Kapasitas Apa?", "id": "Kapasitas Arus." },
  { "en": "Apa Itu Half Wave Rectifier?", "id": "Penyearah Setengah Gelombang." },
  { "en": "Apa Itu Full Wave Center Tap?", "id": "Penyearah Penuh Trafo CT." },
  { "en": "Apa Itu Voltage Doubler?", "id": "Pengganda Tegangan Kapasitor Dioda." },
  { "en": "Apa Fungsi Rangkaian Clipper?", "id": "Memotong Puncak Sinyal." },
  { "en": "Apa Fungsi Rangkaian Clamper?", "id": "Menggeser Level Tegangan DC Sinyal." },
  { "en": "Apa Itu Miller Effect Pada Transistor?", "id": "Kenaikan Kapasitansi Input Penguat." },
  { "en": "Apa Itu Parasitic Capacitance?", "id": "Kapasitansi Liar Antar Komponen." },
  { "en": "Apa Itu Parasitic Inductance?", "id": "Induktansi Liar Jalur PCB." },
  { "en": "Apa Fungsi Gate Driver MOSFET?", "id": "Memperkuat Arus Pemicu Gate." },
  { "en": "Mengapa Gate MOSFET Butuh Arus Besar?", "id": "Mengisi Kapasitansi Gate Cepat." },
  { "en": "Apa Itu Bootstrap Circuit?", "id": "Penyuplai Tegangan Gate Sisi Atas." },
  { "en": "Apa Itu Totem Pole Output?", "id": "Susunan Transistor Dorong Tarik." },
  { "en": "Apa Itu Open Collector Output?", "id": "Kaki Kolektor Menggantung." },
  { "en": "Open Collector Membutuhkan Komponen Apa?", "id": "Resistor Pull Up." },
  { "en": "Apa Itu High Impedance State (Hi-Z)?", "id": "Kondisi Mengambang Tidak Terhubung." },
  { "en": "Apa Itu Tri State Buffer?", "id": "Buffer Dengan Output Hi-Z." },
  { "en": "Apa Itu Modulasi PSK (Phase Shift Keying)?", "id": "Modulasi Pergeseran Fasa." },
  { "en": "Apa Itu Modulasi QAM (Quadrature Amplitude)?", "id": "Kombinasi Amplitudo Dan Fasa." },
  { "en": "Apa Itu Parity Bit Data?", "id": "Bit Cek Kesalahan." },
  { "en": "Apa Itu Start Bit Serial?", "id": "Tanda Awal Kirim Data." },
  { "en": "Apa Itu Stop Bit Serial?", "id": "Tanda Akhir Kirim Data." },
  { "en": "Apa Itu Baud Rate?", "id": "Kecepatan Simbol Per Detik." },
  { "en": "Apa Itu Bit Rate?", "id": "Kecepatan Bit Per Detik." },
  { "en": "Apa Itu Handshaking Hardware?", "id": "Sinyal Kontrol Aliran Data." },
  { "en": "Pin RTS (Request To Send) Adalah?", "id": "Permintaan Kirim Data." },
  { "en": "Pin CTS (Clear To Send) Adalah?", "id": "Siap Terima Data." },
  { "en": "Apa Itu Kabel Twisted Pair Shielded?", "id": "Kabel Pilin Berpelindung." },
  { "en": "Apa Itu Impedansi Karakteristik Kabel?", "id": "Impedansi Kabel Tak Hingga." },
  { "en": "Impedansi Kabel LAN Adalah?", "id": "100 Ohm." },
  { "en": "Impedansi Kabel Antena TV Adalah?", "id": "75 Ohm." },
  { "en": "Impedansi Kabel Radio Komunikasi Adalah?", "id": "50 Ohm." },
  { "en": "Apa Itu Konektor PL-259?", "id": "Konektor Radio UHF." },
  { "en": "Apa Itu Konektor N-Type?", "id": "Konektor Frekuensi Tinggi Tahan Air." },
  { "en": "Apa Itu Attenuator?", "id": "Peredam Kekuatan Sinyal." },
  { "en": "Satuan Attenuasi Adalah?", "id": "Desibel." },
  { "en": "Apa Itu dBm?", "id": "Desibel Miliwatt." },
  { "en": "0 dBm Setara Dengan Berapa Watt?", "id": "1 Miliwatt." },
  { "en": "30 dBm Setara Dengan Berapa Watt?", "id": "1 Watt." },
  { "en": "Apa Itu Antenna Gain dBi?", "id": "Penguatan Relatif Isotropik." },
  { "en": "Apa Itu Antenna Gain dBd?", "id": "Penguatan Relatif Dipole." },
  { "en": "Hubungan dBi Dan dBd Adalah?", "id": "dBi Sama Dengan dBd Tambah 2,15." },
  { "en": "Apa Itu Azimuth Pattern?", "id": "Pola Radiasi Arah Horizontal." },
  { "en": "Apa Itu Elevation Pattern?", "id": "Pola Radiasi Arah Vertikal." },
  { "en": "Apa Itu Beamwidth Antena?", "id": "Lebar Sudut Pancaran Utama." },
  { "en": "Antena Sectoral Digunakan Untuk?", "id": "Pemancar Seluler Sudut Lebar." },
  { "en": "Apa Itu KVA Gen Set?", "id": "Kapasitas Daya Semu Genset." },
  { "en": "Faktor Daya Genset Biasanya?", "id": "0,8." },
  { "en": "Genset 100 KVA Menghasilkan Berapa KW?", "id": "80 Kilo Watt." },
  { "en": "Apa Itu Prime Power Genset?", "id": "Daya Utama Beban Variabel." },
  { "en": "Apa Itu Standby Power Genset?", "id": "Daya Cadangan Darurat." },
  { "en": "Apa Itu Continuous Power Genset?", "id": "Daya Terus Menerus Beban Tetap." },
  { "en": "Apa Itu Turbocharger Pada Genset?", "id": "Penambah Pasokan Udara Mesin." },
  { "en": "Apa Fungsi Intercooler Genset?", "id": "Mendinginkan Udara Turbo." },
  { "en": "Apa Itu Dummy Load Test Genset?", "id": "Uji Beban Buatan Penuh." },
  { "en": "Apa Fungsi Engine Block Heater?", "id": "Menjaga Mesin Hangat Saat Mati." },
  { "en": "Tujuannya Block Heater Adalah?", "id": "Memudahkan Start Mesin Dingin." },
  { "en": "Apa Itu Daily Tank BBM?", "id": "Tangki Harian Bahan Bakar." },
  { "en": "Apa Itu Bulk Tank BBM?", "id": "Tangki Penyimpanan Besar." },
  { "en": "Pompa Transfer BBM Berfungsi?", "id": "Memindah BBM Ke Tangki Harian." },
  { "en": "Apa Itu Water Separator Filter?", "id": "Pemisah Air Dari Solar." },
  { "en": "Apa Warna Asap Genset Normal?", "id": "Transparan Atau Sedikit Abu." },
  { "en": "Asap Hitam Genset Menandakan?", "id": "Solar Terlalu Banyak Kurang Udara." },
  { "en": "Asap Putih Genset Menandakan?", "id": "Air Atau Solar Tidak Terbakar." },
  { "en": "Asap Biru Genset Menandakan?", "id": "Oli Mesin Ikut Terbakar." },
  { "en": "Apa Itu Overspeed Trip?", "id": "Proteksi Putaran Berlebih." },
  { "en": "Apa Itu Low Oil Pressure Trip?", "id": "Proteksi Tekanan Oli Rendah." },
  { "en": "Apa Itu High Water Temp Trip?", "id": "Proteksi Suhu Air Tinggi." },
  { "en": "Apa Itu Crankcase Heater?", "id": "Pemanas Oli Mesin." },
  { "en": "Apa Itu Battery Charger Genset?", "id": "Pengisi Aki Otomatis." },
  { "en": "Tegangan Aki Genset Kecil Adalah?", "id": "12 Volt." },
  { "en": "Tegangan Aki Genset Besar Adalah?", "id": "24 Volt." },
  { "en": "Apa Itu Synchronizing Panel?", "id": "Panel Paralel Genset." },
  { "en": "Load Sharing Berfungsi Untuk?", "id": "Membagi Beban Rata Antar Genset." },
  { "en": "Apa Itu Reverse Power Relay?", "id": "Mencegah Genset Jadi Motor." },
  { "en": "Kapan Reverse Power Terjadi?", "id": "Mesin Mati Tapi Breaker Masuk." },
  { "en": "Apa Itu Vector Control Motor?", "id": "Kontrol Torsi Dan Fluks Terpisah." },
  { "en": "Apa Itu V/f Control Motor?", "id": "Kontrol Tegangan Frekuensi Konstan." },
  { "en": "Kelebihan Vector Control Adalah?", "id": "Respon Torsi Cepat." },
  { "en": "Apa Itu Slip Compensation?", "id": "Menambah Frekuensi Saat Beban Berat." },
  { "en": "Apa Itu PID Auto Tuning?", "id": "Mencari Parameter PID Otomatis." },
  { "en": "Apa Itu Hall Sensor Motor?", "id": "Deteksi Posisi Rotor Magnet." },
  { "en": "Apa Itu Back EMF Constant?", "id": "Tegangan Balik Per RPM." },
  { "en": "Apa Itu Torque Constant Motor?", "id": "Torsi Per Ampere." },
  { "en": "Motor DC Series Memiliki Torsi Awal?", "id": "Sangat Besar." },
  { "en": "Motor DC Shunt Memiliki Kecepatan?", "id": "Relatif Stabil." },
  { "en": "Motor DC Compound Adalah?", "id": "Gabungan Seri Dan Shunt." },
  { "en": "Apa Itu Universal Motor?", "id": "Bisa AC Dan DC." },
  { "en": "Ciri Khas Suara Motor Universal?", "id": "Bising Suara Sikat Arang." },
  { "en": "Apa Kepanjangan HVDC?", "id": "High Voltage Direct Current." },
  { "en": "Apa Keuntungan Utama Transmisi HVDC?", "id": "Rugi Daya Jarak Jauh Kecil." },
  { "en": "Apakah Ada Efek Kulit Pada HVDC?", "id": "Tidak Ada Efek Kulit." },
  { "en": "Apa Itu Konverter Station HVDC?", "id": "Pengubah AC Ke DC." },
  { "en": "Apa Kepanjangan FACTS Dalam Sistem Tenaga?", "id": "Flexible AC Transmission System." },
  { "en": "Apa Fungsi STATCOM Pada Transmisi?", "id": "Kompensasi Daya Reaktif Cepat." },
  { "en": "Apa Fungsi SVC (Static Var Compensator)?", "id": "Menstabilkan Tegangan Transmisi." },
  { "en": "Apa Fungsi Nacelle Kincir Angin?", "id": "Rumah Generator Dan Gearbox." },
  { "en": "Apa Fungsi Yaw System Turbin Angin?", "id": "Memutar Turbin Menghadap Angin." },
  { "en": "Apa Fungsi Pitch Control Turbin Angin?", "id": "Mengatur Sudut Baling Baling." },
  { "en": "Apa Itu Anemometer Pada Turbin Angin?", "id": "Pengukur Kecepatan Angin." },
  { "en": "Apa Itu Betz Limit Energi Angin?", "id": "Batas Efisiensi Turbin Angin." },
  { "en": "Berapa Nilai Maksimal Betz Limit?", "id": "59,3 Persen." },
  { "en": "Apa Itu Cut In Speed Angin?", "id": "Kecepatan Angin Mulai Putar." },
  { "en": "Apa Itu Cut Out Speed Angin?", "id": "Kecepatan Angin Berhenti Aman." },
  { "en": "Apa Karakteristik Filter Butterworth?", "id": "Respon Frekuensi Datar." },
  { "en": "Apa Karakteristik Filter Chebyshev?", "id": "Ada Ripple Di Passband." },
  { "en": "Apa Karakteristik Filter Bessel?", "id": "Respon Fasa Linier." },
  { "en": "Apa Itu Notch Filter?", "id": "Membuang 1 Frekuensi Spesifik." },
  { "en": "Apa Itu Band Stop Filter?", "id": "Menahan Rentang Frekuensi Tertentu." },
  { "en": "Apa Itu Active Filter?", "id": "Menggunakan Op Amp Dan Daya." },
  { "en": "Apa Itu Passive Filter?", "id": "Hanya R L Dan C." },
  { "en": "Apa Kepanjangan DGA Pada Minyak Trafo?", "id": "Dissolved Gas Analysis." },
  { "en": "Gas Asetilen Pada Minyak Trafo Menandakan?", "id": "Adanya Busur Api Listrik." },
  { "en": "Gas Hidrogen Pada Minyak Trafo Menandakan?", "id": "Partial Discharge Atau Corona." },
  { "en": "Apa Itu Breakdown Voltage Test Minyak?", "id": "Uji Tegangan Tembus Minyak." },
  { "en": "Standar Tegangan Tembus Minyak Baru Adalah?", "id": "Minimal 30 kV." },
  { "en": "Apa Itu Purifikasi Minyak Trafo?", "id": "Menyaring Kotoran Dan Air." },
  { "en": "Apa Itu Oil Sludge?", "id": "Endapan Lumpur Minyak Trafo." },
  { "en": "Apa Itu Arc Flash?", "id": "Ledakan Kilat Busur Listrik." },
  { "en": "Apa Itu Arc Blast?", "id": "Gelombang Tekanan Ledakan Listrik." },
  { "en": "Satuan Energi Insiden Arc Flash Adalah?", "id": "Kalori Per Sentimeter Persegi." },
  { "en": "Apa Itu Arc Flash Boundary?", "id": "Batas Jarak Aman Ledakan." },
  { "en": "Pakaian Tahan Api Disebut?", "id": "FR Clothing." },
  { "en": "Kepanjangan FR Pada Pakaian Safety?", "id": "Flame Resistant." },
  { "en": "Apa Itu Ladder Diagram (LD)?", "id": "Bahasa Pemrograman Diagram Tangga." },
  { "en": "Apa Itu Function Block Diagram (FBD)?", "id": "Bahasa Blok Fungsi Logika." },
  { "en": "Apa Itu Structured Text (ST)?", "id": "Bahasa Teks Mirip Pascal." },
  { "en": "Apa Itu Instruction List (IL)?", "id": "Bahasa Teks Mirip Assembly." },
  { "en": "Apa Itu Sequential Function Chart (SFC)?", "id": "Bahasa Alur Proses Sekuensial." },
  { "en": "Apa Itu Exothermic Welding Grounding?", "id": "Las Cadwelding Sambungan Grounding." },
  { "en": "Bahan Las Cadwell Adalah?", "id": "Serbuk Tembaga Dan Aluminium." },
  { "en": "Apa Itu Earth Pit?", "id": "Bak Kontrol Grounding." },
  { "en": "Apa Itu Bentonit Pada Grounding?", "id": "Semen Konduktif Penurun Tahanan." },
  { "en": "Apa Itu Grounding Mesh?", "id": "Jaring Tembaga Bawah Tanah." },
  { "en": "Bahaya Tegangan Langkah Dikurangi Dengan?", "id": "Memasang Batu Kerikil Gravel." },
  { "en": "Apa Itu Konduktor AAC?", "id": "All Aluminium Conductor." },
  { "en": "Apa Itu Konduktor AAAC?", "id": "All Aluminium Alloy Conductor." },
  { "en": "Kabel Twist SUTR Menggunakan Bahan?", "id": "Aluminium Alloy." },
  { "en": "Apa Itu Joint Sleeve SUTM?", "id": "Sambungan Kabel Tegangan Menengah." },
  { "en": "Apa Itu Guy Wire?", "id": "Kawat Skur Penahan Tiang." },
  { "en": "Apa Fungsi Isolator Tumpu?", "id": "Menyangga Kabel Lurus." },
  { "en": "Apa Fungsi Isolator Tarik?", "id": "Menahan Tarikan Kabel Sudut." },
  { "en": "Apa Itu Crest Factor?", "id": "Rasio Puncak Ke RMS." },
  { "en": "Crest Factor Gelombang Sinus Adalah?", "id": "1,414." },
  { "en": "Apa Itu Form Factor Gelombang?", "id": "Rasio RMS Ke Rata Rata." },
  { "en": "Form Factor Gelombang Sinus Adalah?", "id": "1,11." },
  { "en": "Mengapa Angka 1,11 Penting?", "id": "Dasar Kalibrasi Meter Analog." },
  { "en": "Apa Itu Efek Hall?", "id": "Tegangan Akibat Medan Magnet." },
  { "en": "Sensor Hall Effect Digunakan Untuk?", "id": "Deteksi Arus Dan Posisi." },
  { "en": "Apa Itu Reed Switch?", "id": "Sakelar Magnet Dalam Kaca." },
  { "en": "Apa Itu Solid State Drive (SSD)?", "id": "Penyimpanan Memori Flash Cepat." },
  { "en": "Apa Itu Load Balancer Server?", "id": "Pembagi Beban Trafik Jaringan." },
  { "en": "Apa Itu Rack Server Unit (U)?", "id": "Satuan Tinggi Rak Server." },
  { "en": "1U Rack Server Setara Dengan?", "id": "1,75 Inci." },
  { "en": "Apa Itu PDU (Power Distribution Unit)?", "id": "Stopkontak Distribusi Rak Server." },
  { "en": "Apa Itu Kabel Cross Over?", "id": "Hubung Komputer Ke Komputer." },
  { "en": "Apa Itu Kabel Straight Through?", "id": "Hubung Komputer Ke Switch." },
  { "en": "Standar Urutan Warna Kabel LAN?", "id": "T568A Dan T568B." },
  { "en": "Warna Kabel Pin 1 T568B Adalah?", "id": "Putih Oranye." },
  { "en": "Warna Kabel Pin 2 T568B Adalah?", "id": "Oranye." },
  { "en": "Warna Kabel Pin 3 T568B Adalah?", "id": "Putih Hijau." },
  { "en": "Warna Kabel Pin 6 T568B Adalah?", "id": "Hijau." },
  { "en": "Apa Itu Power Budget Fiber Optik?", "id": "Perhitungan Rugi Rugi Cahaya." },
  { "en": "Apa Itu Splicing Loss?", "id": "Rugi Daya Sambungan Kabel." },
  { "en": "Batas Maksimal Splicing Loss Standar?", "id": "0,1 dB Per Sambungan." },
  { "en": "Apa Itu Macro Bending Loss?", "id": "Rugi Akibat Tekukan Besar." },
  { "en": "Apa Itu Micro Bending Loss?", "id": "Rugi Akibat Tekanan Kecil." },
  { "en": "Apa Itu Dark Fiber?", "id": "Kabel Optik Belum Terpakai." },
  { "en": "Apa Itu FTTH (Fiber To The Home)?", "id": "Jaringan Optik Ke Rumah." },
  { "en": "Apa Itu ONT (Optical Network Terminal)?", "id": "Modem Optik Di Rumah." },
  { "en": "Apa Itu OLT (Optical Line Terminal)?", "id": "Pusat Sentral Jaringan Optik." },
  { "en": "Apa Itu Splitter Fiber Optik?", "id": "Pecah Sinyal Optik Pasif." },
  { "en": "Rasio Splitter 1:8 Artinya?", "id": "1 Input 8 Output." },
  { "en": "Apa Itu Crosstalk Pada Audio?", "id": "Bocornya Suara Kanal Sebelah." },
  { "en": "Apa Itu Slew Rate Amplifier?", "id": "Kecepatan Respon Sinyal Output." },
  { "en": "Apa Itu Damping Factor Amplifier?", "id": "Kemampuan Mengontrol Gerak Speaker." },
  { "en": "Damping Factor Tinggi Membuat Bass?", "id": "Lebih Ketat Dan Detail." },
  { "en": "Apa Itu THX Certified?", "id": "Standar Kualitas Audio Bioskop." },
  { "en": "Apa Itu Phantom Power 48V?", "id": "Daya Untuk Mic Kondensor." },
  { "en": "Kabel Mic XLR Pin 1 Adalah?", "id": "Ground Shield." },
  { "en": "Kabel Mic XLR Pin 2 Adalah?", "id": "Positif Hot." },
  { "en": "Kabel Mic XLR Pin 3 Adalah?", "id": "Negatif Cold." },
  { "en": "Apa Itu DI Box (Direct Injection)?", "id": "Ubah Impedansi Gitar Ke Mixer." },
  { "en": "Apa Fungsi Gain Pada Mixer?", "id": "Mengatur Sensitivitas Input Awal." },
  { "en": "Apa Fungsi Fader Pada Mixer?", "id": "Mengatur Volume Output Akhir." },
  { "en": "Apa Itu Headroom Audio?", "id": "Ruang Sinyal Sebelum Pecah." },
  { "en": "Apa Itu Clipping Pada Audio?", "id": "Sinyal Terpotong Karena Overload." },
  { "en": "Apa Itu Pendingin ONAN Pada Trafo?", "id": "Minyak Alami Udara Alami." },
  { "en": "Apa Itu Pendingin ONAF Pada Trafo?", "id": "Minyak Alami Udara Paksa." },
  { "en": "Apa Itu Kode IK Rating?", "id": "Ketahanan Terhadap Benturan Mekanik." },
  { "en": "Kode IK10 Artinya Tahan Benturan Sebesar?", "id": "20 Joule." },
  { "en": "Apa Itu Protokol DMX512?", "id": "Komunikasi Digital Lampu Panggung." },
  { "en": "Konektor DMX Standar Menggunakan Jenis?", "id": "XLR 5 Pin." },
  { "en": "Apa Itu Charge Controller PWM?", "id": "Pengisi Baterai Surya Murah." },
  { "en": "Apa Itu Charge Controller MPPT?", "id": "Efisiensi Tinggi Konversi Daya." },
  { "en": "Efisiensi MPPT Bisa Mencapai Berapa?", "id": "98 Persen." },
  { "en": "Apa Itu Package SOT-23?", "id": "Kemasan Transistor SMD Kecil." },
  { "en": "Apa Itu Package DIP (Dual Inline)?", "id": "Kemasan Kaki Tembus Lubang." },
  { "en": "Apa Itu SOIC (Small Outline)?", "id": "Kemasan IC SMD Standar." },
  { "en": "Apa Itu Cold Joint Solder?", "id": "Sambungan Retak Atau Kusam." },
  { "en": "Apa Itu Flux Residue?", "id": "Sisa Kotoran Cairan Solder." },
  { "en": "Mengapa Flux Harus Dibersihkan?", "id": "Bisa Menjadi Konduktif Korosif." },
  { "en": "Apa Itu IPA (Isopropyl Alcohol)?", "id": "Cairan Pembersih PCB." },
  { "en": "Apa Itu Conformal Coating Silicone?", "id": "Pelapis PCB Tahan Panas." },
  { "en": "Apa Itu Heat Gun Shrink?", "id": "Pemanas Selongsong Kabel." },
  { "en": "Apa Itu Ferrule Kabel?", "id": "Ujung Kabel Serabut Rapih." },
  { "en": "Tang Crimping Ferrule Bentuknya?", "id": "Kotak Atau Segi Enam." },
  { "en": "Apa Itu MCC (Motor Control Center)?", "id": "Pusat Kendali Motor Industri." },
  { "en": "Apa Itu Feeder Pillar?", "id": "Panel Pembagi Listrik Jalan." },
  { "en": "Apa Itu Cubicle 20kV?", "id": "Panel Tegangan Menengah TM." },
  { "en": "Gas SF6 Pada Cubicle Berfungsi?", "id": "Pemadam Busur Api." },
  { "en": "Tekanan Gas SF6 Normal Adalah?", "id": "0,3 Sampai 0,5 Bar." },
  { "en": "Apa Itu Relay VAMP?", "id": "Relay Proteksi Arc Flash." },
  { "en": "Sensor Relay VAMP Mendeteksi Apa?", "id": "Cahaya Kilat Dan Arus." },
  { "en": "Apa Itu Annunciator Panel?", "id": "Panel Alarm Indikator Lampu." },
  { "en": "Tombol Acknowledge Berfungsi Untuk?", "id": "Mematikan Suara Alarm." },
  { "en": "Tombol Reset Alarm Berfungsi Untuk?", "id": "Menghapus Status Alarm." },
  { "en": "Apa Itu Emergency Stop Mushroom?", "id": "Tombol Darurat Kepala Jamur." },
  { "en": "Kontak Emergency Stop Selalu Berjenis?", "id": "Normally Closed NC." },
  { "en": "Apa Itu Limit Switch Roller?", "id": "Sakelar Batas Roda Gulir." },
  { "en": "Apa Itu Float Switch?", "id": "Sakelar Pelampung Air." },
  { "en": "Apa Itu Pressure Switch?", "id": "Sakelar Tekanan Angin Air." },
  { "en": "Apa Itu Flow Switch?", "id": "Sakelar Deteksi Aliran Air." },
  { "en": "Apa Itu Reed Relay?", "id": "Relay Kontak Tabung Kaca." },
  { "en": "Apa Itu Latching Relay?", "id": "Relay Pengunci Posisi Mekanik." },
  { "en": "Cara Reset Latching Relay Adalah?", "id": "Beri Pulsa Koil Reset." },
  { "en": "Apa Itu Stepper Motor NEMA 17?", "id": "Ukuran Standar Motor Printer 3D." },
  { "en": "Apa Itu Microstepping Driver?", "id": "Membagi Langkah Motor Stepper." },
  { "en": "Kelebihan Microstepping Adalah?", "id": "Gerakan Lebih Halus Presisi." },
  { "en": "Apa Itu Holding Torque Stepper?", "id": "Kekuatan Tahan Saat Diam." },
  { "en": "Apa Itu Detent Torque?", "id": "Torsi Putar Saat Mati." },
  { "en": "Motor Servo Menggunakan Umpan Balik?", "id": "Potensiometer Atau Encoder." },
  { "en": "Sinyal Kendali Servo RC Adalah?", "id": "Pulsa 1ms Sampai 2ms." },
  { "en": "Frekuensi Sinyal Servo RC Adalah?", "id": "50 Hz." },
  { "en": "Apa Itu Continuous Rotation Servo?", "id": "Servo Berputar Terus Menerus." },
  { "en": "Apa Itu Brushless Gimbal Motor?", "id": "Motor Stabilizer Kamera Halus." },
  { "en": "Apa Itu Kv Rating Motor BLDC?", "id": "RPM Per Volt Tanpa Beban." },
  { "en": "Motor Kv Tinggi Cocok Untuk?", "id": "Baling Baling Kecil Cepat." },
  { "en": "Motor Kv Rendah Cocok Untuk?", "id": "Baling Baling Besar Torsi." },
  { "en": "Apa Itu ESC Opto?", "id": "ESC Tanpa BEC Internal." },
  { "en": "Apa Itu UBEC (Universal BEC)?", "id": "Regulator Tegangan Eksternal." },
  { "en": "Apa Fungsi BEC Pada ESC?", "id": "Suplai Daya Receiver Servo." },
  { "en": "Apa Itu TVS Diode?", "id": "Penahan Lonjakan Tegangan Transien." },
  { "en": "Kepanjangan TVS Adalah?", "id": "Transient Voltage Suppressor." },
  { "en": "Apa Itu GDT (Gas Discharge Tube)?", "id": "Tabung Gas Anti Petir." },
  { "en": "Apa Itu MOV (Metal Oxide Varistor)?", "id": "Resistor Variabel Tegangan." },
  { "en": "Jika Tegangan Lebih MOV Akan?", "id": "Short Circuit Memutus Fuse." },
  { "en": "Apa Itu Polyfuse (PTC Fuse)?", "id": "Sekering Reset Otomatis." },
  { "en": "Cara Reset Polyfuse Adalah?", "id": "Hilangkan Beban Dan Dinginkan." },
  { "en": "Apa Itu Thermal Fuse Kipas?", "id": "Putus Saat Suhu Panas." },
  { "en": "Letak Thermal Fuse Biasanya Di?", "id": "Dalam Gulungan Dinamo." },
  { "en": "Apa Itu Thermostat Kulkas?", "id": "Sakelar Suhu Gas Kapiler." },
  { "en": "Apa Itu Defrost Heater?", "id": "Pemanas Pencair Bunga Es." },
  { "en": "Apa Itu Timer Defrost?", "id": "Pengatur Waktu Cair Es." },
  { "en": "Apa Itu Kompresor Inverter?", "id": "Kompresor Kecepatan Variabel." },
  { "en": "Kelebihan AC Inverter Adalah?", "id": "Suhu Stabil Dan Hemat Listrik." },
  { "en": "Apa Itu Refrigeran R32?", "id": "Gas Pendingin Ramah Ozon." },
  { "en": "Tekanan Freon R32 Adalah?", "id": "Lebih Tinggi Dari R22." },
  { "en": "Apa Itu Vacuum Pump AC?", "id": "Pompa Hampa Udara Pipa." },
  { "en": "Mengapa Pipa AC Harus Divakum?", "id": "Membuang Udara Dan Uap Air." },
  { "en": "Apa Itu Manifold Gauge?", "id": "Alat Ukur Tekanan Freon." },
  { "en": "Selang Manifold Warna Biru Untuk?", "id": "Tekanan Rendah Low Pressure." },
  { "en": "Selang Manifold Warna Merah Untuk?", "id": "Tekanan Tinggi High Pressure." },
  { "en": "Selang Manifold Warna Kuning Untuk?", "id": "Jalur Pengisian Freon Vakum." },
  { "en": "Apa Itu Kapasitor Running AC?", "id": "Kapasitor Kompresor Dan Fan." },
  { "en": "Apa Itu Overload Protector Kompresor?", "id": "Pelindung Panas Dan Arus." },
  { "en": "Apa Itu Solenoid Valve Kulkas?", "id": "Pengatur Aliran Freon Ganda." },
  { "en": "Apa Itu EER (Energy Efficiency Ratio)?", "id": "Rasio Pendinginan Per Watt." },
  { "en": "Semakin Tinggi Nilai EER Maka?", "id": "Semakin Hemat Listrik." },
  { "en": "Apa Itu COP (Coefficient Of Performance)?", "id": "Efisiensi Pompa Kalor." },
  { "en": "Apa Itu BTU (British Thermal Unit)?", "id": "Satuan Energi Panas Pendinginan." },
  { "en": "AC 1 PK Setara Berapa BTU?", "id": "Sekitar 9000 BTU." },
  { "en": "AC Setengah PK Membutuhkan Daya?", "id": "Sekitar 350 Sampai 400 Watt." },
  { "en": "Apa Itu Inverter Las MMA?", "id": "Las Listrik Elektroda Stick." },
  { "en": "Apa Itu Las MIG (Metal Inert Gas)?", "id": "Las Kawat Gulung Gas." },
  { "en": "Apa Itu Las TIG (Tungsten Inert Gas)?", "id": "Las Elektroda Tungsten Argon." },
  { "en": "Gas Pelindung Las MIG Biasanya?", "id": "CO2 Atau Argon Mix." },
  { "en": "Gas Pelindung Las TIG Wajib?", "id": "Argon Murni." },
  { "en": "Apa Itu Duty Cycle Mesin Las?", "id": "Waktu Las Tanpa Istirahat." },
  { "en": "Duty Cycle 60% Artinya Apa?", "id": "6 Menit Las 4 Menit Diam." },
  { "en": "Apa Itu Kawat Las E6013?", "id": "Kawat Las Baja Lunak Umum." },
  { "en": "Apa Itu Kawat Las E7018?", "id": "Kawat Las Kekuatan Tinggi Low Hydrogen." },
  { "en": "Apa Itu Earth Clamp Las?", "id": "Tang Massa Jepit Benda." },
  { "en": "Apa Itu Electrode Holder?", "id": "Tang Pemegang Kawat Las." },
  { "en": "Apa Itu Slag Las?", "id": "Kerak Pelindung Hasil Las." }


        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 7000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>
