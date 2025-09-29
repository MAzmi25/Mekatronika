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


{ "en": "Apa Definisi Dari Mekatronika?", "id": "Sinergi Antara Teknik Mesin Dan Elektronika." },
{ "en": "Sebutkan Empat Komponen Utama Sistem Mekatronika?", "id": "Sensor, Aktuator, Kontroler, Dan Mekanisme." },
{ "en": "Apa Fungsi Dari Sensor Dalam Mekatronika?", "id": "Mengukur Besaran Fisik Dan Mengubahnya Menjadi Sinyal." },
{ "en": "Apa Fungsi Dari Aktuator Dalam Mekatronika?", "id": "Mengubah Sinyal Listrik Menjadi Gerakan Fisik." },
{ "en": "Apa Fungsi Dari Kontroler Dalam Mekatronika?", "id": "Memproses Informasi Sensor Dan Mengontrol Aktuator." },
{ "en": "Apa Yang Dimaksud Dengan Sistem Kontrol Loop Tertutup?", "id": "Sistem Kontrol Dengan Umpan Balik (Feedback)." },
{ "en": "Apa Yang Dimaksud Dengan Sistem Kontrol Loop Terbuka?", "id": "Sistem Kontrol Tanpa Umpan Balik (Feedback)." },
{ "en": "Apa Kepanjangan Dari PLC (Programmable Logic Controller)?", "id": "Programmable Logic Controller." },
{ "en": "Apa Fungsi Utama Dari PLC (Programmable Logic Controller)?", "id": "Mengontrol Proses Industri Secara Otomatis." },
{ "en": "Apa Itu Mikrokontroler?", "id": "Komputer Mikro Dalam Satu Chip Terintegrasi." },
{ "en": "Sebutkan Contoh Aplikasi Mekatronika Dalam Kehidupan Sehari-Hari?", "id": "Mesin Cuci Otomatis Dan Robot Vacuum Cleaner." },
{ "en": "Apa Perbedaan Antara Robot Dan Mesin Otomatis?", "id": "Robot Dapat Diprogram Ulang Untuk Tugas Berbeda." },
{ "en": "Apa Itu Sistem Akuisisi Data?", "id": "Proses Pengambilan Dan Pengukuran Sinyal Dunia Nyata." },
{ "en": "Apa Kepanjangan Dari CAD (Computer-Aided Design)?", "id": "Computer-Aided Design." },
{ "en": "Apa Fungsi Dari Perangkat Lunak CAD (Computer-Aided Design)?", "id": "Membantu Dalam Merancang Dan Menggambar Produk Teknik." },
{ "en": "Apa Kepanjangan Dari CAM (Computer-Aided Manufacturing)?", "id": "Computer-Aided Manufacturing." },
{ "en": "Apa Fungsi Dari Perangkat Lunak CAM (Computer-Aided Manufacturing)?", "id": "Mengontrol Mesin Manufaktur Melalui Komputer." },
{ "en": "Apa Itu Sensor Proximity?", "id": "Sensor Yang Mendeteksi Keberadaan Objek Tanpa Sentuhan." },
{ "en": "Apa Itu Sensor Suhu?", "id": "Sensor Yang Mengukur Temperatur Suatu Benda." },
{ "en": "Apa Itu Motor Stepper?", "id": "Motor Listrik Yang Bergerak Dalam Langkah Diskrit." },
{ "en": "Apa Itu Motor Servo?", "id": "Motor Listrik Dengan Kontrol Posisi Yang Tepat." },
{ "en": "Apa Itu Pneumatik?", "id": "Sistem Yang Menggunakan Udara Bertekanan Untuk Gerakan." },
{ "en": "Apa Itu Hidrolik?", "id": "Sistem Yang Menggunakan Cairan Bertekanan Untuk Gerakan." },
{ "en": "Apa Keuntungan Sistem Pneumatik?", "id": "Bersih, Cepat, Dan Relatif Murah." },
{ "en": "Apa Keuntungan Sistem Hidrolik?", "id": "Mampu Menghasilkan Gaya Yang Sangat Besar." },
{ "en": "Apa Itu Rangkaian Terintegrasi (IC)?", "id": "Kumpulan Komponen Elektronik Dalam Satu Chip Silikon." },
{ "en": "Apa Fungsi Dari Dioda?", "id": "Mengizinkan Arus Listrik Mengalir Satu Arah." },
{ "en": "Apa Fungsi Dari Transistor?", "id": "Sebagai Saklar Elektronik Dan Penguat Sinyal." },
{ "en": "Apa Itu Gerbang Logika (Logic Gate)?", "id": "Blok Bangunan Dasar Sirkuit Digital." },
{ "en": "Sebutkan Tiga Gerbang Logika Dasar?", "id": "AND, OR, Dan NOT." },
{ "en": "Apa Itu Sistem Bilangan Biner?", "id": "Sistem Angka Yang Hanya Menggunakan 0 Dan 1." },
{ "en": "Apa Itu Bit?", "id": "Unit Informasi Terkecil Dalam Komputasi Digital." },
{ "en": "Apa Itu Byte?", "id": "Kumpulan Delapan Bit." },
{ "en": "Apa Kepanjangan Dari CPU (Central Processing Unit)?", "id": "Central Processing Unit." },
{ "en": "Apa Fungsi Dari CPU (Central Processing Unit)?", "id": "Otak Dari Sistem Komputer Atau Kontroler." },
{ "en": "Apa Kepanjangan Dari RAM (Random Access Memory)?", "id": "Random Access Memory." },
{ "en": "Apa Fungsi Dari RAM (Random Access Memory)?", "id": "Menyimpan Data Sementara Saat Sistem Beroperasi." },
{ "en": "Apa Kepanjangan Dari ROM (Read-Only Memory)?", "id": "Read-Only Memory." },
{ "en": "Apa Fungsi Dari ROM (Read-Only Memory)?", "id": "Menyimpan Instruksi Startup Yang Permanen." },
{ "en": "Apa Itu Algoritma?", "id": "Langkah-Langkah Logis Untuk Menyelesaikan Suatu Masalah." },
{ "en": "Apa Itu Diagram Alir (Flowchart)?", "id": "Representasi Grafis Dari Suatu Algoritma." },
{ "en": "Apa Itu Bahasa Pemrograman?", "id": "Bahasa Untuk Memberikan Instruksi Kepada Komputer." },
{ "en": "Sebutkan Contoh Bahasa Pemrograman Tingkat Rendah?", "id": "Bahasa Assembly." },
{ "en": "Sebutkan Contoh Bahasa Pemrograman Tingkat Tinggi?", "id": "C++, Python, Dan Java." },
{ "en": "Apa Itu Kompilator (Compiler)?", "id": "Menerjemahkan Kode Program Ke Bahasa Mesin." },
{ "en": "Apa Itu Interpreter?", "id": "Mengeksekusi Kode Program Baris Per Baris." },
{ "en": "Apa Itu Jaringan Sensor Nirkabel (Wireless Sensor Network)?", "id": "Jaringan Sensor Yang Berkomunikasi Secara Nirkabel." },
{ "en": "Apa Kepanjangan Dari IoT (Internet of Things)?", "id": "Internet of Things." },
{ "en": "Apa Konsep Dari IoT (Internet of Things)?", "id": "Menghubungkan Objek Fisik Ke Internet." },
{ "en": "Apa Itu Kecerdasan Buatan (Artificial Intelligence)?", "id": "Simulasi Kecerdasan Manusia Pada Mesin." },
{ "en": "Apa Itu Pembelajaran Mesin (Machine Learning)?", "id": "Cabang AI Untuk Belajar Dari Data." },
{ "en": "Apa Itu Sistem Tertanam (Embedded System)?", "id": "Sistem Komputer Khusus Dalam Perangkat Elektronik." },
{ "en": "Apa Itu Kinematika?", "id": "Studi Tentang Gerak Tanpa Mempertimbangkan Penyebabnya." },
{ "en": "Apa Itu Dinamika?", "id": "Studi Tentang Gerak Dan Penyebab Gerak." },
{ "en": "Apa Itu Derajat Kebebasan (Degree of Freedom)?", "id": "Jumlah Gerakan Independen Yang Dimiliki Benda." },
{ "en": "Apa Itu Transduser?", "id": "Mengubah Satu Bentuk Energi Ke Bentuk Lain." },
{ "en": "Apa Itu Osiloskop?", "id": "Alat Untuk Mengamati Bentuk Gelombang Sinyal Listrik." },
{ "en": "Apa Itu Multimeter?", "id": "Alat Ukur Listrik Untuk Tegangan, Arus, Hambatan." },
{ "en": "Apa Itu Sinyal Analog?", "id": "Sinyal Kontinu Yang Mewakili Besaran Fisik." },
{ "en": "Apa Itu Sinyal Digital?", "id": "Sinyal Diskrit Yang Diwakili Oleh Angka Biner." },
{ "en": "Apa Kepanjangan Dari ADC (Analog-to-Digital Converter)?", "id": "Analog-to-Digital Converter." },
{ "en": "Apa Fungsi Dari ADC (Analog-to-Digital Converter)?", "id": "Mengubah Sinyal Analog Menjadi Sinyal Digital." },
{ "en": "Apa Kepanjangan Dari DAC (Digital-to-Analog Converter)?", "id": "Digital-to-Analog Converter." },
{ "en": "Apa Fungsi Dari DAC (Digital-to-Analog Converter)?", "id": "Mengubah Sinyal Digital Menjadi Sinyal Analog." },
{ "en": "Apa Itu Modulasi Lebar Pulsa (PWM)?", "id": "Teknik Untuk Mengontrol Daya Ke Perangkat Listrik." },
{ "en": "Apa Itu Robotika?", "id": "Cabang Teknologi Yang Berurusan Dengan Robot." },
{ "en": "Apa Itu End Effector?", "id": "Alat Di Ujung Lengan Robot." },
{ "en": "Apa Itu Ruang Kerja (Workspace) Robot?", "id": "Area Yang Dapat Dicapai Oleh End Effector." },
{ "en": "Apa Itu Visi Komputer (Computer Vision)?", "id": "Kemampuan Komputer Untuk 'Melihat' Dan Menginterpretasikan Gambar." },
{ "en": "Apa Kepanjangan Dari HMI (Human-Machine Interface)?", "id": "Human-Machine Interface." },
{ "en": "Apa Fungsi HMI (Human-Machine Interface)?", "id": "Antarmuka Pengguna Untuk Berinteraksi Dengan Mesin." },
{ "en": "Apa Itu Sistem Real-Time?", "id": "Sistem Komputasi Dengan Batas Waktu Yang Ketat." },
{ "en": "Apa Itu Noise Dalam Sinyal?", "id": "Gangguan Yang Tidak Diinginkan Pada Sinyal." },
{ "en": "Apa Itu Filter Elektronik?", "id": "Rangkaian Untuk Menghilangkan Frekuensi Yang Tidak Diinginkan." },
{ "en": "Apa Itu Teori Kontrol?", "id": "Studi Tentang Pengendalian Sistem Dinamis." },
{ "en": "Apa Itu Respon Transien?", "id": "Perilaku Sistem Saat Berubah Dari Satu Keadaan." },
{ "en": "Apa Itu Respon Keadaan Tunak (Steady-State)?", "id": "Perilaku Sistem Setelah Waktu Yang Lama." },
{ "en": "Apa Itu Aktuator Listrik?", "id": "Aktuator Yang Digerakkan Oleh Energi Listrik." },
{ "en": "Sebutkan Contoh Aktuator Listrik?", "id": "Motor DC, Motor Stepper, Solenoid." },
{ "en": "Apa Itu Solenoid?", "id": "Kumparan Kawat Yang Menghasilkan Medan Magnet." },
{ "en": "Apa Itu Relay?", "id": "Saklar Yang Dioperasikan Secara Elektromagnetik." },
{ "en": "Apa Itu Sensor Optik?", "id": "Sensor Yang Menggunakan Cahaya Untuk Mendeteksi Objek." },
{ "en": "Apa Itu Encoder?", "id": "Sensor Untuk Mengukur Posisi Sudut Atau Kecepatan." },
{ "en": "Apa Itu Strain Gauge?", "id": "Sensor Untuk Mengukur Regangan Pada Suatu Objek." },
{ "en": "Apa Itu Termokopel?", "id": "Sensor Suhu Yang Menghasilkan Tegangan Listrik." },
{ "en": "Apa Itu Sistem Manufaktur Fleksibel (FMS)?", "id": "Sistem Produksi Yang Mudah Beradaptasi." },
{ "en": "Apa Kepanjangan Dari CNC (Computer Numerical Control)?", "id": "Computer Numerical Control." },
{ "en": "Apa Fungsi Mesin CNC (Computer Numerical Control)?", "id": "Mengontrol Perkakas Mesin Secara Otomatis." },
{ "en": "Apa Itu Otomasi?", "id": "Penggunaan Teknologi Untuk Menggantikan Tenaga Manusia." },
{ "en": "Apa Tujuan Utama Dari Otomasi Industri?", "id": "Meningkatkan Efisiensi, Kualitas, Dan Keamanan." },
{ "en": "Apa Itu Model Matematika Sistem?", "id": "Representasi Sistem Menggunakan Persamaan Matematika." },
{ "en": "Apa Itu Simulasi?", "id": "Peniruan Perilaku Sistem Dunia Nyata." },
{ "en": "Apa Manfaat Dari Simulasi Dalam Mekatronika?", "id": "Menguji Dan Memvalidasi Desain Sebelum Dibangun." },
{ "en": "Apa Itu Logika Fuzzy?", "id": "Bentuk Logika Yang Berurusan Dengan Penalaran 'Kira-Kira'." },
{ "en": "Apa Itu Jaringan Saraf Tiruan (Artificial Neural Network)?", "id": "Model Komputasi Yang Terinspirasi Otak Manusia." },
{ "en": "Apa Itu Desain Mekatronika?", "id": "Pendekatan Desain Terintegrasi Untuk Sistem Mekatronika." },
{ "en": "Apa Itu Sensor Inersia (Inertial Measurement Unit)?", "id": "Mengukur Orientasi Dan Kecepatan Sudut." },
{ "en": "Apa Fungsi Dari Giroskop?", "id": "Mengukur Atau Mempertahankan Orientasi Sudut." },
{ "en": "Apa Fungsi Dari Akselerometer?", "id": "Mengukur Percepatan Linear Suatu Benda." },
{ "en": "Apa Perbedaan Antara Akurasi Dan Presisi?", "id": "Akurasi Kedekatan Nilai Sebenarnya, Presisi Konsistensi Pengukuran." },
{ "en": "Apa Itu Resolusi Sensor?", "id": "Perubahan Terkecil Yang Dapat Dideteksi Sensor." },
{ "en": "Apa Itu Linearitas Sensor?", "id": "Seberapa Lurus Kurva Respon Output Sensor." },
{ "en": "Apa Itu Waktu Respon Sensor?", "id": "Waktu Yang Dibutuhkan Sensor Merespon Perubahan." },
{ "en": "Apa Itu Histeresis Dalam Sensor?", "id": "Perbedaan Output Saat Input Naik Dan Turun." },
{ "en": "Apa Itu Aktuator Piezoelektrik?", "id": "Mengubah Energi Listrik Menjadi Gerakan Mekanis Kecil." },
{ "en": "Apa Itu Material Cerdas (Smart Material)?", "id": "Material Yang Merespon Stimulus Eksternal." },
{ "en": "Sebutkan Contoh Material Cerdas?", "id": "Piezoelektrik Dan Shape Memory Alloy." },
{ "en": "Apa Itu Paduan Memori Bentuk (Shape Memory Alloy)?", "id": "Logam Yang 'Mengingat' Bentuk Aslinya." },
{ "en": "Apa Itu Robot Paralel?", "id": "Robot Dengan Beberapa Lengan Terhubung Ke Platform." },
{ "en": "Apa Itu Robot Serial?", "id": "Robot Dengan Rantai Sambungan Sendi Tunggal." },
{ "en": "Apa Keuntungan Robot Paralel?", "id": "Kecepatan Dan Akurasi Yang Sangat Tinggi." },
{ "en": "Apa Itu Robot Bergerak (Mobile Robot)?", "id": "Robot Yang Mampu Bergerak Di Lingkungannya." },
{ "en": "Apa Kepanjangan Dari AGV (Automated Guided Vehicle)?", "id": "Automated Guided Vehicle." },
{ "en": "Apa Fungsi AGV (Automated Guided Vehicle)?", "id": "Mengangkut Material Secara Otomatis Di Pabrik." },
{ "en": "Apa Itu Sistem Kontrol Adaptif?", "id": "Sistem Kontrol Yang Menyesuaikan Parameternya Sendiri." },
{ "en": "Apa Itu Sistem Kontrol Kokoh (Robust Control)?", "id": "Sistem Yang Tahan Terhadap Ketidakpastian Model." },
{ "en": "Apa Itu Kontroler PID (Proportional-Integral-Derivative)?", "id": "Mekanisme Umpan Balik Kontrol Loop Tertutup." },
{ "en": "Apa Fungsi Aksi Proportional (P) Pada PID?", "id": "Memberikan Respon Proporsional Terhadap Error." },
{ "en": "Apa Fungsi Aksi Integral (I) Pada PID?", "id": "Menghilangkan Error Keadaan Tunak (Steady-State)." },
{ "en": "Apa Fungsi Aksi Derivative (D) Pada PID?", "id": "Memprediksi Error Masa Depan Dan Mengurangi Overshoot." },
{ "en": "Apa Itu Penyetelan (Tuning) PID?", "id": "Proses Menemukan Parameter PID Yang Optimal." },
{ "en": "Apa Itu Bus Komunikasi?", "id": "Sistem Yang Mentransfer Data Antar Komponen." },
{ "en": "Sebutkan Contoh Protokol Komunikasi Serial?", "id": "RS-232, RS-485, Dan I2C." },
{ "en": "Apa Kepanjangan Dari I2C (Inter-Integrated Circuit)?", "id": "Inter-Integrated Circuit." },
{ "en": "Apa Kepanjangan Dari SPI (Serial Peripheral Interface)?", "id": "Serial Peripheral Interface." },
{ "en": "Apa Kepanjangan Dari UART (Universal Asynchronous Receiver-Transmitter)?", "id": "Universal Asynchronous Receiver-Transmitter." },
{ "en": "Apa Itu CAN Bus (Controller Area Network)?", "id": "Protokol Komunikasi Yang Digunakan Dalam Otomotif." },
{ "en": "Apa Itu Mekanisme?", "id": "Kumpulan Komponen Untuk Mentransmisikan Gerak Dan Gaya." },
{ "en": "Apa Itu Roda Gigi (Gear)?", "id": "Komponen Mekanis Untuk Mengubah Torsi Dan Kecepatan." },
{ "en": "Apa Itu Bantalan (Bearing)?", "id": "Komponen Untuk Mengurangi Gesekan Antar Bagian Bergerak." },
{ "en": "Apa Itu Kopling (Coupling)?", "id": "Komponen Untuk Menghubungkan Dua Poros." },
{ "en": "Apa Itu Rem (Brake)?", "id": "Perangkat Mekanis Untuk Memperlambat Atau Menghentikan Gerakan." },
{ "en": "Apa Itu Pemrosesan Sinyal Digital (DSP)?", "id": "Pengolahan Sinyal Menggunakan Teknik Digital." },
{ "en": "Apa Kepanjangan Dari DSP (Digital Signal Processor)?", "id": "Digital Signal Processor." },
{ "en": "Apa Itu Transformasi Fourier?", "id": "Menguraikan Sinyal Menjadi Komponen Frekuensinya." },
{ "en": "Apa Kepanjangan Dari FFT (Fast Fourier Transform)?", "id": "Fast Fourier Transform." },
{ "en": "Apa Itu Filter Low-Pass?", "id": "Melewatkan Sinyal Frekuensi Rendah Dan Meredam Tinggi." },
{ "en": "Apa Itu Filter High-Pass?", "id": "Melewatkan Sinyal Frekuensi Tinggi Dan Meredam Rendah." },
{ "en": "Apa Itu Filter Band-Pass?", "id": "Melewatkan Sinyal Dalam Rentang Frekuensi Tertentu." },
{ "en": "Apa Itu Tegangan (Voltage)?", "id": "Perbedaan Potensial Listrik Antara Dua Titik." },
{ "en": "Apa Itu Arus (Current)?", "id": "Aliran Muatan Listrik Dalam Suatu Rangkaian." },
{ "en": "Apa Itu Hambatan (Resistance)?", "id": "Ukuran Penolakan Terhadap Aliran Arus Listrik." },
{ "en": "Apa Bunyi Hukum Ohm?", "id": "Tegangan Sebanding Dengan Arus Dan Hambatan." },
{ "en": "Apa Itu Daya Listrik?", "id": "Laju Transfer Energi Listrik Dalam Rangkaian." },
{ "en": "Apa Itu Rangkaian Seri?", "id": "Komponen Terhubung Ujung Ke Ujung Dalam Satu Jalur." },
{ "en": "Apa Itu Rangkaian Paralel?", "id": "Komponen Terhubung Dalam Beberapa Jalur Paralel." },
{ "en": "Apa Itu Kapasitor?", "id": "Komponen Elektronik Yang Menyimpan Energi Medan Listrik." },
{ "en": "Apa Itu Induktor?", "id": "Komponen Yang Menyimpan Energi Dalam Medan Magnet." },
{ "en": "Apa Itu Elektronika Daya?", "id": "Aplikasi Elektronika Untuk Kontrol Dan Konversi Daya." },
{ "en": "Apa Itu Penyearah (Rectifier)?", "id": "Mengubah Arus Bolak-Balik (AC) Menjadi Searah (DC)." },
{ "en": "Apa Itu Inverter?", "id": "Mengubah Arus Searah (DC) Menjadi Bolak-Balik (AC)." },
{ "en": "Apa Itu Konverter DC-DC?", "id": "Mengubah Level Tegangan DC Ke Level Lainnya." },
{ "en": "Sebutkan Contoh Konverter DC-DC?", "id": "Buck Converter Dan Boost Converter." },
{ "en": "Apa Itu Sistem Tenaga Fluida?", "id": "Sistem Yang Menggunakan Cairan Atau Gas Bertekanan." },
{ "en": "Apa Itu Katup (Valve)?", "id": "Perangkat Untuk Mengontrol Aliran Fluida." },
{ "en": "Apa Itu Silinder Pneumatik?", "id": "Aktuator Yang Menghasilkan Gerakan Linear Dari Udara." },
{ "en": "Apa Itu Motor Hidrolik?", "id": "Aktuator Yang Menghasilkan Gerakan Rotasi Dari Cairan." },
{ "en": "Apa Itu Manufaktur Aditif?", "id": "Proses Membangun Objek Lapisan Demi Lapisan." },
{ "en": "Apa Nama Lain Manufaktur Aditif?", "id": "Pencetakan Tiga Dimensi (3D Printing)." },
{ "en": "Apa Itu Robotika Kolaboratif (Cobot)?", "id": "Robot Yang Dirancang Bekerja Bersama Manusia." },
{ "en": "Apa Itu Penggerak Langsung (Direct Drive)?", "id": "Motor Terhubung Langsung Ke Beban Tanpa Transmisi." },
{ "en": "Apa Itu Getaran Mekanis?", "id": "Gerakan Osilasi Benda Di Sekitar Titik Keseimbangan." },
{ "en": "Apa Itu Frekuensi Resonansi?", "id": "Frekuensi Yang Menyebabkan Amplitudo Getaran Maksimum." },
{ "en": "Apa Itu Peredaman (Damping)?", "id": "Mekanisme Untuk Mengurangi Atau Mencegah Osilasi." },
{ "en": "Apa Itu Tribologi?", "id": "Studi Tentang Gesekan, Keausan, Dan Pelumasan." },
{ "en": "Apa Itu Kelelahan Material (Material Fatigue)?", "id": "Kelemahan Material Akibat Beban Berulang." },
{ "en": "Apa Itu Analisis Elemen Hingga (FEA)?", "id": "Metode Komputasi Untuk Analisis Teknik." },
{ "en": "Apa Kepanjangan Dari FEA (Finite Element Analysis)?", "id": "Finite Element Analysis." },
{ "en": "Apa Itu Pemodelan Solid (Solid Modeling)?", "id": "Representasi Tiga Dimensi Objek Secara Digital." },
{ "en": "Apa Itu Prototipe Cepat (Rapid Prototyping)?", "id": "Pembuatan Cepat Model Fisik Dari Desain CAD." },
{ "en": "Apa Itu Rekayasa Balik (Reverse Engineering)?", "id": "Proses Menganalisis Produk Untuk Membuat Ulang." },
{ "en": "Apa Itu Integrasi Sistem?", "id": "Proses Menghubungkan Berbagai Subsistem Menjadi Satu." },
{ "en": "Apa Itu Antarmuka (Interface)?", "id": "Titik Interaksi Antara Dua Komponen Atau Sistem." },
{ "en": "Apa Itu Modularitas Dalam Desain?", "id": "Merancang Sistem Dari Komponen Standar Yang Terpisah." },
{ "en": "Apa Itu Skalabilitas?", "id": "Kemampuan Sistem Untuk Menangani Peningkatan Beban." },
{ "en": "Apa Itu Keandalan (Reliability)?", "id": "Probabilitas Sistem Bekerja Tanpa Kegagalan." },
{ "en": "Apa Itu Perawatan (Maintainability)?", "id": "Kemudahan Dalam Memperbaiki Atau Merawat Sistem." },
{ "en": "Apa Itu Diagnostik Sistem?", "id": "Proses Mengidentifikasi Penyebab Kegagalan Sistem." },
{ "en": "Apa Itu Toleransi Kesalahan (Fault Tolerance)?", "id": "Kemampuan Sistem Terus Beroperasi Meski Ada Kesalahan." },
{ "en": "Apa Itu Redundansi?", "id": "Duplikasi Komponen Kritis Untuk Meningkatkan Keandalan." },
{ "en": "Apa Itu Interupsi (Interrupt) Dalam Mikrokontroler?", "id": "Sinyal Yang Menghentikan Sementara Program Utama." },
{ "en": "Apa Itu Timer Dalam Mikrokontroler?", "id": "Periferal Untuk Menghitung Interval Waktu." },
{ "en": "Apa Itu Port Input/Output (I/O)?", "id": "Pin Untuk Komunikasi Mikrokontroler Dengan Dunia Luar." },
{ "en": "Apa Itu Siklus Tugas (Duty Cycle)?", "id": "Persentase Waktu Sinyal Dalam Keadaan Aktif." },
{ "en": "Apa Itu Sistem Kontrol Diskrit?", "id": "Sistem Yang Bekerja Pada Sampel Data Diskrit." },
{ "en": "Apa Itu Proses Manufaktur Subtraktif?", "id": "Membentuk Benda Dengan Membuang Material." },
{ "en": "Sebutkan Contoh Manufaktur Subtraktif?", "id": "Milling, Turning, Dan Drilling." },
{ "en": "Apa Itu Aktuator Magnetostriktif?", "id": "Mengubah Energi Magnetik Menjadi Gerakan Mekanis." },
{ "en": "Apa Itu Sensor Ultrasonik?", "id": "Menggunakan Gelombang Suara Untuk Mengukur Jarak." },
{ "en": "Apa Itu Sensor Efek Hall?", "id": "Mendeteksi Medan Magnet Dan Menghasilkan Tegangan." },
{ "en": "Apa Itu Photodiode?", "id": "Semikonduktor Yang Mengubah Cahaya Menjadi Arus Listrik." },
{ "en": "Apa Itu Phototransistor?", "id": "Transistor Yang Dikontrol Oleh Paparan Cahaya." },
{ "en": "Apa Itu Optocoupler (Opto-isolator)?", "id": "Mengisolasi Rangkaian Menggunakan Cahaya Secara Elektrik." },
{ "en": "Apa Itu Kalibrasi Sensor?", "id": "Menyesuaikan Output Sensor Agar Sesuai Standar." },
{ "en": "Apa Itu Mekanisme Engkol Peluncur (Slider-Crank)?", "id": "Mengubah Gerak Rotasi Menjadi Gerak Translasi." },
{ "en": "Apa Itu Mekanisme Empat Batang (Four-Bar Linkage)?", "id": "Mekanisme Dengan Empat Batang Terhubung Sendi Putar." },
{ "en": "Apa Itu Cam Dan Follower?", "id": "Mekanisme Untuk Menghasilkan Gerakan Resiprokal Yang Khas." },
{ "en": "Apa Itu Sekrup Bola (Ball Screw)?", "id": "Mengubah Gerak Rotasi Menjadi Gerak Linear Presisi." },
{ "en": "Apa Itu Sabuk Penggerak (Drive Belt)?", "id": "Mentransmisikan Daya Antara Poros Menggunakan Sabuk." },
{ "en": "Apa Itu Rantai Penggerak (Drive Chain)?", "id": "Mentransmisikan Daya Menggunakan Rantai Dan Sproket." },
{ "en": "Apa Itu Transmisi Harmonik (Harmonic Drive)?", "id": "Mekanisme Roda Gigi Dengan Rasio Reduksi Tinggi." },
{ "en": "Apa Itu Arsitektur Von Neumann?", "id": "Arsitektur Komputer Dengan Memori Program Dan Data." },
{ "en": "Apa Itu Arsitektur Harvard?", "id": "Arsitektur Dengan Memori Program Dan Data Terpisah." },
{ "en": "Apa Keuntungan Arsitektur Harvard?", "id": "Akses Memori Program Dan Data Secara Bersamaan." },
{ "en": "Apa Itu Watchdog Timer (WDT)?", "id": "Mengatur Ulang Mikrokontroler Jika Terjadi Kegagalan Perangkat Lunak." },
{ "en": "Apa Itu Osilator Kristal?", "id": "Menghasilkan Sinyal Clock Yang Stabil Untuk Mikrokontroler." },
{ "en": "Apa Itu Register Dalam CPU?", "id": "Lokasi Penyimpanan Kecil Dan Sangat Cepat." },
{ "en": "Apa Itu Program Counter (PC)?", "id": "Register Yang Menyimpan Alamat Instruksi Berikutnya." },
{ "en": "Apa Itu Stack Pointer (SP)?", "id": "Register Yang Menunjuk Ke Puncak Stack." },
{ "en": "Apa Itu Debugging?", "id": "Proses Menemukan Dan Memperbaiki Kesalahan Program." },
{ "en": "Apa Itu Simulator?", "id": "Perangkat Lunak Yang Meniru Perilaku Perangkat Keras." },
{ "en": "Apa Itu Emulator?", "id": "Perangkat Keras Yang Meniru Perilaku Sistem Lain." },
{ "en": "Apa Itu Motor DC Tanpa Sikat (BLDC)?", "id": "Motor DC Yang Menggunakan Komutasi Elektronik." },
{ "en": "Apa Kepanjangan Dari BLDC (Brushless DC Motor)?", "id": "Brushless DC Motor." },
{ "en": "Apa Keuntungan Motor BLDC?", "id": "Efisiensi Tinggi, Perawatan Rendah, Dan Tahan Lama." },
{ "en": "Apa Itu Motor Universal?", "id": "Motor Yang Dapat Beroperasi Pada Daya AC/DC." },
{ "en": "Apa Itu Penggerak Mikrostepping (Microstepping Driver)?", "id": "Meningkatkan Resolusi Gerakan Motor Stepper." },
{ "en": "Apa Itu Backlash Dalam Roda Gigi?", "id": "Celah Atau Kelonggaran Antara Gigi Roda Gigi." },
{ "en": "Apa Itu Efek Coriolis?", "id": "Gaya Semu Pada Benda Bergerak Dalam Kerangka Berputar." },
{ "en": "Apa Itu Momen Inersia?", "id": "Ukuran Kelembaman Benda Terhadap Perubahan Rotasi." },
{ "en": "Apa Itu Torsi (Torque)?", "id": "Gaya Rotasi Yang Menyebabkan Benda Berputar." },
{ "en": "Apa Itu Kerja (Work) Dalam Fisika?", "id": "Energi Yang Dipindahkan Oleh Gaya." },
{ "en": "Apa Itu Daya (Power) Dalam Fisika?", "id": "Laju Di Mana Kerja Dilakukan." },
{ "en": "Apa Itu Efisiensi Mekanis?", "id": "Rasio Output Daya Terhadap Input Daya." },
{ "en": "Apa Itu Tegangan (Stress) Mekanis?", "id": "Gaya Internal Per Satuan Luas Material." },
{ "en": "Apa Itu Regangan (Strain) Mekanis?", "id": "Deformasi Material Akibat Tegangan." },
{ "en": "Apa Itu Modulus Young?", "id": "Ukuran Kekakuan Suatu Material Elastis." },
{ "en": "Apa Itu Batas Elastis?", "id": "Tegangan Maksimum Sebelum Deformasi Permanen." },
{ "en": "Apa Itu Kekuatan Luluh (Yield Strength)?", "id": "Tegangan Di Mana Material Mulai Berdeformasi Plastis." },
{ "en": "Apa Itu Kekuatan Tarik (Tensile Strength)?", "id": "Tegangan Maksimum Yang Dapat Ditahan Material." },
{ "en": "Apa Itu Keuletan (Ductility)?", "id": "Kemampuan Material Berdeformasi Plastis Tanpa Patah." },
{ "en": "Apa Itu Kerapuhan (Brittleness)?", "id": "Kecenderungan Material Patah Tanpa Deformasi." },
{ "en": "Apa Itu Kekerasan (Hardness)?", "id": "Ketahanan Material Terhadap Goresan Atau Lekukan." },
{ "en": "Apa Itu Pelumasan?", "id": "Proses Mengurangi Gesekan Antara Permukaan." },
{ "en": "Apa Itu Viskositas?", "id": "Ukuran Ketahanan Fluida Terhadap Aliran." },
{ "en": "Apa Itu Sistem Kontrol Terdistribusi (DCS)?", "id": "Sistem Kontrol Dengan Kontroler Otonom Terdistribusi." },
{ "en": "Apa Kepanjangan Dari DCS (Distributed Control System)?", "id": "Distributed Control System." },
{ "en": "Apa Kepanjangan Dari SCADA (Supervisory Control And Data Acquisition)?", "id": "Supervisory Control And Data Acquisition." },
{ "en": "Apa Fungsi Sistem SCADA?", "id": "Memantau Dan Mengontrol Proses Industri Skala Besar." },
{ "en": "Apa Itu Protokol Modbus?", "id": "Protokol Komunikasi Untuk Otomasi Industri." },
{ "en": "Apa Itu Protokol Profibus?", "id": "Standar Fieldbus Untuk Komunikasi Otomasi." },
{ "en": "Apa Itu Fieldbus?", "id": "Jaringan Digital Industri Untuk Kontrol Real-Time." },
{ "en": "Apa Itu Diagram Tangga (Ladder Logic)?", "id": "Bahasa Pemrograman Grafis Untuk PLC." },
{ "en": "Apa Itu Ergonomi?", "id": "Studi Tentang Interaksi Manusia Dengan Sistem." },
{ "en": "Apa Itu Faktor Keamanan (Factor of Safety)?", "id": "Rasio Kekuatan Ultimate Terhadap Beban Kerja." },
{ "en": "Apa Itu Analisis Modal?", "id": "Studi Tentang Sifat Dinamis Struktur." },
{ "en": "Apa Itu Batang Pengikat (Tie Rod)?", "id": "Batang Ramping Yang Menahan Beban Tarik." },
{ "en": "Apa Itu Poros (Shaft)?", "id": "Komponen Berputar Untuk Mentransmisikan Torsi Dan Daya." },
{ "en": "Apa Itu Pasak (Key)?", "id": "Mencegah Rotasi Relatif Antara Poros Dan Hub." },
{ "en": "Apa Itu Baut (Bolt)?", "id": "Pengencang Berulir Yang Dipasangkan Dengan Mur." },
{ "en": "Apa Itu Sekrup (Screw)?", "id": "Pengencang Berulir Yang Langsung Masuk Ke Lubang." },
{ "en": "Apa Itu Pengelasan (Welding)?", "id": "Proses Penyambungan Logam Dengan Panas." },
{ "en": "Apa Itu Pemesinan (Machining)?", "id": "Proses Pembentukan Material Dengan Cara Memotong." },
{ "en": "Apa Itu Pengecoran (Casting)?", "id": "Proses Menuangkan Logam Cair Ke Dalam Cetakan." },
{ "en": "Apa Itu Penempaan (Forging)?", "id": "Proses Membentuk Logam Menggunakan Gaya Tekan." },
{ "en": "Apa Itu Metrologi?", "id": "Ilmu Tentang Pengukuran." },
{ "en": "Apa Itu Interferometer Laser?", "id": "Alat Ukur Jarak Dengan Presisi Sangat Tinggi." },
{ "en": "Apa Itu Mesin Pengukur Koordinat (CMM)?", "id": "Mengukur Geometri Objek Fisik." },
{ "en": "Apa Kepanjangan Dari CMM (Coordinate Measuring Machine)?", "id": "Coordinate Measuring Machine." },
{ "en": "Apa Itu Robotika Medis?", "id": "Aplikasi Robot Dalam Bidang Kedokteran." },
{ "en": "Apa Itu Robot Bedah Da Vinci?", "id": "Sistem Robotik Untuk Membantu Operasi Bedah." },
{ "en": "Apa Itu Eksoskeleton Robotik?", "id": "Kerangka Luar Yang Dapat Dipakai Untuk Membantu Gerakan." },
{ "en": "Apa Itu Bionik?", "id": "Aplikasi Sistem Biologi Ke Desain Teknik." },
{ "en": "Apa Itu Prostetik?", "id": "Alat Buatan Yang Menggantikan Bagian Tubuh." },
{ "en": "Apa Itu Sistem Mikroelektromekanis (MEMS)?", "id": "Perangkat Mekanis Dan Elektronis Berukuran Mikro." },
{ "en": "Apa Kepanjangan Dari MEMS (Microelectromechanical Systems)?", "id": "Microelectromechanical Systems." },
{ "en": "Sebutkan Contoh Aplikasi MEMS?", "id": "Akselerometer Di Ponsel Dan Sensor Tekanan." },
{ "en": "Apa Itu Nanoteknologi?", "id": "Manipulasi Materi Pada Skala Atom Dan Molekul." },
{ "en": "Apa Itu Robotika Kawanan (Swarm Robotics)?", "id": "Koordinasi Banyak Robot Sederhana Secara Bersamaan." },
{ "en": "Apa Itu Kendaraan Otonom?", "id": "Kendaraan Yang Mampu Beroperasi Tanpa Pengemudi." },
{ "en": "Apa Itu Sistem Pengereman Anti-Terkunci (ABS)?", "id": "Mencegah Roda Terkunci Saat Pengereman Mendadak." },
{ "en": "Apa Kepanjangan Dari ABS (Anti-lock Braking System)?", "id": "Anti-lock Braking System." },
{ "en": "Apa Itu Kontrol Stabilitas Elektronik (ESC)?", "id": "Meningkatkan Stabilitas Kendaraan Dengan Mengerem Roda Individu." },
{ "en": "Apa Kepanjangan Dari ESC (Electronic Stability Control)?", "id": "Electronic Stability Control." },
{ "en": "Apa Itu Sistem Injeksi Bahan Bakar Elektronik (EFI)?", "id": "Menyemprotkan Bahan Bakar Ke Mesin Secara Elektronik." },
{ "en": "Apa Kepanjangan Dari EFI (Electronic Fuel Injection)?", "id": "Electronic Fuel Injection." },
{ "en": "Apa Itu Transmisi Otomatis?", "id": "Transmisi Yang Dapat Berpindah Gigi Secara Otomatis." },
{ "en": "Apa Itu Suspensi Aktif?", "id": "Sistem Suspensi Yang Dikontrol Secara Elektronik." },
{ "en": "Apa Itu Sensor Lidar (Light Detection And Ranging)?", "id": "Mengukur Jarak Menggunakan Sinar Laser." },
{ "en": "Apa Itu Sensor Radar (Radio Detection And Ranging)?", "id": "Menggunakan Gelombang Radio Untuk Mendeteksi Objek." },
{ "en": "Apa Itu Fusi Sensor (Sensor Fusion)?", "id": "Menggabungkan Data Dari Berbagai Sensor." },
{ "en": "Apa Itu Algoritma SLAM (Simultaneous Localization And Mapping)?", "id": "Memetakan Lingkungan Sambil Melacak Lokasi." },
{ "en": "Apa Itu Kalman Filter?", "id": "Algoritma Untuk Mengestimasi Keadaan Sistem Dinamis." },
{ "en": "Apa Itu Sistem Kendali Cerdas?", "id": "Menggunakan Pendekatan Kecerdasan Buatan Untuk Pengendalian." },
{ "en": "Sebutkan Contoh Sistem Kendali Cerdas?", "id": "Logika Fuzzy, Jaringan Saraf Tiruan." },
{ "en": "Apa Itu Robotika Perilaku (Behavior-Based Robotics)?", "id": "Arsitektur Kontrol Robot Berdasarkan Rangkaian Perilaku." },
{ "en": "Apa Itu Telerobotika (Telerobotics)?", "id": "Pengendalian Robot Dari Jarak Jauh." },
{ "en": "Apa Itu Haptik (Haptics)?", "id": "Teknologi Umpan Balik Sentuhan Dan Gaya." },
{ "en": "Apa Fungsi Teknologi Haptik Dalam Mekatronika?", "id": "Memberikan Rasa Sentuhan Kepada Operator Jarak Jauh." },
{ "en": "Apa Itu Aktuator Udara Buatan (Artificial Pneumatic Muscle)?", "id": "Aktuator Ringan Dan Kuat Meniru Otot." },
{ "en": "Apa Itu Elektromagnet?", "id": "Magnet Yang Dihasilkan Oleh Aliran Arus Listrik." },
{ "en": "Apa Itu Latching Relay?", "id": "Relay Yang Mempertahankan Posisinya Tanpa Daya." },
{ "en": "Apa Itu Solid State Relay (SSR)?", "id": "Relay Elektronik Tanpa Bagian Bergerak." },
{ "en": "Apa Keuntungan Solid State Relay (SSR)?", "id": "Lebih Cepat, Tahan Lama, Dan Senyap." },
{ "en": "Apa Itu Sensor Piroelektrik (PIR Sensor)?", "id": "Mendeteksi Perubahan Radiasi Inframerah (Gerakan)." },
{ "en": "Apa Kepanjangan Dari PIR (Passive Infrared)?", "id": "Passive Infrared." },
{ "en": "Apa Itu Sensor Magnetoresistif?", "id": "Mengubah Resistansi Listriknya Karena Medan Magnet." },
{ "en": "Apa Itu Load Cell?", "id": "Transduser Untuk Mengukur Gaya Atau Berat." },
{ "en": "Apa Itu Flow Meter?", "id": "Sensor Untuk Mengukur Laju Aliran Fluida." },
{ "en": "Apa Itu Sensor Level?", "id": "Sensor Untuk Mendeteksi Ketinggian Cairan Atau Padatan." },
{ "en": "Apa Itu Saklar Batas (Limit Switch)?", "id": "Saklar Yang Diaktifkan Oleh Gerakan Mesin." },
{ "en": "Apa Itu Tombol Tekan (Push Button)?", "id": "Saklar Sederhana Yang Diaktifkan Dengan Menekan." },
{ "en": "Apa Itu Saklar Toggle?", "id": "Saklar Yang Dioperasikan Dengan Tuas Kecil." },
{ "en": "Apa Itu Saklar Pemilih (Selector Switch)?", "id": "Saklar Putar Untuk Memilih Satu Dari Beberapa Opsi." },
{ "en": "Apa Itu Potensiometer?", "id": "Resistor Variabel Tiga Terminal." },
{ "en": "Bagaimana Potensiometer Digunakan Sebagai Sensor?", "id": "Untuk Mengukur Posisi Sudut Atau Linear." },
{ "en": "Apa Itu Rangkaian Pembagi Tegangan (Voltage Divider)?", "id": "Rangkaian Untuk Menghasilkan Tegangan Output Lebih Rendah." },
{ "en": "Apa Itu Jembatan Wheatstone?", "id": "Rangkaian Untuk Mengukur Perubahan Resistansi Kecil." },
{ "en": "Apa Itu Penguat Operasional (Op-Amp)?", "id": "Penguat Tegangan DC Dengan Penguatan Sangat Tinggi." },
{ "en": "Apa Itu Konfigurasi Penguat Inverting?", "id": "Output Membalik Fase Sinyal Input." },
{ "en": "Apa Itu Konfigurasi Penguat Non-Inverting?", "id": "Output Se-fase Dengan Sinyal Input." },
{ "en": "Apa Itu Pengikut Tegangan (Voltage Follower)?", "id": "Rangkaian Op-Amp Dengan Penguatan Satu." },
{ "en": "Apa Fungsi Dari Pengikut Tegangan?", "id": "Sebagai Penyangga (Buffer) Untuk Isolasi Rangkaian." },
{ "en": "Apa Itu Komparator (Comparator)?", "id": "Rangkaian Op-Amp Yang Membandingkan Dua Tegangan." },
{ "en": "Apa Itu Osilator?", "id": "Rangkaian Elektronik Yang Menghasilkan Sinyal Periodik." },
{ "en": "Apa Itu Timer 555?", "id": "Sirkuit Terpadu Untuk Membuat Timer Dan Osilator." },
{ "en": "Apa Itu Multiplexer (MUX)?", "id": "Memilih Salah Satu Dari Banyak Sinyal Input." },
{ "en": "Apa Itu Demultiplexer (DEMUX)?", "id": "Mengirimkan Sinyal Input Ke Salah Satu Output." },
{ "en": "Apa Itu Encoder Dan Decoder?", "id": "Mengubah Data Dari Satu Format Ke Format Lain." },
{ "en": "Apa Itu Flip-Flop?", "id": "Elemen Penyimpanan Biner Dasar Dalam Logika Sekuensial." },
{ "en": "Apa Itu Latch?", "id": "Jenis Flip-Flop Yang Sensitif Terhadap Level Sinyal." },
{ "en": "Apa Itu Register Geser (Shift Register)?", "id": "Register Yang Dapat Menggeser Bitnya." },
{ "en": "Apa Itu Pencacah (Counter)?", "id": "Sirkuit Logika Sekuensial Untuk Menghitung Pulsa." },
{ "en": "Apa Itu Memori Akses Acak Statis (SRAM)?", "id": "RAM Yang Menyimpan Data Menggunakan Flip-Flop." },
{ "en": "Apa Kepanjangan Dari SRAM (Static Random Access Memory)?", "id": "Static Random Access Memory." },
{ "en": "Apa Itu Memori Akses Acak Dinamis (DRAM)?", "id": "RAM Yang Menyimpan Data Menggunakan Kapasitor." },
{ "en": "Apa Kepanjangan Dari DRAM (Dynamic Random Access Memory)?", "id": "Dynamic Random Access Memory." },
{ "en": "Mana Yang Lebih Cepat, SRAM Atau DRAM?", "id": "SRAM Lebih Cepat Tetapi Lebih Mahal." },
{ "en": "Apa Itu Memori Flash?", "id": "Memori Non-Volatile Yang Dapat Dihapus Secara Elektrik." },
{ "en": "Apa Itu Keausan Penulisan (Write Wear) Pada Memori Flash?", "id": "Degradasi Sel Memori Setelah Banyak Siklus Tulis." },
{ "en": "Apa Kepanjangan Dari FPGA (Field-Programmable Gate Array)?", "id": "Field-Programmable Gate Array." },
{ "en": "Apa Fungsi Dari FPGA (Field-Programmable Gate Array)?", "id": "Sirkuit Terpadu Yang Dapat Dikonfigurasi Oleh Pengguna." },
{ "en": "Apa Itu Bahasa Deskripsi Perangkat Keras (HDL)?", "id": "Bahasa Pemrograman Untuk Menggambarkan Sirkuit Digital." },
{ "en": "Apa Kepanjangan Dari HDL (Hardware Description Language)?", "id": "Hardware Description Language." },
{ "en": "Sebutkan Contoh Bahasa HDL?", "id": "VHDL Dan Verilog." },
{ "en": "Apa Itu Sistem-Pada-Chip (SoC)?", "id": "Mengintegrasikan Semua Komponen Sistem Ke Satu Chip." },
{ "en": "Apa Kepanjangan Dari SoC (System-on-Chip)?", "id": "System-on-Chip." },
{ "en": "Apa Itu Mesin Keadaan (State Machine)?", "id": "Model Komputasi Berdasarkan Keadaan Dan Transisi." },
{ "en": "Apa Itu Diagram Keadaan (State Diagram)?", "id": "Representasi Grafis Dari Mesin Keadaan." },
{ "en": "Apa Itu Sistem Linear?", "id": "Sistem Yang Mematuhi Prinsip Superposisi." },
{ "en": "Apa Itu Sistem Non-Linear?", "id": "Sistem Yang Tidak Mematuhi Prinsip Superposisi." },
{ "en": "Apa Itu Fungsi Transfer?", "id": "Rasio Output Terhadap Input Dalam Domain Frekuensi." },
{ "en": "Apa Itu Diagram Bode?", "id": "Grafik Respon Frekuensi Suatu Sistem." },
{ "en": "Apa Itu Margin Fasa (Phase Margin)?", "id": "Ukuran Stabilitas Relatif Sistem Kontrol." },
{ "en": "Apa Itu Margin Penguatan (Gain Margin)?", "id": "Ukuran Stabilitas Lainnya Dalam Sistem Kontrol." },
{ "en": "Apa Itu Diagram Nyquist?", "id": "Plot Polar Respon Frekuensi Loop Terbuka." },
{ "en": "Apa Itu Kriteria Stabilitas Nyquist?", "id": "Menentukan Stabilitas Sistem Loop Tertutup." },
{ "en": "Apa Itu Lokus Akar (Root Locus)?", "id": "Plot Kutub Loop Tertutup Sebagai Fungsi Penguatan." },
{ "en": "Apa Itu Kutub (Pole) Sistem?", "id": "Nilai Yang Membuat Fungsi Transfer Tak Terhingga." },
{ "en": "Apa Itu Nol (Zero) Sistem?", "id": "Nilai Yang Membuat Fungsi Transfer Menjadi Nol." },
{ "en": "Apa Itu Sistem Waktu-Kontinu?", "id": "Sistem Di Mana Variabel Berubah Secara Kontinu." },
{ "en": "Apa Itu Sistem Waktu-Diskrit?", "id": "Sistem Di Mana Variabel Berubah Pada Waktu Diskrit." },
{ "en": "Apa Itu Transformasi-Z?", "id": "Alat Matematis Untuk Menganalisis Sinyal Waktu-Diskrit." },
{ "en": "Apa Itu Kovariansi?", "id": "Ukuran Bagaimana Dua Variabel Berubah Bersama." },
{ "en": "Apa Itu Korelasi?", "id": "Mengukur Kekuatan Hubungan Antara Dua Variabel." },
{ "en": "Apa Itu Proses Stokastik?", "id": "Proses Yang Melibatkan Variabel Acak." },
{ "en": "Apa Itu Derau Putih (White Noise)?", "id": "Sinyal Acak Dengan Kepadatan Spektral Daya Konstan." },
{ "en": "Apa Itu Ergonomika Kognitif?", "id": "Fokus Pada Proses Mental Seperti Persepsi Memori." },
{ "en": "Apa Itu Augmented Reality (AR)?", "id": "Menggabungkan Informasi Digital Dengan Dunia Nyata." },
{ "en": "Apa Itu Virtual Reality (VR)?", "id": "Menciptakan Lingkungan Buatan Yang Imersif." },
{ "en": "Bagaimana AR Digunakan Dalam Mekatronika?", "id": "Untuk Bantuan Perakitan Dan Perawatan." },
{ "en": "Apa Itu Kembar Digital (Digital Twin)?", "id": "Representasi Virtual Dari Objek Atau Sistem Fisik." },
{ "en": "Apa Manfaat Dari Kembar Digital?", "id": "Untuk Simulasi, Pemantauan, Dan Prediksi Kinerja." },
{ "en": "Apa Itu Sistem Siber-Fisik (Cyber-Physical System)?", "id": "Integrasi Komputasi Dengan Proses Fisik." },
{ "en": "Apa Hubungan Mekatronika Dengan Industri 4.0?", "id": "Mekatronika Adalah Teknologi Inti Industri 4.0." },
{ "en": "Apa Itu Standar Keselamatan Fungsional?", "id": "Standar Untuk Mengurangi Risiko Pada Sistem Kontrol." },
{ "en": "Apa Itu Standar IEC 61508?", "id": "Standar Internasional Untuk Keselamatan Fungsional." },
{ "en": "Apa Itu Tingkat Integritas Keselamatan (SIL)?", "id": "Tingkat Pengurangan Risiko Oleh Fungsi Keselamatan." },
{ "en": "Apa Kepanjangan Dari SIL (Safety Integrity Level)?", "id": "Safety Integrity Level." },
{ "en": "Apa Itu Analisis Bahaya Dan Operabilitas (HAZOP)?", "id": "Metode Terstruktur Untuk Mengidentifikasi Risiko." },
{ "en": "Apa Kepanjangan Dari HAZOP (Hazard and Operability Study)?", "id": "Hazard and Operability Study." },
{ "en": "Apa Itu Analisis Mode Kegagalan Dan Efek (FMEA)?", "id": "Pendekatan Sistematis Untuk Menganalisis Kegagalan Sistem." },
{ "en": "Apa Kepanjangan Dari FMEA (Failure Mode and Effects Analysis)?", "id": "Failure Mode and Effects Analysis." },
{ "en": "Apa Itu Termografi Inframerah?", "id": "Melihat Radiasi Panas Untuk Inspeksi." },
{ "en": "Apa Itu Analisis Getaran?", "id": "Memantau Getaran Mesin Untuk Mendeteksi Masalah." },
{ "en": "Apa Itu Perawatan Prediktif?", "id": "Menjadwalkan Perawatan Berdasarkan Prediksi Kegagalan." },
{ "en": "Apa Itu Sistem Eksekusi Manufaktur (MES)?", "id": "Sistem Informasi Untuk Melacak Proses Produksi." },
{ "en": "Apa Kepanjangan Dari MES (Manufacturing Execution System)?", "id": "Manufacturing Execution System." },
{ "en": "Apa Itu Motor Sinkron?", "id": "Motor AC Dengan Kecepatan Rotor Sinkron Medan Magnet." },
{ "en": "Apa Itu Motor Asinkron (Induksi)?", "id": "Motor AC Dengan Kecepatan Rotor Lebih Lambat." },
{ "en": "Apa Itu Slip Dalam Motor Induksi?", "id": "Perbedaan Antara Kecepatan Sinkron Dan Kecepatan Rotor." },
{ "en": "Apa Itu Penggerak Frekuensi Variabel (VFD)?", "id": "Mengontrol Kecepatan Motor AC Dengan Mengubah Frekuensi." },
{ "en": "Apa Kepanjangan Dari VFD (Variable Frequency Drive)?", "id": "Variable Frequency Drive." },
{ "en": "Apa Itu Pengereman Regeneratif?", "id": "Mengubah Energi Kinetik Menjadi Listrik Saat Pengereman." },
{ "en": "Apa Itu Pengereman Dinamis?", "id": "Membuang Energi Kinetik Sebagai Panas Melalui Resistor." },
{ "en": "Apa Itu Kontrol Vektor (Vector Control)?", "id": "Metode Kontrol Motor AC Kinerja Tinggi." },
{ "en": "Apa Itu Ruang Keadaan (State Space)?", "id": "Representasi Matematis Sistem Menggunakan Variabel Keadaan." },
{ "en": "Apa Itu Keteramatan (Observability)?", "id": "Kemampuan Menentukan Keadaan Internal Dari Output Eksternal." },
{ "en": "Apa Itu Ketercapaian (Controllability)?", "id": "Kemampuan Menggerakkan Sistem Ke Keadaan Apapun." },
{ "en": "Apa Itu Estimator Keadaan (State Estimator)?", "id": "Memperkirakan Variabel Keadaan Internal Yang Tidak Terukur." },
{ "en": "Sebutkan Contoh Estimator Keadaan?", "id": "Kalman Filter Dan Luenberger Observer." },
{ "en": "Apa Itu Sistem MIMO (Multiple-Input Multiple-Output)?", "id": "Sistem Dengan Banyak Input Dan Banyak Output." },
{ "en": "Apa Itu Sistem SISO (Single-Input Single-Output)?", "id": "Sistem Dengan Satu Input Dan Satu Output." },
{ "en": "Apa Itu Dekomposisi Nilai Singular (SVD)?", "id": "Faktorisasi Matriks Untuk Analisis Sistem MIMO." },
{ "en": "Apa Itu Nilai Eigen (Eigenvalue)?", "id": "Skalar Yang Mewakili Sifat Intrinsik Sistem Linear." },
{ "en": "Apa Itu Vektor Eigen (Eigenvector)?", "id": "Vektor Yang Arahnya Tidak Berubah Oleh Transformasi." },
{ "en": "Apa Itu Mekanika Fluida Komputasi (CFD)?", "id": "Simulasi Aliran Fluida Menggunakan Metode Numerik." },
{ "en": "Apa Kepanjangan Dari CFD (Computational Fluid Dynamics)?", "id": "Computational Fluid Dynamics." },
{ "en": "Apa Itu Bilangan Reynolds?", "id": "Rasio Antara Gaya Inersia Dan Gaya Viskos." },
{ "en": "Apa Itu Aliran Laminer?", "id": "Aliran Fluida Yang Mulus Dan Teratur." },
{ "en": "Apa Itu Aliran Turbulen?", "id": "Aliran Fluida Yang Kacau Dan Tidak Teratur." },
{ "en": "Apa Itu Perpindahan Panas Konduksi?", "id": "Transfer Panas Melalui Kontak Langsung." },
{ "en": "Apa Itu Perpindahan Panas Konveksi?", "id": "Transfer Panas Melalui Pergerakan Fluida." },
{ "en": "Apa Itu Perpindahan Panas Radiasi?", "id": "Transfer Panas Melalui Gelombang Elektromagnetik." },
{ "en": "Apa Itu Sirip Pendingin (Heat Sink)?", "id": "Komponen Untuk Melesapkan Panas Ke Lingkungan." },
{ "en": "Apa Itu Pipa Panas (Heat Pipe)?", "id": "Perangkat Transfer Panas Pasif Sangat Efisien." },
{ "en": "Apa Itu Efek Peltier?", "id": "Pemanasan Atau Pendinginan Sambungan Dua Konduktor Berbeda." },
{ "en": "Apa Itu Termodinamika?", "id": "Studi Tentang Hubungan Antara Panas Dan Energi." },
{ "en": "Apa Bunyi Hukum Pertama Termodinamika?", "id": "Kekekalan Energi Dalam Suatu Sistem." },
{ "en": "Apa Bunyi Hukum Kedua Termodinamika?", "id": "Entropi Total Sistem Terisolasi Selalu Meningkat." },
{ "en": "Apa Itu Entropi?", "id": "Ukuran Ketidakteraturan Atau Keacakan Suatu Sistem." },
{ "en": "Apa Itu Siklus Carnot?", "id": "Siklus Termodinamika Paling Efisien Yang Mungkin." },
{ "en": "Apa Itu Sensor Gambar CMOS?", "id": "Sensor Cahaya Digital Yang Digunakan Dalam Kamera." },
{ "en": "Apa Kepanjangan Dari CMOS (Complementary Metal-Oxide-Semiconductor)?", "id": "Complementary Metal-Oxide-Semiconductor." },
{ "en": "Apa Itu Sensor Gambar CCD?", "id": "Jenis Lain Sensor Cahaya Digital." },
{ "en": "Apa Kepanjangan Dari CCD (Charge-Coupled Device)?", "id": "Charge-Coupled Device." },
{ "en": "Apa Itu Piksel (Pixel)?", "id": "Elemen Gambar Terkecil Dalam Tampilan Digital." },
{ "en": "Apa Itu Resolusi Gambar?", "id": "Jumlah Piksel Dalam Suatu Gambar." },
{ "en": "Apa Itu Pengolahan Citra Digital?", "id": "Penggunaan Algoritma Komputer Untuk Memanipulasi Gambar." },
{ "en": "Apa Itu Deteksi Tepi (Edge Detection)?", "id": "Teknik Mengidentifikasi Batas Objek Dalam Gambar." },
{ "en": "Apa Itu Segmentasi Gambar?", "id": "Membagi Gambar Menjadi Beberapa Segmen Atau Objek." },
{ "en": "Apa Itu Pengenalan Pola?", "id": "Mengidentifikasi Pola Dan Keteraturan Dalam Data." },
{ "en": "Apa Itu Optical Character Recognition (OCR)?", "id": "Mengubah Gambar Teks Menjadi Teks Digital." },
{ "en": "Apa Itu Kode Batang (Barcode)?", "id": "Representasi Optik Data Yang Dapat Dibaca Mesin." },
{ "en": "Apa Itu Kode QR (Quick Response)?", "id": "Jenis Kode Batang Matriks Dua Dimensi." },
{ "en": "Apa Itu RFID (Radio-Frequency Identification)?", "id": "Menggunakan Gelombang Radio Untuk Mengidentifikasi Objek." },
{ "en": "Apa Itu Tag RFID?", "id": "Label Elektronik Kecil Yang Berisi Data." },
{ "en": "Apa Itu Near Field Communication (NFC)?", "id": "Teknologi Komunikasi Nirkabel Jarak Sangat Dekat." },
{ "en": "Apa Itu Bluetooth?", "id": "Standar Teknologi Nirkabel Untuk Pertukaran Data." },
{ "en": "Apa Itu Wi-Fi (Wireless Fidelity)?", "id": "Teknologi Jaringan Area Lokal Nirkabel." },
{ "en": "Apa Itu Ethernet?", "id": "Standar Jaringan Area Lokal Kabel (LAN)." },
{ "en": "Apa Itu Alamat IP (Internet Protocol)?", "id": "Label Numerik Unik Untuk Perangkat Jaringan." },
{ "en": "Apa Itu Alamat MAC (Media Access Control)?", "id": "Pengidentifikasi Unik Perangkat Keras Jaringan." },
{ "en": "Apa Itu Topologi Jaringan?", "id": "Tata Letak Fisik Atau Logis Jaringan." },
{ "en": "Sebutkan Contoh Topologi Jaringan?", "id": "Bus, Star, Ring, Dan Mesh." },
{ "en": "Apa Itu Router?", "id": "Perangkat Yang Meneruskan Paket Data Antar Jaringan." },
{ "en": "Apa Itu Saklar Jaringan (Network Switch)?", "id": "Menghubungkan Perangkat Dalam Jaringan Lokal." },
{ "en": "Apa Itu Firewall?", "id": "Sistem Keamanan Jaringan Yang Memantau Lalu Lintas." },
{ "en": "Apa Itu Enkripsi?", "id": "Proses Mengubah Data Menjadi Kode Rahasia." },
{ "en": "Apa Itu Protokol TCP/IP?", "id": "Kumpulan Protokol Komunikasi Dasar Internet." },
{ "en": "Apa Kepanjangan Dari TCP (Transmission Control Protocol)?", "id": "Transmission Control Protocol." },
{ "en": "Apa Kepanjangan Dari IP (Internet Protocol)?", "id": "Internet Protocol." },
{ "en": "Apa Itu HTTP (Hypertext Transfer Protocol)?", "id": "Protokol Untuk Mengirimkan Halaman Web." },
{ "en": "Apa Itu FTP (File Transfer Protocol)?", "id": "Protokol Untuk Mentransfer File Antar Komputer." },
{ "en": "Apa Itu Sistem Operasi Real-Time (RTOS)?", "id": "Sistem Operasi Untuk Aplikasi Dengan Batasan Waktu." },
{ "en": "Apa Kepanjangan Dari RTOS (Real-Time Operating System)?", "id": "Real-Time Operating System." },
{ "en": "Apa Itu Kernel?", "id": "Komponen Inti Dari Sistem Operasi." },
{ "en": "Apa Itu Penjadwalan Tugas (Task Scheduling)?", "id": "Proses Mengalokasikan Waktu CPU Untuk Tugas." },
{ "en": "Apa Itu Thread?", "id": "Unit Eksekusi Terkecil Dalam Suatu Proses." },
{ "en": "Apa Itu Multitasking?", "id": "Menjalankan Beberapa Tugas Secara Bersamaan." },
{ "en": "Apa Itu Sinkronisasi?", "id": "Koordinasi Eksekusi Beberapa Thread Atau Proses." },
{ "en": "Apa Itu Semaphore?", "id": "Variabel Untuk Mengontrol Akses Ke Sumber Daya Umum." },
{ "en": "Apa Itu Mutex (Mutual Exclusion)?", "id": "Mekanisme Sinkronisasi Untuk Mencegah Race Condition." },
{ "en": "Apa Itu Deadlock?", "id": "Situasi Di Mana Dua Proses Saling Menunggu." },
{ "en": "Apa Itu Arsitektur Perangkat Lunak?", "id": "Struktur Tingkat Tinggi Dari Sistem Perangkat Lunak." },
{ "en": "Apa Itu Desain Berorientasi Objek (OOD)?", "id": "Metodologi Desain Berdasarkan Objek Dan Kelas." },
{ "en": "Apa Itu Enkapsulasi?", "id": "Menggabungkan Data Dan Metode Dalam Satu Unit." },
{ "en": "Apa Itu Pewarisan (Inheritance)?", "id": "Mekanisme Kelas Baru Mewarisi Properti Kelas Lain." },
{ "en": "Apa Itu Polimorfisme?", "id": "Kemampuan Mengambil Banyak Bentuk Berbeda." },
{ "en": "Apa Itu Unified Modeling Language (UML)?", "id": "Bahasa Pemodelan Visual Untuk Sistem Perangkat Lunak." },
{ "en": "Apa Itu Diagram Kelas UML?", "id": "Menggambarkan Struktur Statis Sistem." },
{ "en": "Apa Itu Diagram Urutan UML?", "id": "Menggambarkan Interaksi Objek Dalam Urutan Waktu." },
{ "en": "Apa Itu Pengujian Unit (Unit Testing)?", "id": "Menguji Komponen Perangkat Lunak Individual." },
{ "en": "Apa Itu Pengujian Integrasi?", "id": "Menguji Interaksi Antara Komponen Yang Terintegrasi." },
{ "en": "Apa Itu Kontrol Versi?", "id": "Sistem Untuk Mengelola Perubahan Kode Sumber." },
{ "en": "Sebutkan Contoh Sistem Kontrol Versi?", "id": "Git Dan Subversion (SVN)." },
{ "en": "Apa Itu Material Komposit?", "id": "Material Yang Terbuat Dari Dua Bahan Berbeda." },
{ "en": "Sebutkan Contoh Material Komposit?", "id": "Plastik Diperkuat Serat Karbon (CFRP)." },
{ "en": "Apa Itu Keramik Teknis?", "id": "Bahan Keramik Canggih Untuk Aplikasi Teknik." },
{ "en": "Apa Itu Polimer?", "id": "Makromolekul Yang Terdiri Dari Unit Berulang." },
{ "en": "Apa Itu Termoplastik?", "id": "Polimer Yang Meleleh Saat Dipanaskan." },
{ "en": "Apa Itu Termoset?", "id": "Polimer Yang Mengeras Secara Permanen Setelah Dipanaskan." },
{ "en": "Apa Itu Elastomer?", "id": "Polimer Dengan Sifat Elastisitas Seperti Karet." },
{ "en": "Apa Itu Manufaktur Ramping (Lean Manufacturing)?", "id": "Filosofi Manufaktur Untuk Menghilangkan Pemborosan." },
{ "en": "Apa Itu Enam Sigma (Six Sigma)?", "id": "Metodologi Untuk Peningkatan Kualitas Dan Pengurangan Cacat." },
{ "en": "Apa Itu Kaizen?", "id": "Filosofi Jepang Mengenai Perbaikan Berkesinambungan." },
{ "en": "Apa Itu Poka-Yoke?", "id": "Mekanisme Pencegahan Kesalahan Dalam Proses Manufaktur." },
{ "en": "Apa Itu Just-In-Time (JIT)?", "id": "Strategi Manufaktur Untuk Mengurangi Inventaris." },
{ "en": "Apa Itu Analisis SWOT?", "id": "Kerangka Kerja Untuk Mengevaluasi Kekuatan Dan Kelemahan." },
{ "en": "Apa Kepanjangan Dari SWOT (Strengths, Weaknesses, Opportunities, Threats)?", "id": "Strengths, Weaknesses, Opportunities, Threats." },
{ "en": "Apa Itu Siklus Hidup Produk (Product Lifecycle)?", "id": "Tahapan Yang Dilewati Produk Dari Awal Hingga Akhir." },
{ "en": "Apa Itu Desain Untuk Manufaktur (DFM)?", "id": "Merancang Produk Agar Mudah Diproduksi." },
{ "en": "Apa Itu Desain Untuk Perakitan (DFA)?", "id": "Merancang Produk Agar Mudah Dirakit." },
{ "en": "Apa Itu Hak Kekayaan Intelektual (IP)?", "id": "Perlindungan Hukum Untuk Kreasi Inovatif." },
{ "en": "Apa Itu Paten?", "id": "Hak Eksklusif Yang Diberikan Untuk Sebuah Penemuan." },
{ "en": "Apa Itu Merek Dagang (Trademark)?", "id": "Simbol Yang Membedakan Barang Atau Jasa." },
{ "en": "Apa Itu Hak Cipta (Copyright)?", "id": "Hak Hukum Untuk Karya Tulis Dan Artistik." },
{ "en": "Apa Itu Standar ISO 9001?", "id": "Standar Internasional Untuk Sistem Manajemen Mutu." },
{ "en": "Apa Itu Konduktivitas Termal?", "id": "Kemampuan Bahan Untuk Menghantarkan Panas." },
{ "en": "Apa Itu Ekspansi Termal?", "id": "Kecenderungan Materi Berubah Ukuran Akibat Suhu." },
{ "en": "Apa Itu Panas Jenis?", "id": "Jumlah Panas Untuk Menaikkan Suhu Zat." },
{ "en": "Apa Itu Titik Lebur?", "id": "Suhu Di Mana Zat Padat Menjadi Cair." },
{ "en": "Apa Itu Titik Didih?", "id": "Suhu Di Mana Tekanan Uap Cairan Sama Atmosfer." },
{ "en": "Apa Itu Korosi?", "id": "Kerusakan Material Akibat Reaksi Kimia." },
{ "en": "Bagaimana Cara Mencegah Korosi?", "id": "Pelapisan, Pengecatan, Dan Perlindungan Katodik." },
{ "en": "Apa Itu Anodisasi?", "id": "Proses Pelapisan Oksida Pelindung Pada Logam." },
{ "en": "Apa Itu Pelapisan Galvanis (Galvanizing)?", "id": "Melapisi Besi Atau Baja Dengan Seng." },
{ "en": "Apa Itu Baja Tahan Karat (Stainless Steel)?", "id": "Paduan Besi Yang Tahan Karat Dan Korosi." },
{ "en": "Apa Unsur Paduan Utama Baja Tahan Karat?", "id": "Kromium." },
{ "en": "Apa Itu Titanium?", "id": "Logam Kuat, Ringan, Dan Tahan Korosi." },
{ "en": "Apa Itu Aluminium?", "id": "Logam Ringan Yang Banyak Digunakan Dalam Teknik." },
{ "en": "Apa Itu Paduan (Alloy)?", "id": "Campuran Logam Dengan Unsur Lain." },
{ "en": "Apa Itu Perlakuan Panas (Heat Treatment)?", "id": "Proses Mengubah Sifat Fisik Logam." },
{ "en": "Apa Itu Proses Annealing?", "id": "Memanaskan Dan Mendinginkan Logam Untuk Melunakkannya." },
{ "en": "Apa Itu Proses Quenching?", "id": "Mendinginkan Logam Dengan Cepat Untuk Mengeraskannya." },
{ "en": "Apa Itu Proses Tempering?", "id": "Mengurangi Kerapuhan Logam Yang Telah Dikeraskan." },
{ "en": "Apa Itu Robot Scara?", "id": "Robot Untuk Aplikasi Perakitan Kecepatan Tinggi." },
{ "en": "Apa Kepanjangan Dari SCARA (Selective Compliance Assembly Robot Arm)?", "id": "Selective Compliance Assembly Robot Arm." },
{ "en": "Apa Itu Robot Kartesian?", "id": "Robot Yang Bergerak Pada Tiga Sumbu Linear." },
{ "en": "Apa Itu Robot Silindris?", "id": "Robot Dengan Satu Sendi Rotasi Dan Dua Linear." },
{ "en": "Apa Itu Robot Sferis (Polar)?", "id": "Robot Dengan Dua Sendi Rotasi Dan Satu Linear." },
{ "en": "Apa Itu Robot Artikulasi (Lengan Sendi)?", "id": "Robot Dengan Semua Sendi Rotasi Seperti Lengan Manusia." },
{ "en": "Apa Itu Kinematika Maju (Forward Kinematics)?", "id": "Menghitung Posisi End Effector Dari Sudut Sendi." },
{ "en": "Apa Itu Kinematika Balik (Inverse Kinematics)?", "id": "Menghitung Sudut Sendi Dari Posisi End Effector." },
{ "en": "Apa Itu Matriks Transformasi Homogen?", "id": "Representasi Posisi Dan Orientasi Dalam Robotika." },
{ "en": "Apa Itu Jacobian Dalam Robotika?", "id": "Matriks Yang Menghubungkan Kecepatan Sendi Dan End Effector." },
{ "en": "Apa Itu Perencanaan Gerak (Motion Planning)?", "id": "Menemukan Jalur Bebas Hambatan Untuk Robot." },
{ "en": "Apa Itu Singularitas Robot?", "id": "Konfigurasi Di Mana Robot Kehilangan Derajat Kebebasan." },
{ "en": "Apa Itu Kalibrasi Robot?", "id": "Proses Memperbaiki Akurasi Model Kinematik Robot." },
{ "en": "Apa Itu Bahasa Pemrograman Robot?", "id": "Bahasa Khusus Untuk Mengontrol Gerakan Robot." },
{ "en": "Apa Itu ROS (Robot Operating System)?", "id": "Kerangka Kerja Fleksibel Untuk Menulis Perangkat Lunak Robot." },
{ "en": "Apa Itu Sistem Tenaga Listrik?", "id": "Jaringan Komponen Listrik Untuk Menyalurkan Daya." },
{ "en": "Apa Itu Generator?", "id": "Mengubah Energi Mekanik Menjadi Energi Listrik." },
{ "en": "Apa Itu Transformator (Trafo)?", "id": "Mengubah Level Tegangan AC Tanpa Mengubah Frekuensi." },
{ "en": "Apa Itu Jaringan Transmisi?", "id": "Menyalurkan Listrik Jarak Jauh Pada Tegangan Tinggi." },
{ "en": "Apa Itu Jaringan Distribusi?", "id": "Menurunkan Tegangan Dan Menyalurkan Ke Pelanggan." },
{ "en": "Apa Itu Gardu Induk (Substation)?", "id": "Fasilitas Untuk Mengubah Level Tegangan Listrik." },
{ "en": "Apa Itu Pemutus Sirkuit (Circuit Breaker)?", "id": "Saklar Otomatis Untuk Melindungi Sirkuit." },
{ "en": "Apa Itu Sekring (Fuse)?", "id": "Perangkat Pengaman Yang Meleleh Untuk Memutus Sirkuit." },
{ "en": "Apa Itu Grounding (Pentanahan)?", "id": "Menghubungkan Sirkuit Ke Bumi Untuk Keselamatan." },
{ "en": "Apa Itu Arus Bolak-Balik (AC)?", "id": "Arus Listrik Yang Arahnya Berubah Secara Periodik." },
{ "en": "Apa Itu Arus Searah (DC)?", "id": "Arus Listrik Yang Mengalir Dalam Satu Arah." },
{ "en": "Apa Itu Frekuensi?", "id": "Jumlah Siklus Per Detik Dalam Gelombang AC." },
{ "en": "Apa Satuan Frekuensi?", "id": "Hertz (Hz)." },
{ "en": "Apa Itu Fase Dalam Listrik AC?", "id": "Posisi Relatif Gelombang Tegangan Dan Arus." },
{ "en": "Apa Itu Sistem Tiga Fasa?", "id": "Sistem Daya AC Dengan Tiga Konduktor." },
{ "en": "Apa Keuntungan Sistem Tiga Fasa?", "id": "Penyaluran Daya Lebih Efisien Dan Konstan." },
{ "en": "Apa Itu Faktor Daya (Power Factor)?", "id": "Rasio Daya Nyata Terhadap Daya Semu." },
{ "en": "Apa Yang Menyebabkan Faktor Daya Rendah?", "id": "Beban Induktif Seperti Motor Listrik." },
{ "en": "Bagaimana Cara Memperbaiki Faktor Daya?", "id": "Dengan Menambahkan Kapasitor Ke Dalam Sirkuit." },
{ "en": "Apa Itu Daya Nyata (Real Power)?", "id": "Daya Yang Sebenarnya Digunakan Oleh Beban." },
{ "en": "Apa Itu Daya Reaktif (Reactive Power)?", "id": "Daya Yang Dibutuhkan Untuk Menciptakan Medan Magnet." },
{ "en": "Apa Itu Daya Semu (Apparent Power)?", "id": "Kombinasi Vektor Daya Nyata Dan Reaktif." },
{ "en": "Apa Itu Harmonisa Dalam Sistem Tenaga?", "id": "Tegangan Atau Arus Dengan Frekuensi Kelipatan Fundamental." },
{ "en": "Apa Itu Kualitas Daya (Power Quality)?", "id": "Ukuran Seberapa Baik Daya Listrik Sesuai Spesifikasi." },
{ "en": "Apa Itu Kedip Tegangan (Voltage Sag)?", "id": "Penurunan Singkat Level Tegangan." },
{ "en": "Apa Itu Lonjakan Tegangan (Voltage Swell)?", "id": "Kenaikan Singkat Level Tegangan." },
{ "en": "Apa Itu Transien (Transient)?", "id": "Lonjakan Tegangan Atau Arus Yang Sangat Singkat." },
{ "en": "Apa Itu Uninterruptible Power Supply (UPS)?", "id": "Memberikan Daya Darurat Saat Listrik Padam." },
{ "en": "Apa Itu Jaringan Cerdas (Smart Grid)?", "id": "Jaringan Listrik Modern Dengan Komunikasi Dua Arah." },
{ "en": "Apa Itu Pengukuran Cerdas (Smart Metering)?", "id": "Meteran Listrik Digital Yang Mengirim Data Otomatis." },
{ "en": "Apa Itu Respon Permintaan (Demand Response)?", "id": "Mendorong Konsumen Mengurangi Pemakaian Listrik Saat Puncak." },
{ "en": "Apa Itu Energi Terbarukan?", "id": "Energi Dari Sumber Alam Yang Dapat Diperbarui." },
{ "en": "Sebutkan Contoh Energi Terbarukan?", "id": "Tenaga Surya, Angin, Dan Air." },
{ "en": "Apa Itu Sel Surya (Photovoltaic Cell)?", "id": "Mengubah Energi Cahaya Matahari Menjadi Listrik." },
{ "en": "Apa Itu Turbin Angin?", "id": "Mengubah Energi Kinetik Angin Menjadi Energi Mekanik." },
{ "en": "Apa Itu Pembangkit Listrik Tenaga Air?", "id": "Menggunakan Energi Air Jatuh Untuk Menghasilkan Listrik." },
{ "en": "Apa Itu Biomasa?", "id": "Bahan Organik Yang Digunakan Sebagai Sumber Energi." },
{ "en": "Apa Itu Energi Geotermal?", "id": "Energi Panas Dari Dalam Bumi." },
{ "en": "Apa Itu Sel Bahan Bakar (Fuel Cell)?", "id": "Menghasilkan Listrik Melalui Reaksi Kimia." },
{ "en": "Apa Itu Penyimpanan Energi?", "id": "Menyimpan Energi Untuk Digunakan Di Waktu Lain." },
{ "en": "Sebutkan Contoh Sistem Penyimpanan Energi?", "id": "Baterai, Penyimpanan Pompa Hidro, Dan Flywheel." },
{ "en": "Apa Itu Baterai Lithium-Ion?", "id": "Jenis Baterai Isi Ulang Yang Populer." },
{ "en": "Apa Itu Superkapasitor?", "id": "Menyimpan Energi Dengan Kapasitansi Sangat Tinggi." },
{ "en": "Apa Itu Kendaraan Listrik (EV)?", "id": "Kendaraan Yang Digerakkan Oleh Satu Motor Listrik." },
{ "en": "Apa Itu Kendaraan Listrik Hibrida (HEV)?", "id": "Menggabungkan Mesin Pembakaran Dengan Motor Listrik." },
{ "en": "Apa Itu Kendaraan Listrik Hibrida Plug-in (PHEV)?", "id": "HEV Yang Baterainya Dapat Diisi Dari Listrik Eksternal." },
{ "en": "Apa Itu Stasiun Pengisian Kendaraan Listrik?", "id": "Infrastruktur Untuk Mengisi Ulang Baterai Kendaraan Listrik." },
{ "en": "Apa Itu Manajemen Termal Baterai?", "id": "Menjaga Baterai Dalam Rentang Suhu Optimal." },
{ "en": "Apa Itu Keadaan Pengisian (State of Charge - SOC)?", "id": "Tingkat Pengisian Baterai Relatif Terhadap Kapasitasnya." },
{ "en": "Apa Itu Kesehatan Baterai (State of Health - SOH)?", "id": "Kondisi Baterai Saat Ini Dibandingkan Kondisi Barunya." },
{ "en": "Apa Itu Siklus Hidup Baterai?", "id": "Jumlah Siklus Pengisian-Pengosongan Sebelum Kapasitas Menurun." },
{ "en": "Apa Itu Kepadatan Energi Baterai?", "id": "Jumlah Energi Yang Disimpan Per Satuan Massa." },
{ "en": "Apa Itu Kepadatan Daya Baterai?", "id": "Jumlah Daya Yang Dihasilkan Per Satuan Massa." },
{ "en": "Apa Itu Sistem Manajemen Baterai (BMS)?", "id": "Sistem Elektronik Yang Mengelola Baterai Isi Ulang." },
{ "en": "Apa Fungsi Utama BMS?", "id": "Melindungi Baterai, Memantau, Dan Menyeimbangkan Sel." },
{ "en": "Apa Itu Penyeimbangan Sel (Cell Balancing)?", "id": "Memastikan Semua Sel Baterai Memiliki SOC Sama." },
{ "en": "Apa Itu Optika?", "id": "Cabang Fisika Yang Mempelajari Perilaku Cahaya." },
{ "en": "Apa Itu Refleksi (Pemantulan)?", "id": "Perubahan Arah Gelombang Saat Mengenai Permukaan." },
{ "en": "Apa Itu Refraksi (Pembiasan)?", "id": "Pembelokan Cahaya Saat Melewati Medium Berbeda." },
{ "en": "Apa Itu Difraksi (Lenturan)?", "id": "Penyebaran Gelombang Saat Melewati Celah Sempit." },
{ "en": "Apa Itu Interferensi?", "id": "Superposisi Dua Gelombang Atau Lebih." },
{ "en": "Apa Itu Polarisasi Cahaya?", "id": "Membatasi Orientasi Getaran Gelombang Cahaya." },
{ "en": "Apa Itu Lensa?", "id": "Komponen Optik Yang Memfokuskan Atau Menyebarkan Cahaya." },
{ "en": "Apa Itu Panjang Fokus (Focal Length)?", "id": "Jarak Dari Lensa Ke Titik Fokus." },
{ "en": "Apa Itu Cermin?", "id": "Permukaan Yang Memantulkan Cahaya." },
{ "en": "Apa Itu Serat Optik?", "id": "Media Transmisi Yang Memandu Cahaya." },
{ "en": "Bagaimana Prinsip Kerja Serat Optik?", "id": "Berdasarkan Prinsip Pemantulan Internal Total." },
{ "en": "Apa Itu Laser?", "id": "Sumber Cahaya Koheren, Monokromatik, Dan Terarah." },
{ "en": "Apa Kepanjangan Dari LASER?", "id": "Light Amplification by Stimulated Emission of Radiation." },
{ "en": "Apa Itu LED (Light Emitting Diode)?", "id": "Semikonduktor Yang Memancarkan Cahaya Saat Dialiri Arus." },
{ "en": "Apa Itu Spektroskopi?", "id": "Studi Interaksi Antara Materi Dan Radiasi Elektromagnetik." },
{ "en": "Apa Itu Holografi?", "id": "Teknik Untuk Merekam Dan Merekonstruksi Bidang Cahaya." },
{ "en": "Apa Itu Akustik?", "id": "Cabang Fisika Yang Mempelajari Gelombang Mekanik." },
{ "en": "Apa Itu Gelombang Suara?", "id": "Getaran Yang Merambat Melalui Medium." },
{ "en": "Apa Itu Kecepatan Suara?", "id": "Laju Perambatan Gelombang Suara Melalui Medium." },
{ "en": "Apa Itu Amplitudo Gelombang Suara?", "id": "Menentukan Kekerasan Atau Intensitas Suara." },
{ "en": "Apa Itu Frekuensi Gelombang Suara?", "id": "Menentukan Nada (Pitch) Suara." },
{ "en": "Apa Itu Desibel (dB)?", "id": "Satuan Logaritmik Untuk Mengukur Intensitas Suara." },
{ "en": "Apa Itu Efek Doppler?", "id": "Perubahan Frekuensi Gelombang Akibat Gerak Relatif." },
{ "en": "Apa Itu Gema (Echo)?", "id": "Pantulan Suara Yang Terdengar Setelah Suara Asli." },
{ "en": "Apa Itu Gaung (Reverberation)?", "id": "Kumpulan Pantulan Suara Dalam Ruang Tertutup." },
{ "en": "Apa Itu Ultrasonik?", "id": "Suara Dengan Frekuensi Di Atas Pendengaran Manusia." },
{ "en": "Apa Itu Infrasonik?", "id": "Suara Dengan Frekuensi Di Bawah Pendengaran Manusia." },
{ "en": "Apa Itu Mikrofon?", "id": "Transduser Yang Mengubah Energi Suara Menjadi Listrik." },
{ "en": "Apa Itu Pengeras Suara (Loudspeaker)?", "id": "Transduser Yang Mengubah Sinyal Listrik Menjadi Suara." },
{ "en": "Apa Itu Peredaman Suara (Soundproofing)?", "id": "Mengisolasi Ruangan Dari Suara Eksternal." },
{ "en": "Apa Itu Pembatalan Bising Aktif (Active Noise Cancellation)?", "id": "Mengurangi Bising Dengan Menghasilkan Suara Anti-fase." },
{ "en": "Apa Itu Sonar (Sound Navigation and Ranging)?", "id": "Menggunakan Perambatan Suara Untuk Navigasi Atau Deteksi." },
{ "en": "Apa Itu Biomekatronika?", "id": "Integrasi Mekatronika Dengan Sistem Biologis." },
{ "en": "Apa Itu Antarmuka Otak-Komputer (BCI)?", "id": "Jalur Komunikasi Langsung Antara Otak Dan Komputer." },
{ "en": "Apa Kepanjangan Dari BCI (Brain-Computer Interface)?", "id": "Brain-Computer Interface." },
{ "en": "Apa Itu Sinyal Elektromiografi (EMG)?", "id": "Sinyal Listrik Yang Dihasilkan Oleh Aktivitas Otot." },
{ "en": "Apa Itu Sinyal Elektroensefalografi (EEG)?", "id": "Sinyal Listrik Yang Dihasilkan Oleh Aktivitas Otak." },
{ "en": "Apa Itu Jantung Buatan?", "id": "Perangkat Mekanis Yang Menggantikan Fungsi Jantung." },
{ "en": "Apa Itu Pompa Insulin?", "id": "Perangkat Medis Untuk Pemberian Insulin Penderita Diabetes." },
{ "en": "Apa Itu Robotika Rehabilitasi?", "id": "Robot Yang Membantu Proses Terapi Dan Rehabilitasi." },
{ "en": "Apa Itu Lab-on-a-chip?", "id": "Perangkat Miniatur Untuk Analisis Biokimia." },
{ "en": "Apa Itu Mesin Bubut (Lathe)?", "id": "Mesin Perkakas Untuk Memutar Benda Kerja." },
{ "en": "Apa Itu Mesin Frais (Milling Machine)?", "id": "Mesin Perkakas Menggunakan Pemotong Berputar." },
{ "en": "Apa Itu Mesin Bor (Drilling Machine)?", "id": "Mesin Untuk Membuat Lubang Silindris." },
{ "en": "Apa Itu Mesin Gerinda (Grinding Machine)?", "id": "Menggunakan Roda Abrasif Sebagai Alat Potong." },
{ "en": "Apa Itu Pahat Potong (Cutting Tool)?", "id": "Alat Yang Digunakan Untuk Memotong Material." },
{ "en": "Apa Itu Cairan Pendingin (Coolant)?", "id": "Cairan Untuk Mendinginkan Dan Melumasi Proses Pemesinan." },
{ "en": "Apa Itu G-code?", "id": "Bahasa Pemrograman Untuk Mengontrol Mesin CNC." },
{ "en": "Apa Itu M-code?", "id": "Kode Fungsi Tambahan Dalam Pemrograman CNC." },
{ "en": "Apa Itu Sistem Koordinat Mesin?", "id": "Sistem Koordinat Tetap Pada Mesin CNC." },
{ "en": "Apa Itu Sistem Koordinat Benda Kerja?", "id": "Sistem Koordinat Yang Ditetapkan Pada Benda Kerja." },
{ "en": "Apa Itu Kompensasi Pahat (Tool Compensation)?", "id": "Penyesuaian Untuk Memperhitungkan Ukuran Pahat." },
{ "en": "Apa Itu Pemotongan Laser?", "id": "Proses Fabrikasi Menggunakan Sinar Laser Fokus." },
{ "en": "Apa Itu Pemotongan Jet Air (Waterjet Cutting)?", "id": "Menggunakan Aliran Air Bertekanan Sangat Tinggi." },
{ "en": "Apa Itu Pemotongan Plasma?", "id": "Menggunakan Jet Plasma Panas Untuk Memotong Logam." },
{ "en": "Apa Itu Electrical Discharge Machining (EDM)?", "id": "Membentuk Material Menggunakan Loncatan Listrik." },
{ "en": "Apa Itu Sintering Laser Selektif (SLS)?", "id": "Teknik Pencetakan 3D Menggunakan Laser Dan Bubuk." },
{ "en": "Apa Itu Stereolithography (SLA)?", "id": "Teknik Pencetakan 3D Menggunakan Resin Fotopolimer." },
{ "en": "Apa Itu Fused Deposition Modeling (FDM)?", "id": "Teknik Pencetakan 3D Yang Melelehkan Filamen Plastik." },
{ "en": "Apa Itu Kontrol Numerik Langsung (DNC)?", "id": "Menghubungkan Komputer Pusat Ke Beberapa Mesin CNC." },
{ "en": "Apa Kepanjangan Dari DNC (Direct Numerical Control)?", "id": "Direct Numerical Control." },
{ "en": "Apa Itu Teorema Sampling Nyquist-Shannon?", "id": "Menetapkan Tingkat Sampling Minimum Untuk Sinyal." },
{ "en": "Apa Itu Aliasing?", "id": "Distorsi Sinyal Akibat Tingkat Sampling Terlalu Rendah." },
{ "en": "Apa Itu Kuantisasi?", "id": "Proses Memetakan Nilai Kontinu Ke Set Diskrit." },
{ "en": "Apa Itu Kesalahan Kuantisasi?", "id": "Perbedaan Antara Sinyal Analog Dan Nilai Kuantisasinya." },
{ "en": "Apa Itu Konvolusi?", "id": "Operasi Matematis Pada Dua Sinyal." },
{ "en": "Apa Itu Respon Impuls?", "id": "Output Sistem Ketika Diberi Input Impuls Singkat." },
{ "en": "Apa Itu Filter FIR (Finite Impulse Response)?", "id": "Filter Digital Dengan Respon Impuls Terbatas." },
{ "en": "Apa Itu Filter IIR (Infinite Impulse Response)?", "id": "Filter Digital Dengan Respon Impuls Tak Terbatas." },
{ "en": "Apa Keuntungan Filter FIR?", "id": "Selalu Stabil Dan Memiliki Fase Linear." },
{ "en": "Apa Keuntungan Filter IIR?", "id": "Lebih Efisien Secara Komputasi Daripada FIR." },
{ "en": "Apa Itu Desain Berbantuan Komputer (CAD)?", "id": "Penggunaan Komputer Untuk Membantu Dalam Desain." },
{ "en": "Apa Itu Rekayasa Berbantuan Komputer (CAE)?", "id": "Penggunaan Perangkat Lunak Komputer Untuk Analisis Teknik." },
{ "en": "Apa Itu Manufaktur Berbantuan Komputer (CAM)?", "id": "Penggunaan Perangkat Lunak Untuk Mengontrol Mesin Perkakas." },
{ "en": "Apa Itu Manajemen Siklus Hidup Produk (PLM)?", "id": "Proses Mengelola Perjalanan Produk Dari Awal." },
{ "en": "Apa Itu Perencanaan Sumber Daya Perusahaan (ERP)?", "id": "Sistem Perangkat Lunak Untuk Mengelola Proses Bisnis." },
{ "en": "Apa Itu Getaran Paksa?", "id": "Getaran Yang Disebabkan Oleh Eksitasi Eksternal." },
{ "en": "Apa Itu Getaran Bebas?", "id": "Getaran Yang Terjadi Tanpa Eksitasi Eksternal." },
{ "en": "Apa Itu Mode Getaran (Mode Shape)?", "id": "Pola Getaran Khas Pada Frekuensi Alami." },
{ "en": "Apa Itu Isolator Getaran?", "id": "Perangkat Untuk Mencegah Transmisi Getaran." },
{ "en": "Apa Itu Peredam Getaran (Vibration Damper)?", "id": "Perangkat Untuk Menyerap Energi Getaran." },
{ "en": "Apa Itu Analisis Spektral?", "id": "Menganalisis Sinyal Dalam Domain Frekuensi." },
{ "en": "Apa Itu Bandwidth?", "id": "Rentang Frekuensi Di Mana Sistem Beroperasi." },
{ "en": "Apa Itu Desain Mekanisme?", "id": "Proses Merancang Gerakan Komponen Mekanis." },
{ "en": "Apa Itu Sintesis Mekanisme?", "id": "Menciptakan Mekanisme Untuk Menghasilkan Gerakan Tertentu." },
{ "en": "Apa Itu Analisis Mekanisme?", "id": "Mempelajari Gerakan Mekanisme Yang Sudah Ada." },
{ "en": "Apa Itu Diagram Kinematik?", "id": "Representasi Skematis Sederhana Dari Suatu Mekanisme." },
{ "en": "Apa Itu Pusat Sesaat Rotasi?", "id": "Titik Di Mana Benda Kaku Berotasi Sesaat." },
{ "en": "Apa Itu Persamaan Freudenstein?", "id": "Persamaan Matematis Untuk Sintesis Mekanisme Empat Batang." },
{ "en": "Apa Itu Titik Cognate?", "id": "Titik Pada Mekanisme Berbeda Yang Menghasilkan Kurva Sama." },
{ "en": "Apa Itu Inversi Mekanisme?", "id": "Membuat Batang Yang Berbeda Menjadi Rangka Tetap." },
{ "en": "Apa Itu Hukum Grashof?", "id": "Menentukan Kemampuan Rotasi Penuh Batang Mekanisme Empat Batang." },
{ "en": "Apa Itu Dead Point Dalam Mekanisme?", "id": "Posisi Di Mana Gaya Input Tidak Menghasilkan Torsi." },
{ "en": "Apa Itu Toggle Position?", "id": "Posisi Yang Memberikan Keuntungan Mekanis Sangat Besar." },
{ "en": "Apa Itu Keuntungan Mekanis?", "id": "Rasio Gaya Output Terhadap Gaya Input." },
{ "en": "Apa Itu Sudut Transmisi?", "id": "Ukuran Kualitas Transmisi Gerak Dan Gaya." },
{ "en": "Apa Itu Kurva Coupler?", "id": "Jalur Yang Ditempuh Oleh Titik Pada Batang Coupler." },
{ "en": "Apa Itu Motor Linier?", "id": "Motor Listrik Yang Menghasilkan Gerakan Lurus." },
{ "en": "Apa Itu Aktuator Suara Koil (Voice Coil Actuator)?", "id": "Aktuator Linier Untuk Gerakan Presisi Jarak Pendek." },
{ "en": "Apa Itu Elektrolit Dalam Baterai?", "id": "Medium Kimia Yang Memungkinkan Aliran Ion." },
{ "en": "Apa Itu Anoda Dalam Baterai?", "id": "Elektroda Tempat Terjadinya Oksidasi (Terminal Negatif)." },
{ "en": "Apa Itu Katoda Dalam Baterai?", "id": "Elektroda Tempat Terjadinya Reduksi (Terminal Positif)." },
{ "en": "Apa Itu Pengontrol Muatan Surya (Solar Charge Controller)?", "id": "Mengatur Tegangan Dari Panel Surya Ke Baterai." },
{ "en": "Apa Itu Maximum Power Point Tracking (MPPT)?", "id": "Teknik Untuk Mendapatkan Daya Maksimal Dari Panel Surya." },
{ "en": "Apa Itu Sel Beban Tipe S (S-Type Load Cell)?", "id": "Mengukur Gaya Tarik Dan Tekan." },
{ "en": "Apa Itu Sensor Inframerah (IR)?", "id": "Mendeteksi Radiasi Inframerah Dari Objek." },
{ "en": "Apa Itu Sensor Warna?", "id": "Mendeteksi Warna Suatu Objek." },
{ "en": "Apa Itu Sensor Kelembaban?", "id": "Mengukur Kadar Uap Air Di Udara." },
{ "en": "Apa Itu Sensor Tekanan?", "id": "Mengukur Gaya Per Satuan Luas." },
{ "en": "Apa Itu Sensor Gas?", "id": "Mendeteksi Keberadaan Jenis Gas Tertentu." },
{ "en": "Apa Itu Sensor Giroskop MEMS?", "id": "Giroskop Miniatur Yang Dibuat Dengan Teknologi MEMS." },
{ "en": "Apa Itu Lidah Elektronik (Electronic Tongue)?", "id": "Sensor Untuk Menganalisis Dan Mengidentifikasi Cairan." },
{ "en": "Apa Itu Hidung Elektronik (Electronic Nose)?", "id": "Sensor Untuk Mendeteksi Dan Mengidentifikasi Bau." },
{ "en": "Apa Itu Thermistor?", "id": "Resistor Yang Nilainya Berubah Terhadap Suhu." },
{ "en": "Apa Itu RTD (Resistance Temperature Detector)?", "id": "Sensor Suhu Berdasarkan Perubahan Resistansi Logam." },
{ "en": "Apa Perbedaan Termistor Dan RTD?", "id": "Termistor Lebih Sensitif, RTD Lebih Linear." },
{ "en": "Apa Itu Thyristor?", "id": "Semikonduktor Empat Lapis Sebagai Saklar Daya." },
{ "en": "Apa Itu Silicon Controlled Rectifier (SCR)?", "id": "Jenis Thyristor Yang Paling Umum." },
{ "en": "Apa Itu TRIAC (Triode for Alternating Current)?", "id": "Saklar Semikonduktor Dua Arah Untuk Arus AC." },
{ "en": "Apa Itu DIAC (Diode for Alternating Current)?", "id": "Komponen Pemicu Untuk TRIAC." },
{ "en": "Apa Itu Insulated Gate Bipolar Transistor (IGBT)?", "id": "Komponen Semikonduktor Untuk Aplikasi Daya Tinggi." },
{ "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor Field-Effect Transistor)?", "id": "Jenis Transistor Untuk Aplikasi Switching Cepat." },
{ "en": "Apa Itu Heatsinking?", "id": "Proses Melesapkan Panas Dari Komponen Elektronik." },
{ "en": "Apa Itu Antarmuka Pengguna Grafis (GUI)?", "id": "Antarmuka Visual Untuk Interaksi Dengan Komputer." },
{ "en": "Apa Kepanjangan Dari GUI (Graphical User Interface)?", "id": "Graphical User Interface." },
{ "en": "Apa Itu Antarmuka Baris Perintah (CLI)?", "id": "Antarmuka Berbasis Teks Untuk Memberi Perintah." },
{ "en": "Apa Kepanjangan Dari CLI (Command-Line Interface)?", "id": "Command-Line Interface." },
{ "en": "Apa Itu Open-Source?", "id": "Perangkat Lunak Dengan Kode Sumber Yang Terbuka." },
{ "en": "Apa Itu Perangkat Lunak Proprietary?", "id": "Perangkat Lunak Berpemilik Dengan Lisensi Terbatas." },
{ "en": "Apa Itu API (Application Programming Interface)?", "id": "Antarmuka Yang Menghubungkan Berbagai Aplikasi Perangkat Lunak." },
{ "en": "Apa Itu SDK (Software Development Kit)?", "id": "Kumpulan Alat Untuk Mengembangkan Perangkat Lunak." },
{ "en": "Apa Itu IDE (Integrated Development Environment)?", "id": "Aplikasi Perangkat Lunak Untuk Pengembangan Program." },
{ "en": "Apa Itu GitHub?", "id": "Platform Hosting Untuk Kontrol Versi Menggunakan Git." },
{ "en": "Apa Itu MATLAB?", "id": "Lingkungan Komputasi Numerik Dan Bahasa Pemrograman." },
{ "en": "Apa Itu Simulink?", "id": "Lingkungan Pemodelan Grafis Dalam MATLAB." },
{ "en": "Apa Itu LabVIEW?", "id": "Platform Pengembangan Sistem Dengan Bahasa Pemrograman Grafis." },
{ "en": "Apa Itu Python?", "id": "Bahasa Pemrograman Tingkat Tinggi Serbaguna." },
{ "en": "Apa Itu Perpustakaan (Library) Dalam Pemrograman?", "id": "Kumpulan Kode Yang Telah Ditulis Sebelumnya." },
{ "en": "Apa Itu Firmware?", "id": "Perangkat Lunak Permanen Yang Diprogram Ke ROM." },
{ "en": "Apa Itu Bootloader?", "id": "Program Kecil Yang Memuat Sistem Operasi." },
{ "en": "Apa Itu Komputasi Tepi (Edge Computing)?", "id": "Memproses Data Dekat Dengan Sumbernya." },
{ "en": "Apa Itu Komputasi Awan (Cloud Computing)?", "id": "Menyediakan Layanan Komputasi Melalui Internet." },
{ "en": "Apa Keuntungan Edge Computing?", "id": "Latensi Rendah Dan Penggunaan Bandwidth Berkurang." },
{ "en": "Apa Itu Protokol MQTT?", "id": "Protokol Pesan Ringan Untuk Perangkat IoT." },
{ "en": "Apa Kepanjangan Dari MQTT (Message Queuing Telemetry Transport)?", "id": "Message Queuing Telemetry Transport." },
{ "en": "Apa Itu JSON (JavaScript Object Notation)?", "id": "Format Pertukaran Data Ringan Yang Mudah Dibaca." },
{ "en": "Apa Itu XML (eXtensible Markup Language)?", "id": "Bahasa Markup Untuk Menyimpan Dan Mengangkut Data." },
{ "en": "Apa Itu Basis Data?", "id": "Kumpulan Data Terstruktur Yang Disimpan Secara Elektronik." },
{ "en": "Apa Itu SQL (Structured Query Language)?", "id": "Bahasa Standar Untuk Mengelola Basis Data Relasional." },
{ "en": "Apa Itu Basis Data NoSQL?", "id": "Basis Data Non-relasional Dengan Skema Fleksibel." },
{ "en": "Apa Itu Keamanan Siber (Cybersecurity)?", "id": "Praktik Melindungi Sistem Dari Serangan Digital." },
{ "en": "Apa Itu Malware?", "id": "Perangkat Lunak Berbahaya Yang Dirancang Untuk Merusak." },
{ "en": "Apa Itu Serangan Man-in-the-Middle (MITM)?", "id": "Pihak Ketiga Mencegat Komunikasi Dua Pihak." },
{ "en": "Apa Itu Jaringan Pribadi Virtual (VPN)?", "id": "Menciptakan Koneksi Aman Melalui Jaringan Publik." },
{ "en": "Apa Itu Otentikasi Dua Faktor (2FA)?", "id": "Membutuhkan Dua Metode Verifikasi Identitas." },
{ "en": "Apa Itu Biometrik?", "id": "Otentikasi Menggunakan Karakteristik Fisik Atau Perilaku." },
{ "en": "Sebutkan Contoh Otentikasi Biometrik?", "id": "Sidik Jari, Pengenalan Wajah, Dan Pemindaian Iris." },
{ "en": "Apa Itu Pemeliharaan Preventif?", "id": "Pemeliharaan Terjadwal Untuk Mencegah Kerusakan." },
{ "en": "Apa Itu Analisis Akar Masalah (Root Cause Analysis)?", "id": "Metode Pemecahan Masalah Untuk Mengidentifikasi Akar Masalah." },
{ "en": "Apa Itu Diagram Tulang Ikan (Ishikawa)?", "id": "Alat Visualisasi Untuk Analisis Akar Masalah." },
{ "en": "Apa Itu Diagram Pareto?", "id": "Grafik Yang Menunjukkan Frekuensi Masalah." },
{ "en": "Apa Itu Prinsip Pareto (Aturan 80/20)?", "id": "80% Efek Berasal Dari 20% Penyebab." },
{ "en": "Apa Itu Brainstorming?", "id": "Teknik Kreativitas Kelompok Untuk Menghasilkan Ide." },
{ "en": "Apa Itu Manajemen Proyek?", "id": "Proses Merencanakan, Melaksanakan, Dan Mengendalikan Proyek." },
{ "en": "Apa Itu Diagram Gantt?", "id": "Bagan Batang Yang Mengilustrasikan Jadwal Proyek." },
{ "en": "Apa Itu Jalur Kritis (Critical Path)?", "id": "Urutan Tugas Terpanjang Yang Menentukan Durasi Proyek." },
{ "en": "Apa Itu Tonggak Sejarah (Milestone) Proyek?", "id": "Titik Atau Peristiwa Penting Dalam Proyek." },
{ "en": "Apa Itu Lingkup (Scope) Proyek?", "id": "Pekerjaan Yang Perlu Dilakukan Untuk Menyelesaikan Proyek." },
{ "en": "Apa Itu Etika Rekayasa?", "id": "Prinsip Moral Yang Berlaku Dalam Praktik Rekayasa." },
{ "en": "Apa Itu Tanggung Jawab Profesional?", "id": "Kewajiban Seorang Profesional Terhadap Klien Dan Publik." },
{ "en": "Apa Itu Konflik Kepentingan?", "id": "Situasi Yang Mengganggu Objektivitas Profesional." },
{ "en": "Apa Itu Pembelajaran Seumur Hidup (Lifelong Learning)?", "id": "Pengembangan Pengetahuan Dan Keterampilan Berkelanjutan." },
{ "en": "Apa Itu Tinjauan Sejawat (Peer Review)?", "id": "Evaluasi Karya Oleh Orang Dengan Kompetensi Serupa." },
{ "en": "Apa Itu Sistem Satuan Internasional (SI)?", "id": "Bentuk Modern Dari Sistem Metrik." },
{ "en": "Apa Itu Analisis Dimensi?", "id": "Analisis Hubungan Antara Besaran Fisik." },
{ "en": "Apa Itu Angka Penting?", "id": "Digit Dalam Angka Yang Memberikan Makna." },
{ "en": "Apa Itu Ketidakpastian Pengukuran?", "id": "Keraguan Yang Terkait Dengan Hasil Pengukuran." },
{ "en": "Apa Itu Dokumentasi Teknis?", "id": "Dokumen Yang Menjelaskan Penggunaan Dan Fungsi Produk." },
{ "en": "Apa Itu Laporan Teknis?", "id": "Dokumen Formal Yang Menjelaskan Hasil Proyek Teknik." },
{ "en": "Apa Itu Spesifikasi Teknis?", "id": "Dokumen Yang Menetapkan Persyaratan Untuk Suatu Produk." },
{ "en": "Apa Itu Lembar Data (Datasheet)?", "id": "Dokumen Yang Merangkum Kinerja Komponen." },
{ "en": "Apa Itu Gambar Teknik?", "id": "Gambar Rinci Untuk Mengkomunikasikan Informasi Desain." },
{ "en": "Apa Itu Proyeksi Ortografik?", "id": "Cara Merepresentasikan Objek 3D Dalam 2D." },
{ "en": "Apa Itu Gambar Isometrik?", "id": "Gambar Tiga Dimensi Tanpa Perspektif." },
{ "en": "Apa Itu Toleransi Geometris Dan Dimensi (GD&T)?", "id": "Bahasa Simbolis Untuk Mendefinisikan Toleransi Manufaktur." },
{ "en": "Apa Itu Penunjukan Kekasaran Permukaan?", "id": "Menunjukkan Tingkat Kehalusan Permukaan Yang Diinginkan." },
{ "en": "Apa Itu Daftar Material (Bill of Materials - BOM)?", "id": "Daftar Semua Komponen Yang Dibutuhkan Untuk Produk." },
{ "en": "Apa Itu Analisis Rantai Toleransi (Tolerance Stack-up)?", "id": "Menganalisis Efek Akumulatif Dari Toleransi Komponen." },
{ "en": "Apa Itu Pegas (Spring)?", "id": "Elemen Elastis Yang Menyimpan Energi Mekanis." },
{ "en": "Apa Itu Konstanta Pegas?", "id": "Ukuran Kekakuan Pegas Sesuai Hukum Hooke." },
{ "en": "Apa Itu Peredam (Damper)?", "id": "Perangkat Yang Melesapkan Energi Getaran." },
{ "en": "Apa Itu Viskositas Damping?", "id": "Ukuran Kemampuan Peredam Untuk Melesapkan Energi." },
{ "en": "Apa Itu Sistem Massa-Pegas-Peredam?", "id": "Model Fundamental Untuk Menganalisis Getaran Mekanis." },
{ "en": "Apa Itu Kekakuan (Stiffness)?", "id": "Ketahanan Benda Terhadap Deformasi Akibat Gaya." },
{ "en": "Apa Itu Fleksibilitas (Compliance)?", "id": "Kebalikan Dari Kekakuan." },
{ "en": "Apa Itu Puntiran (Torsion)?", "id": "Pemutaran Objek Akibat Torsi." },
{ "en": "Apa Itu Tekukan (Buckling)?", "id": "Kegagalan Struktural Akibat Gaya Tekan Tinggi." },
{ "en": "Apa Itu Momen Lentur (Bending Moment)?", "id": "Reaksi Dalam Elemen Struktural Saat Diberi Beban." },
{ "en": "Apa Itu Gaya Geser (Shear Force)?", "id": "Gaya Yang Bekerja Paralel Dengan Permukaan." },
{ "en": "Apa Itu Pusat Massa?", "id": "Titik Rata-Rata Distribusi Massa Dalam Benda." },
{ "en": "Apa Itu Sentroid?", "id": "Pusat Geometris Dari Suatu Bentuk." },
{ "en": "Apa Itu Mekanika Kontak?", "id": "Studi Tentang Deformasi Benda Padat Yang Bersentuhan." },
{ "en": "Apa Itu Koefisien Gesek Statis?", "id": "Rasio Gaya Gesek Maksimum Sebelum Benda Bergerak." },
{ "en": "Apa Itu Koefisien Gesek Kinetis?", "id": "Rasio Gaya Gesek Saat Benda Sedang Bergerak." },
{ "en": "Apa Itu Kavitasi?", "id": "Pembentukan Gelembung Uap Dalam Cairan." },
{ "en": "Apa Itu Pompa Sentrifugal?", "id": "Pompa Yang Menggunakan Impeler Berputar Untuk Memindahkan Fluida." },
{ "en": "Apa Itu Kompresor?", "id": "Perangkat Mekanis Untuk Meningkatkan Tekanan Gas." },
{ "en": "Apa Itu Turbin?", "id": "Mesin Berputar Yang Mengambil Energi Dari Aliran Fluida." },
{ "en": "Apa Itu Mesin Pembakaran Dalam (Internal Combustion Engine)?", "id": "Mesin Panas Yang Pembakarannya Terjadi Di Dalam." },
{ "en": "Apa Itu Siklus Otto?", "id": "Siklus Termodinamika Ideal Untuk Mesin Bensin." },
{ "en": "Apa Itu Siklus Diesel?", "id": "Siklus Termodinamika Ideal Untuk Mesin Diesel." },
{ "en": "Apa Itu Aktuator Kimia?", "id": "Aktuator Yang Digerakkan Oleh Reaksi Kimia." },
{ "en": "Apa Itu Propelan?", "id": "Bahan Kimia Yang Menghasilkan Gas Bertekanan." },
{ "en": "Apa Itu Sensor Serat Optik?", "id": "Sensor Yang Menggunakan Serat Optik Untuk Pengukuran." },
{ "en": "Apa Itu Giroskop Serat Optik (FOG)?", "id": "Mengukur Rotasi Menggunakan Interferensi Cahaya Dalam Serat." },
{ "en": "Apa Itu Giroskop Cincin Laser (Ring Laser Gyro)?", "id": "Mengukur Rotasi Menggunakan Sinar Laser Berlawanan Arah." },
{ "en": "Apa Itu Sensor Kapasitif?", "id": "Mengukur Perubahan Kapasitansi Untuk Mendeteksi Sesuatu." },
{ "en": "Apa Itu Sensor Induktif?", "id": "Menggunakan Prinsip Induksi Elektromagnetik Untuk Deteksi." },
{ "en": "Apa Itu Transformator Diferensial Variabel Linier (LVDT)?", "id": "Transduser Untuk Mengukur Perpindahan Linear." },
{ "en": "Apa Kepanjangan Dari LVDT?", "id": "Linear Variable Differential Transformer." },
{ "en": "Apa Itu Resolver?", "id": "Sensor Posisi Sudut Elektromekanis." },
{ "en": "Apa Itu Synchro?", "id": "Jenis Lain Transduser Sudut Elektromekanis." },
{ "en": "Apa Itu Tachometer?", "id": "Alat Untuk Mengukur Kecepatan Putaran Poros." },
{ "en": "Apa Itu Stroboskop?", "id": "Alat Untuk Membuat Objek Bergerak Terlihat Diam." },
{ "en": "Apa Itu Manometer?", "id": "Alat Untuk Mengukur Tekanan Fluida." },
{ "en": "Apa Itu Tabung Pitot?", "id": "Instrumen Pengukur Kecepatan Aliran Fluida." },
{ "en": "Apa Itu Venturi Meter?", "id": "Mengukur Kecepatan Aliran Berdasarkan Prinsip Bernoulli." },
{ "en": "Apa Itu Rotameter?", "id": "Mengukur Laju Aliran Volumetrik Fluida." },
{ "en": "Apa Itu Pengkondisian Sinyal?", "id": "Memanipulasi Sinyal Analog Agar Sesuai Untuk Pemrosesan." },
{ "en": "Apa Itu Amplifikasi?", "id": "Proses Meningkatkan Kekuatan Sinyal." },
{ "en": "Apa Itu Atenuasi?", "id": "Proses Mengurangi Kekuatan Sinyal." },
{ "en": "Apa Itu Isolasi Sinyal?", "id": "Memisahkan Dua Bagian Sirkuit Secara Elektrik." },
{ "en": "Apa Itu Modulasi?", "id": "Memodifikasi Gelombang Pembawa Untuk Mengirim Informasi." },
{ "en": "Apa Itu Demodulasi?", "id": "Mengekstrak Informasi Asli Dari Gelombang Pembawa." },
{ "en": "Apa Itu Modulasi Amplitudo (AM)?", "id": "Memvariasikan Amplitudo Gelombang Pembawa." },
{ "en": "Apa Itu Modulasi Frekuensi (FM)?", "id": "Memvariasikan Frekuensi Gelombang Pembawa." },
{ "en": "Apa Itu Pengkodean Garis (Line Coding)?", "id": "Mengubah Data Biner Menjadi Sinyal Digital." },
{ "en": "Apa Itu Multiplexing?", "id": "Mengirimkan Beberapa Sinyal Melalui Saluran Tunggal." },
{ "en": "Apa Itu Frequency Division Multiplexing (FDM)?", "id": "Membagi Saluran Menjadi Pita Frekuensi Berbeda." },
{ "en": "Apa Itu Time Division Multiplexing (TDM)?", "id": "Membagi Saluran Menjadi Slot Waktu Bergantian." },
{ "en": "Apa Itu Dioda Zener?", "id": "Dioda Yang Dirancang Untuk Bekerja Pada Bias Mundur." },
{ "en": "Apa Fungsi Utama Dioda Zener?", "id": "Sebagai Regulator Tegangan." },
{ "en": "Apa Itu Dioda Schottky?", "id": "Dioda Dengan Penurunan Tegangan Maju Yang Rendah." },
{ "en": "Apa Itu Varactor (Varicap Diode)?", "id": "Dioda Dengan Kapasitansi Yang Bervariasi Terhadap Tegangan." },
{ "en": "Apa Itu Dioda Tunnel?", "id": "Dioda Yang Mampu Beroperasi Pada Kecepatan Tinggi." },
{ "en": "Apa Itu Dioda PIN?", "id": "Dioda Yang Digunakan Sebagai Saklar Frekuensi Radio." },
{ "en": "Apa Itu Komparator Jendela (Window Comparator)?", "id": "Mendeteksi Apakah Tegangan Berada Dalam Rentang Tertentu." },
{ "en": "Apa Itu Pemicu Schmitt (Schmitt Trigger)?", "id": "Komparator Dengan Histeresis Untuk Mengurangi Noise." },
{ "en": "Apa Itu Rangkaian Clamper?", "id": "Menggeser Level DC Sinyal AC." },
{ "en": "Apa Itu Rangkaian Clipper?", "id": "Memotong Sebagian Sinyal Di Atas Level Tertentu." },
{ "en": "Apa Itu Sumber Arus Konstan?", "id": "Rangkaian Yang Memberikan Arus Tetap." },
{ "en": "Apa Itu Cermin Arus (Current Mirror)?", "id": "Menyalin Arus Melalui Satu Perangkat Aktif." },
{ "en": "Apa Itu Penguat Diferensial?", "id": "Menguatkan Perbedaan Antara Dua Sinyal Input." },
{ "en": "Apa Itu Common Mode Rejection Ratio (CMRR)?", "id": "Ukuran Kemampuan Menolak Sinyal Mode Umum." },
{ "en": "Apa Itu Slew Rate?", "id": "Laju Perubahan Maksimum Output Op-Amp." },
{ "en": "Apa Itu Gain-Bandwidth Product?", "id": "Produk Penguatan Loop Terbuka Dan Bandwidth." },
{ "en": "Apa Itu Topologi Catu Daya?", "id": "Pengaturan Komponen Dalam Konverter Daya." },
{ "en": "Apa Itu Konverter Flyback?", "id": "Konverter DC-DC Terisolasi Menggunakan Induktor." },
{ "en": "Apa Itu Konverter Forward?", "id": "Konverter DC-DC Terisolasi Menggunakan Transformator." },
{ "en": "Apa Itu Konverter SEPIC?", "id": "Konverter DC-DC Yang Dapat Menaikkan Atau Menurunkan Tegangan." },
{ "en": "Apa Kepanjangan Dari SEPIC?", "id": "Single-Ended Primary-Inductor Converter." },
{ "en": "Apa Itu Konverter Cuk?", "id": "Konverter DC-DC Dengan Polaritas Output Terbalik." },
{ "en": "Apa Itu Kontrol Mode Arus?", "id": "Teknik Kontrol Catu Daya Switching." },
{ "en": "Apa Itu Kontrol Mode Tegangan?", "id": "Teknik Kontrol Catu Daya Switching Lainnya." },
{ "en": "Apa Itu Soft Switching?", "id": "Teknik Untuk Mengurangi Kerugian Switching." },
{ "en": "Apa Itu Zero Voltage Switching (ZVS)?", "id": "Menghidupkan Transistor Saat Tegangan Nol." },
{ "en": "Apa Itu Zero Current Switching (ZCS)?", "id": "Mematikan Transistor Saat Arus Nol." },
{ "en": "Apa Itu Interferensi Elektromagnetik (EMI)?", "id": "Gangguan Elektromagnetik Yang Mempengaruhi Sirkuit." },
{ "en": "Apa Itu Kompatibilitas Elektromagnetik (EMC)?", "id": "Kemampuan Perangkat Bekerja Tanpa Menyebabkan EMI." },
{ "en": "Apa Itu Perisai EMI (EMI Shielding)?", "id": "Menggunakan Kandang Konduktif Untuk Memblokir EMI." },
{ "en": "Apa Itu Manik Ferit (Ferrite Bead)?", "id": "Komponen Pasif Untuk Menekan Noise Frekuensi Tinggi." },
{ "en": "Apa Itu Agile Methodology?", "id": "Pendekatan Iteratif Untuk Manajemen Proyek." },
{ "en": "Apa Itu Scrum?", "id": "Kerangka Kerja Agile Untuk Mengelola Pengembangan Produk." },
{ "en": "Apa Itu Sprint Dalam Scrum?", "id": "Periode Waktu Singkat Untuk Menyelesaikan Pekerjaan." },
{ "en": "Apa Itu Kanban?", "id": "Metodologi Agile Yang Memvisualisasikan Alur Kerja." },
{ "en": "Apa Itu Product Backlog?", "id": "Daftar Prioritas Fitur Untuk Produk." },
{ "en": "Apa Itu Sprint Backlog?", "id": "Pekerjaan Yang Dipilih Untuk Diselesaikan Dalam Sprint." },
{ "en": "Apa Itu Daily Stand-up (Scrum)?", "id": "Rapat Harian Singkat Untuk Sinkronisasi Tim." },
{ "en": "Apa Itu Tinjauan Sprint (Sprint Review)?", "id": "Pertemuan Untuk Mendemonstrasikan Hasil Kerja Sprint." },
{ "en": "Apa Itu Retrospektif Sprint (Sprint Retrospective)?", "id": "Pertemuan Untuk Merefleksikan Dan Memperbaiki Proses Kerja." },
{ "en": "Apa Itu Pemilik Produk (Product Owner)?", "id": "Orang Yang Bertanggung Jawab Atas Product Backlog." },
{ "en": "Apa Itu Scrum Master?", "id": "Orang Yang Memfasilitasi Dan Mendukung Tim Scrum." },
{ "en": "Apa Itu Tim Pengembang (Development Team)?", "id": "Profesional Yang Mengerjakan Produk." },
{ "en": "Apa Itu Epik (Epic) Dalam Agile?", "id": "Kumpulan Besar Pekerjaan Terkait." },
{ "en": "Apa Itu Cerita Pengguna (User Story)?", "id": "Deskripsi Fitur Dari Perspektif Pengguna." },
{ "en": "Apa Itu Papan Sirkuit Cetak (PCB)?", "id": "Papan Untuk Menghubungkan Komponen Elektronik." },
{ "en": "Apa Kepanjangan Dari PCB (Printed Circuit Board)?", "id": "Printed Circuit Board." },
{ "en": "Apa Itu Lapisan (Layer) Dalam PCB?", "id": "Lapisan Tembaga Untuk Jalur Sirkuit." },
{ "en": "Apa Itu Via Dalam PCB?", "id": "Lubang Konduktif Yang Menghubungkan Lapisan Berbeda." },
{ "en": "Apa Itu Footprint Komponen?", "id": "Pola Bantalan (Pad) Untuk Menyolder Komponen." },
{ "en": "Apa Itu Teknologi Pemasangan Permukaan (SMT)?", "id": "Menyolder Komponen Langsung Ke Permukaan PCB." },
{ "en": "Apa Kepanjangan Dari SMT (Surface-Mount Technology)?", "id": "Surface-Mount Technology." },
{ "en": "Apa Itu Teknologi Through-Hole?", "id": "Memasang Komponen Dengan Kaki Melalui Lubang PCB." },
{ "en": "Apa Itu Solder Mask?", "id": "Lapisan Pelindung Pada PCB Untuk Mencegah Hubungan Singkat." },
{ "en": "Apa Itu Silkscreen?", "id": "Lapisan Tinta Untuk Menandai Komponen Pada PCB." },
{ "en": "Apa Itu Desain Skematik?", "id": "Diagram Yang Menggambarkan Sirkuit Elektronik." },
{ "en": "Apa Itu Tata Letak PCB (PCB Layout)?", "id": "Desain Fisik Jalur Dan Penempatan Komponen." },
{ "en": "Apa Itu File Gerber?", "id": "Format File Standar Untuk Fabrikasi PCB." },
{ "en": "Apa Itu Penyolderan (Soldering)?", "id": "Proses Menyambung Komponen Menggunakan Logam Pengisi." },
{ "en": "Apa Itu Solder?", "id": "Paduan Logam Yang Meleleh Untuk Membuat Sambungan." },
{ "en": "Apa Itu Fluks (Flux)?", "id": "Bahan Kimia Untuk Membersihkan Permukaan Saat Menyolder." },
{ "en": "Apa Itu Solder Reflow?", "id": "Proses Untuk Menyolder Komponen SMT Menggunakan Oven." },
{ "en": "Apa Itu Wave Soldering?", "id": "Proses Menyolder Komponen Through-Hole Secara Massal." },
{ "en": "Apa Itu Desoldering?", "id": "Proses Melepaskan Komponen Yang Telah Disolder." },
{ "en": "Apa Itu Sistem Kontrol Optimal?", "id": "Sistem Kontrol Yang Meminimalkan Fungsi Biaya." },
{ "en": "Apa Itu Pemrograman Dinamis?", "id": "Metode Untuk Memecahkan Masalah Optimisasi Kompleks." },
{ "en": "Apa Itu Persamaan Hamilton-Jacobi-Bellman?", "id": "Persamaan Diferensial Parsial Dalam Teori Kontrol Optimal." },
{ "en": "Apa Itu Linear-Quadratic Regulator (LQR)?", "id": "Jenis Kontroler Umpan Balik Optimal." },
{ "en": "Apa Itu Kontrol Prediktif Model (MPC)?", "id": "Strategi Kontrol Yang Menggunakan Model Untuk Memprediksi." },
{ "en": "Apa Kepanjangan Dari MPC (Model Predictive Control)?", "id": "Model Predictive Control." },
{ "en": "Apa Itu Kontrol Sliding Mode (SMC)?", "id": "Jenis Kontrol Non-Linear Yang Kokoh." },
{ "en": "Apa Itu Fungsi Lyapunov?", "id": "Fungsi Skalar Untuk Menganalisis Stabilitas Sistem." },
{ "en": "Apa Itu Teori Stabilitas Lyapunov?", "id": "Menentukan Stabilitas Sistem Dinamis Tanpa Menyelesaikan Persamaan." },
{ "en": "Apa Itu Kontrol Bang-Bang?", "id": "Kontroler Yang Beralih Antara Dua Keadaan Ekstrem." },
{ "en": "Apa Itu Sistem Hibrida?", "id": "Sistem Dinamis Dengan Interaksi Antara Waktu-Kontinu Dan Diskrit." },
{ "en": "Apa Itu Identifikasi Sistem?", "id": "Membangun Model Matematis Dari Data Pengukuran." },
{ "en": "Apa Itu Sinyal Pseudo-Random Binary Sequence (PRBS)?", "id": "Sinyal Deterministik Yang Menyerupai Derau Putih." },
{ "en": "Apa Itu Metode Kuadrat Terkecil (Least Squares)?", "id": "Metode Standar Untuk Regresi Dan Identifikasi Sistem." },
{ "en": "Apa Itu Sistem Underactuated?", "id": "Sistem Mekanis Dengan Aktuator Lebih Sedikit Dari DOF." },
{ "en": "Sebutkan Contoh Sistem Underactuated?", "id": "Pendulum Terbalik, Drone Quadcopter." },
{ "en": "Apa Itu Mekanika Lagrangian?", "id": "Formulasi Ulang Mekanika Klasik Berdasarkan Energi." },
{ "en": "Apa Itu Mekanika Hamiltonian?", "id": "Formulasi Lain Mekanika Klasik Berdasarkan Energi Total." },
{ "en": "Apa Itu Energi Kinetik?", "id": "Energi Yang Dimiliki Benda Karena Gerakannya." },
{ "en": "Apa Itu Energi Potensial?", "id": "Energi Yang Tersimpan Dalam Benda Karena Posisinya." },
{ "en": "Apa Itu Prinsip Hamilton?", "id": "Menyatakan Bahwa Lintasan Nyata Meminimalkan Aksi." },
{ "en": "Apa Itu Persamaan Euler-Lagrange?", "id": "Persamaan Gerak Yang Diturunkan Dari Lagrangian." },
{ "en": "Apa Itu Kendala Holonomik?", "id": "Kendala Yang Bergantung Hanya Pada Posisi." },
{ "en": "Apa Itu Kendala Non-Holonomik?", "id": "Kendala Yang Bergantung Pada Kecepatan." },
{ "en": "Apa Itu Ruang Konfigurasi?", "id": "Ruang Semua Posisi Yang Mungkin Dari Suatu Sistem." },
{ "en": "Apa Itu Sistem Penglihatan Stereo?", "id": "Menggunakan Dua Kamera Untuk Mendapatkan Persepsi Kedalaman." },
{ "en": "Apa Itu Kalibrasi Kamera?", "id": "Menentukan Parameter Internal Dan Eksternal Kamera." },
{ "en": "Apa Itu Epipolar Geometry?", "id": "Geometri Intrinsik Antara Dua Tampilan Adegan." },
{ "en": "Apa Itu Triangulasi Dalam Visi Komputer?", "id": "Menentukan Posisi Titik 3D Dari Dua Gambar." },
{ "en": "Apa Itu Aliran Optik (Optical Flow)?", "id": "Pola Gerakan Objek Dalam Rangkaian Gambar." },
{ "en": "Apa Itu OpenCV?", "id": "Perpustakaan Pemrograman Populer Untuk Visi Komputer." },
{ "en": "Apa Itu TensorFlow?", "id": "Platform Open-Source Untuk Pembelajaran Mesin." },
{ "en": "Apa Itu PyTorch?", "id": "Platform Pembelajaran Mesin Open-Source Lainnya." },
{ "en": "Apa Itu Keras?", "id": "API Jaringan Saraf Tingkat Tinggi." },
{ "en": "Apa Itu Jaringan Saraf Konvolusional (CNN)?", "id": "Kelas Jaringan Saraf Untuk Menganalisis Data Visual." },
{ "en": "Apa Kepanjangan Dari CNN (Convolutional Neural Network)?", "id": "Convolutional Neural Network." },
{ "en": "Apa Itu Jaringan Saraf Berulang (RNN)?", "id": "Jaringan Saraf Untuk Memproses Data Urutan." },
{ "en": "Apa Kepanjangan Dari RNN (Recurrent Neural Network)?", "id": "Recurrent Neural Network." },
{ "en": "Apa Itu Pembelajaran Mendalam (Deep Learning)?", "id": "Pembelajaran Mesin Menggunakan Jaringan Saraf Dengan Banyak Lapisan." },
{ "en": "Apa Itu Pembelajaran Terawasi (Supervised Learning)?", "id": "Belajar Dari Data Berlabel." },
{ "en": "Apa Itu Pembelajaran Tak Terawasi (Unsupervised Learning)?", "id": "Belajar Dari Data Yang Tidak Berlabel." },
{ "en": "Apa Itu Pembelajaran Penguatan (Reinforcement Learning)?", "id": "Belajar Melalui Coba-Coba Dengan Imbalan Dan Hukuman." },
{ "en": "Apa Itu Overfitting?", "id": "Model Bekerja Baik Pada Data Latih Tapi Buruk." },
{ "en": "Apa Itu Underfitting?", "id": "Model Terlalu Sederhana Untuk Menangkap Pola Data." },
{ "en": "Apa Itu Validasi Silang (Cross-Validation)?", "id": "Teknik Untuk Mengevaluasi Kinerja Model." },
{ "en": "Apa Itu Pemrosesan Bahasa Alami (NLP)?", "id": "Interaksi Antara Komputer Dan Bahasa Manusia." },
{ "en": "Apa Kepanjangan Dari NLP (Natural Language Processing)?", "id": "Natural Language Processing." },
{ "en": "Apa Itu Motor DC Brush?", "id": "Motor DC Yang Menggunakan Sikat Untuk Komutasi." },
{ "en": "Apa Itu Komutator?", "id": "Saklar Putar Mekanis Pada Motor DC." },
{ "en": "Apa Itu Torsi Cogging?", "id": "Interaksi Antara Magnet Rotor Dan Gigi Stator." },
{ "en": "Apa Itu Ripple Torsi?", "id": "Variasi Torsi Output Motor Secara Periodik." },
{ "en": "Apa Itu Kontroler Gerak (Motion Controller)?", "id": "Perangkat Yang Mengontrol Posisi Atau Kecepatan Aktuator." },
{ "en": "Apa Itu Interpolasi?", "id": "Menghitung Titik Antara Dalam Jalur Gerak." },
{ "en": "Apa Itu Kontrol Umpan Maju (Feedforward)?", "id": "Memberikan Sinyal Kontrol Berdasarkan Model Sistem." },
{ "en": "Apa Itu Peredam Getaran Aktif?", "id": "Menggunakan Aktuator Untuk Meredam Getaran Secara Aktif." },
{ "en": "Apa Itu Suspensi Elektromagnetik?", "id": "Suspensi Yang Menggunakan Gaya Elektromagnetik." },
{ "en": "Apa Itu Bantalan Magnetik?", "id": "Bantalan Yang Menopang Beban Menggunakan Levitasi Magnetik." },
{ "en": "Apa Itu Sistem Nanoelektromekanis (NEMS)?", "id": "Perangkat Mekanis Dan Elektronis Skala Nano." },
{ "en": "Apa Kepanjangan Dari NEMS (Nanoelectromechanical Systems)?", "id": "Nanoelectromechanical Systems." },
{ "en": "Apa Itu Mikromanufaktur?", "id": "Proses Fabrikasi Untuk Perangkat Skala Mikro." },
{ "en": "Apa Itu Fotolitografi?", "id": "Proses Menggunakan Cahaya Untuk Mentransfer Pola." },
{ "en": "Apa Itu Etching?", "id": "Proses Menghilangkan Material Secara Kimiawi." },
{ "en": "Apa Itu Deposisi Film Tipis?", "id": "Proses Menumbuhkan Lapisan Tipis Material." },
{ "en": "Apa Itu Ruang Bersih (Cleanroom)?", "id": "Lingkungan Terkontrol Dengan Kontaminan Sangat Rendah." },
{ "en": "Apa Itu Wafer Silikon?", "id": "Substrat Tipis Untuk Fabrikasi Sirkuit Terpadu." },
{ "en": "Apa Itu Mikrofluida?", "id": "Ilmu Dan Teknologi Sistem Dengan Aliran Cairan." },
{ "en": "Apa Itu Teknik Biomedis?", "id": "Aplikasi Prinsip Teknik Ke Dalam Bidang Medis." },
{ "en": "Apa Itu Pencitraan Medis?", "id": "Teknik Untuk Menciptakan Gambar Visual Tubuh." },
{ "en": "Sebutkan Contoh Pencitraan Medis?", "id": "Sinar-X, CT Scan, Dan MRI." },
{ "en": "Apa Itu Magnetic Resonance Imaging (MRI)?", "id": "Menggunakan Medan Magnet Kuat Untuk Pencitraan Tubuh." },
{ "en": "Apa Itu Computed Tomography (CT) Scan?", "id": "Menggunakan Sinar-X Untuk Membuat Gambar Lintas Penampang." },
{ "en": "Apa Itu Ultrasonografi Medis?", "id": "Menggunakan Gelombang Ultrasonik Untuk Pencitraan Organ." },
{ "en": "Apa Itu Elektrokardiogram (ECG/EKG)?", "id": "Merekam Aktivitas Listrik Jantung." },
{ "en": "Apa Itu Pacemaker Jantung?", "id": "Perangkat Elektronik Untuk Mengatur Detak Jantung." },
{ "en": "Apa Itu Defibrilator?", "id": "Memberikan Kejutan Listrik Untuk Memulihkan Irama Jantung." },
{ "en": "Apa Itu Mesin Dialisis?", "id": "Membersihkan Darah Saat Ginjal Gagal Berfungsi." },
{ "en": "Apa Itu Ventilator Medis?", "id": "Mesin Bantu Pernapasan Untuk Pasien." },
{ "en": "Apa Itu Endoskopi?", "id": "Melihat Bagian Dalam Tubuh Menggunakan Tabung Fleksibel." },
{ "en": "Apa Itu Robotika Pertanian (Agri-Mechatronics)?", "id": "Aplikasi Mekatronika Dalam Bidang Pertanian." },
{ "en": "Apa Itu Traktor Otonom?", "id": "Traktor Yang Dapat Beroperasi Tanpa Pengemudi." },
{ "en": "Apa Itu Drone Pertanian?", "id": "Pesawat Tanpa Awak Untuk Pemantauan Tanaman." },
{ "en": "Apa Itu Teknologi Laju Variabel (VRT)?", "id": "Mengaplikasikan Input Pertanian Secara Bervariasi." },
{ "en": "Apa Kepanjangan Dari VRT (Variable Rate Technology)?", "id": "Variable Rate Technology." },
{ "en": "Apa Itu Pertanian Presisi?", "id": "Pendekatan Manajemen Pertanian Menggunakan Teknologi Informasi." },
{ "en": "Apa Itu Pemanenan Robotik?", "id": "Menggunakan Robot Untuk Memanen Buah Dan Sayuran." },
{ "en": "Apa Itu Sistem Penyortiran Otomatis?", "id": "Menyortir Hasil Panen Berdasarkan Ukuran Dan Kualitas." },
{ "en": "Apa Itu Irigasi Cerdas?", "id": "Sistem Irigasi Yang Mengoptimalkan Penggunaan Air." },
{ "en": "Apa Itu Avionik?", "id": "Sistem Elektronik Yang Digunakan Pada Pesawat Terbang." },
{ "en": "Apa Itu Sistem Autopilot?", "id": "Sistem Untuk Mengendalikan Pesawat Secara Otomatis." },
{ "en": "Apa Itu Sistem Navigasi Inersia (INS)?", "id": "Menggunakan Sensor Inersia Untuk Menentukan Posisi." },
{ "en": "Apa Kepanjangan Dari INS (Inertial Navigation System)?", "id": "Inertial Navigation System." },
{ "en": "Apa Itu Sistem Penentuan Posisi Global (GPS)?", "id": "Sistem Navigasi Satelit Untuk Menentukan Lokasi." },
{ "en": "Apa Itu Sistem Fly-by-Wire?", "id": "Menggantikan Kontrol Manual Dengan Antarmuka Elektronik." },
{ "en": "Apa Itu Aktuator Dirgantara?", "id": "Aktuator Yang Digunakan Untuk Menggerakkan Permukaan Kontrol Pesawat." },
{ "en": "Apa Itu Black Box (Perekam Penerbangan)?", "id": "Merekam Data Penerbangan Dan Percakapan Kokpit." },
{ "en": "Apa Itu Wahana Antariksa Tanpa Awak (Drone)?", "id": "Kendaraan Yang Beroperasi Di Luar Angkasa Tanpa Manusia." },
{ "en": "Apa Itu Rover Mars?", "id": "Kendaraan Robotik Yang Menjelajahi Permukaan Mars." },
{ "en": "Apa Itu Sistem Kontrol Reaksi (RCS)?", "id": "Pendorong Kecil Untuk Mengontrol Orientasi Wahana Antariksa." },
{ "en": "Apa Itu Roda Reaksi?", "id": "Menggunakan Roda Berputar Untuk Mengubah Orientasi." },
{ "en": "Apa Itu Giroskop Momen Kontrol (CMG)?", "id": "Perangkat Kontrol Sikap Untuk Wahana Antariksa Besar." },
{ "en": "Apa Itu Pelacakan Bintang (Star Tracker)?", "id": "Sensor Untuk Menentukan Orientasi Menggunakan Bintang." },
{ "en": "Apa Itu Robotika Bawah Air?", "id": "Robot Yang Dirancang Untuk Beroperasi Di Bawah Air." },
{ "en": "Apa Itu Remotely Operated Vehicle (ROV)?", "id": "Kendaraan Bawah Air Yang Dikendalikan Dari Jarak Jauh." },
{ "en": "Apa Itu Autonomous Underwater Vehicle (AUV)?", "id": "Kendaraan Bawah Air Otonom Yang Tidak Terikat." },
{ "en": "Apa Itu Sonar Samping (Side-Scan Sonar)?", "id": "Menciptakan Gambar Dasar Laut." },
{ "en": "Apa Itu Kendaraan Permukaan Tanpa Awak (USV)?", "id": "Perahu Atau Kapal Yang Beroperasi Secara Otonom." },
{ "en": "Apa Itu Analisis Keandalan?", "id": "Studi Dan Prediksi Kegagalan Komponen." },
{ "en": "Apa Itu Waktu Rata-Rata Antar Kegagalan (MTBF)?", "id": "Waktu Operasi Rata-rata Antara Kegagalan." },
{ "en": "Apa Kepanjangan Dari MTBF (Mean Time Between Failures)?", "id": "Mean Time Between Failures." },
{ "en": "Apa Itu Waktu Rata-rata Untuk Perbaikan (MTTR)?", "id": "Waktu Rata-rata Yang Dibutuhkan Untuk Perbaikan." },
{ "en": "Apa Kepanjangan Dari MTTR (Mean Time To Repair)?", "id": "Mean Time To Repair." },
{ "en": "Apa Itu Ketersediaan (Availability)?", "id": "Probabilitas Sistem Beroperasi Saat Dibutuhkan." },
{ "en": "Apa Itu Diagram Blok Keandalan (RBD)?", "id": "Diagram Untuk Menganalisis Keandalan Sistem." },
{ "en": "Apa Itu Analisis Pohon Kegagalan (FTA)?", "id": "Pendekatan Top-down Untuk Menganalisis Penyebab Kegagalan." },
{ "en": "Apa Itu Kurva Bak Mandi (Bathtub Curve)?", "id": "Grafik Tingkat Kegagalan Produk Seiring Waktu." },
{ "en": "Apa Itu Uji Hidup Dipercepat (Accelerated Life Testing)?", "id": "Menguji Produk Di Bawah Kondisi Keras." },
{ "en": "Apa Itu Derating Komponen?", "id": "Mengoperasikan Komponen Di Bawah Peringkat Maksimumnya." },
{ "en": "Apa Itu Bahan Fungsional?", "id": "Material Yang Memiliki Sifat Intrinsik Tertentu." },
{ "en": "Apa Itu Bahan Termoelektrik?", "id": "Mengubah Perbedaan Suhu Menjadi Energi Listrik." },
{ "en": "Apa Itu Bahan Fotokromik?", "id": "Bahan Yang Berubah Warna Saat Terkena Cahaya." },
{ "en": "Apa Itu Bahan Termokromik?", "id": "Bahan Yang Berubah Warna Berdasarkan Suhu." },
{ "en": "Apa Itu Bahan Elektroluminesen?", "id": "Memancarkan Cahaya Sebagai Respon Terhadap Arus Listrik." },
{ "en": "Apa Itu Cairan Magnetoreologikal (MR Fluid)?", "id": "Cairan Yang Viskositasnya Berubah Dengan Medan Magnet." },
{ "en": "Apa Itu Cairan Elektroreologikal (ER Fluid)?", "id": "Cairan Yang Viskositasnya Berubah Dengan Medan Listrik." },
{ "en": "Apa Itu Polimer Konduktif?", "id": "Polimer Organik Yang Dapat Menghantarkan Listrik." },
{ "en": "Apa Itu Aerogel?", "id": "Material Padat Sintetis Dengan Kepadatan Sangat Rendah." },
{ "en": "Apa Itu Graphene?", "id": "Lapisan Tunggal Atom Karbon Dalam Kisi Dua Dimensi." },
{ "en": "Apa Itu Nanotube Karbon?", "id": "Struktur Silinder Dari Atom Karbon." },
{ "en": "Apa Itu Metamaterial?", "id": "Material Buatan Dengan Sifat Yang Tidak Ditemukan." },
{ "en": "Apa Itu Self-Healing Material?", "id": "Material Yang Dapat Memperbaiki Kerusakan Secara Otomatis." },
{ "en": "Apa Itu Sistem Otomotif?", "id": "Sistem Yang Ditemukan Dalam Kendaraan Modern." },
{ "en": "Apa Itu Kontrol Traksi?", "id": "Mencegah Kehilangan Traksi Roda Kendaraan." },
{ "en": "Apa Itu Power Steering Elektronik (EPS)?", "id": "Menggunakan Motor Listrik Untuk Membantu Kemudi." },
{ "en": "Apa Itu Cruise Control Adaptif (ACC)?", "id": "Menjaga Jarak Aman Dengan Kendaraan Di Depan." },
{ "en": "Apa Itu Sistem Peringatan Pindah Jalur?", "id": "Memperingatkan Pengemudi Jika Keluar Jalur Tanpa Sengaja." },
{ "en": "Apa Itu Deteksi Titik Buta (Blind Spot)?", "id": "Mendeteksi Kendaraan Di Area Titik Buta Pengemudi." },
{ "en": "Apa Itu Bantuan Parkir Otomatis?", "id": "Sistem Yang Membantu Mengarahkan Kendaraan Saat Parkir." },
{ "en": "Apa Itu Tampilan Head-Up (HUD)?", "id": "Memproyeksikan Informasi Ke Kaca Depan Kendaraan." },
{ "en": "Apa Itu Kendaraan-ke-Semua (V2X) Komunikasi?", "id": "Teknologi Komunikasi Antara Kendaraan Dan Lingkungannya." },
{ "en": "Apa Itu Gerbang Logika Programmable (PLD)?", "id": "Sirkuit Terpadu Untuk Membangun Sirkuit Digital Kustom." },
{ "en": "Apa Kepanjangan Dari PLD (Programmable Logic Device)?", "id": "Programmable Logic Device." },
{ "en": "Apa Itu Complex PLD (CPLD)?", "id": "PLD Yang Lebih Kompleks Dari Tipe Sederhana." },
{ "en": "Apa Itu Antarmuka JTAG (Joint Test Action Group)?", "id": "Standar Untuk Pengujian Dan Debugging Papan Sirkuit." },
{ "en": "Apa Itu Boundary Scan?", "id": "Metode Pengujian Sirkuit Terpadu Menggunakan JTAG." },
{ "en": "Apa Itu Desain untuk Pengujian (DFT)?", "id": "Teknik Desain Untuk Membuat Pengujian Lebih Mudah." },
{ "en": "Apa Itu Built-In Self-Test (BIST)?", "id": "Sirkuit Yang Memungkinkan Perangkat Menguji Dirinya Sendiri." },
{ "en": "Apa Itu Automatic Test Equipment (ATE)?", "id": "Peralatan Otomatis Untuk Menguji Perangkat Elektronik." },
{ "en": "Apa Itu Analisis Waktu (Timing Analysis)?", "id": "Menganalisis Kinerja Waktu Sirkuit Digital." },
{ "en": "Apa Itu Setup Time?", "id": "Waktu Data Stabil Sebelum Pulsa Clock." },
{ "en": "Apa Itu Hold Time?", "id": "Waktu Data Stabil Setelah Pulsa Clock." },
{ "en": "Apa Itu Clock Skew?", "id": "Perbedaan Waktu Tiba Sinyal Clock." },
{ "en": "Apa Itu Jitter?", "id": "Variasi Periodik Sinyal Dari Posisi Idealnya." },
{ "en": "Apa Itu Metastabilitas?", "id": "Keadaan Tidak Stabil Dalam Sistem Digital." },
{ "en": "Apa Itu Sinkronizer?", "id": "Sirkuit Untuk Mentransfer Sinyal Antara Domain Clock." },
{ "en": "Apa Itu FIFO (First-In, First-Out)?", "id": "Buffer Memori Yang Memproses Data Secara Berurutan." },
{ "en": "Apa Itu Akses Memori Langsung (DMA)?", "id": "Fitur Yang Memungkinkan Perangkat Mengakses Memori Utama." },
{ "en": "Apa Kepanjangan Dari DMA (Direct Memory Access)?", "id": "Direct Memory Access." },
{ "en": "Apa Itu Pengontrol Interupsi?", "id": "Perangkat Keras Yang Mengelola Sinyal Interupsi." },
{ "en": "Apa Itu Floating Point Unit (FPU)?", "id": "Bagian Khusus CPU Untuk Aritmatika Floating Point." },
{ "en": "Apa Itu Pipelining Instruksi?", "id": "Teknik Untuk Meningkatkan Throughput Instruksi CPU." },
{ "en": "Apa Itu Eksekusi Superskalar?", "id": "Arsitektur CPU Yang Dapat Mengeksekusi Beberapa Instruksi." },
{ "en": "Apa Itu Komputasi Paralel?", "id": "Menjalankan Banyak Perhitungan Secara Bersamaan." },
{ "en": "Apa Itu Arsitektur SIMD (Single Instruction, Multiple Data)?", "id": "Mengeksekusi Instruksi Yang Sama Pada Data Berbeda." },
{ "en": "Apa Itu GPGPU (General-Purpose Computing on Graphics Processing Units)?", "id": "Menggunakan GPU Untuk Tugas Komputasi Umum." },
{ "en": "Apa Itu CUDA (Compute Unified Device Architecture)?", "id": "Platform Komputasi Paralel Dari Nvidia." },
{ "en": "Apa Itu OpenCL (Open Computing Language)?", "id": "Standar Terbuka Untuk Pemrograman Paralel." },
{ "en": "Apa Itu Kriptografi?", "id": "Studi Tentang Teknik Komunikasi Aman." },
{ "en": "Apa Itu Algoritma Simetris?", "id": "Menggunakan Kunci Yang Sama Untuk Enkripsi Dan Dekripsi." },
{ "en": "Apa Itu Algoritma Asimetris?", "id": "Menggunakan Kunci Publik Dan Kunci Privat." },
{ "en": "Apa Itu Tanda Tangan Digital?", "id": "Skema Matematis Untuk Memverifikasi Keaslian Pesan Digital." },
{ "en": "Apa Itu Sertifikat Digital?", "id": "File Elektronik Yang Mengikat Kunci Publik." },
{ "en": "Apa Itu Otoritas Sertifikat (Certificate Authority - CA)?", "id": "Entitas Tepercaya Yang Menerbitkan Sertifikat Digital." },
{ "en": "Apa Itu Public Key Infrastructure (PKI)?", "id": "Kerangka Kerja Untuk Mengelola Enkripsi Kunci Publik." },
{ "en": "Apa Itu Hash Function?", "id": "Fungsi Yang Mengubah Data Menjadi String Ukuran Tetap." },
{ "en": "Apa Itu Blockchain?", "id": "Buku Besar Digital Terdistribusi Yang Diamankan Kriptografi." },
{ "en": "Apa Itu Kontrol Akses?", "id": "Membatasi Akses Ke Sumber Daya Sistem." },
{ "en": "Apa Itu Intrusion Detection System (IDS)?", "id": "Memantau Jaringan Untuk Aktivitas Berbahaya." },
{ "en": "Apa Itu Intrusion Prevention System (IPS)?", "id": "Mengidentifikasi Dan Memblokir Ancaman Keamanan." },
{ "en": "Apa Itu Honeypot?", "id": "Mekanisme Jebakan Keamanan Siber Untuk Mendeteksi Penyerang." },
{ "en": "Apa Itu Rekayasa Perangkat Lunak?", "id": "Aplikasi Prinsip Teknik Untuk Pengembangan Perangkat Lunak." },
{ "en": "Apa Itu Model Air Terjun (Waterfall Model)?", "id": "Model Pengembangan Perangkat Lunak Sekuensial." },
{ "en": "Apa Itu Model Spiral?", "id": "Model Pengembangan Perangkat Lunak Berbasis Risiko." },
{ "en": "Apa Itu Prototyping?", "id": "Membuat Versi Awal Produk Untuk Pengujian." },
{ "en": "Apa Itu Verifikasi Perangkat Lunak?", "id": "Memastikan Perangkat Lunak Dibangun Dengan Benar." },
{ "en": "Apa Itu Validasi Perangkat Lunak?", "id": "Memastikan Perangkat Lunak Yang Benar Dibangun." },
{ "en": "Apa Itu Utilitas?", "id": "Ukuran Kepuasan Atau Preferensi Dalam Ekonomi." },
{ "en": "Apa Itu Teori Permainan (Game Theory)?", "id": "Studi Model Matematika Dari Interaksi Strategis." },
{ "en": "Apa Itu Keseimbangan Nash?", "id": "Keadaan Stabil Di Mana Tidak Ada Pemain." },
{ "en": "Apa Itu Optimasi?", "id": "Menemukan Solusi Terbaik Dari Semua Kemungkinan." },
{ "en": "Apa Itu Algoritma Genetika?", "id": "Metode Optimasi Yang Terinspirasi Oleh Seleksi Alam." },
{ "en": "Apa Itu Simulated Annealing?", "id": "Metode Optimasi Probabilistik Untuk Masalah Global." },
{ "en": "Apa Itu Particle Swarm Optimization?", "id": "Metode Optimasi Berdasarkan Perilaku Kawanan." },
{ "en": "Apa Itu Ant Colony Optimization?", "id": "Teknik Probabilistik Untuk Memecahkan Masalah Komputasi." },
{ "en": "Apa Itu Riset Operasi?", "id": "Disiplin Yang Berurusan Dengan Penerapan Metode Analitis." },
{ "en": "Apa Itu Pemrograman Linier?", "id": "Teknik Optimasi Untuk Masalah Dengan Hubungan Linier." },
{ "en": "Apa Itu Algoritma Simplex?", "id": "Algoritma Populer Untuk Pemrograman Linier." },
{ "en": "Apa Itu Masalah Transportasi?", "id": "Jenis Masalah Optimasi Dalam Riset Operasi." },
{ "en": "Apa Itu Teori Antrian?", "id": "Studi Matematis Tentang Garis Tunggu." },
{ "en": "Apa Itu Proses Markov?", "id": "Model Stokastik Yang Menggambarkan Urutan Peristiwa." },
{ "en": "Apa Itu Dinamika Sistem?", "id": "Pendekatan Untuk Memahami Perilaku Sistem Kompleks." },
{ "en": "Apa Itu Causal Loop Diagram?", "id": "Diagram Yang Membantu Memvisualisasikan Hubungan." },
{ "en": "Apa Itu Stok Dan Aliran (Stock and Flow)?", "id": "Elemen Dasar Dalam Diagram Dinamika Sistem." },
{ "en": "Apa Itu Umpan Balik Positif?", "id": "Loop Umpan Balik Yang Memperkuat Perubahan." },
{ "en": "Apa Itu Umpan Balik Negatif?", "id": "Loop Umpan Balik Yang Menstabilkan Sistem." },
{ "en": "Apa Itu Human-in-the-Loop?", "id": "Model Yang Membutuhkan Interaksi Manusia." },
{ "en": "Apa Itu Otomasi Adaptif?", "id": "Otomasi Yang Mengubah Tingkat Bantuan." },
{ "en": "Apa Itu Kesadaran Situasi?", "id": "Persepsi Elemen Lingkungan." },
{ "en": "Apa Itu Beban Kerja Mental?", "id": "Jumlah Usaha Mental Yang Dibutuhkan." },
{ "en": "Apa Itu Interaksi Manusia-Robot (HRI)?", "id": "Studi Interaksi Antara Manusia Dan Robot." },
{ "en": "Apa Itu Keselamatan Dan Kesehatan Kerja (K3)?", "id": "Disiplin Yang Berhubungan Dengan Keselamatan Di Tempat Kerja." },
{ "en": "Apa Itu Alat Pelindung Diri (APD)?", "id": "Peralatan Untuk Melindungi Pekerja Dari Bahaya." },
{ "en": "Apa Itu Analisis Keselamatan Kerja (JSA)?", "id": "Prosedur Untuk Mengidentifikasi Bahaya Sebelum Terjadi." },
{ "en": "Apa Itu Lockout-Tagout (LOTO)?", "id": "Prosedur Keselamatan Untuk Mematikan Mesin Berbahaya." },
{ "en": "Apa Itu Tombol Berhenti Darurat (E-Stop)?", "id": "Saklar Untuk Menghentikan Mesin Dalam Keadaan Darurat." },
{ "en": "Apa Itu Tirai Cahaya Keselamatan?", "id": "Sensor Untuk Mendeteksi Kehadiran Dalam Zona Berbahaya." },
{ "en": "Apa Itu Saklar Interlock?", "id": "Saklar Yang Mencegah Operasi Mesin." },
{ "en": "Apa Itu Kontroler Keselamatan?", "id": "PLC Khusus Untuk Mengimplementasikan Fungsi Keselamatan." },
{ "en": "Apa Itu Kategori Berhenti (Stop Category)?", "id": "Klasifikasi Cara Mesin Dihentikan." },
{ "en": "Apa Itu Kategori Berhenti 0?", "id": "Penghentian Segera Dengan Memutus Daya." },
{ "en": "Apa Itu Kategori Berhenti 1?", "id": "Penghentian Terkendali Dengan Daya Tetap Aktif." },
{ "en": "Apa Itu Kategori Berhenti 2?", "id": "Penghentian Terkendali Tanpa Memutus Daya." },
{ "en": "Apa Itu Penilaian Risiko?", "id": "Proses Keseluruhan Mengidentifikasi Dan Menganalisis Risiko." },
{ "en": "Apa Itu Hirarki Kontrol Bahaya?", "id": "Pendekatan Bertahap Untuk Menghilangkan Bahaya." },
{ "en": "Sebutkan Tingkatan Hirarki Kontrol?", "id": "Eliminasi, Substitusi, Rekayasa, Administratif, APD." },
{ "en": "Apa Itu Ruang Terbatas?", "id": "Ruang Yang Cukup Besar Untuk Masuk." },
{ "en": "Apa Itu Izin Kerja Panas?", "id": "Dokumen Izin Untuk Pekerjaan Yang Menghasilkan Api." },
{ "en": "Apa Itu Mekatronika Otomotif?", "id": "Penerapan Mekatronika Dalam Desain Kendaraan." },
{ "en": "Apa Itu Unit Kontrol Elektronik (ECU)?", "id": "Perangkat Tertanam Yang Mengontrol Fungsi Listrik." },
{ "en": "Apa Itu On-Board Diagnostics (OBD)?", "id": "Sistem Diagnostik Mandiri Dalam Kendaraan." },
{ "en": "Apa Itu Sistem Rem Parkir Elektronik?", "id": "Mengaktifkan Dan Melepas Rem Parkir Secara Elektronik." },
{ "en": "Apa Itu Sistem Pemantauan Tekanan Ban (TPMS)?", "id": "Memantau Tekanan Udara Di Dalam Ban." },
{ "en": "Apa Itu Wiper Sensor Hujan?", "id": "Wiper Yang Aktif Secara Otomatis Saat Hujan." },
{ "en": "Apa Itu Kontrol Iklim Otomatis?", "id": "Mengatur Suhu Kabin Secara Otomatis." },
{ "en": "Apa Itu Kursi Daya Memori?", "id": "Kursi Yang Dapat Menyimpan Beberapa Pengaturan Posisi." },
{ "en": "Apa Itu Spion Otomatis Redup?", "id": "Spion Yang Mengurangi Silau Dari Lampu Belakang." },
{ "en": "Apa Itu Keyless Entry System?", "id": "Membuka Kunci Kendaraan Tanpa Kunci Fisik." },
{ "en": "Apa Itu Tombol Start/Stop?", "id": "Menghidupkan Mesin Tanpa Memutar Kunci." },
{ "en": "Apa Itu Lampu Depan Adaptif?", "id": "Lampu Depan Yang Mengarahkan Sinar." },
{ "en": "Apa Itu Parameter Denavit-Hartenberg (DH)?", "id": "Metode Standar Untuk Menggambarkan Kinematika Robot." },
{ "en": "Apa Itu Kerangka Acuan (Frame)?", "id": "Sistem Koordinat Yang Melekat Pada Suatu Benda." },
{ "en": "Apa Itu Matriks Rotasi?", "id": "Matriks Untuk Melakukan Rotasi Dalam Ruang Euklides." },
{ "en": "Apa Itu Sudut Euler?", "id": "Tiga Sudut Untuk Menggambarkan Orientasi Benda Kaku." },
{ "en": "Apa Itu Quaternion?", "id": "Sistem Bilangan Untuk Merepresentasikan Rotasi." },
{ "en": "Apa Keuntungan Quaternion Atas Sudut Euler?", "id": "Menghindari Masalah Gimbal Lock." },
{ "en": "Apa Itu Gimbal Lock?", "id": "Kehilangan Satu Derajat Kebebasan Rotasi." },
{ "en": "Apa Itu Dinamika Robot?", "id": "Studi Hubungan Antara Gerak Robot Dan Gayanya." },
{ "en": "Apa Itu Persamaan Gerak Euler-Lagrange?", "id": "Metode Untuk Menurunkan Model Dinamis Robot." },
{ "en": "Apa Itu Elipsoid Inersia?", "id": "Representasi Geometris Dari Inersia Rotasi Benda." },
{ "en": "Apa Itu Tensor Inersia?", "id": "Kuantitas Yang Menjelaskan Inersia Rotasi Benda." },
{ "en": "Apa Itu Algoritma Dinamika Maju?", "id": "Menghitung Percepatan Sendi Dari Torsi Yang Diberikan." },
{ "en": "Apa Itu Algoritma Dinamika Balik?", "id": "Menghitung Torsi Sendi Untuk Percepatan Yang Diinginkan." },
{ "en": "Apa Itu Kontrol Torsi Terhitung?", "id": "Strategi Kontrol Berbasis Model Untuk Robot." },
{ "en": "Apa Itu Impedance Control?", "id": "Strategi Kontrol Yang Mengatur Hubungan Gaya-Posisi." },
{ "en": "Apa Itu Admittance Control?", "id": "Strategi Kontrol Yang Mengatur Hubungan Posisi-Gaya." },
{ "en": "Apa Itu Sistem Teleoperasi?", "id": "Sistem Robotik Master-Slave Untuk Operasi Jarak Jauh." },
{ "en": "Apa Itu Transparansi Dalam Teleoperasi?", "id": "Kemampuan Operator Merasakan Lingkungan Jauh." },
{ "en": "Apa Itu Stabilitas Dalam Teleoperasi?", "id": "Menjamin Sistem Tetap Stabil Meskipun Ada Penundaan." },
{ "en": "Apa Itu Penundaan Waktu (Time Delay)?", "id": "Penundaan Dalam Transmisi Sinyal Komunikasi." },
{ "en": "Apa Itu Robotika Ruang Angkasa?", "id": "Robot Yang Digunakan Dalam Misi Luar Angkasa." },
{ "en": "Apa Itu Canadarm?", "id": "Lengan Robotik Yang Digunakan Pada Pesawat Ulang-alik." },
{ "en": "Apa Itu Dextre?", "id": "Robot Manipulator Dua Lengan Di Stasiun Luar Angkasa." },
{ "en": "Apa Itu Robotika Konstruksi?", "id": "Otomasi Tugas Dalam Industri Konstruksi." },
{ "en": "Apa Itu Pencetakan 3D Kontur?", "id": "Metode Pencetakan 3D Skala Besar Untuk Bangunan." },
{ "en": "Apa Itu Robot Pembongkaran?", "id": "Robot Untuk Menghancurkan Struktur Bangunan." },
{ "en": "Apa Itu Robot Pemasang Batu Bata?", "id": "Mesin Otomatis Untuk Meletakkan Batu Bata." },
{ "en": "Apa Itu Inspeksi Infrastruktur Robotik?", "id": "Menggunakan Robot Untuk Memeriksa Jembatan Dan Terowongan." },
{ "en": "Apa Itu Sistem Informasi Geografis (GIS)?", "id": "Sistem Untuk Menangkap Dan Menganalisis Data Geografis." },
{ "en": "Apa Itu Penginderaan Jauh?", "id": "Memperoleh Informasi Tentang Objek Dari Jarak Jauh." },
{ "en": "Apa Itu Fotogrametri?", "id": "Ilmu Pengukuran Dari Foto." },
{ "en": "Apa Itu LiDAR (Light Detection and Ranging)?", "id": "Teknologi Penginderaan Jauh Menggunakan Laser." },
{ "en": "Apa Itu Point Cloud?", "id": "Kumpulan Titik Data Dalam Ruang Tiga Dimensi." },
{ "en": "Apa Itu Robotika Lunak (Soft Robotics)?", "id": "Robot Yang Terbuat Dari Bahan Yang Mudah Disesuaikan." },
{ "en": "Apa Keuntungan Robotika Lunak?", "id": "Aman Untuk Interaksi Dan Mampu Beradaptasi." },
{ "en": "Sebutkan Contoh Aktuator Robot Lunak?", "id": "Otot Buatan Pneumatik (PAM)." },
{ "en": "Apa Itu Bahan Hiperelastis?", "id": "Bahan Elastis Non-linear Seperti Karet." },
{ "en": "Apa Itu Robotika Bio-Inspired?", "id": "Robot Yang Desainnya Terinspirasi Dari Sistem Biologis." },
{ "en": "Apa Itu Robotika Medis?", "id": "Penerapan Robot Dalam Prosedur Medis Dan Bedah." },
{ "en": "Apa Itu Kapsul Endoskopi?", "id": "Kamera Nirkabel Seukuran Pil Untuk Memeriksa Usus." },
{ "en": "Apa Itu Microrobotika?", "id": "Robot Dengan Ukuran Dalam Skala Mikrometer." },
{ "en": "Apa Itu Nanorobotika?", "id": "Robot Dengan Komponen Berukuran Nanometer." },
{ "en": "Apa Itu Penjepit Optik (Optical Tweezers)?", "id": "Menggunakan Sinar Laser Untuk Memanipulasi Objek Mikroskopis." },
{ "en": "Apa Itu Dielektroforesis?", "id": "Gerakan Partikel Akibat Medan Listrik Tidak Seragam." },
{ "en": "Apa Itu Perangkat Keras Komputasi Neuromorfik?", "id": "Perangkat Keras Yang Meniru Otak Manusia." },
{ "en": "Apa Itu Memristor?", "id": "Komponen Elektronik Keempat Setelah Resistor Kapasitor Induktor." },
{ "en": "Apa Itu Spintronik?", "id": "Pemanfaatan Sifat Spin Intrinsik Elektron." },
{ "en": "Apa Itu Komputasi Kuantum?", "id": "Menggunakan Prinsip Mekanika Kuantum Untuk Komputasi." },
{ "en": "Apa Itu Qubit?", "id": "Unit Dasar Informasi Dalam Komputasi Kuantum." },
{ "en": "Apa Itu Superposisi Kuantum?", "id": "Kemampuan Qubit Berada Dalam Beberapa Keadaan Sekaligus." },
{ "en": "Apa Itu Keterkaitan Kuantum (Quantum Entanglement)?", "id": "Keterhubungan Antara Dua Qubit Atau Lebih." },
{ "en": "Apa Itu Gerbang Kuantum?", "id": "Sirkuit Dasar Yang Beroperasi Pada Qubit." },
{ "en": "Apa Itu Mekatronika Olahraga?", "id": "Aplikasi Mekatronika Untuk Meningkatkan Kinerja Atletik." },
{ "en": "Apa Itu Perangkat Wearable?", "id": "Perangkat Elektronik Yang Dikenakan Sebagai Aksesori." },
{ "en": "Sebutkan Contoh Perangkat Wearable?", "id": "Jam Tangan Pintar Dan Pelacak Kebugaran." },
{ "en": "Apa Itu Tekstil Cerdas?", "id": "Kain Yang Ditenun Dengan Komponen Elektronik." },
{ "en": "Apa Itu Sistem Panen Energi (Energy Harvesting)?", "id": "Mengambil Energi Dari Sumber Lingkungan." },
{ "en": "Apa Itu Panen Energi Piezoelektrik?", "id": "Menghasilkan Listrik Dari Getaran Atau Tekanan." },
{ "en": "Apa Itu Panen Energi Termoelektrik?", "id": "Menghasilkan Listrik Dari Perbedaan Suhu." },
{ "en": "Apa Itu Panen Energi RF?", "id": "Mengumpulkan Energi Dari Gelombang Radio Sekitar." },
{ "en": "Apa Itu Mekatronika Hiburan?", "id": "Penggunaan Mekatronika Dalam Film Dan Taman Hiburan." },
{ "en": "Apa Itu Animatronik?", "id": "Penggunaan Robotika Untuk Meniru Makhluk Hidup." },
{ "en": "Apa Itu Motion Capture?", "id": "Merekam Gerakan Objek Atau Orang." },
{ "en": "Apa Itu Sistem Kontrol Pertunjukan?", "id": "Sistem Terkomputerisasi Untuk Mengontrol Acara Langsung." },
{ "en": "Apa Itu Otomasi Rumah (Domotics)?", "id": "Otomatisasi Bangunan Untuk Rumah." },
{ "en": "Apa Itu Termostat Cerdas?", "id": "Termostat Yang Dapat Diprogram Dan Terhubung Internet." },
{ "en": "Apa Itu Pencahayaan Cerdas?", "id": "Sistem Pencahayaan Yang Dapat Dikontrol Jarak Jauh." },
{ "en": "Apa Itu Kunci Pintu Cerdas?", "id": "Kunci Elektromekanis Yang Dikontrol Secara Nirkabel." },
{ "en": "Apa Itu Asisten Virtual?", "id": "Agen Perangkat Lunak Yang Melakukan Tugas." },
{ "en": "Apa Itu Protokol Zigbee?", "id": "Standar Nirkabel Untuk Jaringan Area Pribadi." },
{ "en": "Apa Itu Protokol Z-Wave?", "id": "Protokol Komunikasi Nirkabel Lain Untuk Otomasi Rumah." },
{ "en": "Apa Itu Thread (Protokol Jaringan)?", "id": "Protokol Jaringan Mesh Berbasis IP." },
{ "en": "Apa Itu Matter (Standar)?", "id": "Standar Konektivitas Terpadu Untuk Perangkat Rumah Pintar." },
{ "en": "Apa Itu Otomasi Industri?", "id": "Penggunaan Sistem Kontrol Untuk Mengelola Proses Industri." },
{ "en": "Apa Itu Piramida Otomasi?", "id": "Model Hierarkis Tingkat Otomasi." },
{ "en": "Sebutkan Tingkatan Piramida Otomasi?", "id": "Lapangan, Kontrol, Pengawasan, Perencanaan, Manajemen." },
{ "en": "Apa Itu Tingkat Lapangan (Field Level)?", "id": "Tingkat Terendah Dengan Sensor Dan Aktuator." },
{ "en": "Apa Itu Tingkat Kontrol (Control Level)?", "id": "Tingkat Di Mana PLC Dan Kontroler PID Bekerja." },
{ "en": "Apa Itu Tingkat Pengawasan (Supervisory Level)?", "id": "Tingkat Dengan HMI Dan SCADA Untuk Pemantauan." },
{ "en": "Apa Itu Tingkat Perencanaan (Planning Level)?", "id": "Tingkat Dengan MES Untuk Manajemen Produksi." },
{ "en": "Apa Itu Tingkat Manajemen (Management Level)?", "id": "Tingkat Tertinggi Dengan ERP Untuk Perencanaan Bisnis." },
{ "en": "Apa Itu OPC (Open Platform Communications)?", "id": "Standar Interoperabilitas Untuk Komunikasi Industri." },
{ "en": "Apa Itu EtherNet/IP?", "id": "Protokol Ethernet Industri yang Dikelola oleh ODVA." },
{ "en": "Apa Itu PROFINET?", "id": "Standar Ethernet Industri dari Siemens." },
{ "en": "Apa Itu EtherCAT?", "id": "Protokol Jaringan Berbasis Ethernet Kinerja Tinggi." },
{ "en": "Apa Itu Modbus TCP/IP?", "id": "Varian Modbus Yang Digunakan Melalui Jaringan TCP/IP." },
{ "en": "Apa Itu Foundation Fieldbus?", "id": "Standar Fieldbus Digital Semua Untuk Otomasi Proses." },
{ "en": "Apa Itu HART Protocol?", "id": "Protokol Komunikasi Industri Awal." },
{ "en": "Apa Kepanjangan Dari HART?", "id": "Highway Addressable Remote Transducer." },
{ "en": "Apa Itu Saklar Cerdas?", "id": "Saklar Yang Mampu Mengukur Arus Dan Melakukan Diagnostik." },
{ "en": "Apa Itu Motor Starter?", "id": "Perangkat Untuk Menghidupkan Dan Menghentikan Motor Listrik." },
{ "en": "Apa Itu Soft Starter?", "id": "Starter Elektronik Untuk Mengurangi Torsi Awal Motor." },
{ "en": "Apa Itu Kontaktor?", "id": "Jenis Relay Untuk Mengalihkan Arus Listrik Tinggi." },
{ "en": "Apa Itu Overload Relay?", "id": "Melindungi Motor Dari Kondisi Arus Berlebih." },
{ "en": "Apa Itu Pemrosesan Batch?", "id": "Produksi Di Mana Produk Dibuat Dalam Kelompok." },
{ "en": "Apa Itu Produksi Berkelanjutan?", "id": "Proses Produksi Yang Berlangsung Tanpa Henti." },
{ "en": "Apa Itu Produksi Massal?", "id": "Produksi Barang Standar Dalam Jumlah Besar." },
{ "en": "Apa Itu Kustomisasi Massal?", "id": "Menggabungkan Fleksibilitas Kustom Dengan Biaya Rendah." },
{ "en": "Apa Itu Sel Manufaktur?", "id": "Pengelompokan Mesin Dan Proses Untuk Keluarga Produk." },
{ "en": "Apa Itu Group Technology (GT)?", "id": "Filosofi Manufaktur Untuk Mengelompokkan Suku Cadang Serupa." },
{ "en": "Apa Itu Metrologi In-process?", "id": "Pengukuran Yang Dilakukan Selama Proses Manufaktur." },
{ "en": "Apa Itu Digitalisasi?", "id": "Konversi Informasi Menjadi Format Digital." },
{ "en": "Apa Itu Transformasi Digital?", "id": "Integrasi Teknologi Digital Ke Semua Area Bisnis." },
{ "en": "Apa Itu Analisis Data Besar (Big Data)?", "id": "Menganalisis Kumpulan Data Yang Sangat Besar." },
{ "en": "Apa Itu Ilmu Data (Data Science)?", "id": "Bidang Interdisipliner Untuk Mengekstrak Pengetahuan Dari Data." },
{ "en": "Apa Itu Gudang Data (Data Warehouse)?", "id": "Sistem Penyimpanan Pusat Data Dari Berbagai Sumber." },
{ "en": "Apa Itu Penambangan Data (Data Mining)?", "id": "Menemukan Pola Dalam Kumpulan Data Besar." },
{ "en": "Apa Itu Visualisasi Data?", "id": "Representasi Grafis Informasi Dan Data." },
{ "en": "Apa Itu Dasbor (Dashboard)?", "id": "Tampilan Visual Informasi Kunci." },
{ "en": "Apa Itu Key Performance Indicator (KPI)?", "id": "Ukuran Kuantitatif Untuk Mengevaluasi Keberhasilan." },
{ "en": "Apa Itu Overall Equipment Effectiveness (OEE)?", "id": "Metrik Standar Untuk Mengukur Efektivitas Manufaktur." },
{ "en": "Apa Tiga Komponen OEE?", "id": "Ketersediaan, Kinerja, Dan Kualitas." },
{ "en": "Apa Itu Manajemen Rantai Pasokan (SCM)?", "id": "Pengelolaan Aliran Barang Dan Jasa." },
{ "en": "Apa Itu Logistik?", "id": "Manajemen Aliran Barang Antara Titik Asal." },
{ "en": "Apa Itu Inventaris?", "id": "Barang Dan Bahan Yang Dimiliki Perusahaan." },
{ "en": "Apa Itu Sistem Manajemen Gudang (WMS)?", "id": "Perangkat Lunak Untuk Mengelola Operasi Gudang." },
{ "en": "Apa Itu Kendaraan Otonom Ringan (LGV)?", "id": "Kendaraan Seperti AGV Yang Menggunakan Navigasi Laser." },
{ "en": "Apa Itu Sistem Penyimpanan Dan Pengambilan Otomatis (AS/RS)?", "id": "Robot Untuk Mengelola Inventaris Dalam Gudang." },
{ "en": "Apa Itu Sistem Pick-to-Light?", "id": "Mengarahkan Operator Gudang Menggunakan Lampu LED." },
{ "en": "Apa Itu Sistem Voice Picking?", "id": "Mengarahkan Operator Menggunakan Perintah Suara." },
{ "en": "Apa Itu Cross-docking?", "id": "Membongkar Dan Memuat Barang Dengan Sedikit Penyimpanan." },
{ "en": "Apa Itu Freight Forwarder?", "id": "Perusahaan Yang Mengatur Pengiriman Barang." },
{ "en": "Apa Itu Third-Party Logistics (3PL)?", "id": "Mengalihdayakan Proses Logistik Ke Pihak Ketiga." },
{ "en": "Apa Itu Dropshipping?", "id": "Pengecer Tidak Menyimpan Barang Dalam Stok." },
{ "en": "Apa Itu Kode Etik?", "id": "Kumpulan Prinsip Yang Dirancang Untuk Membantu Profesional." },
{ "en": "Apa Itu Whistleblowing?", "id": "Mengekspos Pelanggaran Dalam Sebuah Organisasi." },
{ "en": "Apa Itu Plagiarisme?", "id": "Mengambil Karya Orang Lain Tanpa Memberi Kredit." },
{ "en": "Apa Itu Desain Universal?", "id": "Merancang Produk Agar Dapat Digunakan Semua Orang." },
{ "en": "Apa Itu Aksesibilitas?", "id": "Tingkat Kemudahan Suatu Produk Dapat Digunakan." },
{ "en": "Apa Itu Teknologi Asistif?", "id": "Perangkat Untuk Membantu Orang Dengan Disabilitas." },
{ "en": "Apa Itu Crowdsourcing?", "id": "Memperoleh Pekerjaan Atau Ide Dari Banyak Orang." },
{ "en": "Apa Itu Crowdfunding?", "id": "Mengumpulkan Dana Dari Sejumlah Besar Orang." },
{ "en": "Apa Itu Model Bisnis Freemium?", "id": "Menawarkan Layanan Dasar Gratis Dan Fitur Premium Berbayar." },
{ "en": "Apa Itu Minimum Viable Product (MVP)?", "id": "Versi Produk Paling Sederhana Untuk Mendapatkan Umpan Balik." },
{ "en": "Apa Itu Analisis Pesaing?", "id": "Mengevaluasi Kekuatan Dan Kelemahan Pesaing." },
{ "en": "Apa Itu Proposisi Nilai (Value Proposition)?", "id": "Janji Nilai Yang Akan Diberikan Kepada Pelanggan." },
{ "en": "Apa Itu Analisis Biaya-Manfaat?", "id": "Membandingkan Biaya Dan Manfaat Suatu Proyek." },
{ "en": "Apa Itu Return on Investment (ROI)?", "id": "Ukuran Kinerja Untuk Mengevaluasi Efisiensi Investasi." },
{ "en": "Apa Itu Total Cost of Ownership (TCO)?", "id": "Biaya Pembelian Ditambah Biaya Operasional Seumur Hidup." },
{ "en": "Apa Itu Amortisasi?", "id": "Penyebaran Biaya Aset Selama Masa Manfaatnya." },
{ "en": "Apa Itu Depresiasi?", "id": "Penurunan Nilai Aset Seiring Waktu." },
{ "en": "Apa Itu Modal Ventura (Venture Capital)?", "id": "Pendanaan Yang Disediakan Untuk Perusahaan Rintisan." },
{ "en": "Apa Itu Angel Investor?", "id": "Individu Kaya Yang Memberikan Modal Untuk Rintisan." },
{ "en": "Apa Itu Ekonomi Sirkular?", "id": "Model Ekonomi Yang Bertujuan Menghilangkan Limbah." },
{ "en": "Apa Itu Desain untuk Lingkungan (DfE)?", "id": "Merancang Produk Dengan Mempertimbangkan Dampak Lingkungan." },
{ "en": "Apa Itu Penilaian Siklus Hidup (LCA)?", "id": "Mengevaluasi Dampak Lingkungan Produk Seumur Hidupnya." },
{ "en": "Apa Itu Jejak Karbon?", "id": "Total Emisi Gas Rumah Kaca." },
{ "en": "Apa Itu Efisiensi Energi?", "id": "Menggunakan Lebih Sedikit Energi Untuk Layanan Yang Sama." },
{ "en": "Apa Itu Standar Energy Star?", "id": "Label Untuk Produk Yang Efisien Secara Energi." },
{ "en": "Apa Itu Green Building?", "id": "Struktur Yang Dirancang Ramah Lingkungan." },
{ "en": "Apa Itu Sertifikasi LEED?", "id": "Sistem Peringkat Untuk Desain Bangunan Hijau." },
{ "en": "Apa Kepanjangan Dari LEED?", "id": "Leadership in Energy and Environmental Design." },
{ "en": "Apa Itu E-waste (Limbah Elektronik)?", "id": "Peralatan Elektronik Bekas Yang Dibuang." },
{ "en": "Apa Itu RoHS (Restriction of Hazardous Substances)?", "id": "Arahan Yang Membatasi Penggunaan Bahan Berbahaya." },
{ "en": "Apa Itu WEEE (Waste Electrical and Electronic Equipment)?", "id": "Arahan Pengelolaan Limbah Elektronik Di Eropa." },
{ "en": "Apa Itu Teori Grafik?", "id": "Studi Tentang Grafik Sebagai Representasi Matematis." },
{ "en": "Apa Itu Simpul (Node) Dalam Grafik?", "id": "Titik Atau Verteks Fundamental Dalam Grafik." },
{ "en": "Apa Itu Tepi (Edge) Dalam Grafik?", "id": "Garis Yang Menghubungkan Dua Simpul." },
{ "en": "Apa Itu Algoritma Dijkstra?", "id": "Algoritma Untuk Menemukan Jalur Terpendek." },
{ "en": "Apa Itu Algoritma A* (A-Star)?", "id": "Algoritma Pencarian Jalur Yang Menggunakan Heuristik." },
{ "en": "Apa Itu Pohon Rentang Minimum (Minimum Spanning Tree)?", "id": "Subset Dari Tepi Grafik." },
{ "en": "Apa Itu Masalah Pedagang Keliling (Traveling Salesman Problem)?", "id": "Masalah Optimasi Untuk Menemukan Rute Terpendek." },
{ "en": "Apa Itu Aljabar Boolean?", "id": "Cabang Aljabar Yang Variabelnya Benar Atau Salah." },
{ "en": "Apa Itu Peta Karnaugh?", "id": "Metode Untuk Menyederhanakan Ekspresi Aljabar Boolean." },
{ "en": "Apa Itu Teorema De Morgan?", "id": "Aturan Yang Berkaitan Dengan Operasi Logika." },
{ "en": "Apa Itu Representasi Komplemen Dua?", "id": "Cara Merepresentasikan Bilangan Bulat Bertanda." },
{ "en": "Apa Itu Aritmatika Floating Point?", "id": "Aritmatika Yang Menggunakan Representasi Rumus Angka." },
{ "en": "Apa Itu Standar IEEE 754?", "id": "Standar Teknis Untuk Komputasi Floating Point." },
{ "en": "Apa Itu Error Pembulatan?", "id": "Perbedaan Antara Angka Sebenarnya Dan Representasinya." },
{ "en": "Apa Itu Analisis Numerik?", "id": "Studi Algoritma Menggunakan Aproksimasi Numerik." },
{ "en": "Apa Itu Metode Euler?", "id": "Metode Numerik Untuk Menyelesaikan Persamaan Diferensial." },
{ "en": "Apa Itu Metode Runge-Kutta?", "id": "Keluarga Metode Numerik Untuk Persamaan Diferensial." },
{ "en": "Apa Itu Transformasi Laplace?", "id": "Mengubah Persamaan Diferensial Menjadi Persamaan Aljabar." },
{ "en": "Apa Itu Domain-S?", "id": "Domain Frekuensi Kompleks Dalam Transformasi Laplace." },
{ "en": "Apa Itu Konvolusi Dalam Domain Waktu?", "id": "Perkalian Dalam Domain Frekuensi." },
{ "en": "Apa Itu Teori Informasi?", "id": "Studi Kuantifikasi, Penyimpanan, Dan Komunikasi Informasi." },
{ "en": "Apa Itu Entropi Informasi?", "id": "Ukuran Ketidakpastian Dalam Variabel Acak." },
{ "en": "Apa Itu Pengkodean Sumber (Source Coding)?", "id": "Mengompresi Data Dengan Menghilangkan Redundansi." },
{ "en": "Apa Itu Pengkodean Huffman?", "id": "Algoritma Populer Untuk Kompresi Data Lossless." },
{ "en": "Apa Itu Pengkodean Kanal (Channel Coding)?", "id": "Menambahkan Redundansi Untuk Mendeteksi Dan Memperbaiki Kesalahan." },
{ "en": "Apa Itu Bit Paritas?", "id": "Bit Yang Ditambahkan Untuk Mendeteksi Kesalahan." },
{ "en": "Apa Itu Kode Hamming?", "id": "Jenis Kode Koreksi Kesalahan Linier." },
{ "en": "Apa Itu Cyclic Redundancy Check (CRC)?", "id": "Fungsi Deteksi Kesalahan Yang Umum Digunakan." },
{ "en": "Apa Itu Kapasitas Kanal?", "id": "Tingkat Maksimum Data Dapat Ditransmisikan." },
{ "en": "Apa Itu Rasio Sinyal Terhadap Derau (SNR)?", "id": "Ukuran Yang Membandingkan Level Sinyal." },
{ "en": "Apa Kepanjangan Dari SNR (Signal-to-Noise Ratio)?", "id": "Signal-to-Noise Ratio." },
{ "en": "Apa Itu Komunikasi Optik Ruang Bebas (FSO)?", "id": "Menggunakan Cahaya Yang Merambat Untuk Transmisi Data." },
{ "en": "Apa Itu Li-Fi (Light Fidelity)?", "id": "Teknologi Komunikasi Nirkabel Menggunakan Cahaya." },
{ "en": "Apa Itu Efek Fotolistrik?", "id": "Emisi Elektron Ketika Cahaya Mengenai Material." },
{ "en": "Apa Itu Emisi Termionik?", "id": "Aliran Elektron Dari Permukaan Logam Panas." },
{ "en": "Apa Itu Superkonduktivitas?", "id": "Fenomena Resistansi Listrik Nol Pada Suhu Rendah." },
{ "en": "Apa Itu Efek Meissner?", "id": "Pengusiran Medan Magnet Dari Superkonduktor." },
{ "en": "Apa Itu Semikonduktor?", "id": "Bahan Dengan Konduktivitas Antara Konduktor Dan Isolator." },
{ "en": "Apa Itu Doping?", "id": "Menambahkan Ketidakmurnian Ke Semikonduktor Untuk Mengubah Sifatnya." },
{ "en": "Apa Itu Semikonduktor Tipe-N?", "id": "Didoping Dengan Donor Elektron." },
{ "en": "Apa Itu Semikonduktor Tipe-P?", "id": "Didoping Dengan Akseptor Elektron (Lubang)." },
{ "en": "Apa Itu Sambungan P-N (P-N Junction)?", "id": "Batas Antara Material Tipe-P Dan Tipe-N." },
{ "en": "Apa Itu Daerah Deplesi?", "id": "Daerah Di Sekitar Sambungan P-N." },
{ "en": "Apa Itu Tegangan Maju (Forward Bias)?", "id": "Menerapkan Tegangan Untuk Mengurangi Daerah Deplesi." },
{ "en": "Apa Itu Tegangan Mundur (Reverse Bias)?", "id": "Menerapkan Tegangan Untuk Memperluas Daerah Deplesi." },
{ "en": "Apa Itu Kerusakan Longsor (Avalanche Breakdown)?", "id": "Proses Perkalian Arus Pada Dioda Mundur." },
{ "en": "Apa Itu Kerusakan Zener (Zener Breakdown)?", "id": "Bentuk Lain Dari Kerusakan Listrik." },
{ "en": "Apa Itu Transistor Efek Medan (FET)?", "id": "Transistor Yang Menggunakan Medan Listrik." },
{ "en": "Apa Itu Transistor Sambungan Bipolar (BJT)?", "id": "Jenis Transistor Yang Menggunakan Sambungan P-N." },
{ "en": "Apa Itu Mode Cut-off?", "id": "Keadaan Di Mana Transistor Tidak Menghantar." },
{ "en": "Apa Itu Mode Saturasi?", "id": "Keadaan Di Mana Transistor Menghantar Sepenuhnya." },
{ "en": "Apa Itu Mode Aktif?", "id": "Daerah Operasi Antara Cut-off Dan Saturasi." },
{ "en": "Apa Itu Garis Beban (Load Line)?", "id": "Grafik Untuk Menganalisis Operasi Sirkuit Transistor." },
{ "en": "Apa Itu Titik Q (Quiescent Point)?", "id": "Titik Operasi DC Transistor Tanpa Sinyal Input." },
{ "en": "Apa Itu Rangkaian Biasing?", "id": "Rangkaian Untuk Menetapkan Titik Q Transistor." },
{ "en": "Apa Itu Umpan Balik Negatif Dalam Penguat?", "id": "Mengurangi Penguatan Tetapi Meningkatkan Stabilitas Dan Linearitas." },
{ "en": "Apa Itu Distorsi Harmonik Total (THD)?", "id": "Ukuran Distorsi Harmonik Dalam Sinyal." },
{ "en": "Apa Itu Intermodulasi Distorsi (IMD)?", "id": "Hasil Dari Sinyal Non-linear." },
{ "en": "Apa Itu Angka Derau (Noise Figure)?", "id": "Ukuran Degradasi SNR Oleh Komponen." },
{ "en": "Apa Itu Suhu Derau (Noise Temperature)?", "id": "Cara Mengekspresikan Level Derau." },
{ "en": "Apa Itu Pencocokan Impedansi?", "id": "Membuat Impedansi Input Sama Dengan Impedansi Output." },
{ "en": "Mengapa Pencocokan Impedansi Penting?", "id": "Untuk Transfer Daya Maksimum Dan Mengurangi Pantulan." },
{ "en": "Apa Itu Smith Chart?", "id": "Alat Grafis Untuk Menyelesaikan Masalah Pencocokan Impedansi." },
{ "en": "Apa Itu Parameter-S (Scattering Parameters)?", "id": "Menjelaskan Perilaku Jaringan Listrik Frekuensi Tinggi." },
{ "en": "Apa Itu Jalur Transmisi?", "id": "Struktur Untuk Memandu Gelombang Elektromagnetik." },
{ "en": "Apa Itu Pandu Gelombang (Waveguide)?", "id": "Struktur Logam Berongga Untuk Memandu Gelombang Mikro." },
{ "en": "Apa Itu Antena?", "id": "Transduser Antara Gelombang Terpandu Dan Ruang Bebas." },
{ "en": "Apa Itu Penguatan Antena (Antenna Gain)?", "id": "Ukuran Kemampuan Antena Mengarahkan Daya." },
{ "en": "Apa Itu Pola Radiasi?", "id": "Representasi Grafis Sifat Radiasi Antena." },
{ "en": "Apa Itu Polarisasi Antena?", "id": "Orientasi Medan Listrik Gelombang Radio." },
{ "en": "Apa Itu Antena Dipol?", "id": "Jenis Antena Radio Paling Sederhana." },
{ "en": "Apa Itu Antena Monopol?", "id": "Setengah Dari Antena Dipol." },
{ "en": "Apa Itu Antena Yagi-Uda?", "id": "Antena Directional Dengan Elemen Parasit." },
{ "en": "Apa Itu Antena Parabola?", "id": "Antena Reflektor Untuk Komunikasi Jarak Jauh." },
{ "en": "Apa Itu Antena Tanduk (Horn Antenna)?", "id": "Antena Berbentuk Terompet Untuk Mengarahkan Gelombang Radio." },
{ "en": "Apa Itu Antena Microstrip (Patch Antenna)?", "id": "Antena Datar Yang Dicetak Pada Papan Sirkuit." },
{ "en": "Apa Itu Phased Array Antenna?", "id": "Kumpulan Antena Yang Sinarannya Dikendalikan Secara Elektronik." },
{ "en": "Apa Itu Beamforming?", "id": "Teknik Mengarahkan Sinyal Nirkabel Ke Arah Tertentu." },
{ "en": "Apa Itu Radome?", "id": "Penutup Pelindung Tahan Cuaca Untuk Antena." },
{ "en": "Apa Itu Sistem MIMO Masif?", "id": "Sistem MIMO Dengan Jumlah Antena Sangat Besar." },
{ "en": "Apa Itu Komunikasi Seluler 5G?", "id": "Generasi Kelima Teknologi Jaringan Seluler." },
{ "en": "Apa Keuntungan Utama 5G?", "id": "Kecepatan Sangat Tinggi Dan Latensi Sangat Rendah." },
{ "en": "Apa Itu Internet of Everything (IoE)?", "id": "Perluasan Konsep IoT Mencakup Manusia Dan Proses." },
{ "en": "Apa Itu Web 3.0?", "id": "Visi Internet Generasi Berikutnya Yang Terdesentralisasi." },
{ "en": "Apa Itu Aset Digital?", "id": "Representasi Digital Dari Sesuatu Yang Bernilai." },
{ "en": "Apa Itu Non-Fungible Token (NFT)?", "id": "Aset Digital Unik Yang Tercatat Di Blockchain." },
{ "en": "Apa Itu Metaverse?", "id": "Ruang Virtual Bersama Yang Diciptakan." },
{ "en": "Apa Itu Extended Reality (XR)?", "id": "Istilah Payung Untuk VR, AR, Dan MR." },
{ "en": "Apa Itu Mixed Reality (MR)?", "id": "Menggabungkan Dunia Nyata Dan Virtual." },
{ "en": "Apa Itu Digitalisasi Industri?", "id": "Penerapan Teknologi Digital Dalam Sektor Industri." },
{ "en": "Apa Itu Pabrik Cerdas (Smart Factory)?", "id": "Pabrik Yang Sangat Terdigitalisasi Dan Terhubung." },
{ "en": "Apa Itu Predictive Maintenance?", "id": "Memprediksi Kapan Kegagalan Peralatan Akan Terjadi." },
{ "en": "Apa Itu Prescriptive Maintenance?", "id": "Memberikan Rekomendasi Tindakan Perawatan." },
{ "en": "Apa Itu Kualitas 4.0?", "id": "Evolusi Manajemen Kualitas Di Era Industri 4.0." },
{ "en": "Apa Itu Zero Defect Manufacturing?", "id": "Filosofi Untuk Menghilangkan Cacat Produksi." },
{ "en": "Apa Itu Analisis Video Cerdas?", "id": "Menggunakan AI Untuk Menganalisis Konten Video." },
{ "en": "Apa Itu Pengenalan Wajah?", "id": "Mengidentifikasi Individu Dari Gambar Atau Video." },
{ "en": "Apa Itu Pengenalan Gestur?", "id": "Menginterpretasikan Gerakan Manusia Dengan Perangkat Komputasi." },
{ "en": "Apa Itu Analisis Sentimen?", "id": "Menentukan Nada Emosional Di Balik Serangkaian Teks." },
{ "en": "Apa Itu Chatbot?", "id": "Program Komputer Yang Mensimulasikan Percakapan Manusia." },
{ "en": "Apa Itu Model Bahasa Besar (LLM)?", "id": "Model AI Yang Dilatih Pada Data Teks Besar." },
{ "en": "Apa Itu Rekayasa Prompt (Prompt Engineering)?", "id": "Proses Merancang Input Untuk Model Generatif." },
{ "en": "Apa Itu Generative AI?", "id": "AI Yang Dapat Menciptakan Konten Baru." },
{ "en": "Apa Itu Deepfake?", "id": "Media Sintetis Di Mana Seseorang Digantikan." },
{ "en": "Apa Itu Etika AI?", "id": "Cabang Etika Yang Berfokus Pada Robot." },
{ "en": "Apa Itu AI Yang Dapat Dijelaskan (XAI)?", "id": "AI Yang Keputusannya Dapat Dipahami Manusia." },
{ "en": "Apa Itu Bias Dalam AI?", "id": "Hasil Yang Tidak Adil Atau Merugikan Dari Algoritma." },
{ "en": "Apa Itu Privasi Diferensial?", "id": "Sistem Untuk Berbagi Informasi Secara Publik." },
{ "en": "Apa Itu Federated Learning?", "id": "Melatih Algoritma Di Beberapa Perangkat Terdesentralisasi." },
{ "en": "Apa Itu Homomorphic Encryption?", "id": "Memungkinkan Perhitungan Pada Data Terenkripsi." },
{ "en": "Apa Itu Zero-Knowledge Proof?", "id": "Metode Untuk Membuktikan Pernyataan Tanpa Mengungkapkan Informasi." },
{ "en": "Apa Itu Quantum Cryptography?", "id": "Menggunakan Prinsip Kuantum Untuk Mengamankan Komunikasi." },
{ "en": "Apa Itu Quantum Key Distribution (QKD)?", "id": "Metode Komunikasi Aman Menggunakan Kriptografi Kuantum." },
{ "en": "Apa Itu Perhitungan Kinerja Tinggi (HPC)?", "id": "Menggunakan Superkomputer Untuk Memecahkan Masalah Kompleks." },
{ "en": "Apa Itu Superkomputer?", "id": "Komputer Dengan Tingkat Kinerja Sangat Tinggi." },
{ "en": "Apa Itu FLOPS (Floating-point Operations Per Second)?", "id": "Ukuran Kinerja Komputer." },
{ "en": "Apa Itu Grid Computing?", "id": "Menghubungkan Komputer Dari Berbagai Lokasi." },
{ "en": "Apa Itu Volunteer Computing?", "id": "Menggunakan Waktu Proses Komputer Sukarelawan." },
{ "en": "Apa Itu Ekosistem Digital?", "id": "Jaringan Saling Terhubung Antara Perusahaan." },
{ "en": "Apa Itu Platform Digital?", "id": "Model Bisnis Yang Memfasilitasi Pertukaran." },
{ "en": "Apa Itu Efek Jaringan?", "id": "Fenomena Di Mana Nilai Produk Meningkat." },
{ "en": "Apa Itu Disintermediasi?", "id": "Menghilangkan Perantara Dalam Rantai Pasokan." },
{ "en": "Apa Itu Co-creation?", "id": "Menciptakan Nilai Bersama Antara Perusahaan Dan Pelanggan." },
{ "en": "Apa Itu Inovasi Terbuka?", "id": "Menggunakan Ide Eksternal Untuk Memajukan Teknologi." },
{ "en": "Apa Itu Inovasi Disruptif?", "id": "Inovasi Yang Menciptakan Pasar Baru." },
{ "en": "Apa Itu Kurva Adopsi Teknologi?", "id": "Model Yang Mengklasifikasikan Adopter Inovasi." },
{ "en": "Siapa Saja Kategori Dalam Kurva Adopsi?", "id": "Inovator, Adopter Awal, Mayoritas Awal, Akhir, Laggard." },
{ "en": "Apa Itu Chasm Dalam Adopsi Teknologi?", "id": "Kesenjangan Antara Adopter Awal Dan Mayoritas Awal." },
{ "en": "Apa Itu Paradigma?", "id": "Satu Set Konsep Atau Pola Pikir." },
{ "en": "Apa Itu Pergeseran Paradigma?", "id": "Perubahan Fundamental Dalam Konsep Dasar." },
{ "en": "Apa Itu Epistemologi?", "id": "Cabang Filsafat Yang Berkaitan Dengan Pengetahuan." },
{ "en": "Apa Itu Ontologi?", "id": "Cabang Filsafat Yang Mempelajari Konsep." },
{ "en": "Apa Itu Metode Ilmiah?", "id": "Prosedur Empiris Untuk Memperoleh Pengetahuan." },
{ "en": "Apa Itu Hipotesis?", "id": "Penjelasan Sementara Untuk Sebuah Fenomena." },
{ "en": "Apa Itu Teori?", "id": "Penjelasan Yang Teruji Baik Untuk Aspek Dunia." },
{ "en": "Apa Itu Hukum Ilmiah?", "id": "Pernyataan Berdasarkan Pengamatan Berulang." },
{ "en": "Apa Itu Empirisme?", "id": "Teori Bahwa Pengetahuan Berasal Dari Pengalaman." },
{ "en": "Apa Itu Rasionalisme?", "id": "Teori Bahwa Akal Adalah Sumber Pengetahuan." },
{ "en": "Apa Itu Skeptisisme?", "id": "Sikap Keraguan Terhadap Klaim Pengetahuan." },
{ "en": "Apa Itu Kesalahan Tipe I?", "id": "Menolak Hipotesis Nol Yang Sebenarnya Benar." },
{ "en": "Apa Itu Kesalahan Tipe II?", "id": "Gagal Menolak Hipotesis Nol Yang Sebenarnya Salah." },
{ "en": "Apa Itu P-value?", "id": "Probabilitas Mendapatkan Hasil Yang Sama Ekstremnya." },
{ "en": "Apa Itu Interval Kepercayaan?", "id": "Rentang Estimasi Untuk Parameter Yang Tidak Diketahui." },
{ "en": "Apa Itu Regresi Linier?", "id": "Pendekatan Pemodelan Hubungan Antara Variabel." },
{ "en": "Apa Itu Regresi Logistik?", "id": "Model Statistik Untuk Variabel Hasil Biner." },
{ "en": "Apa Itu Analisis Varians (ANOVA)?", "id": "Kumpulan Model Statistik Untuk Menganalisis Perbedaan." },
{ "en": "Apa Itu Uji-T?", "id": "Uji Statistik Untuk Membandingkan Rata-rata Dua Kelompok." },
{ "en": "Apa Itu Uji Chi-Kuadrat?", "id": "Uji Statistik Untuk Data Kategorikal." },
{ "en": "Apa Itu Distribusi Normal?", "id": "Distribusi Probabilitas Berbentuk Lonceng." },
{ "en": "Apa Itu Teorema Limit Pusat?", "id": "Menyatakan Distribusi Sampel Akan Mendekati Normal." },
{ "en": "Apa Itu Hukum Bilangan Besar?", "id": "Menyatakan Hasil Akan Mendekati Nilai Harapan." },
{ "en": "Apa Itu Proses Manufaktur Hibrida?", "id": "Menggabungkan Teknik Aditif Dan Subtraktif." },
{ "en": "Apa Itu Pengelasan Gesek Aduk (Friction Stir Welding)?", "id": "Proses Pengelasan Keadaan Padat." },
{ "en": "Apa Itu Pembentukan Logam Superplastis?", "id": "Membentuk Logam Pada Suhu Tinggi." },
{ "en": "Apa Itu Metalurgi Serbuk?", "id": "Proses Membuat Benda Dari Serbuk Logam." },
{ "en": "Apa Itu Injection Molding Logam (MIM)?", "id": "Proses Metalurgi Serbuk Untuk Bentuk Kompleks." },
{ "en": "Apa Itu Hot Isostatic Pressing (HIP)?", "id": "Mengurangi Porositas Logam Dan Meningkatkan Kepadatan." },
{ "en": "Apa Itu Cold Spray?", "id": "Proses Pelapisan Dengan Partikel Berkecepatan Tinggi." },
{ "en": "Apa Itu Shot Peening?", "id": "Proses Kerja Dingin Untuk Meningkatkan Ketahanan Lelah." },
{ "en": "Apa Itu Pengujian Tak Merusak (NDT)?", "id": "Mengevaluasi Sifat Material Tanpa Menyebabkan Kerusakan." },
{ "en": "Sebutkan Contoh Metode NDT?", "id": "Ultrasonik, Radiografi, Partikel Magnetik." },
{ "en": "Apa Itu Pengujian Arus Eddy?", "id": "Metode NDT Elektromagnetik Untuk Mendeteksi Cacat." },
{ "en": "Apa Itu Pengujian Emisi Akustik?", "id": "Mendeteksi Gelombang Elastis Yang Dihasilkan Material." },
{ "en": "Apa Itu Pengujian Penetran Cair?", "id": "Metode NDT Untuk Mendeteksi Retak Permukaan." },
{ "en": "Apa Itu Mekanika Patahan?", "id": "Studi Perambatan Retak Dalam Material." },
{ "en": "Apa Itu Ketangguhan Patahan?", "id": "Ketahanan Material Terhadap Perambatan Retak." },
{ "en": "Apa Itu Konsentrasi Tegangan?", "id": "Lokasi Di Mana Tegangan Jauh Lebih Besar." },
{ "en": "Apa Itu Creep?", "id": "Deformasi Material Perlahan Akibat Tegangan Konstan." },
{ "en": "Apa Itu Relaksasi Tegangan?", "id": "Penurunan Tegangan Seiring Waktu." },
{ "en": "Apa Itu Manajemen Rantai Nilai?", "id": "Kumpulan Aktivitas Untuk Menciptakan Nilai." },
{ "en": "Apa Itu Vertikal Integrasi?", "id": "Perusahaan Mengontrol Beberapa Tahap Rantai Pasokan." },
{ "en": "Apa Itu Outsourcing?", "id": "Mengontrak Pihak Ketiga Untuk Melakukan Operasi." },
{ "en": "Apa Itu Offshoring?", "id": "Memindahkan Proses Bisnis Ke Negara Lain." },
{ "en": "Apa Itu Reshoring?", "id": "Mengembalikan Produksi Domestik Dari Luar Negeri." },
{ "en": "Apa Itu Kewirausahaan Teknologi (Technopreneurship)?", "id": "Menggabungkan Keterampilan Teknologi Dengan Semangat Kewirausahaan." },
{ "en": "Apa Itu Rencana Bisnis?", "id": "Dokumen Tertulis Yang Menjelaskan Tujuan Bisnis." },
{ "en": "Apa Itu Analisis Pasar?", "id": "Proses Mengumpulkan Informasi Tentang Pasar Target." },
{ "en": "Apa Itu Segmentasi Pasar?", "id": "Membagi Pasar Luas Menjadi Kelompok-kelompok Kecil." },
{ "en": "Apa Itu Branding?", "id": "Proses Menciptakan Persepsi Publik Yang Kuat." },
{ "en": "Apa Itu Pemasaran Digital?", "id": "Pemasaran Produk Menggunakan Saluran Digital." },
{ "en": "Apa Itu Search Engine Optimization (SEO)?", "id": "Meningkatkan Visibilitas Situs Web Di Mesin Pencari." },
{ "en": "Apa Itu Customer Relationship Management (CRM)?", "id": "Mengelola Interaksi Perusahaan Dengan Pelanggan." },
{ "en": "Apa Itu Tingkat Konversi?", "id": "Persentase Pengguna Yang Mengambil Tindakan Yang Diinginkan." },
{ "en": "Apa Itu A/B Testing?", "id": "Membandingkan Dua Versi Halaman Web." },
{ "en": "Apa Itu Transfer Belajar (Transfer Learning)?", "id": "Menerapkan Pengetahuan Dari Satu Tugas Ke Tugas Lain." },
{ "en": "Apa Itu One-Shot Learning?", "id": "Belajar Dari Satu Atau Beberapa Contoh Saja." },
{ "en": "Apa Itu Zero-Shot Learning?", "id": "Mengenali Objek Yang Belum Pernah Dilihat." },
{ "en": "Apa Itu Jaringan Permusuhan Generatif (GAN)?", "id": "Arsitektur AI Dengan Dua Jaringan Saraf Bersaing." },
{ "en": "Apa Kepanjangan Dari GAN (Generative Adversarial Network)?", "id": "Generative Adversarial Network." },
{ "en": "Apa Itu Autoencoder?", "id": "Jaringan Saraf Untuk Pembelajaran Fitur Tanpa Pengawasan." },
{ "en": "Apa Itu Peta Self-Organizing (SOM)?", "id": "Jenis Jaringan Saraf Untuk Visualisasi Data." },
{ "en": "Apa Itu Support Vector Machine (SVM)?", "id": "Model Pembelajaran Terawasi Untuk Klasifikasi." },
{ "en": "Apa Itu Pohon Keputusan (Decision Tree)?", "id": "Alat Pendukung Keputusan Berbentuk Seperti Pohon." },
{ "en": "Apa Itu Random Forest?", "id": "Algoritma Pembelajaran Ensemble Yang Menggunakan Banyak Pohon." },
{ "en": "Apa Itu Gradient Boosting?", "id": "Teknik Pembelajaran Mesin Untuk Masalah Regresi." },
{ "en": "Apa Itu K-Means Clustering?", "id": "Metode Pengelompokan Data Tak Terawasi." },
{ "en": "Apa Itu Pengurangan Dimensi?", "id": "Mengurangi Jumlah Variabel Acak Yang Dipertimbangkan." },
{ "en": "Apa Itu Principal Component Analysis (PCA)?", "id": "Teknik Statistik Untuk Pengurangan Dimensi." },
{ "en": "Apa Itu Latent Dirichlet Allocation (LDA)?", "id": "Model Generatif Untuk Pemodelan Topik." },
{ "en": "Apa Itu Rekayasa Fitur (Feature Engineering)?", "id": "Proses Menggunakan Pengetahuan Domain Untuk Membuat Fitur." },
{ "en": "Apa Itu Seleksi Fitur?", "id": "Memilih Subset Fitur Yang Paling Relevan." },
{ "en": "Apa Itu Matriks Kebingungan (Confusion Matrix)?", "id": "Tabel Untuk Mengevaluasi Kinerja Algoritma Klasifikasi." },
{ "en": "Apa Itu Kurva ROC (Receiver Operating Characteristic)?", "id": "Plot Grafis Kinerja Sistem Klasifikasi Biner." },
{ "en": "Apa Itu Area Di Bawah Kurva (AUC)?", "id": "Ukuran Kinerja Untuk Masalah Klasifikasi." },
{ "en": "Apa Itu Presisi Dan Recall?", "id": "Metrik Untuk Mengevaluasi Kinerja Klasifikasi." },
{ "en": "Apa Itu Skor F1?", "id": "Ukuran Akurasi Tes (Rata-rata Presisi Dan Recall)." },
{ "en": "Apa Itu Eksplorasi Data?", "id": "Langkah Awal Dalam Analisis Data." },
{ "en": "Apa Itu Pembersihan Data?", "id": "Memperbaiki Atau Menghapus Data Yang Tidak Akurat." },
{ "en": "Apa Itu Normalisasi Data?", "id": "Menskala Ulang Data Ke Rentang Tertentu." },
{ "en": "Apa Itu Standardisasi Data?", "id": "Menskala Ulang Data Agar Memiliki Rata-rata Nol." },
{ "en": "Apa Itu One-Hot Encoding?", "id": "Representasi Variabel Kategori Sebagai Vektor Biner." },
{ "en": "Apa Itu Kecerdasan Bisnis (BI)?", "id": "Teknologi Untuk Menganalisis Data Untuk Informasi Bisnis." },
{ "en": "Apa Itu Analitik Deskriptif?", "id": "Meringkas Data Historis Untuk Memahami Apa Yang Terjadi." },
{ "en": "Apa Itu Analitik Diagnostik?", "id": "Fokus Pada Mengapa Sesuatu Terjadi." },
{ "en": "Apa Itu Analitik Prediktif?", "id": "Membuat Prediksi Tentang Peristiwa Masa Depan." },
{ "en": "Apa Itu Analitik Preskriptif?", "id": "Menyarankan Tindakan Untuk Mempengaruhi Hasil." },
{ "en": "Apa Itu Sistem Rekomendasi?", "id": "Sistem Yang Memprediksi Preferensi Pengguna." },
{ "en": "Apa Itu Pemfilteran Kolaboratif?", "id": "Teknik Yang Digunakan Oleh Sistem Rekomendasi." },
{ "en": "Apa Itu Pemfilteran Berbasis Konten?", "id": "Menggunakan Karakteristik Item Untuk Merekomendasikan Item Serupa." },
{ "en": "Apa Itu Masalah Cold Start?", "id": "Sistem Tidak Dapat Memberi Rekomendasi Karena Data Kurang." },
{ "en": "Apa Itu Data Terstruktur?", "id": "Data Yang Sangat Terorganisir Dalam Format Tertentu." },
{ "en": "Apa Itu Data Tidak Terstruktur?", "id": "Informasi Yang Tidak Memiliki Model Data." },
{ "en": "Apa Itu Internet of Medical Things (IoMT)?", "id": "Perangkat Medis Dan Aplikasi Yang Terhubung." },
{ "en": "Apa Itu Telemedicine?", "id": "Penyediaan Layanan Kesehatan Jarak Jauh." },
{ "en": "Apa Itu Pemantauan Pasien Jarak Jauh?", "id": "Memantau Pasien Di Luar Pengaturan Klinis." },
{ "en": "Apa Itu HIPAA (Health Insurance Portability and Accountability Act)?", "id": "Hukum AS Untuk Melindungi Data Medis." },
{ "en": "Apa Itu Otomasi Proses Robotik (RPA)?", "id": "Teknologi Untuk Mengotomatiskan Tugas Digital." },
{ "en": "Apa Perbedaan RPA Dan AI?", "id": "RPA Mengikuti Aturan, AI Dapat Belajar." },
{ "en": "Apa Itu Penambangan Proses (Process Mining)?", "id": "Menganalisis Log Peristiwa Untuk Memahami Proses Bisnis." },
{ "en": "Apa Itu Model As-Is?", "id": "Representasi Proses Bisnis Dalam Keadaan Saat Ini." },
{ "en": "Apa Itu Model To-Be?", "id": "Representasi Proses Bisnis Yang Diinginkan." },
{ "en": "Apa Itu Pemodelan Proses Bisnis (BPM)?", "id": "Aktivitas Merepresentasikan Proses Sebuah Perusahaan." },
{ "en": "Apa Itu Notasi Pemodelan Proses Bisnis (BPMN)?", "id": "Representasi Grafis Standar Untuk Proses Bisnis." },
{ "en": "Apa Itu Kerangka Kerja Zachman?", "id": "Ontologi Arsitektur Perusahaan." },
{ "en": "Apa Itu The Open Group Architecture Framework (TOGAF)?", "id": "Metodologi Arsitektur Perusahaan." },
{ "en": "Apa Itu Arsitektur Berorientasi Layanan (SOA)?", "id": "Gaya Desain Perangkat Lunak Berbasis Layanan." },
{ "en": "Apa Itu Layanan Mikro (Microservices)?", "id": "Gaya Arsitektur Yang Menyusun Aplikasi." },
{ "en": "Apa Itu Containerization?", "id": "Virtualisasi Tingkat Sistem Operasi." },
{ "en": "Apa Itu Docker?", "id": "Platform Populer Untuk Containerization." },
{ "en": "Apa Itu Kubernetes?", "id": "Sistem Orkestrasi Kontainer Open-Source." },
{ "en": "Apa Itu DevOps?", "id": "Kombinasi Praktik Yang Menggabungkan Pengembangan." },
{ "en": "Apa Itu Continuous Integration (CI)?", "id": "Mempraktikkan Penggabungan Perubahan Kode Secara Teratur." },
{ "en": "Apa Itu Continuous Delivery/Deployment (CD)?", "id": "Secara Otomatis Merilis Perubahan Kode." },
{ "en": "Apa Itu Infrastructure as Code (IaC)?", "id": "Mengelola Infrastruktur Melalui Kode." },
{ "en": "Apa Itu Komputasi Tanpa Server (Serverless)?", "id": "Model Eksekusi Cloud-Native." },
{ "en": "Apa Itu Function as a Service (FaaS)?", "id": "Kategori Layanan Komputasi Tanpa Server." },
{ "en": "Apa Itu Skalabilitas Vertikal?", "id": "Menambahkan Lebih Banyak Daya Ke Mesin Yang Ada." },
{ "en": "Apa Itu Skalabilitas Horizontal?", "id": "Menambahkan Lebih Banyak Mesin Ke Kumpulan Sumber Daya." },
{ "en": "Apa Itu Penyeimbangan Beban (Load Balancing)?", "id": "Mendistribusikan Lalu Lintas Jaringan Ke Beberapa Server." },
{ "en": "Apa Itu Jaringan Pengiriman Konten (CDN)?", "id": "Jaringan Server Terdistribusi Secara Geografis." },
{ "en": "Apa Itu Caching?", "id": "Menyimpan Salinan Data Di Lokasi Sementara." },
{ "en": "Apa Itu Latensi?", "id": "Penundaan Antara Aksi Pengguna Dan Respon." },
{ "en": "Apa Itu Throughput?", "id": "Tingkat Di Mana Sesuatu Diproses." },
{ "en": "Apa Itu Ketersediaan Tinggi (High Availability)?", "id": "Sistem Yang Beroperasi Terus Menerus." },
{ "en": "Apa Itu Pemulihan Bencana (Disaster Recovery)?", "id": "Rencana Untuk Memulihkan Akses Setelah Bencana." },
{ "en": "Apa Itu Titik Pemulihan Objektif (RPO)?", "id": "Kehilangan Data Maksimum Yang Dapat Ditoleransi." },
{ "en": "Apa Itu Waktu Pemulihan Objektif (RTO)?", "id": "Durasi Waktu Maksimum Yang Dapat Ditoleransi." },
{ "en": "Apa Itu Redundansi Geografis?", "id": "Duplikasi Infrastruktur Di Lokasi Terpisah." },
{ "en": "Apa Itu Failover?", "id": "Beralih Ke Sistem Siaga Secara Otomatis." },
{ "en": "Apa Itu Teorema CAP?", "id": "Menyatakan Sistem Terdistribusi Hanya Dapat Memberikan Dua." },
{ "en": "Apa Tiga Jaminan Teorema CAP?", "id": "Konsistensi, Ketersediaan, Dan Toleransi Partisi." },
{ "en": "Apa Itu Konsistensi Akhirnya (Eventual Consistency)?", "id": "Jaminan Bahwa Data Akan Menjadi Konsisten." },
{ "en": "Apa Itu Transaksi ACID?", "id": "Kumpulan Sifat Yang Menjamin Transaksi Basis Data." },
{ "en": "Apa Saja Sifat-sifat ACID?", "id": "Atomicity, Consistency, Isolation, Durability." },
{ "en": "Apa Itu Basis Data Relasional?", "id": "Basis Data Berdasarkan Model Relasional." },
{ "en": "Apa Itu Kunci Primer (Primary Key)?", "id": "Pengidentifikasi Unik Untuk Setiap Catatan." },
{ "en": "Apa Itu Kunci Asing (Foreign Key)?", "id": "Kunci Yang Menghubungkan Data Di Dua Tabel." },
{ "en": "Apa Itu Indeks Basis Data?", "id": "Struktur Data Untuk Mempercepat Operasi Pengambilan." },
{ "en": "Apa Itu Normalisasi Basis Data?", "id": "Proses Mengorganisir Kolom Dan Tabel." },
{ "en": "Apa Itu Denormalisasi?", "id": "Menambahkan Data Redundan Untuk Mengoptimalkan Kinerja Baca." }



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
