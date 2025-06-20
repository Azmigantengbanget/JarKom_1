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


  { "en": "Kumpulan komputer terhubung?", "id": "Jaringan komputer." },
  { "en": "Jaringan dalam satu gedung?", "id": "LAN (Local Area Network)." },
  { "en": "Jaringan antar kota/negara?", "id": "WAN (Wide Area Network)." },
  { "en": "Jaringan dalam area metropolitan?", "id": "MAN (Metropolitan Area Network)." },
  { "en": "Komputer penyedia layanan?", "id": "Server." },
  { "en": "Komputer pengguna layanan?", "id": "Client." },
  { "en": "Setiap perangkat di jaringan?", "id": "Node atau Host." },
  { "en": "Aturan komunikasi di jaringan?", "id": "Protokol." },
  { "en": "Model referensi jaringan 7 lapis?", "id": "Model OSI." },
  { "en": "Model referensi internet 4 lapis?", "id": "Model TCP/IP." },
  { "en": "Alamat fisik sebuah perangkat?", "id": "MAC Address." },
  { "en": "Alamat logika sebuah perangkat?", "id": "IP Address." },
  { "en": "Perangkat penghubung banyak komputer?", "id": "Switch atau Hub." },
  { "en": "Perangkat penghubung jaringan berbeda?", "id": "Router." },
  { "en": "Kartu untuk terhubung ke jaringan?", "id": "NIC (Network Interface Card)." },
  { "en": "Protokol untuk Browse web?", "id": "HTTP atau HTTPS." },
  { "en": "Protokol transfer file?", "id": "FTP (File Transfer Protocol)." },
  { "en": "Protokol pengiriman email?", "id": "SMTP (Simple Mail Transfer Protocol)." },
  { "en": "Protokol penerimaan email?", "id": "POP3 atau IMAP." },
  { "en": "Protokol pengalamatan IP otomatis?", "id": "DHCP." },
  { "en": "Penerjemah nama domain ke IP?", "id": "DNS (Domain Name System)." },
  { "en": "Unit data kecil di jaringan?", "id": "Paket (Packet)." },
  { "en": "Kabel jaringan paling umum?", "id": "UTP (Unshielded Twisted Pair)." },
  { "en": "Kabel jaringan dari serat kaca?", "id": "Fiber Optik." },
  { "en": "Jaringan tanpa kabel?", "id": "WLAN (Wireless LAN)." },
  { "en": "Perangkat pemancar sinyal nirkabel?", "id": "Access Point (AP)." },
  { "en": "Bentuk susunan fisik jaringan?", "id": "Topologi." },
  { "en": "Topologi dengan hub/switch pusat?", "id": "Topologi Star (Bintang)." },
  { "en": "Topologi dengan satu kabel utama?", "id": "Topologi Bus." },
  { "en": "Topologi membentuk lingkaran tertutup?", "id": "Topologi Ring (Cincin)." },
  { "en": "Topologi paling redundan?", "id": "Topologi Mesh." },
  { "en": "Versi IP Address yang umum?", "id": "IPv4." },
  { "en": "Versi IP Address yang baru?", "id": "IPv6." },
  { "en": "Jumlah bit pada alamat IPv4?", "id": "32 bit." },
  { "en": "Jumlah bit pada alamat IPv6?", "id": "128 bit." },
  { "en": "Pemisah jaringan dan host?", "id": "Subnet Mask." },
  { "en": "Gerbang keluar dari jaringan lokal?", "id": "Default Gateway." },
  { "en": "Protokol koneksi yang andal?", "id": "TCP (Transmission Control Protocol)." },
  { "en": "Protokol koneksi yang cepat?", "id": "UDP (User Datagram Protocol)." },
  { "en": "Proses jabat tangan tiga arah?", "id": "Three-Way Handshake TCP." },
  { "en": "Lapisan terbawah model OSI?", "id": "Physical Layer (Lapisan 1)." },
  { "en": "Lapisan kedua model OSI?", "id": "Data Link Layer (Lapisan 2)." },
  { "en": "Lapisan ketiga model OSI?", "id": "Network Layer (Lapisan 3)." },
  { "en": "Lapisan keempat model OSI?", "id": "Transport Layer (Lapisan 4)." },
  { "en": "Lapisan kelima model OSI?", "id": "Session Layer (Lapisan 5)." },
  { "en": "Lapisan keenam model OSI?", "id": "Presentation Layer (Lapisan 6)." },
  { "en": "Lapisan teratas model OSI?", "id": "Application Layer (Lapisan 7)." },
  { "en": "Fungsi lapisan Physical?", "id": "Mengirim bit melalui media." },
  { "en": "Fungsi lapisan Data Link?", "id": "Mengelola frame dan MAC address." },
  { "en": "Fungsi lapisan Network?", "id": "Mengelola routing dan IP address." },
  { "en": "Fungsi lapisan Transport?", "id": "Mengelola koneksi end-to-end." },
  { "en": "Fungsi lapisan Session?", "id": "Mengelola sesi komunikasi." },
  { "en": "Fungsi lapisan Presentation?", "id": "Enkripsi dan format data." },
  { "en": "Fungsi lapisan Application?", "id": "Antarmuka pengguna ke jaringan." },
  { "en": "Perangkat OSI Layer 1?", "id": "Hub, Repeater, Kabel." },
  { "en": "Perangkat OSI Layer 2?", "id": "Switch, Bridge, NIC." },
  { "en": "Perangkat OSI Layer 3?", "id": "Router." },
  { "en": "Data di Layer 2 disebut?", "id": "Frame." },
  { "en": "Data di Layer 3 disebut?", "id": "Packet." },
  { "en": "Data di Layer 4 disebut?", "id": "Segment (TCP), Datagram (UDP)." },
  { "en": "Kapasitas maksimal transfer data?", "id": "Bandwidth." },
  { "en": "Waktu tunda pengiriman data?", "id": "Latency (Latensi)." },
  { "en": "Alat uji konektivitas jaringan?", "id": "Ping." },
  { "en": "Alat melacak rute paket?", "id": "Traceroute (tracert)." },
  { "en": "Perangkat usang pengganti switch?", "id": "Hub." },
  { "en": "Perbedaan utama Hub dan Switch?", "id": "Switch lebih pintar (cerdas)." },
  { "en": "Hub mengirim data ke?", "id": "Semua port." },
  { "en": "Switch mengirim data ke?", "id": "Port tujuan saja." },
  { "en": "Tabrakan data di jaringan?", "id": "Collision." },
  { "en": "Area potensi tabrakan data?", "id": "Collision Domain." },
  { "en": "Area jangkauan siaran data?", "id": "Broadcast Domain." },
  { "en": "Switch memperkecil domain apa?", "id": "Collision Domain." },
  { "en": "Router memperkecil domain apa?", "id": "Broadcast Domain." },
  { "en": "Perangkat penguat sinyal jaringan?", "id": "Repeater." },
  { "en": "Jaringan pribadi di internet publik?", "id": "VPN (Virtual Private Network)." },
  { "en": "Pencegah akses tidak sah?", "id": "Firewall." },
  { "en": "Server penyimpan cache web?", "id": "Proxy Server." },
  { "en": "Standar untuk jaringan nirkabel?", "id": "IEEE 802.11." },
  { "en": "Nama lain IEEE 802.11?", "id": "Wi-Fi." },
  { "en": "Standar untuk jaringan kabel LAN?", "id": "IEEE 802.3 (Ethernet)." },
  { "en": "Konektor kabel UTP?", "id": "RJ-45." },
  { "en": "Kabel UTP untuk menghubungkan perangkat beda?", "id": "Straight-through." },
  { "en": "Kabel UTP untuk menghubungkan perangkat sama?", "id": "Cross-over." },
  { "en": "Kabel Fiber Optik single-mode untuk?", "id": "Jarak jauh." },
  { "en": "Kabel Fiber Optik multi-mode untuk?", "id": "Jarak pendek." },
  { "en": "IP untuk localhost?", "id": "127.0.0.1." },
  { "en": "Jaringan antar perangkat pribadi?", "id": "PAN (Personal Area Network)." },
  { "en": "Teknologi PAN nirkabel?", "id": "Bluetooth." },
  { "en": "Proses membungkus data dengan header?", "id": "Enkapsulasi." },
  { "en": "Proses membuka bungkusan data?", "id": "Dekapsulasi." },
  { "en": "Alamat MAC terdiri dari?", "id": "48 bit." },
  { "en": "Format alamat MAC?", "id": "Heksadesimal." },
  { "en": "Siapa yang menetapkan MAC Address?", "id": "Pabrikan perangkat keras." },
  { "en": "IP address yang berubah-ubah?", "id": "IP Dinamis." },
  { "en": "IP address yang tetap?", "id": "IP Statis." },
  { "en": "IP publik dapat diakses dari?", "id": "Internet." },
  { "en": "IP privat digunakan di?", "id": "Jaringan lokal (LAN)." },
  { "en": "Penerjemah IP privat ke publik?", "id": "NAT (Network Address Translation)." },
  { "en": "Tipe transmisi satu ke satu?", "id": "Unicast." },
  { "en": "Tipe transmisi satu ke semua?", "id": "Broadcast." },
  { "en": "Tipe transmisi satu ke grup?", "id": "Multicast." },
  { "en": "Protokol pencari MAC address dari IP?", "id": "ARP (Address Resolution Protocol)." },
  { "en": "Isi dari tabel ARP?", "id": "Pasangan alamat IP dan MAC." },
  { "en": "Kebalikan dari ARP?", "id": "RARP atau Reverse ARP." },
  { "en": "Protokol untuk pesan error jaringan?", "id": "ICMP (Internet Control Message Protocol)." },
  { "en": "Perintah 'ping' menggunakan protokol?", "id": "ICMP." },
  { "en": "Pintu komunikasi pada level transport?", "id": "Port." },
  { "en": "Rentang port 0-1023?", "id": "Well-known ports." },
  { "en": "Port untuk HTTP?", "id": "Port 80." },
  { "en": "Port untuk HTTPS?", "id": "Port 443." },
  { "en": "Port untuk FTP?", "id": "Port 20, 21." },
  { "en": "Port untuk DNS?", "id": "Port 53." },
  { "en": "Kombinasi alamat IP dan port?", "id": "Socket." },
  { "en": "Proses DHCP 'D'?", "id": "Discover." },
  { "en": "Proses DHCP 'O'?", "id": "Offer." },
  { "en": "Proses DHCP 'R'?", "id": "Request." },
  { "en": "Proses DHCP 'A'?", "id": "Acknowledge." },
  { "en": "Masa sewa alamat IP?", "id": "Lease time." },
  { "en": "IP otomatis jika DHCP gagal?", "id": "APIPA (Automatic Private IP Addressing)." },
  { "en": "Rentang alamat APIPA?", "id": "169.254.x.x." },
  { "en": "Standar keamanan Wi-Fi terlemah?", "id": "WEP." },
  { "en": "Standar keamanan Wi-Fi yang kuat?", "id": "WPA2 atau WPA3." },
  { "en": "Nama dari sebuah jaringan Wi-Fi?", "id": "SSID (Service Set Identifier)." },
  { "en": "Menyembunyikan nama jaringan Wi-Fi?", "id": "Hidden SSID." },
  { "en": "Mode Wi-Fi via Access Point?", "id": "Infrastructure Mode." },
  { "en": "Mode Wi-Fi langsung antar perangkat?", "id": "Ad-hoc Mode." },
  { "en": "Tabel pada router berisi jalur?", "id": "Tabel routing (Routing table)." },
  { "en": "Routing yang diatur manual?", "id": "Routing statis." },
  { "en": "Routing yang diatur otomatis?", "id": "Routing dinamis." },
  { "en": "Contoh protokol routing dinamis?", "id": "RIP, OSPF, EIGRP." },
  { "en": "Ukuran kualitas jalur routing?", "id": "Metric." },
  { "en": "Metric pada protokol RIP?", "id": "Hop count." },
  { "en": "Membagi satu LAN menjadi beberapa LAN logis?", "id": "VLAN (Virtual LAN)." },
  { "en": "Tujuan utama VLAN?", "id": "Mengurangi broadcast, meningkatkan keamanan." },
  { "en": "Port switch untuk banyak VLAN?", "id": "Trunk port." },
  { "en": "Protokol untuk trunking VLAN?", "id": "IEEE 802.1Q." },
  { "en": "Komunikasi antar VLAN membutuhkan?", "id": "Router atau Layer 3 Switch." },
  { "en": "Metode switch membaca seluruh frame?", "id": "Store-and-forward." },
  { "en": "Metode switch membaca alamat tujuan saja?", "id": "Cut-through." },
  { "en": "Jaringan internal suatu perusahaan?", "id": "Intranet." },
  { "en": "Jaringan internal untuk pihak luar?", "id": "Extranet." },
  { "en": "Proses verifikasi identitas pengguna?", "id": "Authentication (Otentikasi)." },
  { "en": "Proses pemberian hak akses?", "id": "Authorization (Otorisasi)." },
  { "en": "Proses pencatatan aktivitas pengguna?", "id": "Accounting (Akunting)." },
  { "en": "Konsep keamanan tiga proses?", "id": "AAA (Triple A)." },
  { "en": "Protokol manajemen perangkat jaringan?", "id": "SNMP (Simple Network Management Protocol)." },
  { "en": "Sinyal pelemahan seiring jarak?", "id": "Attenuation." },
  { "en": "Gangguan sinyal dari kabel lain?", "id": "Crosstalk." },
  { "en": "Sinyal acak yang tidak diinginkan?", "id": "Noise." },
  { "en": "Transmisi data satu arah?", "id": "Simplex." },
  { "en": "Contoh transmisi simplex?", "id": "Siaran televisi atau radio." },
  { "en": "Transmisi data dua arah bergantian?", "id": "Half-duplex." },
  { "en": "Contoh transmisi half-duplex?", "id": "Walkie-talkie." },
  { "en": "Transmisi data dua arah bersamaan?", "id": "Full-duplex." },
  { "en": "Contoh transmisi full-duplex?", "id": "Telepon." },
  { "en": "Tipe data DNS untuk alamat IPv4?", "id": "A record." },
  { "en": "Tipe data DNS untuk alamat IPv6?", "id": "AAAA record." },
  { "en": "Tipe data DNS untuk alias domain?", "id": "CNAME record." },
  { "en": "Tipe data DNS untuk mail server?", "id": "MX record." },
  { "en": "Tipe data DNS untuk name server?", "id": "NS record." },
  { "en": "Perintah untuk melihat konfigurasi IP?", "id": "ipconfig (Windows), ifconfig (Linux)." },
  { "en": "Perintah melihat cache DNS?", "id": "ipconfig /displaydns." },
  { "en": "Rentang IP privat kelas A?", "id": "10.0.0.0 - 10.255.255.255." },
  { "en": "Rentang IP privat kelas B?", "id": "172.16.0.0 - 172.31.255.255." },
  { "en": "Rentang IP privat kelas C?", "id": "192.168.0.0 - 192.168.255.255." },
  { "en": "Metode penulisan subnet mask ringkas?", "id": "CIDR (Classless Inter-Domain Routing)." },
  { "en": "Contoh notasi CIDR?", "id": "/24, /16, /8." },
  { "en": "Alamat IP pertama dalam subnet?", "id": "Network Address." },
  { "en": "Alamat IP terakhir dalam subnet?", "id": "Broadcast Address." },
  { "en": "Jumlah host di subnet /24?", "id": "254 host." },
  { "en": "Kegunaan perintah 'netstat'?", "id": "Melihat koneksi jaringan aktif." },
  { "en": "Protokol untuk mengamankan koneksi web?", "id": "SSL/TLS." },
  { "en": "Perbedaan utama POP3 dan IMAP?", "id": "IMAP sinkronisasi dengan server." },
  { "en": "Protokol sinkronisasi waktu jaringan?", "id": "NTP (Network Time Protocol)." },
  { "en": "Perangkat yang menghubungkan segmen LAN?", "id": "Bridge." },
  { "en": "Perbedaan Bridge dan Switch?", "id": "Switch punya lebih banyak port." },
  { "en": "Apa itu 'port mirroring'?", "id": "Menyalin trafik port ke port lain." },
  { "en": "Tujuan port mirroring?", "id": "Analisis dan monitoring jaringan." },
  { "en": "Agregasi beberapa link menjadi satu?", "id": "Link Aggregation (LAG)." },
  { "en": "Protokol untuk link aggregation?", "id": "LACP." },
  { "en": "Jaminan kualitas layanan jaringan?", "id": "QoS (Quality of Service)." },
  { "en": "Tujuan QoS?", "id": "Memprioritaskan trafik penting." },
  { "en": "Apa itu 'jitter'?", "id": "Variasi latency atau penundaan." },
  { "en": "Konektor kabel koaksial?", "id": "BNC Connector." },
  { "en": "Jenis kabel koaksial untuk TV?", "id": "RG-6." },
  { "en": "Perangkat konverter media?", "id": "Media converter." },
  { "en": "Contoh media converter?", "id": "Fiber ke Ethernet." },
  { "en": "Struktur fisik jaringan internet?", "id": "Kumpulan jaringan otonom (AS)." },
  { "en": "Protokol routing antar AS?", "id": "BGP (Border Gateway Protocol)." },
  { "en": "Organisasi pengelola nama domain?", "id": "ICANN." },
  { "en": "Pengelola alokasi IP regional?", "id": "RIR (Regional Internet Registry)." },
  { "en": "RIR untuk wilayah Asia Pasifik?", "id": "APNIC." },
  { "en": "Sistem pencegahan intrusi?", "id": "IPS (Intrusion Prevention System)." },
  { "en": "Sistem deteksi intrusi?", "id": "IDS (Intrusion Detection System)." },
  { "en": "Perbedaan IDS dan IPS?", "id": "IPS dapat memblokir ancaman." },
  { "en": "Zona aman antara jaringan internal dan eksternal?", "id": "DMZ (Demilitarized Zone)." },
  { "en": "Server yang biasa diletakkan di DMZ?", "id": "Web server, Mail server." },
  { "en": "Protokol pencegah loop di switch?", "id": "STP (Spanning Tree Protocol)." },
  { "en": "Status port STP yang aktif?", "id": "Forwarding." },
  { "en": "Status port STP yang memblokir?", "id": "Blocking." },
  { "en": "Switch utama dalam STP?", "id": "Root Bridge." },
  { "en": "Bagaimana switch mempelajari alamat MAC?", "id": "Dari source address frame masuk." },
  { "en": "Tabel alamat MAC pada switch?", "id": "MAC address table." },
  { "en": "Daya listrik melalui kabel Ethernet?", "id": "PoE (Power over Ethernet)." },
  { "en": "Perangkat yang butuh PoE?", "id": "IP Camera, Access Point." },
  { "en": "Protokol routing dalam satu AS?", "id": "IGP (Interior Gateway Protocol)." },
  { "en": "Protokol routing antar AS?", "id": "EGP (Exterior Gateway Protocol)." },
  { "en": "Contoh EGP yang digunakan?", "id": "BGP (Border Gateway Protocol)." },
  { "en": "Tipe protokol IGP?", "id": "Distance Vector, Link-State." },
  { "en": "Contoh protokol Distance Vector?", "id": "RIP (Routing Information Protocol)." },
  { "en": "Contoh protokol Link-State?", "id": "OSPF (Open Shortest Path First)." },
  { "en": "Protokol hybrid dari Cisco?", "id": "EIGRP." },
  { "en": "Batas maksimal hop pada RIP?", "id": "15 hop." },
  { "en": "Waktu yang dibutuhkan jaringan stabil?", "id": "Convergence time." },
  { "en": "Tingkat kepercayaan sebuah rute?", "id": "Administrative Distance (AD)." },
  { "en": "Struktur area pada OSPF?", "id": "Area 0 (backbone area)." },
  { "en": "Metrik yang digunakan OSPF?", "id": "Cost (berdasarkan bandwidth)." },
  { "en": "Aturan pemfilteran paket di router?", "id": "ACL (Access Control List)." },
  { "en": "ACL yang hanya filter IP sumber?", "id": "ACL Standar." },
  { "en": "ACL yang lebih detail?", "id": "ACL Extended." },
  { "en": "Fitur keamanan pada port switch?", "id": "Port Security." },
  { "en": "Serangan membanjiri server?", "id": "DoS (Denial of Service)." },
  { "en": "Serangan DoS dari banyak sumber?", "id": "DDoS." },
  { "en": "Serangan menyadap di tengah komunikasi?", "id": "Man-in-the-Middle (MitM)." },
  { "en": "Serangan memalsukan alamat IP?", "id": "IP Spoofing." },
  { "en": "Serangan memalsukan alamat MAC?", "id": "MAC Spoofing." },
  { "en": "Enkripsi menggunakan satu kunci?", "id": "Enkripsi Simetris." },
  { "en": "Enkripsi menggunakan dua kunci?", "id": "Enkripsi Asimetris." },
  { "en": "Kunci pada enkripsi asimetris?", "id": "Kunci publik dan privat." },
  { "en": "Fungsi Hashing pada data?", "id": "Menjaga integritas data." },
  { "en": "Contoh algoritma hashing?", "id": "MD5, SHA-256." },
  { "en": "Sertifikat digital dikeluarkan oleh?", "id": "Certificate Authority (CA)." },
  { "en": "Jaringan virtual di atas fisik?", "id": "Overlay network." },
  { "en": "Pemisahan control plane dan data plane?", "id": "SDN (Software-Defined Networking)." },
  { "en": "Otak dari jaringan SDN?", "id": "SDN Controller." },
  { "en": "Software untuk membuat mesin virtual?", "id": "Hypervisor." },
  { "en": "Contoh hypervisor tipe 1?", "id": "VMware ESXi, Xen." },
  { "en": "Contoh hypervisor tipe 2?", "id": "VirtualBox, VMware Workstation." },
  { "en": "Switch yang beroperasi secara virtual?", "id": "Virtual Switch (vSwitch)." },
  { "en": "Model layanan cloud infrastruktur?", "id": "IaaS (Infrastructure as a Service)." },
  { "en": "Model layanan cloud platform?", "id": "PaaS (Platform as a Service)." },
  { "en": "Model layanan cloud software?", "id": "SaaS (Software as a Service)." },
  { "en": "Contoh layanan IaaS?", "id": "Amazon Web Services (AWS) EC2." },
  { "en": "Contoh layanan SaaS?", "id": "Gmail, Microsoft 365." },
  { "en": "Teknologi WAN berbasis label?", "id": "MPLS (Multiprotocol Label Switching)." },
  { "en": "Fungsi MPLS?", "id": "Mempercepat penerusan paket." },
  { "en": "Koneksi WAN privat khusus?", "id": "Leased Line." },
  { "en": "Perangkat di sisi pelanggan WAN?", "id": "CPE (Customer Premises Equipment)." },
  { "en": "Perangkat terminasi sirkuit WAN?", "id": "CSU/DSU." },
  { "en": "Internet via kabel telepon?", "id": "DSL (Digital Subscriber Line)." },
  { "en": "Internet via kabel TV?", "id": "Cable Internet." },
  { "en": "Internet serat optik ke rumah?", "id": "FTTH (Fiber to the Home)." },
  { "en": "Alamat IPv6 yang diawali FE80::?", "id": "Link-Local Address." },
  { "en": "Fungsi alamat Link-Local?", "id": "Komunikasi di segmen lokal." },
  { "en": "Alamat IPv6 yang bisa dirutekan global?", "id": "Global Unicast Address (GUA)." },
  { "en": "Aturan menyingkat blok nol di IPv6?", "id": "Gunakan dua titik dua (::)." },
  { "en": "Berapa kali '::' bisa digunakan?", "id": "Hanya satu kali." },
  { "en": "Konfigurasi IP otomatis di IPv6?", "id": "SLAAC." },
  { "en": "Protokol pengganti ARP di IPv6?", "id": "NDP (Neighbor Discovery Protocol)." },
  { "en": "Teknik tunneling IPv6 di jaringan IPv4?", "id": "6to4, Teredo." },
  { "en": "Representasi antarmuka 64-bit di IPv6?", "id": "EUI-64." },
  { "en": "Server yang menangani banyak domain?", "id": "Virtual Hosting." },
  { "en": "Kumpulan server bekerja sebagai satu?", "id": "Cluster." },
  { "en": "Tujuan utama clustering?", "id": "High availability, load balancing." },
  { "en": "Distribusi beban trafik ke server?", "id": "Load Balancing." },
  { "en": "Peralatan fisik jaringan komputer?", "id": "Perangkat keras (Hardware)." },
  { "en": "Program dan sistem operasi jaringan?", "id": "Perangkat lunak (Software)." },
  { "en": "Komputer yang sangat kuat untuk komputasi?", "id": "Mainframe." },
  { "en": "Jaringan penyimpanan data khusus?", "id": "SAN (Storage Area Network)." },
  { "en": "Protokol untuk SAN?", "id": "Fibre Channel, iSCSI." },
  { "en": "Penyimpanan data terpusat di jaringan?", "id": "NAS (Network-Attached Storage)." },
  { "en": "Perbedaan NAS dan SAN?", "id": "NAS berbasis file, SAN berbasis blok." },
  { "en": "Mekanisme pemulihan dari bencana?", "id": "Disaster Recovery Plan (DRP)." },
  { "en": "Situs cadangan jika terjadi bencana?", "id": "Disaster Recovery Site." },
  { "en": "Pencadangan data secara berkala?", "id": "Data Backup." },
  { "en": "Pencadangan penuh semua data?", "id": "Full backup." },
  { "en": "Pencadangan data yang berubah saja?", "id": "Incremental/Differential backup." },
  { "en": "Terminal 'bodoh' yang bergantung server?", "id": "Dumb Terminal." },
  { "en": "Arsitektur jaringan tanpa server pusat?", "id": "Peer-to-Peer (P2P)." },
  { "en": "Arsitektur jaringan dengan server pusat?", "id": "Client-Server." },
  { "en": "Standar pemasangan kabel terstruktur?", "id": "TIA/EIA-568." },
  { "en": "Urutan kabel T568A?", "id": "Putih-Hijau, Hijau..." },
  { "en": "Urutan kabel T568B?", "id": "Putih-Oranye, Oranye..." },
  { "en": "Kabel straight menggunakan urutan?", "id": "Kedua ujung sama (misal T568B)." },
  { "en": "Kabel cross menggunakan urutan?", "id": "Satu ujung T568A, satu T568B." },
  { "en": "Kabel untuk menghubungkan PC ke Switch?", "id": "Kabel straight." },
  { "en": "Kabel untuk menghubungkan PC ke PC?", "id": "Kabel cross." },
  { "en": "Kabel untuk konfigurasi router?", "id": "Kabel console (rollover)." },
  { "en": "Panjang maksimal kabel UTP?", "id": "100 meter." },
  { "en": "Standar Ethernet kecepatan 1 Gbps?", "id": "Gigabit Ethernet (1000BASE-T)." },
  { "en": "Standar Ethernet kecepatan 10 Gbps?", "id": "10 Gigabit Ethernet." },
  { "en": "Alat untuk menguji kabel jaringan?", "id": "LAN Tester." },
  { "en": "Alat untuk memasang konektor RJ-45?", "id": "Crimping tool." },
  { "en": "Standar Wi-Fi 4?", "id": "802.11n." },
  { "en": "Standar Wi-Fi 5?", "id": "802.11ac." },
  { "en": "Standar Wi-Fi 6?", "id": "802.11ax." },
  { "en": "Frekuensi kerja 802.11n?", "id": "2.4 GHz dan 5 GHz." },
  { "en": "Frekuensi kerja 802.11ac?", "id": "5 GHz." },
  { "en": "Teknologi multiple antena di Wi-Fi?", "id": "MIMO (Multiple-Input Multiple-Output)." },
  { "en": "Teknologi efisiensi di Wi-Fi 6?", "id": "OFDMA." },
  { "en": "Metode Wi-Fi menghindari tabrakan?", "id": "CSMA/CA." },
  { "en": "Server otentikasi terpusat?", "id": "RADIUS." },
  { "en": "Standar otentikasi berbasis port?", "id": "IEEE 802.1X." },
  { "en": "Pemetaan kekuatan sinyal Wi-Fi?", "id": "Wi-Fi heat map." },
  { "en": "Proses survey sinyal nirkabel?", "id": "Wireless site survey." },
  { "en": "Interferensi dari perangkat non-Wi-Fi?", "id": "Non-Wi-Fi interference." },
  { "en": "Contoh sumber interferensi 2.4 GHz?", "id": "Microwave oven, Bluetooth." },
  { "en": "Lebar kanal Wi-Fi standar?", "id": "20 MHz." },
  { "en": "Penggabungan kanal untuk kecepatan lebih?", "id": "Channel bonding." },
  { "en": "Sistem distribusi nirkabel?", "id": "WDS (Wireless Distribution System)." },
  { "en": "Jaringan Wi-Fi dengan banyak node?", "id": "Wireless Mesh Network." },
  { "en": "Database objek manajemen SNMP?", "id": "MIB (Management Information Base)." },
  { "en": "Identifier unik untuk objek MIB?", "id": "OID (Object Identifier)." },
  { "en": "Perintah SNMP untuk mengambil data?", "id": "SNMP Get." },
  { "en": "Notifikasi spontan dari agen SNMP?", "id": "SNMP Trap." },
  { "en": "Protokol monitoring aliran trafik Cisco?", "id": "NetFlow." },
  { "en": "Standar logging pesan sistem?", "id": "Syslog." },
  { "en": "Tingkat keparahan pesan syslog?", "id": "Severity level." },
  { "en": "Telepon melalui jaringan IP?", "id": "VoIP (Voice over IP)." },
  { "en": "Protokol sinyal untuk memulai sesi VoIP?", "id": "SIP (Session Initiation Protocol)." },
  { "en": "Protokol untuk mengirim media (suara/video)?", "id": "RTP (Real-time Transport Protocol)." },
  { "en": "Algoritma kompresi-dekompresi suara/video?", "id": "Codec." },
  { "en": "Contoh codec suara?", "id": "G.711, G.729, Opus." },
  { "en": "Penyangga untuk mengatasi jitter?", "id": "Jitter buffer." },
  { "en": "Kualitas panggilan VoIP diukur dengan?", "id": "MOS (Mean Opinion Score)." },
  { "en": "Manajemen jaringan menggunakan software?", "id": "Network automation." },
  { "en": "Antarmuka program untuk aplikasi?", "id": "API (Application Programming Interface)." },
  { "en": "API umum untuk layanan web?", "id": "REST API." },
  { "en": "Format data yang mudah dibaca manusia?", "id": "JSON, YAML." },
  { "en": "Mengelola infrastruktur sebagai kode?", "id": "IaC (Infrastructure as Code)." },
  { "en": "Contoh alat otomasi jaringan?", "id": "Ansible, Puppet, Chef." },
  { "en": "Firewall yang melacak status koneksi?", "id": "Stateful firewall." },
  { "en": "Firewall yang tidak melacak koneksi?", "id": "Stateless firewall." },
  { "en": "Firewall modern dengan inspeksi aplikasi?", "id": "NGFW (Next-Generation Firewall)." },
  { "en": "Perlindungan terhadap aplikasi web?", "id": "WAF (Web Application Firewall)." },
  { "en": "Sistem perangkap untuk penyerang?", "id": "Honeypot." },
  { "en": "Manajemen log dan event keamanan terpusat?", "id": "SIEM." },
  { "en": "Keamanan pada perangkat akhir pengguna?", "id": "Endpoint security." },
  { "en": "Membatasi akses berdasarkan MAC address?", "id": "MAC filtering." },
  { "en": "Jaringan terisolasi untuk menguji malware?", "id": "Sandbox." },
  { "en": "Panel terminasi untuk kabel jaringan?", "id": "Patch panel." },
  { "en": "Sistem pengkabelan yang terorganisir?", "id": "Structured cabling." },
  { "en": "Kabel antara patch panel dan outlet?", "id": "Horizontal cabling." },
  { "en": "Kabel utama antar lantai atau gedung?", "id": "Backbone cabling." },
  { "en": "Ruang tempat peralatan jaringan utama?", "id": "Main Distribution Frame (MDF)." },
  { "en": "Ruang distribusi per lantai?", "id": "Intermediate Distribution Frame (IDF)." },
  { "en": "Standar tingkat keandalan data center?", "id": "Tier Standard (I, II, III, IV)." },
  { "en": "Data center dengan redundansi tertinggi?", "id": "Tier IV." },
  { "en": "Penyedia daya darurat dari baterai?", "id": "UPS (Uninterruptible Power Supply)." },
  { "en": "Unit distribusi daya di rak?", "id": "PDU (Power Distribution Unit)." },
  { "en": "Metode troubleshooting dari layer atas?", "id": "Top-down approach." },
  { "en": "Metode troubleshooting dari layer bawah?", "id": "Bottom-up approach." },
  { "en": "Dokumentasi topologi fisik jaringan?", "id": "Physical network diagram." },
  { "en": "Dokumentasi topologi logis jaringan?", "id": "Logical network diagram." },
  { "en": "Proses manajemen perubahan jaringan?", "id": "Change management." },
  { "en": "Analisa paket data jaringan?", "id": "Packet sniffing/analysis." },
  { "en": "Alat populer untuk packet sniffing?", "id": "Wireshark." },
  { "en": "Mode NIC untuk menangkap semua trafik?", "id": "Promiscuous mode." },
  { "en": "Pengiriman paket dengan IP sumber palsu?", "id": "IP spoofing." },
  { "en": "Serangan terhadap cache ARP?", "id": "ARP poisoning/spoofing." },
  { "en": "Serangan membanjiri tabel MAC switch?", "id": "MAC flooding." },
  { "en": "Serangan membajak sesi pengguna?", "id": "Session hijacking." },
  { "en": "Serangan rekayasa sosial via email?", "id": "Phishing." },
  { "en": "Phishing yang menargetkan individu tertentu?", "id": "Spear phishing." },
  { "en": "Malware yang menyandera data?", "id": "Ransomware." },
  { "en": "Malware yang menyamar program berguna?", "id": "Trojan horse." },
  { "en": "Malware yang mereplikasi diri?", "id": "Worm." },
  { "en": "Keamanan fisik untuk ruang server?", "id": "Akses terbatas, CCTV, pendingin." },
  { "en": "Proses identifikasi aset dan risiko?", "id": "Risk assessment." },
  { "en": "Kebijakan penggunaan jaringan yang diizinkan?", "id": "Acceptable Use Policy (AUP)." },
  { "en": "Teknologi 'single sign-on' (SSO)?", "id": "Satu login untuk banyak aplikasi." },
  { "en": "Otentikasi dengan lebih dari satu faktor?", "id": "Multi-Factor Authentication (MFA)." },
  { "en": "Contoh faktor otentikasi?", "id": "Password, sidik jari, token." },
  { "en": "Sinyal yang melintasi batas segmen?", "id": "Hop." },
  { "en": "Jaringan pengiriman konten global?", "id": "CDN (Content Delivery Network)." },
  { "en": "Tujuan CDN?", "id": "Mempercepat pengiriman konten." },
  { "en": "Bagaimana CDN bekerja?", "id": "Menyimpan cache di server terdekat." },
  { "en": "Koneksi langsung antar dua jaringan?", "id": "Peering." },
  { "en": "Tempat pertukaran trafik internet?", "id": "IXP (Internet Exchange Point)." },
  { "en": "Jaringan yang bisa pulih sendiri?", "id": "Self-healing network." },
  { "en": "IP Address yang tidak dirutekan di internet?", "id": "Private IP address." },
  { "en": "Apa itu 'air gap'?", "id": "Isolasi fisik jaringan total." },
  { "en": "Jaringan untuk perangkat IoT?", "id": "IoT Network." },
  { "en": "Protokol ringan untuk IoT?", "id": "MQTT, CoAP." },
  { "en": "Jaringan area luas daya rendah?", "id": "LPWAN (Low-Power Wide-Area Network)." },
  { "en": "Contoh teknologi LPWAN?", "id": "LoRaWAN, NB-IoT." },
  { "en": "Titik demarkasi jaringan provider?", "id": "Demarcation point." },
  { "en": "Kabel dari demarc ke CPE?", "id": "Local loop." },
  { "en": "Arsitektur jaringan data center modern?", "id": "Spine-Leaf." },
  { "en": "Tujuan arsitektur Spine-Leaf?", "id": "Skalabilitas dan latensi rendah." },
  { "en": "Switch di atas setiap rak?", "id": "Top-of-Rack (ToR) switch." },
  { "en": "Teknologi overlay untuk perluasan VLAN?", "id": "VXLAN." },
  { "en": "Menjalankan protokol Fibre Channel di Ethernet?", "id": "FCoE (Fibre Channel over Ethernet)." },
  { "en": "Protokol redundansi gateway default?", "id": "FHRP (First Hop Redundancy Protocol)." },
  { "en": "Contoh FHRP milik Cisco?", "id": "HSRP (Hot Standby Router Protocol)." },
  { "en": "Standar terbuka untuk FHRP?", "id": "VRRP (Virtual Router Redundancy Protocol)." },
  { "en": "FHRP yang melakukan load balancing?", "id": "GLBP (Gateway Load Balancing Protocol)." },
  { "en": "Routing berdasarkan kebijakan kustom?", "id": "PBR (Policy-Based Routing)." },
  { "en": "Protokol klien untuk join grup multicast?", "id": "IGMP (Internet Group Management Protocol)." },
  { "en": "Fungsi IGMP Snooping pada switch?", "id": "Meneruskan multicast ke port terdaftar." },
  { "en": "Protokol routing untuk trafik multicast?", "id": "PIM (Protocol Independent Multicast)." },
  { "en": "Protokol routing inti di internet?", "id": "BGP (Border Gateway Protocol)." },
  { "en": "BGP yang berjalan dalam satu AS?", "id": "iBGP (internal BGP)." },
  { "en": "BGP yang berjalan antar AS berbeda?", "id": "eBGP (external BGP)." },
  { "en": "Atribut BGP berisi daftar AS?", "id": "AS_PATH." },
  { "en": "Tujuan atribut AS_PATH?", "id": "Mencegah routing loop." },
  { "en": "Alat BGP untuk menyederhanakan mesh iBGP?", "id": "Route Reflector." },
  { "en": "Jenis iklan pada OSPF?", "id": "LSA (Link-State Advertisement)." },
  { "en": "Router OSPF yang menghubungkan ke jaringan eksternal?", "id": "ASBR (Autonomous System Boundary Router)." },
  { "en": "Router OSPF yang menghubungkan antar area?", "id": "ABR (Area Border Router)." },
  { "en": "Tujuan pemilihan DR/BDR di OSPF?", "id": "Mengurangi trafik update OSPF." },
  { "en": "Tipe jaringan OSPF default di Ethernet?", "id": "Broadcast." },
  { "en": "Kecepatan transfer data aktual?", "id": "Goodput." },
  { "en": "Mekanisme kontrol aliran data TCP?", "id": "Sliding Window." },
  { "en": "Ukuran jendela TCP dinamis?", "id": "TCP Window Scaling." },
  { "en": "Mekanisme TCP menghindari kemacetan?", "id": "Congestion Control." },
  { "en": "Waktu bolak-balik paket data?", "id": "RTT (Round Trip Time)." },
  { "en": "Paket yang hilang dan perlu dikirim ulang?", "id": "Packet Loss/Retransmission." },
  { "en": "Teknologi VPN berbasis standar IP?", "id": "IPsec." },
  { "en": "Dua mode operasi IPsec?", "id": "Tunnel mode dan Transport mode." },
  { "en": "Protokol IPsec untuk enkripsi data?", "id": "ESP (Encapsulating Security Payload)." },
  { "en": "Protokol IPsec untuk otentikasi header?", "id": "AH (Authentication Header)." },
  { "en": "Kerangka kerja manajemen kunci IPsec?", "id": "IKE (Internet Key Exchange)." },
  { "en": "VPN yang diakses via browser?", "id": "SSL VPN." },
  { "en": "Teknologi untuk membuat tunnel generik?", "id": "GRE (Generic Routing Encapsulation)." },
  { "en": "Model keamanan 'jangan percaya, selalu verifikasi'?", "id": "Zero Trust Architecture." },
  { "en": "Broker keamanan untuk akses cloud?", "id": "CASB (Cloud Access Security Broker)." },
  { "en": "Proses menjaga integritas bukti digital?", "id": "Chain of Custody." },
  { "en": "Informasi tentang ancaman siber?", "id": "Threat Intelligence." },
  { "en": "Tipe switching WAN zaman dulu?", "id": "Circuit switching." },
  { "en": "Tipe switching WAN modern?", "id": "Packet switching." },
  { "en": "Standar jaringan optik kecepatan tinggi?", "id": "SONET/SDH." },
  { "en": "Teknologi mengirim banyak panjang gelombang cahaya?", "id": "DWDM." },
  { "en": "Metode transisi IPv6 menjalankan keduanya?", "id": "Dual Stack." },
  { "en": "Metode transisi membungkus IPv6 dalam IPv4?", "id": "Tunneling." },
  { "en": "Metode transisi menerjemahkan IPv6 ke IPv4?", "id": "Translation (NAT64)." },
  { "en": "Alamat IPv6 unik untuk komunikasi lokal?", "id": "Unique Local Address (ULA)." },
  { "en": "Alamat IPv6 untuk satu-ke-terdekat?", "id": "Anycast." },
  { "en": "DHCPv6 yang tidak melacak state klien?", "id": "Stateless DHCPv6." },
  { "en": "DHCPv6 yang melacak state klien?", "id": "Stateful DHCPv6." },
  { "en": "Opsi pada NDP untuk informasi tambahan?", "id": "RA (Router Advertisement) options." },
  { "en": "Layanan direktori berbasis standar X.500?", "id": "Directory Services." },
  { "en": "Contoh implementasi directory service?", "id": "Active Directory." },
  { "en": "Protokol akses direktori ringan?", "id": "LDAP." },
  { "en": "Protokol otentikasi jaringan dari MIT?", "id": "Kerberos." },
  { "en": "Tiket dalam sistem Kerberos?", "id": "TGT (Ticket-Granting Ticket)." },
  { "en": "Jaringan pengiriman daya listrik?", "id": "Power grid." },
  { "en": "Komunikasi melalui jaringan listrik?", "id": "PLC (Power-Line Communication)." },
  { "en": "Satuan throughput data?", "id": "bps, Mbps, Gbps." },
  { "en": "Representasi numerik dari sebuah karakter?", "id": "Character encoding (ASCII, Unicode)." },
  { "en": "Perbedaan antara Bandwidth dan Throughput?", "id": "Kapasitas vs kecepatan aktual." },
  { "en": "Faktor yang mempengaruhi throughput?", "id": "Latency, packet loss, congestion." },
  { "en": "Kemampuan sistem menangani beban meningkat?", "id": "Scalability (Skalabilitas)." },
  { "en": "Kemampuan sistem untuk tetap beroperasi?", "id": "Reliability (Keandalan)." },
  { "en": "Ukuran waktu sistem tersedia?", "id": "Availability (Ketersediaan)." },
  { "en": "Contoh availability tinggi?", "id": "99.999% (five nines)." },
  { "en": "Menghilangkan satu titik kegagalan?", "id": "Redundancy (Redundansi)." },
  { "en": "Kemampuan jaringan pulih dari kegagalan?", "id": "Fault tolerance." },
  { "en": "Peralatan cadangan yang siaga?", "id": "Hot standby." },
  { "en": "Peralatan cadangan yang perlu diaktifkan?", "id": "Cold standby." },
  { "en": "Perjanjian tingkat layanan?", "id": "SLA (Service Level Agreement)." },
  { "en": "Tujuan dari SLA?", "id": "Menetapkan ekspektasi layanan." },
  { "en": "Alat simulasi jaringan?", "id": "Network simulator." },
  { "en": "Contoh network simulator?", "id": "GNS3, Cisco Packet Tracer." },
  { "en": "Jaringan virtual dalam skala besar?", "id": "Network virtualization." },
  { "en": "Fungsi jaringan yang divirtualisasi?", "id": "NFV (Network Functions Virtualization)." },
  { "en": "Contoh fungsi NFV?", "id": "Router, firewall virtual." },
  { "en": "Koneksi nirkabel jarak dekat?", "id": "NFC (Near Field Communication)." },
  { "en": "Jaringan nirkabel untuk sensor?", "id": "WSN (Wireless Sensor Network)." },
  { "en": "Protokol populer untuk WSN?", "id": "Zigbee, Z-Wave." },
  { "en": "Komputasi di tepi jaringan?", "id": "Edge computing." },
  { "en": "Tujuan edge computing?", "id": "Mengurangi latency, menghemat bandwidth." },
  { "en": "Komputasi di dekat perangkat IoT?", "id": "Fog computing." },
  { "en": "Sistem kontrol untuk proses industri?", "id": "SCADA." },
  { "en": "Header yang ditambahkan pada enkapsulasi?", "id": "PDU Header." },
  { "en": "Trailer yang ditambahkan pada enkapsulasi?", "id": "PDU Trailer." },
  { "en": "Bagian dari trailer frame Ethernet?", "id": "FCS (Frame Check Sequence)." },
  { "en": "Fungsi FCS?", "id": "Pengecekan kesalahan (error detection)." },
  { "en": "Metode pengecekan kesalahan sederhana?", "id": "Parity check." },
  { "en": "Metode pengecekan kesalahan lebih andal?", "id": "CRC (Cyclic Redundancy Check)." },
  { "en": "Proses segmentasi data di layer transport?", "id": "Memecah data besar." },
  { "en": "Proses penyusunan kembali segmen?", "id": "Reassembly." },
  { "en": "Nomor untuk mengurutkan segmen?", "id": "Sequence number." },
  { "en": "Nomor konfirmasi penerimaan data?", "id": "Acknowledgement number (ACK)." },
  { "en": "TCP mengirim ulang data jika?", "id": "Tidak menerima ACK." },
  { "en": "Subnetting dengan panjang subnet bervariasi?", "id": "VLSM (Variable Length Subnet Masking)." },
  { "en": "Tujuan utama VLSM?", "id": "Efisiensi penggunaan alamat IP." },
  { "en": "Menggabungkan beberapa rute menjadi satu?", "id": "Route Summarization (agregasi)." },
  { "en": "Tujuan route summarization?", "id": "Mengecilkan tabel routing." },
  { "en": "Ukuran paket data maksimum?", "id": "MTU (Maximum Transmission Unit)." },
  { "en": "Proses menentukan MTU terendah di jalur?", "id": "Path MTU Discovery." },
  { "en": "Deteksi tabrakan pada jaringan Ethernet?", "id": "CSMA/CD." },
  { "en": "Frame Ethernet yang lebih besar dari standar?", "id": "Jumbo frame." },
  { "en": "Bagian awal frame Ethernet?", "id": "Preamble dan SFD." },
  { "en": "Bagian akhir frame Ethernet untuk cek eror?", "id": "FCS (Frame Check Sequence)." },
  { "en": "Protokol WAN point-to-point klasik?", "id": "PPP (Point-to-Point Protocol)." },
  { "en": "Fitur otentikasi pada PPP?", "id": "PAP, CHAP." },
  { "en": "Perintah untuk query server DNS?", "id": "nslookup atau dig." },
  { "en": "Perintah untuk melihat tabel ARP?", "id": "arp -a." },
  { "en": "Perintah untuk melihat tabel routing?", "id": "route print atau ip route." },
  { "en": "Alat serbaguna untuk koneksi TCP/UDP?", "id": "Netcat (nc)." },
  { "en": "Trafik broadcast berlebihan di jaringan?", "id": "Broadcast storm." },
  { "en": "Penyebab broadcast storm?", "id": "Looping pada switch (tanpa STP)." },
  { "en": "Jalur paket pergi dan pulang berbeda?", "id": "Asymmetric routing." },
  { "en": "Proxy yang bertindak atas nama klien?", "id": "Forward Proxy." },
  { "en": "Proxy yang bertindak atas nama server?", "id": "Reverse Proxy." },
  { "en": "Ekstensi keamanan untuk protokol DNS?", "id": "DNSSEC." },
  { "en": "Tujuan DNSSEC?", "id": "Mencegah DNS spoofing/poisoning." },
  { "en": "Level hierarki server waktu NTP?", "id": "Stratum." },
  { "en": "Server NTP paling akurat?", "id": "Stratum 0 (jam atomik)." },
  { "en": "Konektor fiber optik kecil populer?", "id": "LC (Lucent Connector)." },
  { "en": "Konektor fiber optik tipe tusuk-putar?", "id": "SC (Subscriber Connector)." },
  { "en": "Alat untuk mencari lokasi putus kabel?", "id": "TDR (Time-Domain Reflectometer)." },
  { "en": "Standar Gigabit Ethernet fiber multimode?", "id": "1000BASE-SX." },
  { "en": "Standar Gigabit Ethernet fiber single-mode?", "id": "1000BASE-LX." },
  { "en": "Serat optik yang tidak digunakan provider?", "id": "Dark fiber." },
  { "en": "Crosstalk di ujung dekat kabel?", "id": "NEXT (Near-End Crosstalk)." },
  { "en": "Crosstalk di ujung jauh kabel?", "id": "FEXT (Far-End Crosstalk)." },
  { "en": "Jaringan yang memahami 'maksud' administrator?", "id": "IBN (Intent-Based Networking)." },
  { "en": "Gabungan layanan keamanan dan SD-WAN?", "id": "SASE (Secure Access Service Edge)." },
  { "en": "Fitur utama jaringan 5G?", "id": "Kecepatan tinggi, latensi rendah." },
  { "en": "Pembagian jaringan 5G secara logis?", "id": "Network Slicing." },
  { "en": "Fitur utama Wi-Fi 6E?", "id": "Menggunakan pita frekuensi 6 GHz." },
  { "en": "Jaringan yang terhubung ke internet?", "id": "Online." },
  { "en": "Jaringan yang tidak terhubung internet?", "id": "Offline atau Air-gapped." },
  { "en": "Port sementara yang digunakan klien?", "id": "Ephemeral port." },
  { "en": "Koneksi TCP yang tidak selesai handshake?", "id": "Half-open connection." },
  { "en": "Serangan menggunakan half-open connection?", "id": "SYN flood attack." },
  { "en": "Badan yang menetapkan standar internet?", "id": "IETF (Internet Engineering Task Force)." },
  { "en": "Dokumen publikasi dari IETF?", "id": "RFC (Request for Comments)." },
  { "en": "Badan pengelola alokasi IP global?", "id": "IANA (Internet Assigned Numbers Authority)." },
  { "en": "Apa itu 'default route'?", "id": "Rute untuk tujuan tak dikenal." },
  { "en": "Notasi untuk default route?", "id": "0.0.0.0/0." },
  { "en": "Router terakhir sebelum ke internet?", "id": "Edge router." },
  { "en": "Jaringan tumpang tindih (IP sama)?", "id": "Overlapping networks." },
  { "en": "Konsep 'split-horizon' pada routing?", "id": "Mencegah routing loop." },
  { "en": "Bagaimana split-horizon bekerja?", "id": "Jangan kirim info rute kembali." },
  { "en": "Keracunan rute (route poisoning)?", "id": "Mengiklankan rute gagal (metric tak hingga)." },
  { "en": "Waktu tunggu sebelum rute dihapus?", "id": "Hold-down timer." },
  { "en": "Ukuran paket data di level aplikasi?", "id": "Payload." },
  { "en": "Kecepatan data yang dijanjikan provider?", "id": "CIR (Committed Information Rate)." },
  { "en": "Kecepatan data melebihi CIR?", "id": "Burst rate." },
  { "en": "Antrian paket pada perangkat jaringan?", "id": "Queue." },
  { "en": "Metode antrian 'pertama masuk, pertama keluar'?", "id": "FIFO (First-In, First-Out)." },
  { "en": "Metode antrian untuk prioritas trafik?", "id": "Weighted Fair Queuing (WFQ)." },
  { "en": "Membuang paket saat antrian penuh?", "id": "Tail drop." },
  { "en": "Penyebab utama bufferbloat?", "id": "Buffer antrian terlalu besar." },
  { "en": "Masalah paket terhalang paket besar?", "id": "Head-of-Line Blocking." },
  { "en": "Perangkat jaringan multiguna?", "id": "UTM (Unified Threat Management)." },
  { "en": "Apa isi dari UTM?", "id": "Firewall, IDS/IPS, Antivirus." },
  { "en": "Perangkat lunak untuk menyembunyikan IP?", "id": "Anonymous proxy." },
  { "en": "Jaringan anonimitas terenkripsi?", "id": "Tor (The Onion Router)." },
  { "en": "Kumpulan botnet dikendalikan oleh?", "id": "Botmaster atau C&C server." },
  { "en": "Titik akses nirkabel palsu?", "id": "Evil Twin." },
  { "en": "Tujuan serangan Evil Twin?", "id": "Mencuri kredensial pengguna." },
  { "en": "Serangan menurunkan tingkat keamanan koneksi?", "id": "Downgrade attack." },
  { "en": "Serangan dengan mencoba banyak password?", "id": "Brute-force attack." },
  { "en": "Serangan menggunakan daftar kata?", "id": "Dictionary attack." },
  { "en": "Menambahkan 'garam' pada hashing password?", "id": "Salting." },
  { "en": "Tujuan salting?", "id": "Melawan serangan rainbow table." },
  { "en": "Akses fisik tidak sah ke port?", "id": "Port scanning." },
  { "en": "Alat populer untuk port scanning?", "id": "Nmap." },
  { "en": "Identifikasi sistem operasi dari jarak jauh?", "id": "OS fingerprinting." },
  { "en": "Akses jarak jauh ke desktop?", "id": "Remote Desktop Protocol (RDP)." },
  { "en": "Protokol akses shell yang aman?", "id": "SSH (Secure Shell)." },
  { "en": "Protokol transfer file yang aman?", "id": "SFTP, SCP." },
  { "en": "Nomor port default SSH?", "id": "Port 22." },
  { "en": "Nomor port default RDP?", "id": "Port 3389." },
  { "en": "Nomor port default Telnet?", "id": "Port 23." },
  { "en": "Kelemahan utama Telnet?", "id": "Tidak terenkripsi." },
  { "en": "Area pada hard drive untuk memori virtual?", "id": "Swap space." },
  { "en": "Sinyal jam untuk sinkronisasi Ethernet?", "id": "Clocking." },
  { "en": "Jaringan yang sepenuhnya nirkabel?", "id": "Wireless ad-hoc network." },
  { "en": "Jaringan area tubuh?", "id": "BAN (Body Area Network)." },
  { "en": "Jaringan untuk komunikasi kendaraan?", "id": "VANET (Vehicular Ad-Hoc Network)." },
  { "en": "Standar untuk VANET?", "id": "IEEE 802.11p." },
  { "en": "Teknologi 'carrier sense' pada CSMA?", "id": "Mendengarkan sebelum mengirim." },
  { "en": "Siapa pengembang model TCP/IP?", "id": "Departemen Pertahanan AS (DoD)." },
  { "en": "Siapa pengembang model OSI?", "id": "ISO (International Organization for Standardization)." },
  { "en": "Lapisan TCP/IP yang setara Session/Presentation/Application?", "id": "Application Layer." },
  { "en": "Jaringan komputer pertama di dunia?", "id": "ARPANET." }



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
