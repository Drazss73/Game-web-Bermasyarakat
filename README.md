# Game-web-Bermasyarakat
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simulasi Warga Desa Sukamaju - Game Gotong Royong</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Font Awesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap" rel="stylesheet">
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Plus Jakarta Sans', 'sans-serif'],
                    },
                    colors: {
                        desagreen: {
                            50: '#f0fdf4',
                            100: '#dcfce7',
                            500: '#22c55e',
                            600: '#16a34a',
                            700: '#15803d',
                        }
                    }
                }
            }
        }
    </script>
    <style>
        /* Custom Styles & Scrollbar */
        ::-webkit-scrollbar {
            width: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #f1f5f9;
        }
        ::-webkit-scrollbar-thumb {
            background: #10b981;
            border-radius: 9999px;
        }
        .modal-active {
            display: flex !important;
        }
        .bounce-custom {
            animation: bounceSlow 2s infinite;
        }
        @keyframes bounceSlow {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-5px); }
        }
    </style>
</head>
<body class="bg-slate-100 font-sans text-slate-800 min-h-screen flex flex-col justify-between selection:bg-emerald-500 selection:text-white">

    <!-- TOP HEADER / STATUS BAR -->
    <header class="bg-white border-b border-slate-200 sticky top-0 z-30 shadow-sm">
        <div class="max-w-6xl mx-auto px-4 py-3 flex flex-wrap justify-between items-center gap-3">
            
            <!-- Logo & Title -->
            <div class="flex items-center gap-3">
                <div class="w-11 h-11 rounded-2xl bg-gradient-to-tr from-emerald-500 to-teal-600 text-white flex items-center justify-center font-bold text-xl shadow-md">
                    <i class="fa-solid fa-house-flag"></i>
                </div>
                <div>
                    <div class="flex items-center gap-2">
                        <h1 class="font-extrabold text-slate-900 text-base sm:text-lg leading-none">Desa Sukamaju</h1>
                        <span id="badge-title" class="bg-emerald-100 text-emerald-800 text-[10px] font-extrabold px-2 py-0.5 rounded-full border border-emerald-300">Warga Baru</span>
                    </div>
                    <p class="text-xs text-slate-500 font-medium mt-1">Hari ke-<span id="stat-day" class="text-emerald-600 font-bold">1</span> • Gotong Royong &amp; Keharmonisan</p>
                </div>
            </div>

            <!-- Stats Bar -->
            <div class="flex items-center gap-2 sm:gap-4 bg-slate-50 px-3 py-1.5 sm:px-4 sm:py-2 rounded-2xl border border-slate-200/80">
                
                <!-- Energi -->
                <div class="flex items-center gap-2 min-w-[80px]" title="Energi harian untuk melakukan aktivitas">
                    <i class="fa-solid fa-bolt text-amber-500 text-base sm:text-lg"></i>
                    <div>
                        <div class="text-[9px] uppercase font-bold text-slate-400 leading-tight">Energi</div>
                        <div class="text-xs sm:text-sm font-extrabold text-slate-800" id="stat-energy">100 / 100</div>
                    </div>
                </div>

                <div class="h-6 w-[1px] bg-slate-200"></div>

                <!-- Poin Kebaikan -->
                <div class="flex items-center gap-2 min-w-[80px]" title="Social Score / Rekam Kebaikan">
                    <i class="fa-solid fa-heart text-rose-500 text-base sm:text-lg"></i>
                    <div>
                        <div class="text-[9px] uppercase font-bold text-slate-400 leading-tight">Poin Kebaikan</div>
                        <div class="text-xs sm:text-sm font-extrabold text-slate-800" id="stat-score">50</div>
                    </div>
                </div>

                <div class="h-6 w-[1px] bg-slate-200"></div>

                <!-- Keharmonisan Desa -->
                <div class="flex items-center gap-2 min-w-[80px]" title="Tingkat Kerukunan & Kebahagiaan Desa">
                    <i class="fa-solid fa-handshake text-indigo-500 text-base sm:text-lg"></i>
                    <div>
                        <div class="text-[9px] uppercase font-bold text-slate-400 leading-tight">Keharmonisan</div>
                        <div class="text-xs sm:text-sm font-extrabold text-slate-800" id="stat-harmony">75%</div>
                    </div>
                </div>
            </div>

            <!-- Rest / Next Day Button -->
            <button onclick="nextDay()" class="bg-gradient-to-r from-emerald-600 to-teal-700 hover:from-emerald-700 hover:to-teal-800 text-white font-bold text-xs sm:text-sm px-4 py-2.5 rounded-xl shadow-md transition flex items-center gap-2 active:scale-95">
                <span>Istirahat (Hari Baru)</span>
                <i class="fa-solid fa-moon"></i>
            </button>
        </div>
    </header>

    <!-- MAIN GAME CONTAINER -->
    <main class="max-w-6xl mx-auto px-4 py-6 w-full flex-grow">
        
        <!-- HERO BANNER -->
        <div class="bg-gradient-to-r from-emerald-700 via-teal-700 to-cyan-800 rounded-3xl p-6 mb-6 text-white shadow-xl relative overflow-hidden">
            <div class="relative z-10 max-w-2xl">
                <span class="bg-white/20 backdrop-blur-md text-white text-[10px] font-extrabold uppercase tracking-widest px-3 py-1 rounded-full mb-3 inline-block">
                    🌱 Game Kemasyarakatan Interaktif
                </span>
                <h2 class="text-2xl sm:text-3xl font-extrabold mb-2 leading-tight">Bangun Desa Sukamaju Bersama! 🤝</h2>
                <p class="text-emerald-100 text-xs sm:text-sm leading-relaxed">
                    Setiap aksi gotong royong, toleransi, dan kepedulian lingkungan yang kamu lakukan akan berdampak langsung pada keharmonisan warga. Pilih lokasi dan bantu warga desa hari ini!
                </p>
            </div>
            <i class="fa-solid fa-people-roof absolute -right-6 -bottom-8 text-[160px] text-white/10 pointer-events-none"></i>
        </div>

        <!-- NAVIGATION TABS -->
        <div class="flex gap-2 mb-6 border-b border-slate-200 pb-2 overflow-x-auto">
            <button onclick="switchTab('map')" id="tab-map" class="tab-btn bg-emerald-600 text-white px-4 py-2.5 rounded-xl text-xs sm:text-sm font-extrabold shadow-sm transition flex items-center gap-2 whitespace-nowrap">
                <i class="fa-solid fa-map-location-dot"></i> Peta Desa
            </button>
            <button onclick="switchTab('quests')" id="tab-quests" class="tab-btn bg-white text-slate-600 hover:bg-slate-100 px-4 py-2.5 rounded-xl text-xs sm:text-sm font-extrabold border border-slate-200 transition flex items-center gap-2 whitespace-nowrap">
                <i class="fa-solid fa-list-check text-emerald-600"></i> Misi Gotong Royong
            </button>
            <button onclick="switchTab('minigame')" id="tab-minigame" class="tab-btn bg-white text-slate-600 hover:bg-slate-100 px-4 py-2.5 rounded-xl text-xs sm:text-sm font-extrabold border border-slate-200 transition flex items-center gap-2 whitespace-nowrap">
                <i class="fa-solid fa-recycle text-emerald-600"></i> Bank Sampah Desa
            </button>
            <button onclick="switchTab('journal')" id="tab-journal" class="tab-btn bg-white text-slate-600 hover:bg-slate-100 px-4 py-2.5 rounded-xl text-xs sm:text-sm font-extrabold border border-slate-200 transition flex items-center gap-2 whitespace-nowrap">
                <i class="fa-solid fa-award text-amber-500"></i> Prestasi &amp; Catatan
            </button>
        </div>

        <!-- TAB CONTENT 1: DESA MAP -->
        <div id="content-map" class="tab-content grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-5">
            
            <!-- Lokasi 1: Balai Desa -->
            <div class="bg-white rounded-2xl p-5 border border-slate-200 shadow-sm hover:shadow-lg transition group flex flex-col justify-between">
                <div>
                    <div class="flex items-start justify-between mb-3">
                        <div class="w-12 h-12 rounded-2xl bg-amber-100 text-amber-600 flex items-center justify-center text-2xl group-hover:scale-110 transition shadow-sm">
                            <i class="fa-solid fa-building-columns"></i>
                        </div>
                        <span class="bg-amber-50 text-amber-700 text-[10px] font-extrabold px-2.5 py-1 rounded-full border border-amber-200">Pusat Musyawarah</span>
                    </div>
                    <h3 class="font-extrabold text-slate-800 text-base mb-1">Balai Desa Sukamaju</h3>
                    <p class="text-slate-500 text-xs leading-relaxed mb-4">Tempat Pak RT dan tokoh warga bermusyawarah menyusun kegiatan gotong royong dan pembangunan fasilitas desa.</p>
                </div>
                <div class="flex items-center justify-between border-t border-slate-100 pt-3">
                    <span class="text-[11px] font-semibold text-slate-500"><i class="fa-solid fa-user-tie text-amber-500 mr-1"></i> Pak RT Sudirman</span>
                    <button onclick="openInteraction('balai')" class="bg-slate-900 hover:bg-amber-600 text-white text-xs font-bold px-3.5 py-2 rounded-xl transition shadow">Musyawarah</button>
                </div>
            </div>

            <!-- Lokasi 2: Taman & Lingkungan -->
            <div class="bg-white rounded-2xl p-5 border border-slate-200 shadow-sm hover:shadow-lg transition group flex flex-col justify-between">
                <div>
                    <div class="flex items-start justify-between mb-3">
                        <div class="w-12 h-12 rounded-2xl bg-emerald-100 text-emerald-600 flex items-center justify-center text-2xl group-hover:scale-110 transition shadow-sm">
                            <i class="fa-solid fa-tree"></i>
                        </div>
                        <span class="bg-emerald-50 text-emerald-700 text-[10px] font-extrabold px-2.5 py-1 rounded-full border border-emerald-200">Area Hijau &amp; Asri</span>
                    </div>
                    <h3 class="font-extrabold text-slate-800 text-base mb-1">Taman Desa Sukamaju</h3>
                    <p class="text-slate-500 text-xs leading-relaxed mb-4">Taman umum tempat anak-anak bermain dan warga berkumpul. Perlu perawatan dan pembersihan rutin bersama.</p>
                </div>
                <div class="flex items-center justify-between border-t border-slate-100 pt-3">
                    <span class="text-[11px] font-semibold text-slate-500"><i class="fa-solid fa-broom text-emerald-500 mr-1"></i> Kerja Bakti</span>
                    <button onclick="openInteraction('taman')" class="bg-slate-900 hover:bg-emerald-600 text-white text-xs font-bold px-3.5 py-2 rounded-xl transition shadow">Ikut Bersihkan</button>
                </div>
            </div>

            <!-- Lokasi 3: Pemukiman Tetangga -->
            <div class="bg-white rounded-2xl p-5 border border-slate-200 shadow-sm hover:shadow-lg transition group flex flex-col justify-between">
                <div>
                    <div class="flex items-start justify-between mb-3">
                        <div class="w-12 h-12 rounded-2xl bg-sky-100 text-sky-600 flex items-center justify-center text-2xl group-hover:scale-110 transition shadow-sm">
                            <i class="fa-solid fa-people-roof"></i>
                        </div>
                        <span class="bg-sky-50 text-sky-700 text-[10px] font-extrabold px-2.5 py-1 rounded-full border border-sky-200">Pemukiman Warga</span>
                    </div>
                    <h3 class="font-extrabold text-slate-800 text-base mb-1">Gang Kerukunan</h3>
                    <p class="text-slate-500 text-xs leading-relaxed mb-4">Kompleks rumah tetangga. Menjaga komunikasi, silaturahmi, dan saling bantu saat tetangga butuh pertolongan.</p>
                </div>
                <div class="flex items-center justify-between border-t border-slate-100 pt-3">
                    <span class="text-[11px] font-semibold text-slate-500"><i class="fa-solid fa-comments text-sky-500 mr-1"></i> Silaturahmi Warga</span>
                    <button onclick="openInteraction('tetangga')" class="bg-slate-900 hover:bg-sky-600 text-white text-xs font-bold px-3.5 py-2 rounded-xl transition shadow">Bantu Tetangga</button>
                </div>
            </div>

            <!-- Lokasi 4: Pos Ronda -->
            <div class="bg-white rounded-2xl p-5 border border-slate-200 shadow-sm hover:shadow-lg transition group flex flex-col justify-between">
                <div>
                    <div class="flex items-start justify-between mb-3">
                        <div class="w-12 h-12 rounded-2xl bg-indigo-100 text-
