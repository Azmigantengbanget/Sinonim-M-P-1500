<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Arduino Dasar</title>
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
        <h1>Pengetahuan Arduino Dasar</h1>
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


  { "en": "Metode?", "id": "Cara." },
  { "en": "Metodis?", "id": "Teratur." },
  { "en": "Metodologi?", "id": "Ilmu metode." },
  { "en": "Metonimi?", "id": "Gaya bahasa." },
  { "en": "Metrik?", "id": "Berkaitan ukuran." },
  { "en": "Metris?", "id": "Bersajak." },
  { "en": "Metro?", "id": "Kereta bawah tanah." },
  { "en": "Metrologi?", "id": "Ilmu pengukuran." },
  { "en": "Metronom?", "id": "Pengatur tempo." },
  { "en": "Metropolis?", "id": "Kota raya." },
  { "en": "Metropolitan?", "id": "Berkaitan kota raya." },
  { "en": "Mewah?", "id": "Megah." },
  { "en": "Mewek?", "id": "Menangis." },
  { "en": "Mezanin?", "id": "Lantai tengah." },
  { "en": "Mezero?", "id": "Tanaman." },
  { "en": "Mi?", "id": "Makanan." },
  { "en": "Miana?", "id": "Tanaman." },
  { "en": "Miang?", "id": "Bulu gatal." },
  { "en": "Miasma?", "id": "Uap busuk." },
  { "en": "Midar?", "id": "Keliling." },
  { "en": "Midi?", "id": "Ukuran sedang." },
  { "en": "Midik?", "id": "Teliti." },
  { "en": "Midodareni?", "id": "Malam sebelum nikah (Jawa)." },
  { "en": "Migrain?", "id": "Sakit kepala sebelah." },
  { "en": "Migran?", "id": "Orang berpindah." },
  { "en": "Migrasi?", "id": "Perpindahan." },
  { "en": "Mihrab?", "id": "Tempat imam salat." },
  { "en": "Mik?", "id": "Mikrofon." },
  { "en": "Mika?", "id": "Mineral." },
  { "en": "Mikologi?", "id": "Ilmu jamur." },
  { "en": "Mikoprotein?", "id": "Protein jamur." },
  { "en": "Mikosis?", "id": "Penyakit jamur." },
  { "en": "Mikotoksin?", "id": "Racun jamur." },
  { "en": "Mikro?", "id": "Kecil." },
  { "en": "Mikroba?", "id": "Jasad renik." },
  { "en": "Mikrobiologi?", "id": "Ilmu jasad renik." },
  { "en": "Mikrobisida?", "id": "Pembasmi mikroba." },
  { "en": "Mikrobus?", "id": "Bus kecil." },
  { "en": "Mikrodensitometer?", "id": "Pengukur kepadatan." },
  { "en": "Mikroelektronika?", "id": "Elektronika mikro." },
  { "en": "Mikroekonomi?", "id": "Ekonomi skala kecil." },
  { "en": "Mikroelemen?", "id": "Unsur renik." },
  { "en": "Mikrofag?", "id": "Sel pemakan kecil." },
  { "en": "Mikrofibril?", "id": "Serabut halus." },
  { "en": "Mikrofilm?", "id": "Film kecil." },
  { "en": "Mikrofita?", "id": "Tumbuhan renik." },
  { "en": "Mikrofon?", "id": "Pengeras suara." },
  { "en": "Mikrofotografi?", "id": "Fotografi benda kecil." },
  { "en": "Mikrogamet?", "id": "Sel kelamin jantan." },
  { "en": "Mikrogelombang?", "id": "Gelombang pendek." },
  { "en": "Mikrograf?", "id": "Gambar mikro." },
  { "en": "Mikrografi?", "id": "Teknik gambar mikro." },
  { "en": "Mikrogram?", "id": "Satuan berat." },
  { "en": "Mikrohabitat?", "id": "Habitat kecil." },
  { "en": "Mikroklimat?", "id": "Iklim mikro." },
  { "en": "Mikrokomputer?", "id": "Komputer kecil." },
  { "en": "Mikrokosmos?", "id": "Dunia kecil." },
  { "en": "Mikrolet?", "id": "Angkutan kota." },
  { "en": "Mikrolinguistik?", "id": "Linguistik mikro." },
  { "en": "Mikrolit?", "id": "Batuan kecil." },
  { "en": "Mikroliter?", "id": "Satuan volume." },
  { "en": "Mikrom?", "id": "Satuan panjang." },
  { "en": "Mikrometer?", "id": "Alat ukur." },
  { "en": "Mikron?", "id": "Mikrometer." },
  { "en": "Mikronukleus?", "id": "Inti sel kecil." },
  { "en": "Mikroorganisme?", "id": "Jasad renik." },
  { "en": "Mikropil?", "id": "Lubang kecil." },
  { "en": "Mikroprosesor?", "id": "Chip komputer." },
  { "en": "Mikrosefali?", "id": "Kepala kecil." },
  { "en": "Mikrosekon?", "id": "Satuan waktu." },
  { "en": "Mikroskop?", "id": "Alat lihat benda kecil." },
  { "en": "Mikroskopis?", "id": "Sangat kecil." },
  { "en": "Mikrospora?", "id": "Spora kecil." },
  { "en": "Mikrotom?", "id": "Alat iris tipis." },
  { "en": "Mikrovilus?", "id": "Tonjolan sel." },
  { "en": "Mikser?", "id": "Pengaduk." },
  { "en": "Miksoedema?", "id": "Penyakit." },
  { "en": "Mil?", "id": "Satuan jarak." },
  { "en": "Milad?", "id": "Hari lahir." },
  { "en": "Milenium?", "id": "Seribu tahun." },
  { "en": "Miler?", "id": "Pelari mil." },
  { "en": "Mili?", "id": "Seperibu (awalan)." },
  { "en": "Milia?", "id": "Bintik kulit." },
  { "en": "Miliar?", "id": "Seribu juta." },
  { "en": "Miliarder?", "id": "Orang kaya." },
  { "en": "Milibar?", "id": "Satuan tekanan." },
  { "en": "Milieu?", "id": "Lingkungan (Prancis)." },
  { "en": "Miligram?", "id": "Satuan berat." },
  { "en": "Milik?", "id": "Kepunyaan." },
  { "en": "Mililiter?", "id": "Satuan volume." },
  { "en": "Milimeter?", "id": "Satuan panjang." },
  { "en": "Milimikron?", "id": "Satuan panjang." },
  { "en": "Milimol?", "id": "Satuan jumlah zat." },
  { "en": "Miling?", "id": "Gila (Inggris)." },
  { "en": "Milir?", "id": "Hilir." },
  { "en": "Milis?", "id": "Daftar surel." },
  { "en": "Milisi?", "id": "Pasukan rakyat." },
  { "en": "Militan?", "id": "Agresif." },
  { "en": "Militer?", "id": "Tentara." },
  { "en": "Militerisme?", "id": "Paham militer." },
  { "en": "Milivolt?", "id": "Satuan tegangan." },
  { "en": "Milu?", "id": "Jagung (Gorontalo)." },
  { "en": "Mimbar?", "id": "Panggung." },
  { "en": "Mimesis?", "id": "Peniruan." },
  { "en": "Mimetis?", "id": "Meniru." },
  { "en": "Mimi?", "id": "Belangkas." },
  { "en": "Mimik?", "id": "Ekspresi wajah." },
  { "en": "Mimikri?", "id": "Peniruan." },
  { "en": "Mimis?", "id": "Peluru senapan angin." },
  { "en": "Mimisan?", "id": "Pendarahan hidung." },
  { "en": "Mimo?", "id": "Drama tanpa kata." },
  { "en": "Mimosa?", "id": "Putri malu." },
  { "en": "Mimpi?", "id": "Bunga tidur." },
  { "en": "Min?", "id": "Kurang (matematika)." },
  { "en": "Mina?", "id": "Ikan (Sanskerta)." },
  { "en": "Minat?", "id": "Keinginan." },
  { "en": "Minaret?", "id": "Menara masjid." },
  { "en": "Mindi?", "id": "Pohon." },
  { "en": "Mineral?", "id": "Bahan tambang." },
  { "en": "Mineralogi?", "id": "Ilmu mineral." },
  { "en": "Minerva?", "id": "Dewi Romawi." },
  { "en": "Minggat?", "id": "Kabur." },
  { "en": "Minggu?", "id": "Hari." },
  { "en": "Mingkin?", "id": "Mungkin (arkais)." },
  { "en": "Mini?", "id": "Kecil." },
  { "en": "Miniatur?", "id": "Tiruan kecil." },
  { "en": "Minibus?", "id": "Bus kecil." },
  { "en": "Minim?", "id": "Sangat sedikit." },
  { "en": "Minimal?", "id": "Paling sedikit." },
  { "en": "Minimum?", "id": "Batas terendah." },
  { "en": "Minor?", "id": "Kecil." },
  { "en": "Minoritas?", "id": "Golongan kecil." },
  { "en": "Minta?", "id": "Mohon." },
  { "en": "Mintak?", "id": "Minyak (arkais)." },
  { "en": "Mintakat?", "id": "Zona." },
  { "en": "Mintakulburuj?", "id": "Zodiak." },
  { "en": "Minum?", "id": "Teguk." },
  { "en": "Minus?", "id": "Kurang." },
  { "en": "Minyak?", "id": "Cairan lemak." },
  { "en": "Mio?", "id": "Otot (awalan)." },
  { "en": "Mioglobin?", "id": "Protein otot." },
  { "en": "Miokard?", "id": "Otot jantung." },
  { "en": "Miokarditis?", "id": "Radang otot jantung." },
  { "en": "Mioma?", "id": "Tumor otot." },
  { "en": "Miosen?", "id": "Kala geologi." },
  { "en": "Miositis?", "id": "Radang otot." },
  { "en": "Mipan?", "id": "Kain." },
  { "en": "Mirah?", "id": "Batu permata." },
  { "en": "Mirakel?", "id": "Keajaiban." },
  { "en": "Mirat?", "id": "Cermin (Arab)." },
  { "en": "Miri?", "id": "Kemiri." },
  { "en": "Mirih?", "id": "Perih (arkais)." },
  { "en": "Miring?", "id": "Condong." },
  { "en": "Mirip?", "id": "Serupa." },
  { "en": "Miris?", "id": "Sedih." },
  { "en": "Mis?", "id": "Roti (Belanda)." },
  { "en": "Misa?", "id": "Ibadah Katolik." },
  { "en": "Misai?", "id": "Kumis." },
  { "en": "Misal?", "id": "Contoh." },
  { "en": "Misantrop?", "id": "Pembenci manusia." },
  { "en": "Misel?", "id": "Partikel koloid." },
  { "en": "Misil?", "id": "Peluru kendali." },
  { "en": "Misi?", "id": "Tugas." },
  { "en": "Misionaris?", "id": "Penyebar agama." },
  { "en": "Misioner?", "id": "Bersifat misi." },
  { "en": "Miskin?", "id": "Fakir." },
  { "en": "Misoa?", "id": "Mi." },
  { "en": "Misofobia?", "id": "Takut kotor." },
  { "en": "Misogami?", "id": "Benci perkawinan." },
  { "en": "Misogini?", "id": "Benci wanita." },
  { "en": "Misologi?", "id": "Benci pengetahuan." },
  { "en": "Misoneisme?", "id": "Benci hal baru." },
  { "en": "Mister?", "id": "Tuan (Inggris)." },
  { "en": "Misteri?", "id": "Rahasia." },
  { "en": "Misterius?", "id": "Penuh rahasia." },
  { "en": "Mistik?", "id": "Gaib." },
  { "en": "Mistis?", "id": "Mistik." },
  { "en": "Mistisisme?", "id": "Paham mistik." },
  { "en": "Mistri?", "id": "Tukang kayu (India)." },
  { "en": "Mistura?", "id": "Campuran obat." },
  { "en": "Mitasi?", "id": "Tiruan (arkais)." },
  { "en": "Mite?", "id": "Cerita dewa." },
  { "en": "Mitiga?", "id": "Senggama (arkais)." },
  { "en": "Mitigasi?", "id": "Pengurangan risiko." },
  { "en": "Mitisida?", "id": "Pembasmi tungau." },
  { "en": "Mitokondria?", "id": "Bagian sel." },
  { "en": "Mitologi?", "id": "Ilmu mite." },
  { "en": "Mitologis?", "id": "Bersifat mitos." },
  { "en": "Mitoma?", "id": "Jaringan." },
  { "en": "Mitosis?", "id": "Pembelahan sel." },
  { "en": "Mitra?", "id": "Rekan." },
  { "en": "Mitraliur?", "id": "Senapan mesin." },
  { "en": "Mizab?", "id": "Pancuran (Arab)." },
  { "en": "Mizan?", "id": "Timbangan (Arab)." },
  { "en": "Moa?", "id": "Burung purba." },
  { "en": "Mobil?", "id": "Kendaraan roda empat." },
  { "en": "Mobile?", "id": "Bergerak." },
  { "en": "Mobilisasi?", "id": "Pengerahan." },
  { "en": "Mobilitas?", "id": "Pergerakan." },
  { "en": "Moblong?", "id": "Berlubang besar." },
  { "en": "Mobrok?", "id": "Rusak." },
  { "en": "Mocok?", "id": "Kerja serabutan (Jawa)." },
  { "en": "Modal?", "id": "Kapital." },
  { "en": "Modalitas?", "id": "Cara." },
  { "en": "Mode?", "id": "Gaya." },
  { "en": "Model?", "id": "Contoh." },
  { "en": "Modeling?", "id": "Peragaan busana." },
  { "en": "Modem?", "id": "Alat koneksi internet." },
  { "en": "Moderamen?", "id": "Badan pimpinan gereja." },
  { "en": "Moderat?", "id": "Sedang." },
  { "en": "Moderato?", "id": "Tempo sedang (musik)." },
  { "en": "Moderator?", "id": "Pemimpin diskusi." },
  { "en": "Modern?", "id": "Mutakhir." },
  { "en": "Modernisasi?", "id": "Pembaruan." },
  { "en": "Modernisme?", "id": "Paham modern." },
  { "en": "Modifikasi?", "id": "Perubahan." },
  { "en": "Modin?", "id": "Pegawai masjid." },
  { "en": "Modis?", "id": "Bergaya." },
  { "en": "Modiste?", "id": "Perancang busana wanita." },
  { "en": "Modol?", "id": "Berak (kasar)." },
  { "en": "Modul?", "id": "Satuan pelajaran." },
  { "en": "Modular?", "id": "Terdiri dari modul." },
  { "en": "Modulasi?", "id": "Perubahan nada." },
  { "en": "Modulator?", "id": "Alat modulasi." },
  { "en": "Modus?", "id": "Cara." },
  { "en": "Moga?", "id": "Semoga." },
  { "en": "Mogok?", "id": "Berhenti bekerja." },
  { "en": "Mohair?", "id": "Wol kambing." },
  { "en": "Mohon?", "id": "Minta." },
  { "en": "Moka?", "id": "Kopi." },
  { "en": "Moke?", "id": "Minuman keras." },
  { "en": "Moko?", "id": "Gendang perunggu." },
  { "en": "Moksa?", "id": "Kebebasan jiwa (Hindu)." },
  { "en": "Mol?", "id": "Satuan jumlah zat." },
  { "en": "Mola?", "id": "Ikan." },
  { "en": "Molar?", "id": "Gigi geraham." },
  { "en": "Mole?", "id": "Saus Meksiko." },
  { "en": "Molek?", "id": "Cantik." },
  { "en": "Molekuler?", "id": "Berkaitan molekul." },
  { "en": "Molekul?", "id": "Partikel." },
  { "en": "Moler?", "id": "Tanah." },
  { "en": "Moles?", "id": "Oles." },
  { "en": "Molibden?", "id": "Unsur kimia." },
  { "en": "Molibdenum?", "id": "Molibden." },
  { "en": "Molor?", "id": "Tidur (Jawa)." },
  { "en": "Molos?", "id": "Lolos." },
  { "en": "Moluska?", "id": "Hewan lunak." },
  { "en": "Momen?", "id": "Saat." },
  { "en": "Momentum?", "id": "Daya gerak." },
  { "en": "Momok?", "id": "Hantu." },
  { "en": "Momong?", "id": "Asuh." },
  { "en": "Monarki?", "id": "Kerajaan." },
  { "en": "Moncong?", "id": "Mulut hewan." },
  { "en": "Mondar-mandir?", "id": "Bolak-balik." },
  { "en": "Mondok?", "id": "Menginap." },
  { "en": "Monel?", "id": "Aloi nikel." },
  { "en": "Moneter?", "id": "Keuangan." },
  { "en": "Monggo?", "id": "Silakan (Jawa)." },
  { "en": "Mongol?", "id": "Bangsa." },
  { "en": "Mongolia?", "id": "Negara." },
  { "en": "Mongoloid?", "id": "Ras." },
  { "en": "Monisme?", "id": "Paham kesatuan." },
  { "en": "Monitor?", "id": "Layar." },
  { "en": "Mono?", "id": "Satu (awalan)." },
  { "en": "Monodi?", "id": "Nyanyian tunggal." },
  { "en": "Monodrama?", "id": "Drama satu pemain." },
  { "en": "Monoecus?", "id": "Berumah satu (tumbuhan)." },
  { "en": "Monofag?", "id": "Pemakan satu jenis." },
  { "en": "Monofobia?", "id": "Takut sendiri." },
  { "en": "Monogam?", "id": "Satu pasangan." },
  { "en": "Monogami?", "id": "Sistem satu pasangan." },
  { "en": "Monogenesis?", "id": "Asal tunggal." },
  { "en": "Monografi?", "id": "Tulisan satu subjek." },
  { "en": "Monogram?", "id": "Simbol huruf." },
  { "en": "Monokarpa?", "id": "Berbuah sekali." },
  { "en": "Monokel?", "id": "Kacamata satu lensa." },
  { "en": "Monokini?", "id": "Pakaian renang." },
  { "en": "Monoklin?", "id": "Sistem kristal." },
  { "en": "Monoklinal?", "id": "Lipatan batuan." },
  { "en": "Monokotil?", "id": "Tumbuhan biji tunggal." },
  { "en": "Monokotiledon?", "id": "Monokotil." },
  { "en": "Monoksida?", "id": "Senyawa." },
  { "en": "Monokrom?", "id": "Satu warna." },
  { "en": "Monokromatik?", "id": "Bersifat satu warna." },
  { "en": "Monokultur?", "id": "Satu jenis tanaman." },
  { "en": "Monolingual?", "id": "Satu bahasa." },
  { "en": "Monolit?", "id": "Batu tunggal." },
  { "en": "Monolitik?", "id": "Utuh." },
  { "en": "Monolog?", "id": "Bicara sendiri." },
  { "en": "Monomania?", "id": "Obsesi tunggal." },
  { "en": "Monomer?", "id": "Molekul dasar." },
  { "en": "Monoploid?", "id": "Kromosom tunggal." },
  { "en": "Monopoli?", "id": "Penguasaan tunggal." },
  { "en": "Monopolistik?", "id": "Bersifat monopoli." },
  { "en": "Monorel?", "id": "Kereta rel tunggal." },
  { "en": "Monosakarida?", "id": "Gula sederhana." },
  { "en": "Monosek?", "id": "Satu jenis kelamin." },
  { "en": "Monosemantik?", "id": "Satu makna." },
  { "en": "Monosilabel?", "id": "Satu suku kata." },
  { "en": "Monosilabisme?", "id": "Sifat satu suku kata." },
  { "en": "Monosit?", "id": "Sel darah putih." },
  { "en": "Monosodium glutamat?", "id": "MSG." },
  { "en": "Monosperm?", "id": "Satu sperma." },
  { "en": "Monotipe?", "id": "Mesin cetak." },
  { "en": "Monoteis?", "id": "Penganut satu Tuhan." },
  { "en": "Monoteisme?", "id": "Paham satu Tuhan." },
  { "en": "Monoton?", "id": "Membosankan." },
  { "en": "Monotrem?", "id": "Mamalia bertelur." },
  { "en": "Monsinyur?", "id": "Gelar gereja." },
  { "en": "Monster?", "id": "Makhluk mengerikan." },
  { "en": "Monstera?", "id": "Tanaman." },
  { "en": "Montase?", "id": "Teknik film/seni." },
  { "en": "Montir?", "id": "Mekanik." },
  { "en": "Montok?", "id": "Berisi." },
  { "en": "Monumen?", "id": "Tugu." },
  { "en": "Monumental?", "id": "Megah." },
  { "en": "Monyet?", "id": "Kera." },
  { "en": "Monyong?", "id": "Menjulur (mulut)." },
  { "en": "Monyos?", "id": "Tidak menarik." },
  { "en": "Mop?", "id": "Alat pel." },
  { "en": "Moped?", "id": "Sepeda motor kecil." },
  { "en": "Mor?", "id": "Ibu (Belanda)." },
  { "en": "Mora?", "id": "Penundaan (hukum)." },
  { "en": "Moral?", "id": "Akhlak." },
  { "en": "Moralis?", "id": "Ahli moral." },
  { "en": "Moralitas?", "id": "Kesopanan." },
  { "en": "Morang-maring?", "id": "Cerai-berai." },
  { "en": "Morat-marit?", "id": "Kacau balau." },
  { "en": "Moratorium?", "id": "Penundaan." },
  { "en": "Morbid?", "id": "Sakit." },
  { "en": "Morbiditas?", "id": "Angka kesakitan." },
  { "en": "Morbili?", "id": "Campak." },
  { "en": "Mordan?", "id": "Zat pewarna." },
  { "en": "Moreng?", "id": "Coreng." },
  { "en": "Mores?", "id": "Adat istiadat." },
  { "en": "Morfem?", "id": "Satuan bahasa terkecil." },
  { "en": "Morfemik?", "id": "Berkaitan morfem." },
  { "en": "Morfin?", "id": "Obat bius." },
  { "en": "Morfinis?", "id": "Pecandu morfin." },
  { "en": "Morfofonem?", "id": "Gabungan fonem." },
  { "en": "Morfofonemik?", "id": "Ilmu morfofonem." },
  { "en": "Morfofonemis?", "id": "Morfofonemik." },
  { "en": "Morfogenesis?", "id": "Perkembangan bentuk." },
  { "en": "Morfologi?", "id": "Ilmu bentuk kata." },
  { "en": "Mori?", "id": "Kain putih." },
  { "en": "Moril?", "id": "Moral." },
  { "en": "Morin?", "id": "Zat warna." },
  { "en": "Mormon?", "id": "Aliran agama." },
  { "en": "Moron?", "id": "Dungu." },
  { "en": "Morong?", "id": "Wadah air." },
  { "en": "Morose?", "id": "Murung." },
  { "en": "Morfemis?", "id": "Berkaitan morfem." },
  { "en": "Mortal?", "id": "Fana." },
  { "en": "Mortalitas?", "id": "Angka kematian." },
  { "en": "Mortar?", "id": "Adukan semen." },
  { "en": "Mortir?", "id": "Senjata." },
  { "en": "Mosaik?", "id": "Seni tempel." },
  { "en": "Mosek?", "id": "Gerak (arkais)." },
  { "en": "Mosi?", "id": "Pernyataan tidak percaya." },
  { "en": "Mosok?", "id": "Masak (ungkapan)." },
  { "en": "Moster?", "id": "Contoh (Belanda)." },
  { "en": "Mota?", "id": "Kain." },
  { "en": "Motel?", "id": "Penginapan." },
  { "en": "Motif?", "id": "Corak." },
  { "en": "Motivasi?", "id": "Dorongan." },
  { "en": "Motivator?", "id": "Pemberi dorongan." },
  { "en": "Moto?", "id": "Semboyan." },
  { "en": "Motor?", "id": "Mesin." },
  { "en": "Motorik?", "id": "Berkaitan gerak." },
  { "en": "Motoris?", "id": "Pengendara motor." },
  { "en": "Motu?", "id": "Pulau karang." },
  { "en": "Moyang?", "id": "Nenek moyang." },
  { "en": "Mozaik?", "id": "Mosaik." },
  { "en": "Mu?", "id": "Kamu (singkatan)." },
  { "en": "Muai?", "id": "Mengembang." },
  { "en": "Muak?", "id": "Jijik." },
  { "en": "Mual?", "id": "Ingin muntah." },
  { "en": "Mualaf?", "id": "Orang baru masuk Islam." },
  { "en": "Mualim?", "id": "Guru." },
  { "en": "Muamalah?", "id": "Hubungan antarmanusia (Islam)." },
  { "en": "Muara?", "id": "Ujung sungai." },
  { "en": "Muarikh?", "id": "Ahli sejarah (Arab)." },
  { "en": "Muasasah?", "id": "Lembaga (Arab)." },
  { "en": "Muat?", "id": "Berisi." },
  { "en": "Muatan?", "id": "Isi." },
  { "en": "Muazam?", "id": "Besar (Arab)." },
  { "en": "Muazin?", "id": "Pengumandang azan." },
  { "en": "Mubah?", "id": "Boleh (Islam)." },
  { "en": "Mubalig?", "id": "Pendakwah." },
  { "en": "Mubaligat?", "id": "Pendakwah wanita." },
  { "en": "Mubarat?", "id": "Perceraian (Islam)." },
  { "en": "Mubazir?", "id": "Sia-sia." },
  { "en": "Mubut?", "id": "Rapuh." },
  { "en": "Mudarabah?", "id": "Bagi hasil (Islam)." },
  { "en": "Mudarat?", "id": "Kerugian." },
  { "en": "Mudasir?", "id": "Orang berselimut (Arab)." },
  { "en": "Mudah?", "id": "Gampang." },
  { "en": "Mudik?", "id": "Pulang kampung." },
  { "en": "Mudra?", "id": "Sikap tangan (Hindu/Buddha)." },
  { "en": "Mudun?", "id": "Turun." },
  { "en": "Mufakat?", "id": "Setuju." },
  { "en": "Mufarik?", "id": "Berbeda (Arab)." },
  { "en": "Mufasal?", "id": "Terperinci (Arab)." },
  { "en": "Muflis?", "id": "Bangkrut." },
  { "en": "Mufsid?", "id": "Perusak (Arab)." },
  { "en": "Mufti?", "id": "Pemberi fatwa." },
  { "en": "Mughalutah?", "id": "Kesalahan berpikir." },
  { "en": "Muh?", "id": "Muka (arkais)." },
  { "en": "Muhabah?", "id": "Cinta (Arab)." },
  { "en": "Muhajat?", "id": "Perdebatan (Arab)." },
  { "en": "Muhajir?", "id": "Orang berhijrah." },
  { "en": "Muhajirin?", "id": "Para muhajir." },
  { "en": "Muhal?", "id": "Mustahil." },
  { "en": "Muhalil?", "id": "Penghalal nikah." },
  { "en": "Muharam?", "id": "Bulan Hijriah." },
  { "en": "Muhasabah?", "id": "Introspeksi." },
  { "en": "Muhib?", "id": "Pencinta (Arab)." },
  { "en": "Muhibah?", "id": "Kunjungan persahabatan." },
  { "en": "Muhit?", "id": "Lingkungan (Arab)." },
  { "en": "Muhlik?", "id": "Membinasakan (Arab)." },
  { "en": "Muhsin?", "id": "Orang berbuat baik." },
  { "en": "Muhtasyam?", "id": "Megah (Arab)." },
  { "en": "Muih?", "id": "Sedih (arkais)." },
  { "en": "Muir?", "id": "Kapten (arkais)." },
  { "en": "Mujadid?", "id": "Pembaruan." },
  { "en": "Mujahadah?", "id": "Perjuangan sungguh-sungguh." },
  { "en": "Mujahid?", "id": "Pejuang." },
  { "en": "Mujahidin?", "id": "Para pejuang." },
  { "en": "Mujair?", "id": "Ikan." },
  { "en": "Mujarab?", "id": "Manjur." },
  { "en": "Mujarat?", "id": "Pengalaman (Arab)." },
  { "en": "Mujari?", "id": "Ikut serta (arkais)." },
  { "en": "Mujarad?", "id": "Abstrak (Arab)." },
  { "en": "Mujur?", "id": "Beruntung." },
  { "en": "Muka?", "id": "Wajah." },
  { "en": "Mukadim?", "id": "Pendahulu (Arab)." },
  { "en": "Mukadimah?", "id": "Pendahuluan." },
  { "en": "Mukadis?", "id": "Suci (Arab)." },
  { "en": "Mukah?", "id": "Zina (arkais)." },
  { "en": "Mukalaf?", "id": "Dewasa (Islam)." },
  { "en": "Mukaram?", "id": "Dimuliakan (Arab)." },
  { "en": "Mukena?", "id": "Pakaian salat wanita." },
  { "en": "Mukhabarah?", "id": "Bagi hasil pertanian." },
  { "en": "Mukhlis?", "id": "Ikhlas (Arab)." },
  { "en": "Mukhtasar?", "id": "Ringkasan (Arab)." },
  { "en": "Mukim?", "id": "Penduduk tetap." },
  { "en": "Mukimin?", "id": "Para mukim." },
  { "en": "Mukjizat?", "id": "Keajaiban." },
  { "en": "Mukmin?", "id": "Orang beriman." },
  { "en": "Mukminat?", "id": "Wanita beriman." },
  { "en": "Mukminin?", "id": "Para mukmin." },
  { "en": "Muksa?", "id": "Moksa (arkais)." },
  { "en": "Muktabar?", "id": "Terhormat (Arab)." },
  { "en": "Muktamad?", "id": "Dipercaya (Arab)." },
  { "en": "Muktamar?", "id": "Kongres." },
  { "en": "Muktazilah?", "id": "Aliran teologi Islam." },
  { "en": "Mula?", "id": "Awal." },
  { "en": "Mulai?", "id": "Awal." },
  { "en": "Mulas?", "id": "Sakit perut." },
  { "en": "Mulato?", "id": "Keturunan campuran." },
  { "en": "Mulazamah?", "id": "Ketaatan (Arab)." },
  { "en": "Mulhid?", "id": "Ateis (Arab)." },
  { "en": "Mulia?", "id": "Luhur." },
  { "en": "Mullah?", "id": "Ulama (Persia)." },
  { "en": "Muluk?", "id": "Tinggi." },
  { "en": "Mulur?", "id": "Melar." },
  { "en": "Mulus?", "id": "Halus." },
  { "en": "Mulut?", "id": "Organ bicara." },
  { "en": "Mumayis?", "id": "Dapat membedakan." },
  { "en": "Mumbang?", "id": "Buah kelapa muda." },
  { "en": "Mumbul?", "id": "Naik." },
  { "en": "Mumi?", "id": "Mayat awetan." },
  { "en": "Mumifikasi?", "id": "Pengawetan mumi." },
  { "en": "Mumuk?", "id": "Lapuk." },
  { "en": "Mumut?", "id": "Lumut." },
  { "en": "Munafik?", "id": "Bermuka dua." },
  { "en": "Munajat?", "id": "Doa." },
  { "en": "Munasabah?", "id": "Masuk akal." },
  { "en": "Muncang?", "id": "Kemiri." },
  { "en": "Muncul?", "id": "Timbul." },
  { "en": "Muncikari?", "id": "Germo." },
  { "en": "Muncrat?", "id": "Pancut." },
  { "en": "Mundam?", "id": "Mangkuk." },
  { "en": "Munding?", "id": "Kerbau (Sunda)." },
  { "en": "Mundu?", "id": "Buah." },
  { "en": "Mundur?", "id": "Surut." },
  { "en": "Mung?", "id": "Hanya (arkais)." },
  { "en": "Munggu?", "id": "Bukit kecil." },
  { "en": "Mungil?", "id": "Kecil." },
  { "en": "Mungkar?", "id": "Dilarang (Islam)." },
  { "en": "Mungkir?", "id": "Ingkar." },
  { "en": "Mungkin?", "id": "Boleh jadi." },
  { "en": "Mungkur?", "id": "Hiasan atap." },
  { "en": "Mungmung?", "id": "Gong kecil." },
  { "en": "Mungsret?", "id": "Meleset (Sunda)." },
  { "en": "Mungut?", "id": "Ambil." },
  { "en": "Munib?", "id": "Kembali (Arab)." },
  { "en": "Munisi?", "id": "Amunisi." },
  { "en": "Munjung?", "id": "Penuh meluap." },
  { "en": "Muno?", "id": "Dungu (arkais)." },
  { "en": "Munsyi?", "id": "Guru bahasa." },
  { "en": "Muntaber?", "id": "Muntah berak." },
  { "en": "Muntah?", "id": "Keluar isi perut." },
  { "en": "Muntu?", "id": "Palu." },
  { "en": "Muntul?", "id": "Tumpul." },
  { "en": "Munyuk?", "id": "Monyet (Jawa)." },
  { "en": "Muon?", "id": "Partikel." },
  { "en": "Mupus?", "id": "Punah (Jawa)." },
  { "en": "Mur?", "id": "Baut." },
  { "en": "Mura?", "id": "Batu permata." },
  { "en": "Murad?", "id": "Maksud (Arab)." },
  { "en": "Murah?", "id": "Tidak mahal." },
  { "en": "Murai?", "id": "Burung." },
  { "en": "Murakab?", "id": "Gabungan (Arab)." },
  { "en": "Mural?", "id": "Lukisan dinding." },
  { "en": "Muram?", "id": "Sedih." },
  { "en": "Murang?", "id": "Sombong (arkais)." },
  { "en": "Muras?", "id": "Senapan." },
  { "en": "Murbei?", "id": "Buah." },
  { "en": "Muri?", "id": "Seruling." },
  { "en": "Murid?", "id": "Siswa." },
  { "en": "Muring?", "id": "Marah (Jawa)." },
  { "en": "Muris?", "id": "Rakus." },
  { "en": "Murka?", "id": "Marah besar." },
  { "en": "Murni?", "id": "Asli." },
  { "en": "Mursal?", "id": "Utusan (Arab)." },
  { "en": "Mursyid?", "id": "Guru spiritual." },
  { "en": "Murtad?", "id": "Keluar agama." },
  { "en": "Muruah?", "id": "Harga diri (Arab)." },
  { "en": "Murung?", "id": "Sedih." },
  { "en": "Murup?", "id": "Menyala (Jawa)." },
  { "en": "Musa?", "id": "Nabi." },
  { "en": "Musabab?", "id": "Sebab." },
  { "en": "Musafir?", "id": "Pengembara." },
  { "en": "Musafirin?", "id": "Para musafir." },
  { "en": "Musala?", "id": "Tempat salat." },
  { "en": "Musang?", "id": "Hewan." },
  { "en": "Musara?", "id": "Gaji (arkais)." },
  { "en": "Musibah?", "id": "Bencana." },
  { "en": "Musik?", "id": "Seni suara." },
  { "en": "Musikal?", "id": "Bersifat musik." },
  { "en": "Musikalitas?", "id": "Bakat musik." },
  { "en": "Musikan?", "id": "Pemain musik." },
  { "en": "Musikus?", "id": "Pemain musik." },
  { "en": "Musim?", "id": "Waktu tertentu." },
  { "en": "Musiran?", "id": "Sia-sia (arkais)." },
  { "en": "Musing?", "id": "Pusing." },
  { "en": "Muskil?", "id": "Sulit." },
  { "en": "Muslim?", "id": "Penganut Islam." },
  { "en": "Muslimat?", "id": "Wanita muslim." },
  { "en": "Muslimin?", "id": "Pria muslim." },
  { "en": "Muslin?", "id": "Kain." },
  { "en": "Musnah?", "id": "Hilang." },
  { "en": "Musonik?", "id": "Berkaitan musim." },
  { "en": "Mustahak?", "id": "Penting." },
  { "en": "Mustahik?", "id": "Berhak menerima zakat." },
  { "en": "Mustahil?", "id": "Tidak mungkin." },
  { "en": "Mustajab?", "id": "Manjur." },
  { "en": "Mustaka?", "id": "Puncak." },
  { "en": "Mustakim?", "id": "Lurus (Arab)." },
  { "en": "Mustamik?", "id": "Pendengar (Arab)." },
  { "en": "Mustang?", "id": "Kuda liar." },
  { "en": "Mustar?", "id": "Rempah." },
  { "en": "Mustari?", "id": "Planet Jupiter (Arab)." },
  { "en": "Musuh?", "id": "Lawan." },
  { "en": "Musyaraf?", "id": "Dimuliakan (Arab)." },
  { "en": "Musyarakah?", "id": "Kerja sama (Islam)." },
  { "en": "Musyawarah?", "id": "Rundingan." },
  { "en": "Musykil?", "id": "Muskil." },
  { "en": "Musyrik?", "id": "Menyekutukan Tuhan." },
  { "en": "Mutabar?", "id": "Terhormat." },
  { "en": "Mutagen?", "id": "Penyebab mutasi." },
  { "en": "Mutah?", "id": "Pemberian cerai." },
  { "en": "Mutakadim?", "id": "Terdahulu (Arab)." },
  { "en": "Mutakalim?", "id": "Ahli teologi Islam." },
  { "en": "Mutakhir?", "id": "Terbaru." },
  { "en": "Mutaki?", "id": "Bertakwa (Arab)." },
  { "en": "Mutamad?", "id": "Terpercaya." },
  { "en": "Mutan?", "id": "Hasil mutasi." },
  { "en": "Mutasawif?", "id": "Ahli tasawuf." },
  { "en": "Mutasi?", "id": "Perubahan gen." },
  { "en": "Mutawatir?", "id": "Berturut-turut (hadis)." },
  { "en": "Mute?", "id": "Bisu (Inggris)." },
  { "en": "Mutiara?", "id": "Permata." },
  { "en": "Mutih?", "id": "Puasa putih." },
  { "en": "Mutilasi?", "id": "Pemotongan tubuh." },
  { "en": "Mutisme?", "id": "Kebisuan." },
  { "en": "Mutlak?", "id": "Absolut." },
  { "en": "Mutmainah?", "id": "Tenang (jiwa)." },
  { "en": "Moto?", "id": "Semboyan." },
  { "en": "Mutual?", "id": "Saling." },
  { "en": "Mutualis?", "id": "Penganut mutualisme." },
  { "en": "Mutualisme?", "id": "Simbiosis saling untung." },
  { "en": "Mutu?", "id": "Kualitas." },
  { "en": "Muwajahah?", "id": "Pertemuan (Arab)." },
  { "en": "Muwakil?", "id": "Wakil (Arab)." },
  { "en": "Muzakarah?", "id": "Diskusi (Arab)." },
  { "en": "Muzaki?", "id": "Pembayar zakat." },
  { "en": "Muzamil?", "id": "Orang berselimut (Arab)." },
  { "en": "Muzawir?", "id": "Pemandu ziarah." },
  { "en": "Muzik?", "id": "Musik (ejaan lama)." },
  { "en": "Nabatah?", "id": "Tumbuh-tumbuhan (Arab)." },
  { "en": "Nabati?", "id": "Bersifat tumbuhan." },
  { "en": "Nabi?", "id": "Utusan Tuhan." },
  { "en": "Nabiah?", "id": "Nabi wanita." },
  { "en": "Nabob?", "id": "Orang kaya (India)." },
  { "en": "Nada?", "id": "Tinggi rendah bunyi." },
  { "en": "Nadi?", "id": "Pembuluh darah." },
  { "en": "Nadim?", "id": "Sahabat (Arab)." },
  { "en": "Nadir?", "id": "Titik terendah." },
  { "en": "Naf?", "id": "Pusar (Belanda)." },
  { "en": "Nafa?", "id": "Menolak (Arab)." },
  { "en": "Nafas?", "id": "Napas." },
  { "en": "Nafi?", "id": "Penolakan." },
  { "en": "Nafiri?", "id": "Terompet." },
  { "en": "Nafkah?", "id": "Rezeki." },
  { "en": "Nafsi?", "id": "Diri sendiri (Arab)." },
  { "en": "Nafsu?", "id": "Keinginan." },
  { "en": "Naga?", "id": "Makhluk mitologi." },
  { "en": "Nagara?", "id": "Negara (Sanskerta)." },
  { "en": "Nagari?", "id": "Desa (Minang)." },
  { "en": "Nagasari?", "id": "Kue." },
  { "en": "Nah?", "id": "Seruan." },
  { "en": "Nahak?", "id": "Sial (arkais)." },
  { "en": "Nahas?", "id": "Celaka." },
  { "en": "Nahi?", "id": "Larangan (Arab)." },
  { "en": "Nahkoda?", "id": "Kapten kapal." },
  { "en": "Nahu?", "id": "Tata bahasa Arab." },
  { "en": "Naib?", "id": "Wakil." },
  { "en": "Naik?", "id": "Meningkat." },
  { "en": "Naif?", "id": "Polos." },
  { "en": "Najis?", "id": "Kotor." },
  { "en": "Nak?", "id": "Anak (panggilan)." },
  { "en": "Naka?", "id": "Tidak (arkais)." },
  { "en": "Nakal?", "id": "Jail." },
  { "en": "Nakara?", "id": "Gendang." },
  { "en": "Nakhoda?", "id": "Nahkoda." },
  { "en": "Nal?", "id": "Pajak (arkais)." },
  { "en": "Nala?", "id": "Jantung (Sanskerta)." },
  { "en": "Nalam?", "id": "Paham (arkais)." },
  { "en": "Nalan?", "id": "Terang (arkais)." },
  { "en": "Nalih?", "id": "Ukuran (arkais)." },
  { "en": "Naluri?", "id": "Insting." },
  { "en": "Nama?", "id": "Sebutan." },
  { "en": "Namaskara?", "id": "Sembah (Sanskerta)." },
  { "en": "Namibia?", "id": "Negara." },
  { "en": "Nampan?", "id": "Baki." },
  { "en": "Nampak?", "id": "Tampak." },
  { "en": "Namun?", "id": "Tetapi." },
  { "en": "Nana?", "id": "Nanas (arkais)." },
  { "en": "Nanah?", "id": "Cairan infeksi." },
  { "en": "Nanak?", "id": "Tanah." },
  { "en": "Nanam?", "id": "Tanam (arkais)." },
  { "en": "Nanas?", "id": "Buah." },
  { "en": "Nancap?", "id": "Tancap." },
  { "en": "Nanda?", "id": "Anak." },
  { "en": "Nandi?", "id": "Lembu Siwa." },
  { "en": "Nang?", "id": "Yang (arkais)." },
  { "en": "Nangka?", "id": "Buah." },
  { "en": "Nangkring?", "id": "Bertengger." },
  { "en": "Naniao?", "id": "Burung." },
  { "en": "Nano?", "id": "Sangat kecil (awalan)." },
  { "en": "Nanofarad?", "id": "Satuan kapasitansi." },
  { "en": "Nanogram?", "id": "Satuan berat." },
  { "en": "Nanometer?", "id": "Satuan panjang." },
  { "en": "Nanoplankton?", "id": "Plankton kecil." },
  { "en": "Nanosekon?", "id": "Satuan waktu." },
  { "en": "Nanti?", "id": "Kelak." },
  { "en": "Nap?", "id": "Kain bulu." },
  { "en": "Napalm?", "id": "Bahan bakar bom." },
  { "en": "Napas?", "id": "Udara hidup." },
  { "en": "Napi?", "id": "Narapidana." },
  { "en": "Napkin?", "id": "Serbet." },
  { "en": "Napuh?", "id": "Pelanduk." },
  { "en": "Nara?", "id": "Orang (Sanskerta)." },
  { "en": "Narablog?", "id": "Penulis blog." },
  { "en": "Narapati?", "id": "Raja." },
  { "en": "Narapidana?", "id": "Orang hukuman." },
  { "en": "Narapraja?", "id": "Pegawai negeri." },
  { "en": "Narasi?", "id": "Kisah." },
  { "en": "Naratif?", "id": "Bersifat kisah." },
  { "en": "Narasumber?", "id": "Informan." },
  { "en": "Narator?", "id": "Pencerita." },
  { "en": "Narawastu?", "id": "Tanaman." },
  { "en": "Nareswari?", "id": "Permaisuri." },
  { "en": "Narkah?", "id": "Bau tak sedap." },
  { "en": "Narkis?", "id": "Bunga." },
  { "en": "Narkisisme?", "id": "Cinta diri." },
  { "en": "Narkolepsi?", "id": "Serangan tidur." },
  { "en": "Narkoma?", "id": "Narkotika." },
  { "en": "Narkomani?", "id": "Kecanduan narkotika." },
  { "en": "Narkose?", "id": "Pembiusan." },
  { "en": "Narkosis?", "id": "Keadaan terbius." },
  { "en": "Narkotik?", "id": "Obat bius." },
  { "en": "Narpati?", "id": "Raja (Sanskerta)." },
  { "en": "Narsisis?", "id": "Orang cinta diri." },
  { "en": "Narsisme?", "id": "Narkisisme." },
  { "en": "Nartos?", "id": "Mentimun (arkais)." },
  { "en": "Narwastu?", "id": "Minyak wangi." },
  { "en": "Nas?", "id": "Dalil Al-Quran/Hadis." },
  { "en": "Nasab?", "id": "Keturunan." },
  { "en": "Nasabah?", "id": "Pelanggan bank." },
  { "en": "Nasakh?", "id": "Penghapusan hukum." },
  { "en": "Nasal?", "id": "Bunyi hidung." },
  { "en": "Nasalisasi?", "id": "Proses nasal." },
  { "en": "Nasi?", "id": "Makanan pokok." },
  { "en": "Nasib?", "id": "Takdir." },
  { "en": "Nasihat?", "id": "Saran." },
  { "en": "Nasional?", "id": "Kebangsaan." },
  { "en": "Nasionalis?", "id": "Pencinta bangsa." },
  { "en": "Nasionalisme?", "id": "Paham kebangsaan." },
  { "en": "Nasionalisasi?", "id": "Pengambilalihan negara." },
  { "en": "Naskah?", "id": "Tulisan." },
  { "en": "Nasr?", "id": "Pertolongan (Arab)." },
  { "en": "Nasrani?", "id": "Kristen." },
  { "en": "Nastari?", "id": "Nampan." },
  { "en": "Nastiti?", "id": "Teliti (Jawa)." },
  { "en": "Nasut?", "id": "Kemanusiaan (tasawuf)." },
  { "en": "Natal?", "id": "Hari kelahiran Yesus." },
  { "en": "Natalitas?", "id": "Angka kelahiran." },
  { "en": "Natang?", "id": "Jelas (arkais)." },
  { "en": "Natar?", "id": "Dasar (warna)." },
  { "en": "Nata?", "id": "Raja (Sanskerta)." },
  { "en": "Natator?", "id": "Perenang." },
  { "en": "Natol?", "id": "Buta (arkais)." },
  { "en": "Natrium?", "id": "Unsur kimia." },
  { "en": "Natural?", "id": "Alami." },
  { "en": "Naturalis?", "id": "Penganut naturalisme." },
  { "en": "Naturalisme?", "id": "Aliran seni/filsafat." },
  { "en": "Naturalisasi?", "id": "Pewarganegaraan." },
  { "en": "Natur?", "id": "Alam." },
  { "en": "Naung?", "id": "Lindung." },
  { "en": "Naungan?", "id": "Perlindungan." },
  { "en": "Nauplius?", "id": "Larva udang." },
  { "en": "Nausea?", "id": "Mual." },
  { "en": "Nautik?", "id": "Pelayaran." },
  { "en": "Nautika?", "id": "Ilmu pelayaran." },
  { "en": "Nautilus?", "id": "Moluska." },
  { "en": "Navigasi?", "id": "Ilmu pelayaran." },
  { "en": "Navigator?", "id": "Juru mudi." },
  { "en": "Nawala?", "id": "Surat." },
  { "en": "Nayam?", "id": "Enak (arkais)." },
  { "en": "Nayaka?", "id": "Menteri (Sanskerta)." },
  { "en": "Nayam?", "id": "Nikmat." },
  { "en": "Nayap?", "id": "Mencuri (malam)." },
  { "en": "Nazar?", "id": "Janji." },
  { "en": "Nazi?", "id": "Paham Jerman." },
  { "en": "Naziat?", "id": "Malaikat pencabut nyawa." },
  { "en": "Nazim?", "id": "Pengatur (Arab)." },
  { "en": "Nazir?", "id": "Pengawas wakaf." },
  { "en": "Ndara?", "id": "Tuan (Jawa)." },
  { "en": "Ndari?", "id": "Dari (arkais)." },
  { "en": "Ndoro?", "id": "Tuan." },
  { "en": "Neala?", "id": "Kain." },
  { "en": "Nebis?", "id": "Tipis (arkais)." },
  { "en": "Nebula?", "id": "Kabut antarbintang." },
  { "en": "Nebulisator?", "id": "Alat uap." },
  { "en": "Necis?", "id": "Rapi." },
  { "en": "Nef?", "id": "Bagian tengah gereja." },
  { "en": "Nefas?", "id": "Melahirkan (Arab)." },
  { "en": "Nefelometer?", "id": "Pengukur kekeruhan." },
  { "en": "Nefridium?", "id": "Organ ekskresi." },
  { "en": "Nefrit?", "id": "Batu giok." },
  { "en": "Nefritis?", "id": "Radang ginjal." },
  { "en": "Nefrologi?", "id": "Ilmu ginjal." },
  { "en": "Nefron?", "id": "Unit ginjal." },
  { "en": "Nefrosis?", "id": "Penyakit ginjal." },
  { "en": "Negasi?", "id": "Penyangkalan." },
  { "en": "Negatif?", "id": "Menyangkal." },
  { "en": "Negativisme?", "id": "Sikap negatif." },
  { "en": "Neger?", "id": "Hitam (Belanda)." },
  { "en": "Negeri?", "id": "Negara." },
  { "en": "Negosi?", "id": "Perundingan (arkais)." },
  { "en": "Negosiasi?", "id": "Perundingan." },
  { "en": "Negosiator?", "id": "Perunding." },
  { "en": "Negro?", "id": "Ras kulit hitam." },
  { "en": "Negroid?", "id": "Mirip Negro." },
  { "en": "Negus?", "id": "Gelar raja Etiopia." },
  { "en": "Nek?", "id": "Leher (Belanda)." },
  { "en": "Neka?", "id": "Beraneka." },
  { "en": "Nekad?", "id": "Nekat." },
  { "en": "Nekara?", "id": "Gendang perunggu." },
  { "en": "Nekat?", "id": "Berani." },
  { "en": "Nekrofag?", "id": "Pemakan bangkai." },
  { "en": "Nekrofili?", "id": "Suka mayat." },
  { "en": "Nekrofor?", "id": "Kumbang." },
  { "en": "Nekrolog?", "id": "Berita kematian." },
  { "en": "Nekrologi?", "id": "Catatan kematian." },
  { "en": "Nekromansi?", "id": "Ilmu sihir mayat." },
  { "en": "Nekropolis?", "id": "Kuburan besar." },
  { "en": "Nekropsi?", "id": "Autopsi." },
  { "en": "Nekrosis?", "id": "Kematian jaringan." },
  { "en": "Nektar?", "id": "Cairan manis bunga." },
  { "en": "Nektarin?", "id": "Buah." },
  { "en": "Nelayan?", "id": "Penangkap ikan." },
  { "en": "Neli?", "id": "Pewarna." },
  { "en": "Nemagon?", "id": "Pestisida." },
  { "en": "Nematisida?", "id": "Pembasmi nematoda." },
  { "en": "Nematoda?", "id": "Cacing." },
  { "en": "Nematosista?", "id": "Sel sengat." },
  { "en": "Nenes?", "id": "Nanas." },
  { "en": "Nener?", "id": "Bibit bandeng." },
  { "en": "Neng?", "id": "Panggilan wanita muda." },
  { "en": "Neodimium?", "id": "Unsur kimia." },
  { "en": "Neoik?", "id": "Zaman geologi." },
  { "en": "Neokolonialisme?", "id": "Penjajahan baru." },
  { "en": "Neolit?", "id": "Zaman batu muda." },
  { "en": "Neolitik?", "id": "Bersifat neolit." },
  { "en": "Neolitikum?", "id": "Zaman neolit." },
  { "en": "Neologisme?", "id": "Kata baru." },
  { "en": "Neon?", "id": "Gas." },
  { "en": "Neonatus?", "id": "Bayi baru lahir." },
  { "en": "Neoplasma?", "id": "Tumor." },
  { "en": "Neoplatonisme?", "id": "Filsafat." },
  { "en": "Neoprena?", "id": "Karet sintetis." },
  { "en": "Nepotisme?", "id": "Pilih kasih kerabat." },
  { "en": "Neptunium?", "id": "Unsur kimia." },
  { "en": "Neptunus?", "id": "Planet." },
  { "en": "Neraca?", "id": "Timbangan." },
  { "en": "Nerak?", "id": "Rusak (arkais)." },
  { "en": "Neraka?", "id": "Tempat siksaan." },
  { "en": "Neritik?", "id": "Zona laut." },
  { "en": "Neritopelagik?", "id": "Hewan laut." },
  { "en": "Nerium?", "id": "Tanaman." },
  { "en": "Nervasi?", "id": "Susunan urat daun." },
  { "en": "Nervur?", "id": "Urat daun." },
  { "en": "Nes?", "id": "Pulau kecil (Belanda)." },
  { "en": "Nesan?", "id": "Nisan." },
  { "en": "Nestapa?", "id": "Sedih." },
  { "en": "Net?", "id": "Jaring." },
  { "en": "Neting?", "id": "Pukulan (bulu tangkis)." },
  { "en": "Neto?", "id": "Bersih (berat)." },
  { "en": "Netral?", "id": "Tidak memihak." },
  { "en": "Netralis?", "id": "Orang netral." },
  { "en": "Netralisasi?", "id": "Proses netral." },
  { "en": "Netralisme?", "id": "Paham netral." },
  { "en": "Netralitas?", "id": "Sikap netral." },
  { "en": "Neutrino?", "id": "Partikel." },
  { "en": "Neutron?", "id": "Partikel atom." },
  { "en": "Newton?", "id": "Satuan gaya." },
  { "en": "Ngabei?", "id": "Gelar bangsawan Jawa." },
  { "en": "Ngadat?", "id": "Merajuk." },
  { "en": "Nganga?", "id": "Terbuka (mulut)." },
  { "en": "Ngeri?", "id": "Takut." },
  { "en": "Ngilu?", "id": "Rasa sakit." },
  { "en": "Ngoko?", "id": "Bahasa Jawa kasar." },
  { "en": "Ngos-ngosan?", "id": "Tersengal." },
  { "en": "Ngungun?", "id": "Termenung (Jawa)." },
  { "en": "Ni?", "id": "Ini (arkais)." },
  { "en": "Nia?", "id": "Niat (arkais)." },
  { "en": "Niaga?", "id": "Dagang." },
  { "en": "Niah?", "id": "Niat (arkais)." },
  { "en": "Nian?", "id": "Sangat." },
  { "en": "Nias?", "id": "Suku." },
  { "en": "Niat?", "id": "Maksud." },
  { "en": "Nibung?", "id": "Palma." },
  { "en": "Nicu?", "id": "Unit perawatan bayi." },
  { "en": "Nidasi?", "id": "Pelekatan embrio." },
  { "en": "Nidera?", "id": "Tidur (arkais)." },
  { "en": "Nidium?", "id": "Telur kutu." },
  { "en": "Nifak?", "id": "Kemunafikan (Arab)." },
  { "en": "Nifas?", "id": "Darah setelah melahirkan." },
  { "en": "Nih?", "id": "Ini (partikel)." },
  { "en": "Nihil?", "id": "Kosong." },
  { "en": "Nik?", "id": "Nomor Induk Kependudukan." },
  { "en": "Nikah?", "id": "Kawin." },
  { "en": "Nikel?", "id": "Unsur kimia." },
  { "en": "Nikmat?", "id": "Enak." },
  { "en": "Nikotin?", "id": "Zat tembakau." },
  { "en": "Nila?", "id": "Warna biru." },
  { "en": "Nilai?", "id": "Harga." },
  { "en": "Nilakandi?", "id": "Batu nilam." },
  { "en": "Nilam?", "id": "Tanaman." },
  { "en": "Nilem?", "id": "Ikan." },
  { "en": "Nimbrung?", "id": "Ikut campur." },
  { "en": "Nimfa?", "id": "Serangga muda." },
  { "en": "Nimfomania?", "id": "Nafsu seks wanita berlebih." },
  { "en": "Nina?", "id": "Gadis (Spanyol)." },
  { "en": "Ninabobo?", "id": "Lagu tidur." },
  { "en": "Ning?", "id": "Bening (arkais)." },
  { "en": "Ningrat?", "id": "Bangsawan." },
  { "en": "Nini?", "id": "Nenek." },
  { "en": "Ninik?", "id": "Nini." },
  { "en": "Ninitowok?", "id": "Permainan jelangkung." },
  { "en": "Nio?", "id": "Patung penjaga kuil." },
  { "en": "Nipa?", "id": "Nipah." },
  { "en": "Nipah?", "id": "Palma." },
  { "en": "Nipis?", "id": "Tipis." },
  { "en": "Nir?", "id": "Tanpa." },
  { "en": "Niraksara?", "id": "Tanpa tulisan." },
  { "en": "Nirasmara?", "id": "Tanpa cinta." },
  { "en": "Nirawa?", "id": "Neraka (arkais)." },
  { "en": "Nirbana?", "id": "Nirwana." },
  { "en": "Nirbaya?", "id": "Selamat." },
  { "en": "Nirbhaya?", "id": "Nirbaya." },
  { "en": "Nirdosa?", "id": "Tidak berdosa." },
  { "en": "Nirfaedah?", "id": "Tak berguna." },
  { "en": "Nirgagasan?", "id": "Tanpa ide." },
  { "en": "Nirguna?", "id": "Sia-sia." },
  { "en": "Niriah?", "id": "Tidak tampak." },
  { "en": "Nirjamah?", "id": "Tidak berpenghuni." },
  { "en": "Nirjiwa?", "id": "Mati." },
  { "en": "Nirkabel?", "id": "Tanpa kabel." },
  { "en": "Nirlaba?", "id": "Nonprofit." },
  { "en": "Nirleka?", "id": "Niraksara." },
  { "en": "Nirmala?", "id": "Suci." },
  { "en": "Nirmana?", "id": "Tata rupa." },
  { "en": "Nirselera?", "id": "Tak berselera." },
  { "en": "Nirwana?", "id": "Surga (Buddha)." },
  { "en": "Nirwasita?", "id": "Bijaksana." },
  { "en": "Nisan?", "id": "Tanda kubur." },
  { "en": "Nisab?", "id": "Batas zakat." },
  { "en": "Nisbah?", "id": "Perbandingan." },
  { "en": "Nisbi?", "id": "Relatif." },
  { "en": "Niscaya?", "id": "Pasti." },
  { "en": "Niskala?", "id": "Abstrak." },
  { "en": "Nista?", "id": "Hina." },
  { "en": "Nistagmus?", "id": "Gerak mata tak sadar." },
  { "en": "Nistatin?", "id": "Obat jamur." },
  { "en": "Nit?", "id": "Satuan terang." },
  { "en": "Nitrat?", "id": "Garam asam nitrat." },
  { "en": "Nitrifikasi?", "id": "Proses kimia." },
  { "en": "Nitril?", "id": "Senyawa." },
  { "en": "Nitrit?", "id": "Garam asam nitrit." },
  { "en": "Nitrobenzena?", "id": "Senyawa." },
  { "en": "Nitrofil?", "id": "Suka nitrogen." },
  { "en": "Nitrofit?", "id": "Tumbuhan nitrogen." },
  { "en": "Nitrogen?", "id": "Unsur gas." },
  { "en": "Nitrogliserin?", "id": "Bahan peledak." },
  { "en": "Nitroselulosa?", "id": "Bahan peledak." },
  { "en": "Nitu?", "id": "Roh (arkais)." },
  { "en": "Nivelir?", "id": "Alat ukur datar." },
  { "en": "Nivo?", "id": "Tingkat (Belanda)." },
  { "en": "Niyaga?", "id": "Penabuh gamelan." },
  { "en": "Niza?", "id": "Perselisihan (Arab)." },
  { "en": "Noa?", "id": "Nama dewa Hawaii." },
  { "en": "Nobelium?", "id": "Unsur kimia." },
  { "en": "Nobesitas?", "id": "Penolakan obesitas." },
  { "en": "Noda?", "id": "Cacat." },
  { "en": "Nodul?", "id": "Benjolan." },
  { "en": "Noja?", "id": "Bintang (Arab)." },
  { "en": "Nok?", "id": "Ujung atap." },
  { "en": "Noken?", "id": "Tas Papua." },
  { "en": "Noktah?", "id": "Titik." },
  { "en": "Nokturnal?", "id": "Aktif malam hari." },
  { "en": "Nokturno?", "id": "Musik malam." },
  { "en": "Nol?", "id": "Angka." },
  { "en": "Nom?", "id": "Hukum (Yunani)." },
  { "en": "Nomadik?", "id": "Berpindah-pindah." },
  { "en": "Nomina?", "id": "Kata benda." },
  { "en": "Nominal?", "id": "Berkaitan nama." },
  { "en": "Nominasi?", "id": "Pencalonan." },
  { "en": "Nominatif?", "id": "Kasus subjek." },
  { "en": "Nominator?", "id": "Pencalon." },
  { "en": "Nomine?", "id": "Calon." },
  { "en": "Nomogram?", "id": "Grafik hitung." },
  { "en": "Nomor?", "id": "Angka." },
  { "en": "Nomos?", "id": "Aturan (Yunani)." },
  { "en": "Non?", "id": "Bukan." },
  { "en": "Nona?", "id": "Gadis." },
  { "en": "Nonagresi?", "id": "Tidak menyerang." },
  { "en": "Nonblok?", "id": "Tidak memihak." },
  { "en": "Nondepartemental?", "id": "Luar departemen." },
  { "en": "None?", "id": "Nada kesembilan." },
  { "en": "Nonfiksi?", "id": "Bukan rekaan." },
  { "en": "Nonformal?", "id": "Tidak resmi." },
  { "en": "Nong?", "id": "Panggilan wanita (Betawi)." },
  { "en": "Nonhistoris?", "id": "Tidak bersejarah." },
  { "en": "Nonius?", "id": "Skala ukur." },
  { "en": "Nonok?", "id": "Gadis kecil." },
  { "en": "Nonol?", "id": "Tidak nol." },
  { "en": "Nonong?", "id": "Dahi menonjol." },
  { "en": "Nonorganik?", "id": "Anorganik." },
  { "en": "Nonparticipan?", "id": "Tidak ikut serta." },
  { "en": "Nonpemerintah?", "id": "Swasta." },
  { "en": "Nonpolitik?", "id": "Tidak politis." },
  { "en": "Nonprofit?", "id": "Nirlaba." },
  { "en": "Nonproduktif?", "id": "Tidak menghasilkan." },
  { "en": "Nonprotein?", "id": "Bukan protein." },
  { "en": "Nonreguler?", "id": "Tidak teratur." },
  { "en": "Nonresidensial?", "id": "Bukan tempat tinggal." },
  { "en": "Nonsens?", "id": "Omong kosong." },
  { "en": "Nonstandar?", "id": "Tidak baku." },
  { "en": "Nonstop?", "id": "Tanpa henti." },
  { "en": "Nonteknis?", "id": "Bukan teknis." },
  { "en": "Nonverbal?", "id": "Bukan lisan." },
  { "en": "Nopember?", "id": "November." },
  { "en": "Norak?", "id": "Kampungan." },
  { "en": "Nordik?", "id": "Eropa Utara." },
  { "en": "Nori?", "id": "Rumput laut kering." },
  { "en": "Norit?", "id": "Karbon aktif." },
  { "en": "Norma?", "id": "Aturan." },
  { "en": "Normal?", "id": "Biasa." },
  { "en": "Normalisasi?", "id": "Proses normal." },
  { "en": "Normatif?", "id": "Sesuai norma." },
  { "en": "Nos?", "id": "Selang (arkais)." },
  { "en": "Nosologi?", "id": "Ilmu klasifikasi penyakit." },
  { "en": "Nostalgia?", "id": "Kenangan manis." },
  { "en": "Nostrum?", "id": "Obat rahasia." },
  { "en": "Not?", "id": "Tanda nada." },
  { "en": "Nota?", "id": "Catatan." },
  { "en": "Notabene?", "id": "Catatan tambahan." },
  { "en": "Notariat?", "id": "Jabatan notaris." },
  { "en": "Notaris?", "id": "Pejabat pembuat akta." },
  { "en": "Notasi?", "id": "Sistem lambang." },
  { "en": "Notes?", "id": "Buku catatan." },
  { "en": "Notifikasi?", "id": "Pemberitahuan." },
  { "en": "Notula?", "id": "Catatan rapat." },
  { "en": "Notulen?", "id": "Notula." },
  { "en": "Notulis?", "id": "Pencatat rapat." },
  { "en": "Nova?", "id": "Bintang baru." },
  { "en": "Novel?", "id": "Roman." },
  { "en": "Novela?", "id": "Cerita pendek." },
  { "en": "Novelis?", "id": "Penulis novel." },
  { "en": "November?", "id": "Bulan kesebelas." },
  { "en": "Novi?", "id": "Baru (arkais)." },
  { "en": "Novisiat?", "id": "Masa percobaan biarawan." },
  { "en": "Novokaina?", "id": "Obat bius." },
  { "en": "Nu?", "id": "Nahdlatul Ulama." },
  { "en": "Nuala?", "id": "Sial (arkais)." },
  { "en": "Nuansa?", "id": "Perbedaan halus." },
  { "en": "Nubar?", "id": "Buah pertama." },
  { "en": "Nubuat?", "id": "Ramalan." },
  { "en": "Nudis?", "id": "Orang telanjang." },
  { "en": "Nudisme?", "id": "Paham telanjang." },
  { "en": "Nuditas?", "id": "Ketelanjangan." },
  { "en": "Nugat?", "id": "Permen." },
  { "en": "Nugraha?", "id": "Anugerah." },
  { "en": "Nujum?", "id": "Ramalan." },
  { "en": "Nukil?", "id": "Kutip." },
  { "en": "Nukilan?", "id": "Kutipan." },
  { "en": "Nukleolus?", "id": "Anak inti sel." },
  { "en": "Nukleon?", "id": "Partikel inti atom." },
  { "en": "Nukleoprotein?", "id": "Protein inti sel." },
  { "en": "Nukleus?", "id": "Inti sel." },
  { "en": "Nuklida?", "id": "Jenis inti atom." },
  { "en": "Nuklir?", "id": "Berkaitan inti atom." },
  { "en": "Numen?", "id": "Kekuatan gaib." },
  { "en": "Numenal?", "id": "Tidak terindra." },
  { "en": "Numerator?", "id": "Pembilang." },
  { "en": "Numerik?", "id": "Angka." },
  { "en": "Numeris?", "id": "Berupa angka." },
  { "en": "Numismatika?", "id": "Ilmu mata uang." },
  { "en": "Nun?", "id": "Di sana." },
  { "en": "Nunatak?", "id": "Puncak gunung es." },
  { "en": "Nunsius?", "id": "Duta Paus." },
  { "en": "Nunut?", "id": "Ikut (Jawa)." },
  { "en": "Nur?", "id": "Cahaya (Arab)." },
  { "en": "Nuraga?", "id": "Kasih (arkais)." },
  { "en": "Nurani?", "id": "Hati." },
  { "en": "Nurbisa?", "id": "Racun." },
  { "en": "Nuri?", "id": "Burung." },
  { "en": "Nuriah?", "id": "Cahaya." },
  { "en": "Nus?", "id": "Akal (Yunani)." },
  { "en": "Nusa?", "id": "Pulau." },
  { "en": "Nusantara?", "id": "Indonesia." },
  { "en": "Nusyuz?", "id": "Pembangkangan istri." },
  { "en": "Nut?", "id": "Mur (Inggris)." },
  { "en": "Nutan?", "id": "Perubahan (arkais)." },
  { "en": "Nutasi?", "id": "Goyangan sumbu." },
  { "en": "Nutriea?", "id": "Hewan pengerat." },
  { "en": "Nutrisi?", "id": "Gizi." },
  { "en": "Nutrisional?", "id": "Bergizi." },
  { "en": "Nuzul?", "id": "Turun (Al-Quran)." },
  { "en": "Nuzulul Quran?", "id": "Turunnya Al-Quran." },
  { "en": "Nya?", "id": "Kata ganti orang ketiga." },
  { "en": "Nyah?", "id": "Enyah." },
  { "en": "Nyai?", "id": "Panggilan wanita." },
  { "en": "Nyak?", "id": "Ibu (Betawi)." },
  { "en": "Nyala?", "id": "Api." },
  { "en": "Nyalang?", "id": "Terbuka lebar (mata)." },
  { "en": "Nyalar?", "id": "Selalu (arkais)." },
  { "en": "Nyale?", "id": "Cacing laut." },
  { "en": "Nyaman?", "id": "Enak." },
  { "en": "Nyambing?", "id": "Benalu." },
  { "en": "Nyamikan?", "id": "Makanan ringan (Jawa)." },
  { "en": "Nyamuk?", "id": "Serangga." },
  { "en": "Nyamur?", "id": "Menyamar (arkais)." },
  { "en": "Nyana?", "id": "Sangka." },
  { "en": "Nyang?", "id": "Yang (Betawi)." },
  { "en": "Nyanya?", "id": "Lembut (arkais)." },
  { "en": "Nyanyah?", "id": "Cerewet." },
  { "en": "Nyanyi?", "id": "Lagu." },
  { "en": "Nyanyian?", "id": "Lagu." },
  { "en": "Nyapang?", "id": "Cabang." },
  { "en": "Nyarai?", "id": "Nyaring." },
  { "en": "Nyarik?", "id": "Sobek (arkais)." },
  { "en": "Nyaring?", "id": "Keras (suara)." },
  { "en": "Nyaris?", "id": "Hampir." },
  { "en": "Nyata?", "id": "Jelas." },
  { "en": "Nyatuh?", "id": "Pohon." },
  { "en": "Nyawa?", "id": "Jiwa." },
  { "en": "Nyawang?", "id": "Melihat (Jawa)." },
  { "en": "Nyedar?", "id": "Tidur (arkais)." },
  { "en": "Nyek?", "id": "Kecil (arkais)." },
  { "en": "Nyelang?", "id": "Pinjam (Jawa)." },
  { "en": "Nyelekit?", "id": "Menyakitkan (ucapan)." },
  { "en": "Nyemplong?", "id": "Masuk (air)." },
  { "en": "Nyenyai?", "id": "Lembek." },
  { "en": "Nyenyak?", "id": "Lelap." },
  { "en": "Nyenyeh?", "id": "Cerewet." },
  { "en": "Nyenyep?", "id": "Sunyi." },
  { "en": "Nyepi?", "id": "Hari raya Hindu." },
  { "en": "Nyeri?", "id": "Sakit." },
  { "en": "Nyes?", "id": "Bunyi." },
  { "en": "Nyi?", "id": "Panggilan wanita." },
  { "en": "Nyinyir?", "id": "Cerewet." },
  { "en": "Nyiri?", "id": "Pohon." },
  { "en": "Nyiru?", "id": "Tampah." },
  { "en": "Nyiur?", "id": "Kelapa." },
  { "en": "Nyol?", "id": "Kain (arkais)." },
  { "en": "Nyoman?", "id": "Nama Bali." },
  { "en": "Nyong?", "id": "Panggilan anak laki-laki (Ambon)." },
  { "en": "Nyonya?", "id": "Panggilan wanita menikah." },
  { "en": "Nyonyong?", "id": "Menonjol." },
  { "en": "Nyunyut?", "id": "Isap." },
  { "en": "O?", "id": "Huruf." },
  { "en": "Oasis?", "id": "Daerah subur di gurun." },
  { "en": "Oat?", "id": "Gandum." },
  { "en": "Obduksi?", "id": "Autopsi." },
  { "en": "Obelisk?", "id": "Tugu." },
  { "en": "Obeng?", "id": "Alat." },
  { "en": "Ober?", "id": "Pelayan (Belanda)." },
  { "en": "Obes?", "id": "Gemuk." },
  { "en": "Obesitas?", "id": "Kegemukan." },
  { "en": "Obi?", "id": "Ikat pinggang kimono." },
  { "en": "Obituari?", "id": "Berita kematian." },
  { "en": "Objek?", "id": "Sasaran." },
  { "en": "Objektif?", "id": "Adil." },
  { "en": "Objektivisme?", "id": "Paham objektivitas." },
  { "en": "Objektivitas?", "id": "Keadilan." },
  { "en": "Oblat?", "id": "Persembahan." },
  { "en": "Oblatin?", "id": "Kertas obat." },
  { "en": "Obligasi?", "id": "Surat utang." },
  { "en": "Oblik?", "id": "Miring." },
  { "en": "Obliterasi?", "id": "Penghapusan." },
  { "en": "Oblong?", "id": "Lonjong." },
  { "en": "Oboe?", "id": "Alat musik tiup." },
  { "en": "Oboed?", "id": "Perintah (arkais)." },
  { "en": "Obok?", "id": "Cuci (tangan)." },
  { "en": "Obol?", "id": "Mata uang Yunani kuno." },
  { "en": "Obor?", "id": "Pelita." },
  { "en": "Obral?", "id": "Jual murah." },
  { "en": "Obrol?", "id": "Bicara." },
  { "en": "Obrolan?", "id": "Percakapan." },
  { "en": "Obsesi?", "id": "Pikiran terus-menerus." },
  { "en": "Obsesif?", "id": "Bersifat obsesi." },
  { "en": "Obsidian?", "id": "Batuan." },
  { "en": "Obskur?", "id": "Gelap." },
  { "en": "Obskurantisme?", "id": "Penghalangan pengetahuan." },
  { "en": "Observasi?", "id": "Pengamatan." },
  { "en": "Observasional?", "id": "Bersifat pengamatan." },
  { "en": "Observatorium?", "id": "Tempat pengamatan bintang." },
  { "en": "Obsolet?", "id": "Kuno." },
  { "en": "Obstetri?", "id": "Ilmu kebidanan." },
  { "en": "Obstetri?", "id": "Ilmu persalinan." },
  { "en": "Obstinasi?", "id": "Keras kepala." },
  { "en": "Obstinat?", "id": "Keras kepala." },
  { "en": "Obstruen?", "id": "Bunyi hambat." },
  { "en": "Obstruksi?", "id": "Halangan." },
  { "en": "Obvi?", "id": "Menghilangkan (arkais)." },
  { "en": "Obyek?", "id": "Objek." },
  { "en": "Obyektif?", "id": "Objektif." },
  { "en": "Ocek?", "id": "Lepas (arkais)." },
  { "en": "Ode?", "id": "Sajak pujian." },
  { "en": "Odema?", "id": "Bengkak." },
  { "en": "Odeon?", "id": "Gedung musik Yunani." },
  { "en": "Odikal?", "id": "Ode." },
  { "en": "Odoh?", "id": "Jelek (arkais)." },
  { "en": "Odolan?", "id": "Pasta gigi." },
  { "en": "Odol?", "id": "Pasta gigi." },
  { "en": "Odometer?", "id": "Pengukur jarak." },
  { "en": "Odontoblas?", "id": "Sel gigi." },
  { "en": "Odontoid?", "id": "Seperti gigi." },
  { "en": "Odontologi?", "id": "Ilmu gigi." },
  { "en": "Odor?", "id": "Bau." },
  { "en": "Odoran?", "id": "Pewangi." },
  { "en": "Oedipus kompleks?", "id": "Istilah psikoanalisis." },
  { "en": "Oekumene?", "id": "Ekumene." },
  { "en": "Oersted?", "id": "Satuan medan magnet." },
  { "en": "Of?", "id": "Atau (Belanda)." },
  { "en": "Ofensif?", "id": "Serangan." },
  { "en": "Oferte?", "id": "Tawaran." },
  { "en": "Ofisial?", "id": "Resmi." },
  { "en": "Ofset?", "id": "Cetak." },
  { "en": "Oftalmia?", "id": "Radang mata." },
  { "en": "Oftalmoskop?", "id": "Alat periksa mata." },
  { "en": "Ogal-agil?", "id": "Goyang." },
  { "en": "Ogah?", "id": "Enggan." },
  { "en": "Ogah-ogahan?", "id": "Malas-malasan." },
  { "en": "Ogam?", "id": "Aksara Irlandia." },
  { "en": "Ogive?", "id": "Lengkung runcing." },
  { "en": "Ogoh-ogoh?", "id": "Patung Bali." },
  { "en": "Ogut?", "id": "Saya (prokem)." },
  { "en": "Oh?", "id": "Seruan." },
  { "en": "Ohm?", "id": "Satuan hambatan listrik." },
  { "en": "Oi?", "id": "Seruan." },
  { "en": "Oidipuskompleks?", "id": "Oedipus kompleks." },
  { "en": "Oikumene?", "id": "Ekumene." },
  { "en": "Oja?", "id": "Ajak (arkais)." },
  { "en": "Ojek?", "id": "Transportasi motor." },
  { "en": "Ojen?", "id": "Mencoba (arkais)." },
  { "en": "Ojol?", "id": "Ojek online." },
  { "en": "Okapi?", "id": "Hewan Afrika." },
  { "en": "Oke?", "id": "Setuju." },
  { "en": "Oker?", "id": "Warna tanah." },
  { "en": "Oklokrasi?", "id": "Pemerintahan massa." },
  { "en": "Oklusi?", "id": "Penutupan." },
  { "en": "Oklusal?", "id": "Permukaan kunyah gigi." },
  { "en": "Oknum?", "id": "Perorangan." },
  { "en": "Oktaf?", "id": "Interval delapan nada." },
  { "en": "Oktagon?", "id": "Segidelapan." },
  { "en": "Oktahedron?", "id": "Bidang delapan." },
  { "en": "Oktal?", "id": "Basis delapan." },
  { "en": "Oktan?", "id": "Ukuran bensin." },
  { "en": "Oktana?", "id": "Senyawa hidrokarbon." },
  { "en": "Oktant?", "id": "Alat navigasi." },
  { "en": "Oktet?", "id": "Kelompok delapan." },
  { "en": "Oktober?", "id": "Bulan kesepuluh." },
  { "en": "Oktopoda?", "id": "Hewan berkaki delapan." },
  { "en": "Oktopus?", "id": "Gurita." },
  { "en": "Okular?", "id": "Berkaitan mata." },
  { "en": "Okuler?", "id": "Lensa mata." },
  { "en": "Okulis?", "id": "Ahli mata." },
  { "en": "Okultis?", "id": "Ahli gaib." },
  { "en": "Okultisme?", "id": "Paham kegaiban." },
  { "en": "Okupasi?", "id": "Pendudukan." },
  { "en": "Okupasional?", "id": "Berkaitan pekerjaan." },
  { "en": "Olah?", "id": "Kerja." },
  { "en": "Olahraga?", "id": "Aktivitas fisik." },
  { "en": "Olahragawan?", "id": "Atlet." },
  { "en": "Olak?", "id": "Pusaran air." },
  { "en": "Olak-olak?", "id": "Pusaran." },
  { "en": "Olanda?", "id": "Belanda (arkais)." },
  { "en": "Oleander?", "id": "Tanaman." },
  { "en": "Oleat?", "id": "Garam asam oleat." },
  { "en": "Oleh?", "id": "Karena." },
  { "en": "Oleh-oleh?", "id": "Buah tangan." },
  { "en": "Oleng?", "id": "Goyang." },
  { "en": "Oleografi?", "id": "Litografi minyak." },
  { "en": "Oleometer?", "id": "Pengukur minyak." },
  { "en": "Oles?", "id": "Sapu." },
  { "en": "Olet?", "id": "Kain lap." },
  { "en": "Pertanyaanoleum?", "id": "Minyak." },
  { "en": "Olfaksi?", "id": "Penciuman." },
  { "en": "Olfaktometer?", "id": "Pengukur bau." },
  { "en": "Olfaktori?", "id": "Berkaitan penciuman." },
  { "en": "Oligarki?", "id": "Pemerintahan segelintir." },
  { "en": "Oligofagus?", "id": "Pemakan sedikit jenis." },
  { "en": "Oligofrenia?", "id": "Keterbelakangan mental." },
  { "en": "Oligopoli?", "id": "Pasar beberapa penjual." },
  { "en": "Oligopolistis?", "id": "Bersifat oligopoli." },
  { "en": "Oligosen?", "id": "Kala geologi." },
  { "en": "Oligosakarida?", "id": "Karbohidrat." },
  { "en": "Oligospermia?", "id": "Kurang sperma." },
  { "en": "Oligotropik?", "id": "Kurang nutrisi (air)." },
  { "en": "Oliguria?", "id": "Kurang kencing." },
  { "en": "Olimpiade?", "id": "Pesta olahraga." },
  { "en": "Olin?", "id": "Minyak (arkais)." },
  { "en": "Olok?", "id": "Ejek." },
  { "en": "Olok-olok?", "id": "Ejekan." },
  { "en": "Om?", "id": "Paman." },
  { "en": "Oma?", "id": "Nenek." },
  { "en": "Omega?", "id": "Huruf Yunani." },
  { "en": "Omel?", "id": "Mengomel." },
  { "en": "Omerta?", "id": "Sumpah diam mafia." },
  { "en": "Omfalitis?", "id": "Radang pusar." },
  { "en": "Omnibora?", "id": "Omnivor." },
  { "en": "Omnibus?", "id": "Kendaraan besar." },
  { "en": "Omnipotens?", "id": "Mahakuasa." },
  { "en": "Omnipoten?", "id": "Mahakuasa." },
  { "en": "Omnispora?", "id": "Spora." },
  { "en": "Omnivor?", "id": "Pemakan segala." },
  { "en": "Omo?", "id": "Bahu (Yunani)." },
  { "en": "Omong?", "id": "Bicara." },
  { "en": "Ompol?", "id": "Ngompol." },
  { "en": "Ompong?", "id": "Tidak bergigi." },
  { "en": "Omprog?", "id": "Cerobong (Jawa)." },
  { "en": "Omset?", "id": "Pendapatan." },
  { "en": "Omur?", "id": "Umur (arkais)." },
  { "en": "Onagata?", "id": "Pemeran wanita (kabuki)." },
  { "en": "Onak?", "id": "Duri." },
  { "en": "Onani?", "id": "Masturbasi." },
  { "en": "Onar?", "id": "Keributan." },
  { "en": "Oncat?", "id": "Lari." },
  { "en": "Once?", "id": "Pipa rokok." },
  { "en": "Oncem?", "id": "Rendam (arkais)." },
  { "en": "Oncer?", "id": "Mencret." },
  { "en": "Oncom?", "id": "Makanan." },
  { "en": "Ondel-ondel?", "id": "Boneka Betawi." },
  { "en": "Onderdil?", "id": "Suku cadang." },
  { "en": "Ondok?", "id": "Sembunyi (arkais)." },
  { "en": "Ondolan?", "id": "Pemain utama (Jawa)." },
  { "en": "Ondong?", "id": "Bujuk (arkais)." },
  { "en": "Onga?", "id": "Bodoh (arkais)." },
  { "en": "Ongah-angih?", "id": "Terengah-engah." },
  { "en": "Onggok?", "id": "Tumpuk." },
  { "en": "Ongji?", "id": "Candik." },
  { "en": "Ongkang-ongkang?", "id": "Duduk santai." },
  { "en": "Ongkok?", "id": "Jongkok (arkais)." },
  { "en": "Ongkos?", "id": "Biaya." },
  { "en": "Oniks?", "id": "Batu permata." },
  { "en": "Onkogen?", "id": "Gen pemicu kanker." },
  { "en": "Onkogenik?", "id": "Menyebabkan kanker." },
  { "en": "Onkolog?", "id": "Ahli kanker." },
  { "en": "Onkologi?", "id": "Ilmu kanker." },
  { "en": "Onkolisis?", "id": "Perusakan tumor." },
  { "en": "Onomasiologi?", "id": "Ilmu penamaan." },
  { "en": "Onomastika?", "id": "Ilmu nama diri." },
  { "en": "Onomatope?", "id": "Tiruan bunyi." },
  { "en": "Onor?", "id": "Kehormatan (arkais)." },
  { "en": "Onosmodium?", "id": "Tanaman." },
  { "en": "Ons?", "id": "Satuan berat." },
  { "en": "Onset?", "id": "Awal." },
  { "en": "Onta?", "id": "Unta." },
  { "en": "Ontogeni?", "id": "Perkembangan individu." },
  { "en": "Ontologi?", "id": "Filsafat keberadaan." },
  { "en": "Onyah-anyih?", "id": "Gelisah (arkais)." },
  { "en": "Onyak-anyik?", "id": "Goyang." },
  { "en": "Onyok?", "id": "Sodok (arkais)." },
  { "en": "Oogenesis?", "id": "Pembentukan sel telur." },
  { "en": "Oogonium?", "id": "Sel induk telur." },
  { "en": "Oolit?", "id": "Batuan." },
  { "en": "Oologi?", "id": "Ilmu telur." },
  { "en": "Oom?", "id": "Paman." },
  { "en": "Oosfer?", "id": "Sel telur." },
  { "en": "Oosista?", "id": "Tahap parasit." },
  { "en": "Oospora?", "id": "Spora." },
  { "en": "Opa?", "id": "Kakek." },
  { "en": "Opak?", "id": "Kerupuk." },
  { "en": "Opal?", "id": "Batu permata." },
  { "en": "Opalesen?", "id": "Berkilau." },
  { "en": "Opas?", "id": "Penjaga." },
  { "en": "Opel?", "id": "Mobil." },
  { "en": "Open?", "id": "Terbuka (Inggris)." },
  { "en": "Oper?", "id": "Ambil alih." },
  { "en": "Opera?", "id": "Sandiwara musik." },
  { "en": "Operasi?", "id": "Tindakan." },
  { "en": "Operasional?", "id": "Siap pakai." },
  { "en": "Operatif?", "id": "Efektif." },
  { "en": "Operator?", "id": "Pelaksana." },
  { "en": "Operet?", "id": "Opera ringan." },
  { "en": "Operkulum?", "id": "Tutup insang." },
  { "en": "Opini?", "id": "Pendapat." },
  { "en": "Opisometer?", "id": "Pengukur jarak peta." },
  { "en": "Opium?", "id": "Candu." },
  { "en": "Oplah?", "id": "Jumlah cetak." },
  { "en": "Oplos?", "id": "Campur." },
  { "en": "Opname?", "id": "Rekam." },
  { "en": "Oponen?", "id": "Lawan." },
  { "en": "Oportunis?", "id": "Pencari kesempatan." },
  { "en": "Oportunisme?", "id": "Sikap oportunis." },
  { "en": "Oportunitas?", "id": "Kesempatan." },
  { "en": "Oposan?", "id": "Oponen." },
  { "en": "Oposisi?", "id": "Pihak lawan." },
  { "en": "Opsen?", "id": "Pajak tambahan." },
  { "en": "Opsi?", "id": "Pilihan." },
  { "en": "Opsional?", "id": "Pilihan." },
  { "en": "Opsiner?", "id": "Pengawas (Belanda)." },
  { "en": "Opstal?", "id": "Bangunan (Belanda)." },
  { "en": "Optatif?", "id": "Menyatakan harapan." },
  { "en": "Optik?", "id": "Berkaitan cahaya." },
  { "en": "Optimal?", "id": "Terbaik." },
  { "en": "Optimis?", "id": "Berpengharapan baik." },
  { "en": "Optimisme?", "id": "Sikap optimis." },
  { "en": "Optimum?", "id": "Kondisi terbaik." },
  { "en": "Optisien?", "id": "Ahli kacamata." },
  { "en": "Optoelektronika?", "id": "Elektronika cahaya." },
  { "en": "Optometer?", "id": "Pengukur mata." },
  { "en": "Optometri?", "id": "Ilmu pengukuran mata." },
  { "en": "Opus?", "id": "Karya musik." },
  { "en": "Or?", "id": "Atau (Inggris)." },
  { "en": "Ora?", "id": "Tidak (Jawa)." },
  { "en": "Orad?", "id": "Penyakit kuda." },
  { "en": "Oral?", "id": "Lisan." },
  { "en": "Oralit?", "id": "Obat diare." },
  { "en": "Orang?", "id": "Manusia." },
  { "en": "Oranye?", "id": "Jingga." },
  { "en": "Orasi?", "id": "Pidato." },
  { "en": "Orator?", "id": "Ahli pidato." },
  { "en": "Oratoria?", "id": "Musik vokal." },
  { "en": "Oratorio?", "id": "Oratoria." },
  { "en": "Oratorium?", "id": "Tempat ibadah." },
  { "en": "Orbit?", "id": "Jalur edar." },
  { "en": "Orbital?", "id": "Berkaitan orbit." },
  { "en": "Orde?", "id": "Tatanan." },
  { "en": "Order?", "id": "Pesanan." },
  { "en": "Ordik?", "id": "Bentak (arkais)." },
  { "en": "Ordinal?", "id": "Menyatakan tingkatan." },
  { "en": "Ordinasi?", "id": "Penahbisan." },
  { "en": "Ordinat?", "id": "Sumbu Y." },
  { "en": "Ordo?", "id": "Golongan (biologi)." },
  { "en": "Ordonans?", "id": "Peraturan." },
  { "en": "Ordonansi?", "id": "Ordonans." },
  { "en": "Oregano?", "id": "Rempah." },
  { "en": "Orek?", "id": "Aduk." },
  { "en": "Oren?", "id": "Oranye." },
  { "en": "Oreng?", "id": "Orang (Madura)." },
  { "en": "Organ?", "id": "Alat tubuh." },
  { "en": "Organdi?", "id": "Kain." },
  { "en": "Organel?", "id": "Bagian sel." },
  { "en": "Organik?", "id": "Hayati." },
  { "en": "Organis?", "id": "Hidup." },
  { "en": "Organisasi?", "id": "Perkumpulan." },
  { "en": "Organisator?", "id": "Pengatur." },
  { "en": "Organisatoris?", "id": "Berkaitan organisasi." },
  { "en": "Organisme?", "id": "Makhluk hidup." },
  { "en": "Organogenesis?", "id": "Pembentukan organ." },
  { "en": "Organogram?", "id": "Bagan organisasi." },
  { "en": "Organon?", "id": "Alat (Yunani)." },
  { "en": "Orgasme?", "id": "Puncak kenikmatan seksual." },
  { "en": "Orgel?", "id": "Alat musik." },
  { "en": "Orgi?", "id": "Pesta seks." },
  { "en": "Orientasi?", "id": "Pengenalan." },
  { "en": "Ornamen?", "id": "Hiasan." },
  { "en": "Ornamental?", "id": "Bersifat hiasan." },
  { "en": "Ornitofil?", "id": "Penyerbukan burung." },
  { "en": "Ornitologi?", "id": "Ilmu burung." },
  { "en": "Ornitosis?", "id": "Penyakit burung." },
  { "en": "Oro-oro?", "id": "Tanah lapang." },
  { "en": "Orogenesis?", "id": "Pembentukan gunung." },
  { "en": "Orografi?", "id": "Ilmu relief daratan." },
  { "en": "Orong-orong?", "id": "Serangga." },
  { "en": "Orseket?", "id": "Kain (arkais)." },
  { "en": "Orsinil?", "id": "Asli." },
  { "en": "Ortodoks?", "id": "Kuno." },
  { "en": "Ortodoksi?", "id": "Paham ortodoks." },
  { "en": "Ortodrom?", "id": "Jarak terpendek." },
  { "en": "Ortoep?", "id": "Pengucapan benar." },
  { "en": "Ortodontika?", "id": "Ilmu meratakan gigi." },
  { "en": "Ortografi?", "id": "Sistem ejaan." },
  { "en": "Ortoklas?", "id": "Mineral." },
  { "en": "Ortopedi?", "id": "Ilmu tulang." },
  { "en": "Ortopedis?", "id": "Berkaitan ortopedi." },
  { "en": "Os?", "id": "Tulang (Latin)." },
  { "en": "Osean?", "id": "Samudra." },
  { "en": "Oseania?", "id": "Kawasan Pasifik." },
  { "en": "Oseanografi?", "id": "Ilmu laut." },
  { "en": "Oseanologi?", "id": "Oseanografi." },
  { "en": "Oselot?", "id": "Kucing hutan." },
  { "en": "Osifikasi?", "id": "Penulangan." },
  { "en": "Oskilasi?", "id": "Getaran." },
  { "en": "Oskilator?", "id": "Penggetar." },
  { "en": "Oskulum?", "id": "Lubang spons." },
  { "en": "Osmeter?", "id": "Pengukur bau." },
  { "en": "Osmiridium?", "id": "Aloi." },
  { "en": "Osmium?", "id": "Unsur kimia." },
  { "en": "Osmometer?", "id": "Pengukur tekanan osmotik." },
  { "en": "Osmoregulasi?", "id": "Pengaturan cairan tubuh." },
  { "en": "Osmosis?", "id": "Perpindahan cairan." },
  { "en": "Osteitis?", "id": "Radang tulang." },
  { "en": "Osteoblas?", "id": "Sel pembentuk tulang." },
  { "en": "Osteogenesis?", "id": "Pembentukan tulang." },
  { "en": "Osteokalsitonin?", "id": "Hormon." },
  { "en": "Osteoklas?", "id": "Sel perusak tulang." },
  { "en": "Osteologi?", "id": "Ilmu tulang." },
  { "en": "Osteoma?", "id": "Tumor tulang." },
  { "en": "Osteomalasia?", "id": "Pelunakan tulang." },
  { "en": "Osteomielitis?", "id": "Radang sumsum tulang." },
  { "en": "Osteopati?", "id": "Penyakit tulang." },
  { "en": "Osteoporosis?", "id": "Pengeroposan tulang." },
  { "en": "Osteosit?", "id": "Sel tulang." },
  { "en": "Ostia?", "id": "Lubang." },
  { "en": "Ostium?", "id": "Mulut (Latin)." },
  { "en": "Otak?", "id": "Benak." },
  { "en": "Otak-otak?", "id": "Makanan." },
  { "en": "Otar?", "id": "Perisai (Arab)." },
  { "en": "Otek?", "id": "Goyang." },
  { "en": "Otentik?", "id": "Asli." },
  { "en": "Oter?", "id": "Berang-berang." },
  { "en": "Otitis?", "id": "Radang telinga." },
  { "en": "Oto?", "id": "Mobil." },
  { "en": "Otobus?", "id": "Bus." },
  { "en": "Otodidak?", "id": "Belajar sendiri." },
  { "en": "Otoklaf?", "id": "Sterilisator." },
  { "en": "Otokrasi?", "id": "Pemerintahan tunggal." },
  { "en": "Otokrat?", "id": "Penguasa tunggal." },
  { "en": "Otokritik?", "id": "Kritik diri." },
  { "en": "Otolit?", "id": "Batu telinga." },
  { "en": "Otologi?", "id": "Ilmu telinga." },
  { "en": "Otomasi?", "id": "Pengotomatisan." },
  { "en": "Otomatis?", "id": "Sendirinya." },
  { "en": "Otomatisasi?", "id": "Otomasi." },
  { "en": "Otomat?", "id": "Mesin otomatis." },
  { "en": "Otomobil?", "id": "Mobil." },
  { "en": "Otomotif?", "id": "Berkaitan mobil." },
  { "en": "Otonom?", "id": "Mandiri." },
  { "en": "Otonomi?", "id": "Kemandirian." },
  { "en": "Otopet?", "id": "Skuter." },
  { "en": "Otorhinolaringologi?", "id": "Ilmu THT." },
  { "en": "Otorisator?", "id": "Pemberi wewenang." },
  { "en": "Otorisasi?", "id": "Pemberian wewenang." },
  { "en": "Otoritas?", "id": "Wewenang." },
  { "en": "Otoriter?", "id": "Berkuasa mutlak." },
  { "en": "Otoskop?", "id": "Alat periksa telinga." },
  { "en": "Otot?", "id": "Daging." },
  { "en": "Oval?", "id": "Lonjong." },
  { "en": "Ovari?", "id": "Indung telur (arkais)." },
  { "en": "Ovarium?", "id": "Indung telur." },
  { "en": "Ovasi?", "id": "Tepuk tangan meriah." },
  { "en": "Oven?", "id": "Pemanggang." },
  { "en": "Over?", "id": "Berlebihan (Inggris)." },
  { "en": "Overdosis?", "id": "Dosis berlebih." },
  { "en": "Overkompensasi?", "id": "Kompensasi berlebih." },
  { "en": "Overpopulasi?", "id": "Kelebihan penduduk." },
  { "en": "Overproduksi?", "id": "Produksi berlebih." },
  { "en": "Oviduk?", "id": "Saluran telur." },
  { "en": "Ovipar?", "id": "Bertelur." },
  { "en": "Oviparitas?", "id": "Cara bertelur." },
  { "en": "Ovipositor?", "id": "Alat bertelur serangga." },
  { "en": "Ovisida?", "id": "Pembasmi telur." },
  { "en": "Ovovivipar?", "id": "Bertelur melahirkan." },
  { "en": "Ovulasi?", "id": "Pelepasan sel telur." },
  { "en": "Ovulum?", "id": "Bakal biji." },
  { "en": "Ovum?", "id": "Sel telur." },
  { "en": "Oyak?", "id": "Goyang (arkais)." },
  { "en": "Oyok?", "id": "Kejar." },
  { "en": "Oyong?", "id": "Sayuran." },
  { "en": "Ozon?", "id": "Gas." },
  { "en": "Ozonisasi?", "id": "Proses ozon." },
  { "en": "Ozonosfer?", "id": "Lapisan ozon." },
  { "en": "Pa?", "id": "Ayah (singkatan)." },
  { "en": "Pabean?", "id": "Bea cukai." },
  { "en": "Pabian?", "id": "Pabean (arkais)." },
  { "en": "Pabrik?", "id": "Tempat produksi." },
  { "en": "Pacai?", "id": "Inai." },
  { "en": "Pacak?", "id": "Tancap." },
  { "en": "Pacal?", "id": "Budak." },
  { "en": "Pacar?", "id": "Kekasih." },
  { "en": "Pacau?", "id": "Jimat." },
  { "en": "Pace?", "id": "Mengkudu." },
  { "en": "Pacek?", "id": "Kawin (hewan)." },
  { "en": "Pacero?", "id": "Penyakit (arkais)." },
  { "en": "Pacet?", "id": "Lintah darat." },
  { "en": "Pacih?", "id": "Pejal (arkais)." },
  { "en": "Pacik?", "id": "Pegang." },
  { "en": "Pacu?", "id": "Lomba cepat." },
  { "en": "Pacuk?", "id": "Patuk." },
  { "en": "Pacul?", "id": "Cangkul." },
  { "en": "Pada?", "id": "Di." },
  { "en": "Padah?", "id": "Akibat." },
  { "en": "Padahal?", "id": "Sedangkan." },
  { "en": "Padak?", "id": "Terang (arkais)." },
  { "en": "Padam?", "id": "Mati (api)." },
  { "en": "Padan?", "id": "Sesuai." },
  { "en": "Padang?", "id": "Lapangan." },
  { "en": "Padas?", "id": "Batu keras." },
  { "en": "Padasan?", "id": "Tempayan air." },
  { "en": "Padat?", "id": "Pejal." },
  { "en": "Padepokan?", "id": "Perguruan." },
  { "en": "Padi?", "id": "Tanaman." },
  { "en": "Padri?", "id": "Pendeta Kristen." },
  { "en": "Padu?", "id": "Satupadu." },
  { "en": "Padudan?", "id": "Tempat candu." },
  { "en": "Paduk?", "id": "Patuk (arkais)." },
  { "en": "Paduka?", "id": "Tuan." },
  { "en": "Paedofil?", "id": "Penyuka anak." },
  { "en": "Paedofilia?", "id": "Kelainan seksual." },
  { "en": "Paes?", "id": "Hiasan dahi." },
  { "en": "Pagai?", "id": "Pegang (arkais)." },
  { "en": "Pagan?", "id": "Penyembah berhala." },
  { "en": "Paganisme?", "id": "Paham pagan." },
  { "en": "Pagar?", "id": "Pembatas." },
  { "en": "Pagas?", "id": "Pangkas." },
  { "en": "Pagebluk?", "id": "Wabah." },
  { "en": "Pagelaran?", "id": "Pertunjukan." },
  { "en": "Pagi?", "id": "Waktu subuh." },
  { "en": "Pagoda?", "id": "Kuil Buddha." },
  { "en": "Pagon?", "id": "Aksara Arab-Jawa." },
  { "en": "Pagu?", "id": "Batas atas." },
  { "en": "Pagun?", "id": "Kuat (arkais)." },
  { "en": "Pagut?", "id": "Patuk." },
  { "en": "Paguyuban?", "id": "Perkumpulan." },
  { "en": "Pah?", "id": "Seruan." },
  { "en": "Paha?", "id": "Bagian kaki." },
  { "en": "Pahala?", "id": "Ganjaran baik." },
  { "en": "Paham?", "id": "Mengerti." },
  { "en": "Pahar?", "id": "Nampan." },
  { "en": "Pahat?", "id": "Ukir." },
  { "en": "Pahatan?", "id": "Ukiran." },
  { "en": "Paheman?", "id": "Perkumpulan (Sunda)." },
  { "en": "Pahit?", "id": "Rasa." },
  { "en": "Pahlawan?", "id": "Hero." },
  { "en": "Pai?", "id": "Kue." },
  { "en": "Paido?", "id": "Kritik (Jawa)." },
  { "en": "Pail?", "id": "Ukuran (Belanda)." },
  { "en": "Pailit?", "id": "Bangkrut." },
  { "en": "Pair?", "id": "Pasangan (Inggris)." },
  { "en": "Pais?", "id": "Pepes." },
  { "en": "Pait?", "id": "Pahit (arkais)." },
  { "en": "Pajak?", "id": "Iuran wajib." },
  { "en": "Pajal?", "id": "Tekan (arkais)." },
  { "en": "Pajang?", "id": "Hias." },
  { "en": "Pajar?", "id": "Fajar (arkais)." },
  { "en": "Pajis?", "id": "Jijik (arkais)." },
  { "en": "Pak?", "id": "Bapak (panggilan)." },
  { "en": "Paka?", "id": "Putih (arkais)." },
  { "en": "Pakai?", "id": "Guna." },
  { "en": "Pakaian?", "id": "Busana." },
  { "en": "Pakal?", "id": "Dempol." },
  { "en": "Pakalam?", "id": "Diam (arkais)." },
  { "en": "Pakan?", "id": "Makanan ternak." },
  { "en": "Pakansi?", "id": "Liburan." },
  { "en": "Pakar?", "id": "Ahli." },
  { "en": "Pakaryan?", "id": "Pekerjaan (Jawa)." },
  { "en": "Pakat?", "id": "Sepakat." },
  { "en": "Pakcik?", "id": "Paman." },
  { "en": "Pakde?", "id": "Paman (Jawa)." },
  { "en": "Pakem?", "id": "Baku." },
  { "en": "Paket?", "id": "Bungkusan." },
  { "en": "Pakih?", "id": "Fakih." },
  { "en": "Pakis?", "id": "Tumbuhan paku." },
  { "en": "Paklik?", "id": "Paman." },
  { "en": "Pakma?", "id": "Bunga raflesia." },
  { "en": "Paksa?", "id": "Desak." },
  { "en": "Paksi?", "id": "Burung." },
  { "en": "Paksina?", "id": "Utara (Sanskerta)." },
  { "en": "Pakta?", "id": "Perjanjian." },
  { "en": "Paku?", "id": "Pancang." },
  { "en": "Pakuh?", "id": "Paku (arkais)." },
  { "en": "Pakuk?", "id": "Penyakit." },
  { "en": "Pal?", "id": "Tonggak jarak." },
  { "en": "Pala?", "id": "Rempah." },
  { "en": "Paladium?", "id": "Unsur kimia." },
  { "en": "Palagan?", "id": "Medan perang." },
  { "en": "Palai?", "id": "Pohon." },
  { "en": "Palak?", "id": "Pintu sorong." },
  { "en": "Palaka?", "id": "Pala (arkais)." },
  { "en": "Palam?", "id": "Sumbat." },
  { "en": "Palang?", "id": "Rintangan." },
  { "en": "Palapa?", "id": "Sumpah Gajah Mada." },
  { "en": "Palar?", "id": "Harap (arkais)." },
  { "en": "Palari?", "id": "Pelari." },
  { "en": "Palas?", "id": "Daun." },
  { "en": "Palasik?", "id": "Hantu." },
  { "en": "Palat?", "id": "Langit-langit mulut." },
  { "en": "Palatal?", "id": "Bunyi langit-langit." },
  { "en": "Palatalisasi?", "id": "Proses palatal." },
  { "en": "Palato-alveolar?", "id": "Bunyi." },
  { "en": "Palatum?", "id": "Langit-langit mulut." },
  { "en": "Palau?", "id": "Negara." },
  { "en": "Pale?", "id": "Warna pucat (Inggris)." },
  { "en": "Palem?", "id": "Palma." },
  { "en": "Paleobotani?", "id": "Ilmu tumbuhan purba." },
  { "en": "Paleoekologi?", "id": "Ekologi purba." },
  { "en": "Paleogen?", "id": "Zaman geologi." },
  { "en": "Paleogeografi?", "id": "Geografi purba." },
  { "en": "Paleografi?", "id": "Ilmu tulisan kuno." },
  { "en": "Paleoklimatologi?", "id": "Iklim purba." },
  { "en": "Paleolitik?", "id": "Zaman batu tua." },
  { "en": "Paleolitikum?", "id": "Paleolitik." },
  { "en": "Paleontolog?", "id": "Ahli fosil." },
  { "en": "Paleontologi?", "id": "Ilmu fosil." },
  { "en": "Paleosen?", "id": "Kala geologi." },
  { "en": "Paleozoikum?", "id": "Zaman hidup tua." },
  { "en": "Pales?", "id": "Pucat (arkais)." },
  { "en": "Palestina?", "id": "Negara." },
  { "en": "Palet?", "id": "Papan cat." },
  { "en": "Pali?", "id": "Bahasa kuno India." },
  { "en": "Paliatif?", "id": "Meringankan." },
  { "en": "Paling?", "id": "Ter." },
  { "en": "Palin?", "id": "Putar (arkais)." },
  { "en": "Palindrom?", "id": "Kata bolak-balik." },
  { "en": "Palinologi?", "id": "Ilmu serbuk sari." },
  { "en": "Palir?", "id": "Aliran air." },
  { "en": "Palit?", "id": "Oles." },
  { "en": "Palka?", "id": "Ruang muat kapal." },
  { "en": "Palma?", "id": "Jenis pohon." },
  { "en": "Palsu?", "id": "Tiruan." },
  { "en": "Palu?", "id": "Martil." },
  { "en": "Paluh?", "id": "Rawa." },
  { "en": "Palung?", "id": "Dasar laut dalam." },
  { "en": "Pam?", "id": "Bibi (Belanda)." },
  { "en": "Pamah?", "id": "Tanah datar." },
  { "en": "Paman?", "id": "Saudara ayah/ibu." },
  { "en": "Pameo?", "id": "Ungkapan populer." },
  { "en": "Pamer?", "id": "Tunjuk." },
  { "en": "Pameran?", "id": "Ekshibisi." },
  { "en": "Pamflet?", "id": "Selebaran." },
  { "en": "Pamit?", "id": "Permisi." },
  { "en": "Pamoja?", "id": "Bersama (Swahili)." },
  { "en": "Pamong?", "id": "Pengurus." },
  { "en": "Pamor?", "id": "Gengsi." },
  { "en": "Pampa?", "id": "Padang rumput." },
  { "en": "Pampan?", "id": "Papan." },
  { "en": "Pampas?", "id": "Pampa." },
  { "en": "Pampat?", "id": "Padat." },
  { "en": "Pamper?", "id": "Manjakan (Inggris)." },
  { "en": "Pamrih?", "id": "Imbalan." },
  { "en": "Pamungkas?", "id": "Terakhir." },
  { "en": "Pan?", "id": "Wajan (Inggris)." },
  { "en": "Pana?", "id": "Panas (arkais)." },
  { "en": "Panah?", "id": "Senjata." },
  { "en": "Panai?", "id": "Uang mahar." },
  { "en": "Panakawan?", "id": "Pengiring (wayang)." },
  { "en": "Panar?", "id": "Terang (arkais)." },
  { "en": "Panas?", "id": "Suhu tinggi." },
  { "en": "Panasea?", "id": "Obat segala penyakit." },
  { "en": "Panau?", "id": "Panu." },
  { "en": "Panca?", "id": "Lima (Sanskerta)." },
  { "en": "Pancabicara?", "id": "Banyak bicara." },
  { "en": "Pancaindera?", "id": "Lima indra." },
  { "en": "Pancaka?", "id": "Tempat bakar mayat." },
  { "en": "Pancakara?", "id": "Perkelahian." },
  { "en": "Pancalogi?", "id": "Lima prinsip." },
  { "en": "Pancalomba?", "id": "Lomba lima cabang." },
  { "en": "Pancamarga?", "id": "Lima jalan." },
  { "en": "Pancamuka?", "id": "Bermuka lima." },
  { "en": "Pancang?", "id": "Tiang." },
  { "en": "Pancar?", "id": "Sembur." },
  { "en": "Pancaragam?", "id": "Beraneka ragam." },
  { "en": "Pancaroba?", "id": "Masa peralihan." },
  { "en": "Pancasila?", "id": "Dasar negara Indonesia." },
  { "en": "Pancasona?", "id": "Ajian sakti." },
  { "en": "Pancasuara?", "id": "Lima suara." },
  { "en": "Pancat?", "id": "Pancut." },
  { "en": "Pancawalikrama?", "id": "Upacara Bali." },
  { "en": "Pancawarsa?", "id": "Lima tahun." },
  { "en": "Panci?", "id": "Wadah masak." },
  { "en": "Pancing?", "id": "Alat tangkap ikan." },
  { "en": "Pancir?", "id": "Sisa (arkais)." },
  { "en": "Pancit?", "id": "Pancut." },
  { "en": "Pancol?", "id": "Curam (arkais)." },
  { "en": "Pancong?", "id": "Kue." },
  { "en": "Pancung?", "id": "Penggal." },
  { "en": "Pancur?", "id": "Alir." },
  { "en": "Pancut?", "id": "Sembur." },
  { "en": "Pandai?", "id": "Cerdas." },
  { "en": "Pandak?", "id": "Pendek." },
  { "en": "Pandam?", "id": "Obor." },
  { "en": "Pandan?", "id": "Tanaman." },
  { "en": "Pandang?", "id": "Lihat." },
  { "en": "Pandau?", "id": "Pucat (arkais)." },
  { "en": "Pandawa?", "id": "Lima bersaudara (wayang)." },
  { "en": "Pandemi?", "id": "Wabah luas." },
  { "en": "Pandemik?", "id": "Pandemi." },
  { "en": "Pandialektal?", "id": "Semua dialek." },
  { "en": "Pandit?", "id": "Pendeta Hindu." },
  { "en": "Pandita?", "id": "Pendeta." },
  { "en": "Pandom?", "id": "Kompas (Belanda)." },
  { "en": "Pandora?", "id": "Kotak mitologi." },
  { "en": "Pandu?", "id": "Penunjuk jalan." }

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
            }, 4000);
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
