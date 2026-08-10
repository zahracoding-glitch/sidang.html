<!DOCTYPE html>
<html lang="id" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Praharsa - Portal Antrian Digital Pengadilan</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700;800&family=Space+Grotesk:wght@700&display=swap" rel="stylesheet">
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        brand: {
                            50: '#fffbeb',
                            100: '#fef3c7',
                            200: '#fde68a',
                            300: '#fcd34d',
                            400: '#fbbf24',
                            500: '#f59e0b',
                            600: '#d97706',
                            700: '#b45309',
                        }
                    },
                    fontFamily: {
                        sans: ['Plus Jakarta Sans', 'sans-serif'],
                        display: ['Space Grotesk', 'sans-serif'],
                    }
                }
            }
        }
    </script>
    <style>
        .glass-panel {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(16px);
            -webkit-backdrop-filter: blur(16px);
            border: 1px solid rgba(251, 191, 36, 0.3);
        }
        .ticket-cutout {
            background-color: #ffffff;
            background-image: radial-gradient(circle at 0px 50%, transparent 14px, #fffbeb 15px),
                              radial-gradient(circle at 100% 50%, transparent 14px, #fffbeb 15px);
        }
        .pulse-amber {
            box-shadow: 0 0 0 0 rgba(245, 158, 11, 0.4);
            animation: pulse-amber 2s infinite;
        }
        @keyframes pulse-amber {
            0% { transform: scale(0.98); box-shadow: 0 0 0 0 rgba(245, 158, 11, 0.5); }
            70% { transform: scale(1); box-shadow: 0 0 0 12px rgba(245, 158, 11, 0); }
            100% { transform: scale(0.98); box-shadow: 0 0 0 0 rgba(245, 158, 11, 0); }
        }
        @media print {
            body * { visibility: hidden; }
            #ticket-preview, #ticket-preview * { visibility: visible; }
            #ticket-preview { position: absolute; left: 0; top: 0; width: 100%; border: none !important; }
            .no-print { display: none !important; }
        }
    </style>
</head>
<body class="bg-gradient-to-br from-white via-amber-50/50 to-amber-100/30 min-h-screen font-sans text-slate-800 antialiased selection:bg-amber-300">

    <!-- Toast Notification Container -->
    <div id="toast-container" class="fixed top-24 right-5 z-50 space-y-3 no-print"></div>

    <!-- Header Navigation -->
    <header class="sticky top-0 z-40 glass-panel border-b border-amber-200/60 shadow-sm no-print">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-20 flex items-center justify-between">
            <div class="flex items-center gap-3">
                <div class="w-12 h-12 rounded-2xl bg-gradient-to-br from-amber-400 to-amber-500 flex items-center justify-center text-white shadow-lg shadow-amber-400/30">
                    <i data-lucide="scale" class="w-6 h-6"></i>
                </div>
                <div>
                    <h1 class="font-display font-extrabold text-xl tracking-wider text-slate-900">PRAHARSA<span class="text-amber-500">.</span></h1>
                    <p class="text-[10px] sm:text-xs text-slate-500 font-semibold tracking-wide uppercase">Pengadilan Negeri Digital Service</p>
                </div>
            </div>
            
            <!-- Navigation Tabs -->
            <nav class="flex bg-amber-100/60 p-1.5 rounded-2xl gap-1 text-xs sm:text-sm font-bold border border-amber-200/50">
                <button onclick="switchTab('daftar')" id="tab-daftar" class="px-4 py-2.5 rounded-xl transition-all duration-300 bg-white text-amber-600 shadow-md shadow-amber-500/5 flex items-center gap-2">
                    <i data-lucide="ticket" class="w-4 h-4"></i>
                    <span class="hidden sm:inline">Daftar Antrian</span>
                </button>
                <button onclick="switchTab('status')" id="tab-status" class="px-4 py-2.5 rounded-xl transition-all duration-300 text-slate-600 hover:text-amber-700 flex items-center gap-2">
                    <i data-lucide="monitor" class="w-4 h-4"></i>
                    <span class="hidden sm:inline">Live Monitor</span>
                </button>
                <button onclick="switchTab('cek')" id="tab-cek" class="px-4 py-2.5 rounded-xl transition-all duration-300 text-slate-600 hover:text-amber-700 flex items-center gap-2">
                    <i data-lucide="search" class="w-4 h-4"></i>
                    <span class="hidden sm:inline">Cek Tiket</span>
                </button>
                <button onclick="switchTab('petugas')" id="tab-petugas" class="px-4 py-2.5 rounded-xl transition-all duration-300 text-slate-600 hover:text-amber-700 flex items-center gap-2">
                    <i data-lucide="shield" class="w-4 h-4"></i>
                    <span class="hidden sm:inline">Panel Petugas</span>
                </button>
            </nav>
        </div>
    </header>

    <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-10">

        <!-- ================= SECTION 1: DAFTAR ANTRIAN ================= -->
        <section id="sec-daftar" class="space-y-8 no-print">
            <div class="max-w-2xl mx-auto text-center space-y-2">
                <span class="px-3.5 py-1 rounded-full bg-amber-100 text-amber-800 text-xs font-bold tracking-wider uppercase border border-amber-300/60">Pelayanan Terpadu Satu Pintu (PTSP)</span>
                <h2 class="text-3xl sm:text-4xl font-extrabold text-slate-900 font-display">Registrasi Antrian Sidang</h2>
                <p class="text-slate-600 text-sm">Ambil Nomor Antrian Anda Secara Mandiri untuk Persidangan hari ini.</p>
            </div>

            <div class="grid lg:grid-cols-12 gap-8 items-start max-w-6xl mx-auto">
                <!-- Form Input -->
                <div class="lg:col-span-7 glass-panel p-6 sm:p-8 rounded-3xl shadow-xl shadow-amber-500/5 space-y-6">
                    <form id="form-antrian" onsubmit="submitForm(event)" class="space-y-5">
                        <div>
                            <label class="block text-xs font-bold uppercase tracking-wider text-slate-600 mb-2">Pilih Jadwal Ruang Sidang</label>
                            <select id="input-sidang" class="w-full px-4 py-3.5 rounded-xl bg-amber-50/30 border border-amber-200 focus:ring-2 focus:ring-amber-400 focus:bg-white outline-none font-medium transition-all text-sm">
                                <option value="S1">Ruang Cakra — 123/Pdt.G/2026/PN (Perdata)</option>
                                <option value="S2">Ruang Chandra — 456/Pid.B/2026/PN (Pidana)</option>
                                <option value="S3">Ruang Kartika — 789/Pdt.P/2026/PN (Permohonan)</option>
                            </select>
                        </div>

                        <div class="grid md:grid-cols-2 gap-4">
                            <div>
                                <label class="block text-xs font-bold uppercase tracking-wider text-slate-600 mb-2">Nama Lengkap</label>
                                <input type="text" id="input-nama" required placeholder="Sesuai KTP / Identitas" class="w-full px-4 py-3.5 rounded-xl bg-amber-50/30 border border-amber-200 focus:ring-2 focus:ring-amber-400 focus:bg-white outline-none font-medium text-sm transition-all">
                            </div>
                            <div>
                                <label class="block text-xs font-bold uppercase tracking-wider text-slate-600 mb-2">NIK (16 Digit)</label>
                                <input type="text" id="input-nik" maxlength="16" required placeholder="3201xxxxxxxxxxxx" class="w-full px-4 py-3.5 rounded-xl bg-amber-50/30 border border-amber-200 focus:ring-2 focus:ring-amber-400 focus:bg-white outline-none font-medium text-sm transition-all">
                            </div>
                        </div>

                        <div>
                            <label class="block text-xs font-bold uppercase tracking-wider text-slate-600 mb-2">Peran Dalam Sidang</label>
                            <select id="input-peran" class="w-full px-4 py-3.5 rounded-xl bg-amber-50/30 border border-amber-200 focus:ring-2 focus:ring-amber-400 focus:bg-white outline-none font-medium text-sm transition-all">
                                <option value="Saksi / Ahli">Saksi / Saksi Ahli</option>
                                <option value="Penggugat / Tergugat">Penggugat / Tergugat / Terdakwa</option>
                                <option value="Kuasa Hukum">Kuasa Hukum / Advokat</option>
                                <option value="Pengunjung Umum">Masyarakat Umum / Penonton</option>
                            </select>
                        </div>

                        <button type="submit" class="w-full py-4 rounded-2xl bg-amber-400 hover:bg-amber-500 text-slate-900 font-extrabold text-base shadow-lg shadow-amber-400/30 transition-all duration-300 flex items-center justify-center gap-2">
                            <i data-lucide="ticket" class="w-5 h-5"></i>
                            <span>Dapatkan Nomor Antrian Digital</span>
                        </button>
                    </form>
                </div>

                <!-- Tiket Display Card -->
                <div class="lg:col-span-5">
                    <div id="ticket-preview" class="ticket-cutout glass-panel p-8 rounded-3xl border-2 border-amber-400 shadow-2xl relative overflow-hidden hidden space-y-6">
                        <div class="text-center space-y-2 border-b-2 border-dashed border-amber-200 pb-6">
                            <div class="inline-flex items-center gap-1.5 px-3 py-1 bg-amber-100 text-amber-800 rounded-full text-xs font-extrabold border border-amber-300">
                                <i data-lucide="check-circle2" class="w-3.5 h-3.5 text-amber-600"></i>
                                <span>TIKET ANTRIAN RESMI PENGADILAN NEGERI</span>
                            </div>
                            <p class="text-[11px] text-slate-500 font-bold uppercase tracking-wider">Pengadilan Negeri Digital</p>
                            <div id="ticket-number" class="text-5xl font-black font-display text-amber-500 tracking-wider my-2">S1-000</div>
                            <p id="ticket-nama" class="font-extrabold text-slate-900 text-lg">Nama Pengunjung</p>
                            <span id="ticket-peran" class="inline-block text-xs bg-amber-100/70 px-3 py-1 rounded-full text-amber-900 font-semibold">Peran Pengunjung</span>
                        </div>

                        <div class="space-y-2.5 text-xs font-semibold">
                            <div class="flex justify-between text-slate-600">
                                <span>Ruang Sidang:</span>
                                <span id="ticket-ruang" class="text-slate-900 font-bold">-</span>
                            </div>
                            <div class="flex justify-between text-slate-600">
                                <span>Nomor Perkara:</span>
                                <span id="ticket-perkara" class="text-slate-900 font-bold">-</span>
                            </div>
                            <div class="flex justify-between text-slate-600">
                                <span>Waktu Mendaftar:</span>
                                <span id="ticket-waktu" class="text-slate-900 font-bold">-</span>
                            </div>
                        </div>

                        <div class="pt-2 no-print">
                            <button onclick="window.print()" class="w-full py-3 rounded-xl bg-slate-900 hover:bg-slate-800 text-white text-xs font-bold transition-all flex items-center justify-center gap-2 shadow-md">
                                <i data-lucide="printer" class="w-4 h-4"></i>
                                <span>Cetak Tiket / Simpan PDF</span>
                            </button>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- ================= SECTION 2: LIVE MONITOR ================= -->
        <section id="sec-status" class="space-y-8 hidden no-print">
            <div class="max-w-2xl mx-auto text-center space-y-2">
                <span class="px-3.5 py-1 rounded-full bg-amber-100 text-amber-800 text-xs font-bold tracking-wider uppercase border border-amber-300/60">Layar Informasi Publik</span>
                <h2 class="text-3xl sm:text-4xl font-extrabold text-slate-900 font-display">Status Pemanggilan Sidang</h2>
                <p class="text-slate-600 text-sm">Pantau nomor antrian yang sedang dipanggil di setiap ruangan secara langsung.</p>
            </div>

            <div id="status-cards-container" class="grid md:grid-cols-3 gap-6">
                <!-- Cards Updated via JS -->
            </div>
        </section>

        <!-- ================= SECTION 3: CEK TIKET ================= -->
        <section id="sec-cek" class="space-y-8 hidden no-print">
            <div class="max-w-2xl mx-auto text-center space-y-2">
                <span class="px-3.5 py-1 rounded-full bg-amber-100 text-amber-800 text-xs font-bold tracking-wider uppercase border border-amber-300/60">Layanan Kemudahan Pengunjung</span>
                <h2 class="text-3xl sm:text-4xl font-extrabold text-slate-900 font-display">Lacak Antrian Anda</h2>
                <p class="text-slate-600 text-sm">Masukkan Kode Tiket Anda Untuk Mengetahui Perkiraan Antrian.</p>
            </div>

            <div class="max-w-xl mx-auto glass-panel p-8 rounded-3xl shadow-xl space-y-6">
                <div class="flex gap-3">
                    <input type="text" id="input-search-ticket" placeholder="Masukkan Kode Tiket" class="uppercase flex-1 px-4 py-3.5 rounded-xl bg-amber-50/30 border border-amber-200 focus:ring-2 focus:ring-amber-400 outline-none font-bold text-sm">
                    <button onclick="cariTiket()" class="px-6 py-3.5 rounded-xl bg-amber-400 hover:bg-amber-500 text-slate-900 font-bold text-sm flex items-center gap-2 shadow-md transition-all">
                        <i data-lucide="search" class="w-4 h-4"></i>
                        <span>Lacak</span>
                    </button>
                </div>

                <div id="result-cek-ticket" class="hidden p-6 rounded-2xl bg-amber-50 border border-amber-200 space-y-3">
                    <!-- Dynamic Result -->
                </div>
            </div>
        </section>

        <!-- ================= SECTION 4: PANEL PETUGAS ================= -->
        <section id="sec-petugas" class="space-y-8 hidden no-print">
            <div class="max-w-2xl mx-auto text-center space-y-2">
                <span class="px-3.5 py-1 rounded-full bg-slate-200 text-slate-800 text-xs font-bold tracking-wider uppercase">Hak Akses Operator</span>
                <h2 class="text-3xl sm:text-4xl font-extrabold text-slate-900 font-display">Panel Pemanggilan Antrian</h2>
                <p class="text-slate-600 text-sm">Panggil dan atur urutan pengunjung sidang yang telah mendaftar.</p>
            </div>

            <div class="grid lg:grid-cols-12 gap-8 max-w-6xl mx-auto">
                <!-- Control Button & Call Panel -->
                <div class="lg:col-span-5 glass-panel p-6 rounded-3xl shadow-xl space-y-6">
                    <div>
                        <label class="block text-xs font-bold uppercase tracking-wider text-slate-600 mb-2">Pilih Ruangan Operasional</label>
                        <select id="select-petugas-ruang" onchange="renderPanelPetugas()" class="w-full px-4 py-3 rounded-xl bg-amber-50/30 border border-amber-200 focus:ring-2 focus:ring-amber-400 font-bold text-sm outline-none">
                            <option value="S1">Ruang Cakra (Perdata)</option>
                            <option value="S2">Ruang Garuda (Pidana)</option>
                            <option value="S3">Ruang Kartika (Permohonan)</option>
                        </select>
                    </div>

                    <button onclick="panggilAntrianNext()" class="w-full py-4 rounded-2xl bg-amber-400 hover:bg-amber-500 text-slate-900 font-extrabold text-base shadow-lg shadow-amber-400/20 flex items-center justify-center gap-3 transition-all">
                        <i data-lucide="volume-2" class="w-5 h-5 text-slate-900"></i>
                        <span>PANGGIL & BUNYIKAN VOICE</span>
                    </button>

                    <div class="bg-gradient-to-b from-amber-50 to-amber-100/40 rounded-2xl p-6 border border-amber-200 text-center space-y-2">
                        <span class="text-[11px] font-extrabold text-amber-800 uppercase tracking-widest">Sedang Dipanggil</span>
                        <div id="call-number" class="text-4xl font-black font-display text-amber-500">-</div>
                        <p id="call-name" class="font-extrabold text-slate-900 text-base">Tidak Ada Pemanggilan</p>
                    </div>
                </div>

                <!-- Table Queue List -->
                <div class="lg:col-span-7 glass-panel p-6 rounded-3xl shadow-xl space-y-4">
                    <div class="flex justify-between items-center">
                        <h3 class="font-bold text-slate-900 text-base flex items-center gap-2">
                            <i data-lucide="users" class="text-amber-500"></i> Daftar Pengunjung Menunggu
                        </h3>
                    </div>
                    <div class="overflow-x-auto">
                        <table class="w-full text-left text-xs">
                            <thead class="bg-amber-100/60 text-slate-700 font-extrabold uppercase">
                                <tr>
                                    <th class="p-3 rounded-l-xl">No. Tiket</th>
                                    <th class="p-3">Nama</th>
                                    <th class="p-3">Peran</th>
                                    <th class="p-3 rounded-r-xl">Jam</th>
                                </tr>
                            </thead>
                            <tbody id="queue-table-body" class="divide-y divide-amber-100 font-semibold">
                                <!-- Dynamic Rows -->
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </section>

    </main>

    <!-- App Logic JavaScript -->
    <script>
        // State Store
        const state = {
            jadwal: {
                S1: { ruang: "Ruang Cakra", perkara: "123/Pdt.G/2026/PN", jenis: "Perdata" },
                S2: { ruang: "Ruang Garuda", perkara: "456/Pid.B/2026/PN", jenis: "Pidana" },
                S3: { ruang: "Ruang Kartika", perkara: "789/Pdt.P/2026/PN", jenis: "Permohonan" }
            },
            antrian: { S1: [], S2: [], S3: [] },
            counter: { S1: 1, S2: 1, S3: 1 },
            dipanggil: { S1: null, S2: null, S3: null }
        };

        // Initialize Lucide Icons
        lucide.createIcons();

        // Toast Notification System
        function showToast(message, type = 'success') {
            const container = document.getElementById('toast-container');
            const toast = document.createElement('div');
            const colorClass = type === 'success' ? 'bg-slate-900 text-amber-400 border-amber-400' : 'bg-red-900 text-white border-red-500';
            
            toast.className = `flex items-center gap-3 px-5 py-3.5 rounded-2xl shadow-2xl border text-xs font-bold transition-all duration-500 transform translate-y-2 ${colorClass}`;
            toast.innerHTML = `<i data-lucide="${type === 'success' ? 'check-circle' : 'alert-circle'}" class="w-4 h-4"></i> <span>${message}</span>`;
            
            container.appendChild(toast);
            lucide.createIcons();

            setTimeout(() => {
                toast.classList.add('opacity-0', '-translate-y-2');
                setTimeout(() => toast.remove(), 500);
            }, 3500);
        }

        // Voice Text-To-Speech System
        function speakCall(nomor, ruang) {
            if ('speechSynthesis' in window) {
                window.speechSynthesis.cancel(); // Reset audio
                const text = `Nomor antrian ${nomor}, silakan masuk ke ${ruang}`;
                const utterance = new SpeechSynthesisUtterance(text);
                utterance.lang = 'id-ID';
                utterance.rate = 0.9;
                utterance.pitch = 1.0;
                window.speechSynthesis.speak(utterance);
            }
        }

        // Switch Active Tabs
        function switchTab(tabName) {
            ['daftar', 'status', 'cek', 'petugas'].forEach(t => {
                document.getElementById(`sec-${t}`).classList.add('hidden');
                const btn = document.getElementById(`tab-${t}`);
                btn.className = "px-4 py-2.5 rounded-xl transition-all duration-300 text-slate-600 hover:text-amber-700 flex items-center gap-2";
            });

            document.getElementById(`sec-${tabName}`).classList.remove('hidden');
            const activeBtn = document.getElementById(`tab-${tabName}`);
            activeBtn.className = "px-4 py-2.5 rounded-xl transition-all duration-300 bg-white text-amber-600 shadow-md shadow-amber-500/5 flex items-center gap-2 font-bold";

            if(tabName === 'status') renderStatusCards();
            if(tabName === 'petugas') renderPanelPetugas();
        }

        // Form Submission
        function submitForm(event) {
            event.preventDefault();
            const idSidang = document.getElementById('input-sidang').value;
            const nama = document.getElementById('input-nama').value;
            const nik = document.getElementById('input-nik').value;
            const peran = document.getElementById('input-peran').value;

            const formatNo = `${idSidang}-${String(state.counter[idSidang]).padStart(3, '0')}`;
            state.counter[idSidang]++;

            const data = {
                nomor: formatNo,
                nama: nama,
                nik: nik,
                peran: peran,
                waktu: new Date().toLocaleTimeString('id-ID', { hour: '2-digit', minute: '2-digit' })
            };

            state.antrian[idSidang].push(data);

            // Populate Ticket Preview
            document.getElementById('ticket-number').innerText = data.nomor;
            document.getElementById('ticket-nama').innerText = data.nama;
            document.getElementById('ticket-peran').innerText = `${data.peran} (NIK: ${data.nik})`;
            document.getElementById('ticket-ruang').innerText = state.jadwal[idSidang].ruang;
            document.getElementById('ticket-perkara').innerText = state.jadwal[idSidang].perkara;
            document.getElementById('ticket-waktu').innerText = data.waktu;
            
            document.getElementById('ticket-preview').classList.remove('hidden');
            showToast(`Tiket ${formatNo} Berhasil Dibuat!`);

            // Reset Input
            document.getElementById('input-nama').value = '';
            document.getElementById('input-nik').value = '';
        }

        // Render Status Cards
        function renderStatusCards() {
            const container = document.getElementById('status-cards-container');
            container.innerHTML = '';

            Object.keys(state.jadwal).forEach(id => {
                const info = state.jadwal[id];
                const active = state.dipanggil[id];
                const totalSisa = state.antrian[id].length;

                const cardHTML = `
                    <div class="glass-panel p-6 rounded-3xl shadow-lg border-t-4 border-amber-400 space-y-4">
                        <div class="flex justify-between items-center">
                            <span class="px-2.5 py-1 bg-amber-100 text-amber-800 font-bold text-[10px] rounded-full uppercase tracking-wider border border-amber-200">${info.jenis}</span>
                            <span class="text-xs text-slate-500 font-bold">Menunggu: ${totalSisa} Orang</span>
                        </div>
                        <div>
                            <h3 class="text-xl font-black font-display text-slate-900">${info.ruang}</h3>
                            <p class="text-xs text-slate-500 font-mono">${info.perkara}</p>
                        </div>
                        <div class="bg-amber-50/80 p-4 rounded-2xl text-center border border-amber-200 ${active ? 'pulse-amber' : ''}">
                            <p class="text-[10px] text-slate-500 font-bold uppercase tracking-widest mb-1">Dipanggil Saat Ini</p>
                            <div class="text-4xl font-black font-display text-amber-500">${active ? active.nomor : '-'}</div>
                            <p class="text-xs font-bold text-slate-800 truncate mt-1">${active ? active.nama : 'Belum Ada Panggilan'}</p>
                        </div>
                    </div>
                `;
                container.innerHTML += cardHTML;
            });
        }

        // Render Panel Petugas
        function renderPanelPetugas() {
            const idSidang = document.getElementById('select-petugas-ruang').value;
            const activeCall = state.dipanggil[idSidang];

            document.getElementById('call-number').innerText = activeCall ? activeCall.nomor : '-';
            document.getElementById('call-name').innerText = activeCall ? `${activeCall.nama} (${activeCall.peran})` : 'Tidak Ada Pemanggilan';

            const tbody = document.getElementById('queue-table-body');
            tbody.innerHTML = '';

            if (state.antrian[idSidang].length === 0) {
                tbody.innerHTML = `<tr><td colspan="4" class="p-4 text-center text-slate-400">Belum ada antrian tersimpan</td></tr>`;
                return;
            }

            state.antrian[idSidang].forEach(item => {
                const row = `
                    <tr class="hover:bg-amber-50/60 transition-colors">
                        <td class="p-3 font-bold text-amber-600">${item.nomor}</td>
                        <td class="p-3">${item.nama}</td>
                        <td class="p-3 text-slate-500">${item.peran}</td>
                        <td class="p-3 text-slate-400 text-[11px]">${item.waktu}</td>
                    </tr>
                `;
                tbody.innerHTML += row;
            });
        }

        // Panggil Next + Voice
        function panggilAntrianNext() {
            const idSidang = document.getElementById('select-petugas-ruang').value;
            if (state.antrian[idSidang].length > 0) {
                const dipanggil = state.antrian[idSidang].shift();
                state.dipanggil[idSidang] = dipanggil;
                
                renderPanelPetugas();
                speakCall(dipanggil.nomor, state.jadwal[idSidang].ruang);
                showToast(`Memanggil Antrian ${dipanggil.nomor}`);
            } else {
                showToast("Tidak ada antrian tersisa di ruang ini.", "error");
            }
        }

        // Search Ticket Feature
        function cariTiket() {
            const query = document.getElementById('input-search-ticket').value.trim().toUpperCase();
            const resContainer = document.getElementById('result-cek-ticket');
            resContainer.classList.remove('hidden');

            if (!query) {
                resContainer.innerHTML = `<p class="text-xs text-red-600 font-bold">Harap masukkan kode tiket!</p>`;
                return;
            }

            const prefix = query.split('-')[0];
            if (!state.jadwal[prefix]) {
                resContainer.innerHTML = `<p class="text-xs text-red-600 font-bold">Kode Tiket Tidak Valid!</p>`;
                return;
            }

            const queueList = state.antrian[prefix];
            const activeCall = state.dipanggil[prefix];

            if (activeCall && activeCall.nomor === query) {
                resContainer.innerHTML = `
                    <div class="text-center space-y-1">
                        <span class="px-3 py-1 bg-amber-400 text-slate-900 rounded-full text-[10px] font-bold">STATUS: SEDANG DIPANGGIL</span>
                        <h4 class="font-extrabold text-slate-900 text-base mt-2">Silakan Masuk ke ${state.jadwal[prefix].ruang} Sekarang!</h4>
                    </div>
                `;
                return;
            }

            const indexInQueue = queueList.findIndex(item => item.nomor === query);
            if (indexInQueue !== -1) {
                resContainer.innerHTML = `
                    <div class="text-slate-800 space-y-1">
                        <p class="text-xs font-bold text-slate-500">Hasil Pencarian untuk: <span class="text-amber-600">${query}</span></p>
                        <p class="text-sm font-extrabold">Posisi Anda: Menunggu di Urutan Ke-${indexInQueue + 1}</p>
                        <p class="text-xs text-slate-600">Sisa ${indexInQueue} antrian lagi sebelum giliran Anda dipanggil.</p>
                    </div>
                `;
            } else {
                resContainer.innerHTML = `<p class="text-xs text-slate-600 font-bold">Tiket tidak ditemukan dalam antrian aktif atau sudah selesai dipanggil.</p>`;
            }
        }
    </script>
</body>
</html>
