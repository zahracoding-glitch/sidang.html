<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>PRAHARSA — Sistem Antrian Pengadilan Digital</title>

<script src="https://cdn.tailwindcss.com"></script>
<script src="https://unpkg.com/lucide@latest"></script>
<script src="https://cdn.jsdelivr.net/npm/qrcodejs@1.0.0/qrcode.min.js"></script>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&family=Plus+Jakarta+Sans:wght@500;600;700;800&display=swap" rel="stylesheet">

<script>
tailwind.config = {
  theme: {
    extend: {
      colors: {
        brand: {
          50:'#fffdf5',100:'#fff8d9',200:'#ffef9d',300:'#ffe36b',
          400:'#ffd43b',500:'#f5b800',600:'#d99b00',700:'#a96f00',
          800:'#7a5000',900:'#4d3300'
        }
      },
      fontFamily:{sans:['Inter','sans-serif'],display:['Plus Jakarta Sans','sans-serif']}
    }
  }
}
</script>

<style>
*{scrollbar-width:thin;scrollbar-color:#f5b800 transparent}
body{font-family:Inter,sans-serif;background:#f8fafc}
.font-display{font-family:"Plus Jakarta Sans",sans-serif}
.sidebar{transition:.3s ease}
.glass{background:rgba(255,255,255,.88);backdrop-filter:blur(18px);border:1px solid rgba(245,184,0,.16)}
.card{box-shadow:0 12px 35px rgba(15,23,42,.06)}
.nav-item{transition:.2s}
.nav-item.active{background:#fff4c2;color:#8a5a00;box-shadow:inset 3px 0 #f5b800}
.nav-item:hover:not(.active){background:#f8fafc}
.page{animation:fade .25s ease}
@keyframes fade{from{opacity:0;transform:translateY(5px)}to{opacity:1;transform:none}}
.pulse{animation:pulse 2s infinite}
@keyframes pulse{0%,100%{box-shadow:0 0 0 0 rgba(245,184,0,.35)}50%{box-shadow:0 0 0 10px rgba(245,184,0,0)}}
.modal-bg{background:rgba(15,23,42,.45);backdrop-filter:blur(4px)}
.ticket{
  background:linear-gradient(135deg,#fff 0%,#fffdf2 100%);
  border:2px dashed #f5b800;
}
@media print{
  body *{visibility:hidden!important}
  #ticket-print,#ticket-print *{visibility:visible!important}
  #ticket-print{position:absolute;left:0;top:0;width:100%;box-shadow:none!important}
}
</style>
</head>

<body class="text-slate-800">

<div id="toast" class="fixed top-5 right-5 z-[100] space-y-3"></div>

<!-- MOBILE OVERLAY -->
<div id="sidebarOverlay" onclick="toggleSidebar()" class="fixed inset-0 bg-slate-900/40 z-40 hidden lg:hidden"></div>

<!-- SIDEBAR -->
<aside id="sidebar" class="sidebar fixed left-0 top-0 bottom-0 z-50 w-72 bg-white border-r border-slate-200 -translate-x-full lg:translate-x-0">
  <div class="h-full flex flex-col">
    <div class="p-5 border-b border-slate-100">
      <div class="flex items-center gap-3">
        <div class="w-12 h-12 rounded-2xl bg-gradient-to-br from-brand-300 to-brand-600 text-slate-900 flex items-center justify-center shadow-lg shadow-brand-500/20">
          <i data-lucide="scale" class="w-6 h-6"></i>
        </div>
        <div>
          <div class="font-display font-extrabold tracking-wide text-lg">PRAHARSA<span class="text-brand-500">.</span></div>
          <div class="text-[10px] text-slate-400 font-bold uppercase tracking-widest">Court Digital Service</div>
        </div>
      </div>
    </div>

    <div class="px-4 py-5 overflow-y-auto flex-1">
      <div class="text-[10px] font-extrabold text-slate-400 uppercase tracking-widest px-3 mb-2">Menu Utama</div>
      <nav class="space-y-1">
        <button class="nav-item active w-full text-left px-4 py-3 rounded-xl flex items-center gap-3 text-sm font-bold" data-page="dashboard" onclick="showPage('dashboard')">
          <i data-lucide="layout-dashboard" class="w-5 h-5"></i> Dashboard
        </button>
        <button class="nav-item w-full text-left px-4 py-3 rounded-xl flex items-center gap-3 text-sm font-semibold text-slate-600" data-page="daftar" onclick="showPage('daftar')">
          <i data-lucide="ticket" class="w-5 h-5"></i> Daftar Antrian
        </button>
        <button class="nav-item w-full text-left px-4 py-3 rounded-xl flex items-center gap-3 text-sm font-semibold text-slate-600" data-page="monitor" onclick="showPage('monitor')">
          <i data-lucide="monitor" class="w-5 h-5"></i> Live Monitor
        </button>
        <button class="nav-item w-full text-left px-4 py-3 rounded-xl flex items-center gap-3 text-sm font-semibold text-slate-600" data-page="cek" onclick="showPage('cek')">
          <i data-lucide="search" class="w-5 h-5"></i> Cek Tiket
        </button>
        <button class="nav-item w-full text-left px-4 py-3 rounded-xl flex items-center gap-3 text-sm font-semibold text-slate-600" data-page="petugas" onclick="showPage('petugas')">
          <i data-lucide="headphones" class="w-5 h-5"></i> Panel Petugas
        </button>
      </nav>

      <div class="text-[10px] font-extrabold text-slate-400 uppercase tracking-widest px-3 mt-7 mb-2">Informasi</div>
      <nav class="space-y-1">
        <button class="nav-item w-full text-left px-4 py-3 rounded-xl flex items-center gap-3 text-sm font-semibold text-slate-600" data-page="jadwal" onclick="showPage('jadwal')">
          <i data-lucide="calendar-days" class="w-5 h-5"></i> Jadwal Sidang
        </button>
        <button class="nav-item w-full text-left px-4 py-3 rounded-xl flex items-center gap-3 text-sm font-semibold text-slate-600" data-page="layanan" onclick="showPage('layanan')">
          <i data-lucide="grid-2x2" class="w-5 h-5"></i> Layanan PTSP
        </button>
        <button class="nav-item w-full text-left px-4 py-3 rounded-xl flex items-center gap-3 text-sm font-semibold text-slate-600" data-page="riwayat" onclick="showPage('riwayat')">
          <i data-lucide="history" class="w-5 h-5"></i> Riwayat
        </button>
        <button class="nav-item w-full text-left px-4 py-3 rounded-xl flex items-center gap-3 text-sm font-semibold text-slate-600" data-page="bantuan" onclick="showPage('bantuan')">
          <i data-lucide="circle-help" class="w-5 h-5"></i> Bantuan
        </button>
      </nav>
    </div>

    <div class="p-4 border-t border-slate-100">
      <div class="rounded-2xl bg-gradient-to-br from-brand-50 to-white border border-brand-100 p-4">
        <div class="flex items-center gap-3">
          <div class="w-10 h-10 rounded-full bg-brand-400 flex items-center justify-center font-black">P</div>
          <div class="min-w-0">
            <div class="text-sm font-extrabold truncate">Portal Publik</div>
            <div class="text-[11px] text-slate-500">Layanan mandiri</div>
          </div>
        </div>
        <button onclick="showPage('layanan')" class="mt-3 w-full py-2 rounded-xl bg-white border border-brand-200 text-xs font-bold text-brand-800">Lihat Layanan</button>
      </div>
    </div>
  </div>
</aside>

<!-- MAIN -->
<div class="lg:ml-72 min-h-screen">
  <header class="sticky top-0 z-30 h-[72px] bg-white/90 backdrop-blur-xl border-b border-slate-200">
    <div class="h-full px-4 sm:px-6 flex items-center justify-between">
      <div class="flex items-center gap-3">
        <button onclick="toggleSidebar()" class="lg:hidden w-10 h-10 rounded-xl border border-slate-200 flex items-center justify-center">
          <i data-lucide="menu" class="w-5 h-5"></i>
        </button>
        <div>
          <div id="breadcrumb" class="text-[11px] text-slate-400 font-bold uppercase tracking-widest">PRAHARSA / Dashboard</div>
          <div id="clock" class="text-sm font-extrabold text-slate-800">--:--:--</div>
        </div>
      </div>
      <div class="flex items-center gap-2">
        <button onclick="showToast('Tidak ada notifikasi baru.')" class="relative w-10 h-10 rounded-xl border border-slate-200 bg-white flex items-center justify-center hover:bg-slate-50">
          <i data-lucide="bell" class="w-5 h-5"></i>
          <span class="absolute top-2 right-2 w-2 h-2 rounded-full bg-red-500"></span>
        </button>
        <button onclick="showPage('bantuan')" class="hidden sm:flex w-10 h-10 rounded-xl border border-slate-200 bg-white items-center justify-center">
          <i data-lucide="circle-help" class="w-5 h-5"></i>
        </button>
      </div>
    </div>
  </header>

  <main class="p-4 sm:p-6 lg:p-8 max-w-[1500px] mx-auto">

    <!-- DASHBOARD -->
    <section id="page-dashboard" class="page">
      <div class="rounded-[28px] bg-gradient-to-br from-slate-900 via-slate-800 to-slate-900 text-white p-6 sm:p-8 mb-7 overflow-hidden relative">
        <div class="absolute -right-20 -top-20 w-72 h-72 rounded-full bg-brand-400/10"></div>
        <div class="absolute right-24 bottom-[-80px] w-56 h-56 rounded-full bg-brand-400/10"></div>
        <div class="relative">
          <div class="inline-flex items-center gap-2 px-3 py-1.5 rounded-full bg-white/10 border border-white/10 text-brand-300 text-[11px] font-bold uppercase tracking-widest">
            <span class="w-2 h-2 bg-green-400 rounded-full animate-pulse"></span> Sistem Online
          </div>
          <h1 class="font-display text-2xl sm:text-4xl font-extrabold mt-4">Selamat Datang di PRAHARSA</h1>
          <p class="text-slate-300 mt-2 max-w-2xl text-sm sm:text-base">Portal layanan antrian digital Pengadilan Negeri. Daftar antrian, pantau panggilan, cek tiket, dan akses informasi pelayanan dalam satu tempat.</p>
          <div class="flex flex-wrap gap-3 mt-6">
            <button onclick="showPage('daftar')" class="px-5 py-3 rounded-xl bg-brand-400 text-slate-900 font-extrabold text-sm hover:bg-brand-300 flex items-center gap-2">
              <i data-lucide="ticket" class="w-4 h-4"></i> Ambil Antrian
            </button>
            <button onclick="showPage('monitor')" class="px-5 py-3 rounded-xl bg-white/10 border border-white/15 font-bold text-sm hover:bg-white/15 flex items-center gap-2">
              <i data-lucide="monitor" class="w-4 h-4"></i> Lihat Monitor
            </button>
          </div>
        </div>
      </div>

      <div class="grid grid-cols-2 xl:grid-cols-4 gap-4 mb-7">
        <div class="glass card rounded-2xl p-5">
          <div class="flex justify-between"><div class="w-11 h-11 rounded-xl bg-blue-50 text-blue-600 flex items-center justify-center"><i data-lucide="users"></i></div><span class="text-green-600 text-xs font-bold">Hari ini</span></div>
          <div id="stat-total" class="text-2xl font-black mt-4">0</div><div class="text-xs text-slate-500 font-semibold mt-1">Total pendaftar</div>
        </div>
        <div class="glass card rounded-2xl p-5">
          <div class="flex justify-between"><div class="w-11 h-11 rounded-xl bg-brand-50 text-brand-700 flex items-center justify-center"><i data-lucide="clock-3"></i></div><span class="text-xs font-bold text-slate-400">Aktif</span></div>
          <div id="stat-waiting" class="text-2xl font-black mt-4">0</div><div class="text-xs text-slate-500 font-semibold mt-1">Sedang menunggu</div>
        </div>
        <div class="glass card rounded-2xl p-5">
          <div class="flex justify-between"><div class="w-11 h-11 rounded-xl bg-green-50 text-green-600 flex items-center justify-center"><i data-lucide="check-circle-2"></i></div><span class="text-green-600 text-xs font-bold">Selesai</span></div>
          <div id="stat-done" class="text-2xl font-black mt-4">0</div><div class="text-xs text-slate-500 font-semibold mt-1">Layanan selesai</div>
        </div>
        <div class="glass card rounded-2xl p-5">
          <div class="flex justify-between"><div class="w-11 h-11 rounded-xl bg-purple-50 text-purple-600 flex items-center justify-center"><i data-lucide="door-open"></i></div><span class="text-xs font-bold text-slate-400">Ruang</span></div>
          <div class="text-2xl font-black mt-4">3</div><div class="text-xs text-slate-500 font-semibold mt-1">Ruang aktif</div>
        </div>
      </div>

      <div class="grid xl:grid-cols-3 gap-6">
        <div class="xl:col-span-2 glass card rounded-3xl p-6">
          <div class="flex justify-between items-center mb-5">
            <div><h2 class="font-display font-extrabold text-lg">Status Ruang Sidang</h2><p class="text-xs text-slate-500 mt-1">Pemanggilan terbaru setiap ruang</p></div>
            <button onclick="showPage('monitor')" class="text-xs font-bold text-brand-700 hover:underline">Lihat semua</button>
          </div>
          <div id="dashboardRooms" class="grid md:grid-cols-3 gap-4"></div>
        </div>
        <div class="glass card rounded-3xl p-6">
          <div class="flex justify-between items-center mb-5"><div><h2 class="font-display font-extrabold text-lg">Aksi Cepat</h2><p class="text-xs text-slate-500 mt-1">Akses fitur utama</p></div></div>
          <div class="space-y-3">
            <button onclick="showPage('daftar')" class="w-full p-4 rounded-2xl border border-slate-200 hover:border-brand-300 hover:bg-brand-50/40 flex items-center gap-3 text-left"><span class="w-10 h-10 rounded-xl bg-brand-100 text-brand-800 flex items-center justify-center"><i data-lucide="ticket"></i></span><span><b class="block text-sm">Ambil Nomor</b><small class="text-xs text-slate-500">Daftar antrian sidang</small></span></button>
            <button onclick="showPage('cek')" class="w-full p-4 rounded-2xl border border-slate-200 hover:border-brand-300 hover:bg-brand-50/40 flex items-center gap-3 text-left"><span class="w-10 h-10 rounded-xl bg-blue-50 text-blue-600 flex items-center justify-center"><i data-lucide="search"></i></span><span><b class="block text-sm">Cek Tiket</b><small class="text-xs text-slate-500">Lacak posisi antrian</small></span></button>
            <button onclick="showPage('jadwal')" class="w-full p-4 rounded-2xl border border-slate-200 hover:border-brand-300 hover:bg-brand-50/40 flex items-center gap-3 text-left"><span class="w-10 h-10 rounded-xl bg-purple-50 text-purple-600 flex items-center justify-center"><i data-lucide="calendar-days"></i></span><span><b class="block text-sm">Jadwal Sidang</b><small class="text-xs text-slate-500">Lihat agenda hari ini</small></span></button>
          </div>
        </div>
      </div>
    </section>

    <!-- DAFTAR -->
    <section id="page-daftar" class="page hidden">
      <div class="mb-7"><div class="text-brand-700 text-xs font-extrabold uppercase tracking-widest">Pelayanan PTSP</div><h1 class="font-display text-3xl font-extrabold mt-2">Registrasi Antrian</h1><p class="text-slate-500 text-sm mt-1">Isi data dengan benar untuk mendapatkan tiket antrian digital.</p></div>
      <div class="grid xl:grid-cols-5 gap-6">
        <div class="xl:col-span-3 glass card rounded-3xl p-6 sm:p-8">
          <form id="queueForm" class="space-y-5" onsubmit="submitQueue(event)">
            <div><label class="label">Ruang / Jadwal Sidang</label><select id="input-sidang" class="field"></select></div>
            <div class="grid sm:grid-cols-2 gap-4">
              <div><label class="label">Nama Lengkap</label><input id="input-nama" class="field" required placeholder="Nama sesuai identitas"></div>
              <div><label class="label">NIK</label><input id="input-nik" class="field" required maxlength="16" inputmode="numeric" placeholder="16 digit NIK"></div>
            </div>
            <div><label class="label">Peran Dalam Sidang</label><select id="input-peran" class="field"><option>Saksi / Saksi Ahli</option><option>Penggugat / Tergugat / Terdakwa</option><option>Kuasa Hukum / Advokat</option><option>Masyarakat Umum / Penonton</option></select></div>
            <div><label class="label">Nomor Perkara (opsional)</label><input id="input-perkara" class="field" placeholder="Contoh: 123/Pdt.G/2026/PN"></div>
            <div class="grid sm:grid-cols-2 gap-4">
              <div><label class="label">Nomor Telepon (opsional)</label><input id="input-phone" class="field" placeholder="08xxxxxxxxxx"></div>
              <div><label class="label">Catatan (opsional)</label><input id="input-note" class="field" placeholder="Keterangan tambahan"></div>
            </div>
            <label class="flex gap-3 items-start text-xs text-slate-500"><input id="agree" type="checkbox" required class="mt-0.5 accent-yellow-500"><span>Saya memastikan data yang dimasukkan benar dan bersedia mengikuti tata tertib pelayanan pengadilan.</span></label>
            <button class="w-full py-4 rounded-2xl bg-brand-400 hover:bg-brand-300 text-slate-900 font-extrabold shadow-lg shadow-brand-400/20 flex justify-center items-center gap-2"><i data-lucide="ticket"></i> Dapatkan Tiket Antrian</button>
          </form>
        </div>
        <div class="xl:col-span-2">
          <div id="ticket-print" class="ticket rounded-3xl p-6 shadow-xl hidden">
            <div class="text-center border-b border-dashed border-brand-300 pb-5">
              <div class="w-12 h-12 rounded-xl bg-brand-400 mx-auto flex items-center justify-center"><i data-lucide="scale"></i></div>
              <div class="text-xs font-black mt-3">PENGADILAN NEGERI</div>
              <div class="text-[10px] text-slate-500">Tiket Antrian Digital PRAHARSA</div>
              <div id="ticket-number" class="font-display text-5xl font-black text-brand-600 mt-4">S1-001</div>
              <div id="ticket-name" class="font-extrabold mt-1">Nama</div>
            </div>
            <div class="py-5 space-y-3 text-xs">
              <div class="flex justify-between gap-4"><span class="text-slate-500">Ruang</span><b id="ticket-room">-</b></div>
              <div class="flex justify-between gap-4"><span class="text-slate-500">Peran</span><b id="ticket-role">-</b></div>
              <div class="flex justify-between gap-4"><span class="text-slate-500">Waktu</span><b id="ticket-time">-</b></div>
              <div class="flex justify-between gap-4"><span class="text-slate-500">Perkara</span><b id="ticket-case">-</b></div>
            </div>
            <div id="qrcode" class="flex justify-center py-2"></div>
            <p class="text-[10px] text-center text-slate-400">Simpan kode tiket untuk mengecek posisi antrian.</p>
            <div class="grid grid-cols-2 gap-2 mt-5 no-print">
              <button onclick="window.print()" class="py-3 rounded-xl bg-slate-900 text-white text-xs font-bold flex justify-center gap-2"><i data-lucide="printer" class="w-4 h-4"></i> Cetak</button>
              <button onclick="showPage('cek')" class="py-3 rounded-xl bg-brand-400 text-slate-900 text-xs font-bold">Cek Tiket</button>
            </div>
          </div>
          <div id="ticket-empty" class="glass rounded-3xl p-10 text-center border-dashed">
            <div class="w-16 h-16 mx-auto rounded-2xl bg-brand-50 text-brand-700 flex items-center justify-center"><i data-lucide="ticket" class="w-8 h-8"></i></div>
            <h3 class="font-extrabold mt-4">Tiket Anda akan muncul di sini</h3>
            <p class="text-xs text-slate-500 mt-2">Setelah formulir berhasil dikirim.</p>
          </div>
        </div>
      </div>
    </section>

    <!-- MONITOR -->
    <section id="page-monitor" class="page hidden">
      <div class="mb-7"><div class="text-brand-700 text-xs font-extrabold uppercase tracking-widest">Layar Publik</div><h1 class="font-display text-3xl font-extrabold mt-2">Live Monitor Antrian</h1><p class="text-slate-500 text-sm mt-1">Pantau nomor yang sedang dipanggil secara real-time.</p></div>
      <div id="monitorGrid" class="grid md:grid-cols-3 gap-5"></div>
      <div class="glass card rounded-3xl p-6 mt-6">
        <div class="flex items-center justify-between"><div><h2 class="font-display font-extrabold">Informasi Pelayanan</h2><p class="text-xs text-slate-500 mt-1">Perkiraan waktu berdasarkan antrian aktif.</p></div><span class="px-3 py-1.5 rounded-full bg-green-50 text-green-700 text-xs font-bold">● Online</span></div>
        <div class="grid sm:grid-cols-3 gap-4 mt-5">
          <div class="p-4 rounded-2xl bg-slate-50"><div class="text-xs text-slate-400">Estimasi per orang</div><b class="text-lg">± 10 menit</b></div>
          <div class="p-4 rounded-2xl bg-slate-50"><div class="text-xs text-slate-400">Jam layanan</div><b class="text-lg">08.00–16.00</b></div>
          <div class="p-4 rounded-2xl bg-slate-50"><div class="text-xs text-slate-400">Hari layanan</div><b class="text-lg">Senin–Jumat</b></div>
        </div>
      </div>
    </section>

    <!-- CEK -->
    <section id="page-cek" class="page hidden">
      <div class="max-w-2xl mx-auto text-center mb-8"><div class="text-brand-700 text-xs font-extrabold uppercase tracking-widest">Pelacakan Tiket</div><h1 class="font-display text-3xl font-extrabold mt-2">Cek Posisi Antrian</h1><p class="text-slate-500 text-sm mt-1">Masukkan kode tiket yang Anda dapatkan.</p></div>
      <div class="max-w-2xl mx-auto glass card rounded-3xl p-6 sm:p-8">
        <div class="flex gap-2"><input id="searchTicket" class="field uppercase" placeholder="Contoh: S1-001"><button onclick="searchTicket()" class="px-5 rounded-xl bg-brand-400 font-extrabold flex gap-2 items-center"><i data-lucide="search"></i> Lacak</button></div>
        <div id="searchResult" class="mt-5 hidden"></div>
      </div>
    </section>

    <!-- PETUGAS -->
    <section id="page-petugas" class="page hidden">
      <div class="mb-7"><div class="text-brand-700 text-xs font-extrabold uppercase tracking-widest">Operator</div><h1 class="font-display text-3xl font-extrabold mt-2">Panel Petugas</h1><p class="text-slate-500 text-sm mt-1">Kelola pemanggilan dan penyelesaian antrian.</p></div>
      <div class="grid xl:grid-cols-5 gap-6">
        <div class="xl:col-span-2 glass card rounded-3xl p-6">
          <label class="label">Pilih Ruang</label><select id="operatorRoom" class="field" onchange="renderOperator()"></select>
          <button onclick="callNext()" class="w-full mt-4 py-4 rounded-2xl bg-slate-900 hover:bg-slate-800 text-white font-extrabold flex justify-center gap-2"><i data-lucide="volume-2"></i> PANGGIL ANTRIAN BERIKUTNYA</button>
          <div class="mt-5 rounded-3xl bg-brand-50 border border-brand-200 p-6 text-center">
            <div class="text-[10px] font-black uppercase tracking-widest text-brand-800">Sedang Dipanggil</div>
            <div id="operatorCurrent" class="font-display text-5xl font-black text-brand-600 mt-3">-</div>
            <div id="operatorName" class="font-bold mt-2">Tidak ada pemanggilan</div>
            <button id="finishBtn" onclick="finishCurrent()" disabled class="w-full mt-5 py-3 rounded-xl bg-slate-200 text-slate-400 font-bold">Selesaikan Antrian</button>
            <button onclick="repeatCall()" class="w-full mt-2 py-3 rounded-xl bg-white border border-brand-200 text-brand-800 font-bold text-xs">🔊 Panggil Ulang</button>
          </div>
        </div>
        <div class="xl:col-span-3 glass card rounded-3xl p-6">
          <div class="flex justify-between items-center mb-4"><div><h2 class="font-display font-extrabold">Antrian Menunggu</h2><p class="text-xs text-slate-500">Urutan akan dipanggil sesuai pendaftaran.</p></div><span id="operatorCount" class="px-3 py-1 rounded-full bg-brand-100 text-brand-800 text-xs font-bold">0 orang</span></div>
          <div class="overflow-x-auto"><table class="w-full text-sm"><thead><tr class="text-[10px] uppercase text-slate-400 border-b"><th class="text-left p-3">Tiket</th><th class="text-left p-3">Nama</th><th class="text-left p-3">Peran</th><th class="text-left p-3">Waktu</th></tr></thead><tbody id="operatorTable"></tbody></table></div>
        </div>
      </div>
    </section>

    <!-- JADWAL -->
    <section id="page-jadwal" class="page hidden">
      <div class="mb-7"><div class="text-brand-700 text-xs font-extrabold uppercase tracking-widest">Agenda Pengadilan</div><h1 class="font-display text-3xl font-extrabold mt-2">Jadwal Sidang Hari Ini</h1><p class="text-slate-500 text-sm mt-1">Informasi ruang dan perkara yang sedang tersedia.</p></div>
      <div class="grid lg:grid-cols-3 gap-5" id="scheduleGrid"></div>
    </section>

    <!-- LAYANAN -->
    <section id="page-layanan" class="page hidden">
      <div class="mb-7"><div class="text-brand-700 text-xs font-extrabold uppercase tracking-widest">Pelayanan Terpadu Satu Pintu</div><h1 class="font-display text-3xl font-extrabold mt-2">Layanan PTSP</h1><p class="text-slate-500 text-sm mt-1">Pilih layanan yang Anda perlukan.</p></div>
      <div class="grid sm:grid-cols-2 xl:grid-cols-4 gap-5">
        <div class="service-card"><i data-lucide="file-text"></i><b>Informasi Perkara</b><p>Cek informasi dan status perkara.</p></div>
        <div class="service-card"><i data-lucide="folder-open"></i><b>Permohonan Salinan</b><p>Informasi pengajuan salinan dokumen.</p></div>
        <div class="service-card"><i data-lucide="landmark"></i><b>Perdata</b><p>Layanan administrasi perkara perdata.</p></div>
        <div class="service-card"><i data-lucide="gavel"></i><b>Pidana</b><p>Layanan administrasi perkara pidana.</p></div>
        <div class="service-card"><i data-lucide="clipboard-list"></i><b>Permohonan</b><p>Layanan untuk perkara permohonan.</p></div>
        <div class="service-card"><i data-lucide="credit-card"></i><b>Informasi Biaya</b><p>Informasi pembayaran dan biaya perkara.</p></div>
        <div class="service-card"><i data-lucide="calendar-check"></i><b>Jadwal Sidang</b><p>Lihat agenda dan ruang sidang.</p></div>
        <div class="service-card"><i data-lucide="message-circle"></i><b>Pengaduan</b><p>Sampaikan pertanyaan atau pengaduan.</p></div>
      </div>
    </section>

    <!-- RIWAYAT -->
    <section id="page-riwayat" class="page hidden">
      <div class="mb-7"><div class="text-brand-700 text-xs font-extrabold uppercase tracking-widest">Rekam Layanan</div><h1 class="font-display text-3xl font-extrabold mt-2">Riwayat Antrian</h1><p class="text-slate-500 text-sm mt-1">Data tersimpan di browser perangkat ini.</p></div>
      <div class="glass card rounded-3xl p-6 overflow-x-auto">
        <div class="flex justify-between items-center mb-5"><h2 class="font-display font-extrabold">Riwayat Pendaftaran</h2><button onclick="clearHistory()" class="text-xs text-red-600 font-bold">Hapus Riwayat</button></div>
        <table class="w-full text-sm"><thead><tr class="text-[10px] uppercase text-slate-400 border-b"><th class="text-left p-3">Tiket</th><th class="text-left p-3">Nama</th><th class="text-left p-3">Ruang</th><th class="text-left p-3">Waktu</th><th class="text-left p-3">Status</th></tr></thead><tbody id="historyTable"></tbody></table>
      </div>
    </section>

    <!-- BANTUAN -->
    <section id="page-bantuan" class="page hidden">
      <div class="max-w-3xl mx-auto">
        <div class="text-center mb-8"><div class="text-brand-700 text-xs font-extrabold uppercase tracking-widest">Pusat Bantuan</div><h1 class="font-display text-3xl font-extrabold mt-2">Pertanyaan Umum</h1><p class="text-slate-500 text-sm mt-1">Panduan singkat penggunaan sistem antrian.</p></div>
        <div class="space-y-3">
          <details class="glass card rounded-2xl p-5"><summary class="font-bold cursor-pointer">Bagaimana cara mengambil nomor antrian?</summary><p class="text-sm text-slate-500 mt-3">Buka menu Daftar Antrian, pilih ruang sidang, isi identitas, lalu tekan Dapatkan Tiket Antrian.</p></details>
          <details class="glass card rounded-2xl p-5"><summary class="font-bold cursor-pointer">Bagaimana mengetahui posisi antrian?</summary><p class="text-sm text-slate-500 mt-3">Buka Cek Tiket dan masukkan kode tiket seperti S1-001. Sistem akan menampilkan posisi atau status tiket.</p></details>
          <details class="glass card rounded-2xl p-5"><summary class="font-bold cursor-pointer">Apakah tiket bisa dicetak?</summary><p class="text-sm text-slate-500 mt-3">Bisa. Setelah tiket dibuat, gunakan tombol Cetak untuk mencetak atau menyimpan sebagai PDF melalui dialog printer browser.</p></details>
          <details class="glass card rounded-2xl p-5"><summary class="font-bold cursor-pointer">Apakah data tersimpan setelah browser ditutup?</summary><p class="text-sm text-slate-500 mt-3">Versi ini menggunakan localStorage browser. Untuk sistem produksi yang dipakai banyak perangkat, diperlukan backend/database seperti Flask, PHP, Node.js, atau Laravel.</p></details>
        </div>
      </div>
    </section>

  </main>
</div>

<!-- CSS helper -->
<style>
.label{display:block;font-size:11px;font-weight:800;text-transform:uppercase;letter-spacing:.08em;color:#64748b;margin-bottom:8px}
.field{width:100%;padding:13px 15px;border:1px solid #e2e8f0;background:#fff;border-radius:13px;outline:none;font-size:13px;font-weight:600;transition:.2s}
.field:focus{border-color:#f5b800;box-shadow:0 0 0 3px rgba(245,184,0,.14)}
.service-card{background:#fff;border:1px solid #e2e8f0;border-radius:22px;padding:22px;box-shadow:0 10px 30px rgba(15,23,42,.05);transition:.2s}
.service-card:hover{transform:translateY(-3px);border-color:#ffd43b}
.service-card svg{width:30px;height:30px;color:#d99b00;margin-bottom:18px}
.service-card b{display:block;font-size:15px}
.service-card p{font-size:12px;color:#64748b;margin-top:6px;line-height:1.6}
</style>

<script>
const defaultState = {
  schedules:{
    S1:{room:"Ruang Cakra",caseNo:"123/Pdt.G/2026/PN",type:"Perdata",time:"09.00 WIB"},
    S2:{room:"Ruang Garuda",caseNo:"456/Pid.B/2026/PN",type:"Pidana",time:"10.00 WIB"},
    S3:{room:"Ruang Kartika",caseNo:"789/Pdt.P/2026/PN",type:"Permohonan",time:"11.00 WIB"}
  },
  queues:{S1:[],S2:[],S3:[]},
  counters:{S1:1,S2:1,S3:1},
  current:{S1:null,S2:null,S3:null},
  history:[],
  done:0
};

let state = JSON.parse(localStorage.getItem("praharSAState") || "null") || defaultState;

function save(){localStorage.setItem("praharSAState",JSON.stringify(state))}
function icons(){lucide.createIcons()}
function toast(msg,type="success"){
  const el=document.createElement("div");
  el.className=`px-4 py-3 rounded-2xl shadow-xl text-sm font-bold border bg-white ${type==="error"?"border-red-200 text-red-600":"border-brand-200 text-slate-800"} flex items-center gap-2`;
  el.innerHTML=`<i data-lucide="${type==="error"?"circle-alert":"circle-check"}" class="w-4 h-4"></i>${msg}`;
  document.getElementById("toast").appendChild(el); icons();
  setTimeout(()=>{el.style.opacity="0";el.style.transform="translateX(20px)";setTimeout(()=>el.remove(),250)},3000)
}
function toggleSidebar(){
  document.getElementById("sidebar").classList.toggle("-translate-x-full");
  document.getElementById("sidebarOverlay").classList.toggle("hidden");
}
function showPage(page){
  document.querySelectorAll("main section").forEach(s=>s.classList.add("hidden"));
  const target=document.getElementById("page-"+page);
  if(target) target.classList.remove("hidden");
  document.querySelectorAll(".nav-item").forEach(n=>{
    n.classList.toggle("active",n.dataset.page===page);
    if(n.dataset.page!==page)n.classList.add("text-slate-600");
  });
  const labels={dashboard:"Dashboard",daftar:"Daftar Antrian",monitor:"Live Monitor",cek:"Cek Tiket",petugas:"Panel Petugas",jadwal:"Jadwal Sidang",layanan:"Layanan PTSP",riwayat:"Riwayat",bantuan:"Bantuan"};
  document.getElementById("breadcrumb").textContent="PRAHARSA / "+labels[page];
  if(window.innerWidth<1024){document.getElementById("sidebar").classList.add("-translate-x-full");document.getElementById("sidebarOverlay").classList.add("hidden")}
  if(page==="dashboard")renderDashboard();
  if(page==="monitor")renderMonitor();
  if(page==="petugas")renderOperator();
  if(page==="jadwal")renderSchedules();
  if(page==="riwayat")renderHistory();
  icons();
}
function totalQueues(){return Object.values(state.queues).reduce((a,b)=>a+b.length,0)}
function totalCurrent(){return Object.values(state.current).filter(Boolean).length}
function renderDashboard(){
  document.getElementById("stat-total").textContent=state.history.length;
  document.getElementById("stat-waiting").textContent=totalQueues();
  document.getElementById("stat-done").textContent=state.done;
  document.getElementById("dashboardRooms").innerHTML=Object.entries(state.schedules).map(([id,x])=>{
    const c=state.current[id];
    return `<div class="rounded-2xl border border-slate-200 p-4 ${c?"bg-brand-50/50":"bg-white"}">
      <div class="flex justify-between"><span class="text-[10px] font-black uppercase text-brand-700">${x.type}</span><span class="text-[10px] text-slate-400">${state.queues[id].length} menunggu</span></div>
      <b class="block mt-3">${x.room}</b><div class="text-[11px] text-slate-400 mt-1">${x.caseNo}</div>
      <div class="mt-4 rounded-xl bg-slate-50 p-3 text-center"><div class="text-[9px] uppercase text-slate-400 font-bold">Dipanggil</div><div class="text-2xl font-black text-brand-600">${c?c.nomor:"-"}</div><div class="text-[10px] font-bold truncate">${c?c.nama:"Belum ada panggilan"}</div></div>
    </div>`
  }).join(""); icons()
}
function fillRoomSelects(){
  const opts=Object.entries(state.schedules).map(([id,x])=>`<option value="${id}">${x.room} — ${x.type}</option>`).join("");
  document.getElementById("input-sidang").innerHTML=opts;
  document.getElementById("operatorRoom").innerHTML=opts;
}
function submitQueue(e){
  e.preventDefault();
  const id=document.getElementById("input-sidang").value;
  const name=document.getElementById("input-nama").value.trim();
  const nik=document.getElementById("input-nik").value.trim();
  if(!/^\d{16}$/.test(nik)){toast("NIK harus terdiri dari 16 digit.","error");return}
  const nomor=`${id}-${String(state.counters[id]).padStart(3,"0")}`;
  state.counters[id]++;
  const item={
    nomor,nama:name,nik,
    peran:document.getElementById("input-peran").value,
    perkara:document.getElementById("input-perkara").value.trim()||state.schedules[id].caseNo,
    phone:document.getElementById("input-phone").value.trim(),
    note:document.getElementById("input-note").value.trim(),
    room:state.schedules[id].room,
    id,
    time:new Date().toLocaleTimeString("id-ID",{hour:"2-digit",minute:"2-digit"})
  };
  state.queues[id].push(item);
  state.history.unshift({...item,status:"Menunggu"});
  save();
  document.getElementById("ticket-empty").classList.add("hidden");
  document.getElementById("ticket-print").classList.remove("hidden");
  document.getElementById("ticket-number").textContent=nomor;
  document.getElementById("ticket-name").textContent=name;
  document.getElementById("ticket-room").textContent=item.room;
  document.getElementById("ticket-role").textContent=item.peran;
  document.getElementById("ticket-time").textContent=item.time;
  document.getElementById("ticket-case").textContent=item.perkara;
  const qr=document.getElementById("qrcode"); qr.innerHTML="";
  new QRCode(qr,{text:`PRAHARSA|${nomor}|${name}`,width:110,height:110,colorDark:"#111827",colorLight:"#ffffff"});
  document.getElementById("queueForm").reset();
  document.getElementById("input-sidang").value=id;
  toast(`Tiket ${nomor} berhasil dibuat.`);
  icons();
}
function renderMonitor(){
  document.getElementById("monitorGrid").innerHTML=Object.entries(state.schedules).map(([id,x])=>{
    const c=state.current[id];
    return `<div class="glass card rounded-3xl p-6 border-t-4 border-brand-400">
      <div class="flex justify-between items-center"><span class="px-2.5 py-1 rounded-full bg-brand-100 text-brand-800 text-[10px] font-black uppercase">${x.type}</span><span class="text-xs font-bold text-slate-400">${state.queues[id].length} menunggu</span></div>
      <h2 class="font-display font-extrabold text-xl mt-5">${x.room}</h2><p class="text-xs text-slate-400">${x.caseNo}</p>
      <div class="mt-5 rounded-2xl ${c?"bg-brand-50 pulse":"bg-slate-50"} p-6 text-center border ${c?"border-brand-200":"border-slate-100"}">
        <div class="text-[10px] font-black uppercase tracking-widest text-slate-400">Nomor Saat Ini</div>
        <div class="font-display text-5xl font-black text-brand-600 mt-2">${c?c.nomor:"-"}</div>
        <div class="font-bold text-sm mt-2">${c?c.nama:"Menunggu panggilan"}</div>
      </div>
      <div class="flex justify-between mt-4 text-xs"><span class="text-slate-400">Jadwal</span><b>${x.time}</b></div>
    </div>`
  }).join(""); icons()
}
function renderOperator(){
  const id=document.getElementById("operatorRoom").value;
  const c=state.current[id], q=state.queues[id];
  document.getElementById("operatorCurrent").textContent=c?c.nomor:"-";
  document.getElementById("operatorName").textContent=c?`${c.nama} • ${c.peran}`:"Tidak ada pemanggilan";
  const btn=document.getElementById("finishBtn");
  btn.disabled=!c; btn.className=c?"w-full mt-5 py-3 rounded-xl bg-green-500 hover:bg-green-600 text-white font-bold":"w-full mt-5 py-3 rounded-xl bg-slate-200 text-slate-400 font-bold";
  document.getElementById("operatorCount").textContent=q.length+" orang";
  document.getElementById("operatorTable").innerHTML=q.length?q.map(x=>`<tr class="border-b border-slate-100"><td class="p-3 font-black text-brand-700">${x.nomor}</td><td class="p-3 font-semibold">${x.nama}</td><td class="p-3 text-xs text-slate-500">${x.peran}</td><td class="p-3 text-xs text-slate-400">${x.time}</td></tr>`).join(""):`<tr><td colspan="4" class="p-8 text-center text-slate-400 text-sm">Tidak ada antrian menunggu.</td></tr>`;
  icons()
}
function callNext(){
  const id=document.getElementById("operatorRoom").value;
  if(state.current[id]){toast("Selesaikan antrian saat ini terlebih dahulu.","error");return}
  if(!state.queues[id].length){toast("Tidak ada antrian menunggu.","error");return}
  const item=state.queues[id].shift(); state.current[id]=item;
  state.history=state.history.map(h=>h.nomor===item.nomor?{...h,status:"Sedang Dipanggil"}:h);
  save(); renderOperator(); renderMonitor(); renderDashboard();
  speak(item.nomor,state.schedules[id].room); toast(`Memanggil ${item.nomor} — ${item.nama}`);
}
function finishCurrent(){
  const id=document.getElementById("operatorRoom").value;
  const c=state.current[id]; if(!c)return;
  state.current[id]=null; state.done++;
  state.history=state.history.map(h=>h.nomor===c.nomor?{...h,status:"Selesai"}:h);
  save(); renderOperator(); renderMonitor(); renderDashboard(); toast(`${c.nomor} telah diselesaikan.`);
}
function repeatCall(){
  const id=document.getElementById("operatorRoom").value,c=state.current[id];
  if(!c){toast("Belum ada antrian yang dipanggil.","error");return}
  speak(c.nomor,state.schedules[id].room); toast("Panggilan diulang.");
}
function speak(nomor,room){
  if(!("speechSynthesis" in window))return;
  speechSynthesis.cancel(); const u=new SpeechSynthesisUtterance(`Nomor antrian ${nomor}, silakan menuju ${room}.`);
  u.lang="id-ID";u.rate=.85;speechSynthesis.speak(u)
}
function searchTicket(){
  const code=document.getElementById("searchTicket").value.trim().toUpperCase();
  const box=document.getElementById("searchResult");box.classList.remove("hidden");
  if(!/^(S1|S2|S3)-\d{3}$/.test(code)){box.innerHTML=`<div class="p-5 rounded-2xl bg-red-50 text-red-600 text-sm font-bold">Format tiket tidak valid.</div>`;return}
  const id=code.split("-")[0], current=state.current[id];
  if(current&&current.nomor===code){box.innerHTML=`<div class="p-6 rounded-2xl bg-brand-50 border border-brand-200 text-center"><div class="text-xs font-black text-brand-800">SEDANG DIPANGGIL</div><div class="text-3xl font-black text-brand-600 mt-2">${code}</div><p class="font-bold mt-2">Silakan menuju ${state.schedules[id].room} sekarang.</p></div>`;return}
  const idx=state.queues[id].findIndex(x=>x.nomor===code);
  if(idx>=0){box.innerHTML=`<div class="p-6 rounded-2xl bg-blue-50 border border-blue-100"><b>${code}</b><p class="text-sm mt-2">Posisi Anda: <strong>ke-${idx+1}</strong></p><p class="text-xs text-slate-500 mt-1">Masih ada ${idx} orang sebelum giliran Anda.</p></div>`;return}
  const old=state.history.find(x=>x.nomor===code);
  box.innerHTML=old?`<div class="p-6 rounded-2xl bg-slate-50 border"><b>${code}</b><p class="text-sm mt-2">Status: <strong>${old.status}</strong></p></div>`:`<div class="p-5 rounded-2xl bg-red-50 text-red-600 text-sm font-bold">Tiket tidak ditemukan.</div>`;
}
function renderSchedules(){
  document.getElementById("scheduleGrid").innerHTML=Object.entries(state.schedules).map(([id,x])=>`<div class="glass card rounded-3xl p-6"><div class="flex justify-between"><span class="px-2.5 py-1 rounded-full bg-brand-100 text-brand-800 text-[10px] font-black uppercase">${x.type}</span><span class="text-xs font-bold">${x.time}</span></div><h2 class="font-display text-xl font-extrabold mt-5">${x.room}</h2><p class="font-mono text-xs text-slate-500 mt-2">${x.caseNo}</p><div class="mt-5 flex justify-between items-center"><span class="text-xs text-slate-400">Antrian</span><b>${state.queues[id].length} orang</b></div><button onclick="document.getElementById('input-sidang').value='${id}';showPage('daftar')" class="w-full mt-5 py-3 rounded-xl bg-brand-400 font-bold text-sm">Ambil Antrian</button></div>`).join("");icons()
}
function renderHistory(){
  const body=document.getElementById("historyTable");
  body.innerHTML=state.history.length?state.history.map(x=>`<tr class="border-b border-slate-100"><td class="p-3 font-black text-brand-700">${x.nomor}</td><td class="p-3 font-semibold">${x.nama}</td><td class="p-3 text-xs">${x.room}</td><td class="p-3 text-xs text-slate-400">${x.time}</td><td class="p-3"><span class="px-2 py-1 rounded-full text-[10px] font-bold ${x.status==="Selesai"?"bg-green-50 text-green-700":x.status==="Sedang Dipanggil"?"bg-brand-100 text-brand-800":"bg-blue-50 text-blue-700"}">${x.status}</span></td></tr>`).join(""):`<tr><td colspan="5" class="p-10 text-center text-slate-400">Belum ada riwayat.</td></tr>`
}
function clearHistory(){if(confirm("Hapus semua riwayat?")){state.history=[];save();renderHistory();toast("Riwayat dihapus.")}}
function updateClock(){document.getElementById("clock").textContent=new Date().toLocaleString("id-ID",{weekday:"short",day:"2-digit",month:"short",year:"numeric",hour:"2-digit",minute:"2-digit",second:"2-digit"})}

fillRoomSelects();renderDashboard();updateClock();setInterval(updateClock,1000);icons();
</script>
</body>
</html>
