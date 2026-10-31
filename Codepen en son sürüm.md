<!DOCTYPE html>
<html lang="tr" class="dark">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>ŞantiyePRO - Şantiye ve Proje Takip Sistemi</title>

  <!-- PWA MANIFEST & MOBİL META ETİKETLERİ -->
  <meta name="theme-color" content="#0c0d10">
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
  <meta name="apple-mobile-web-app-title" content="ŞantiyePRO">
  
  <!-- DİNAMİK PWA MANIFEST (BLOB URL İLE TEK DOSYA İÇİNDE ÇALIŞIR) -->
  <script>
    (function() {
      const manifestData = {
        "name": "ŞantiyePRO - Şantiye ve Proje Takip",
        "short_name": "ŞantiyePRO",
        "start_url": "./",
        "display": "standalone",
        "background_color": "#0c0d10",
        "theme_color": "#0080ff",
        "icons": [
          {
            "src": "https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/svgs/solid/helmet-safety.svg",
            "sizes": "192x192",
            "type": "image/svg+xml"
          }
        ]
      };
      const stringified = JSON.stringify(manifestData);
      const blob = new Blob([stringified], {type: 'application/json'});
      const manifestURL = URL.createObjectURL(blob);
      const link = document.createElement('link');
      link.rel = 'manifest';
      link.href = manifestURL;
      document.head.appendChild(link);
    })();
  </script>

  <!-- ERKEN TEMA YÜKLEME BETİĞİ -->
  <script>
    (function() {
      try {
        var savedTheme = localStorage.getItem('insaTakip_theme') || 'dark';
        if (savedTheme === 'light') {
          document.documentElement.classList.remove('dark');
          document.documentElement.classList.add('light');
        } else {
          document.documentElement.classList.remove('light');
          document.documentElement.classList.add('dark');
        }
      } catch (e) {
        document.documentElement.classList.add('dark');
      }
    })();
  </script>

  <!-- Tailwind CSS & FontAwesome CDN -->
  <script src="https://cdn.tailwindcss.com"></script>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  
  <style>
    /* TEMA RENK DEĞİŞKENLERİ */
    .dark {
      --bg-main: #0c0d10;
      --bg-card: #1a1c23;
      --bg-header: #1a1c23;
      --text-header: #ffffff;
      --bg-bottom-nav: #1a1c23;
      --text-bottom-nav-muted: #9e9ea7;
      --border-color: #282a36;
      --text-main: #ffffff;
      --text-muted: #9e9ea7;
      --accent: #0080ff;
      --card-shadow: 0 4px 20px -2px rgba(0, 0, 0, 0.6);
    }
    
    .light {
      --bg-main: #f0f7ff;
      --bg-card: #ffffff;
      --bg-header: #0080ff;
      --text-header: #ffffff;
      --bg-bottom-nav: #0080ff;
      --text-bottom-nav-muted: #e0f2fe;
      --border-color: #dbeafe;
      --text-main: #0a2540;
      --text-muted: #64748b;
      --accent: #0080ff;
      --card-shadow: 0 4px 15px -2px rgba(0, 128, 255, 0.08);
    }

    html, body {
      height: 100%;
      width: 100%;
      margin: 0;
      padding: 0;
      overflow: hidden;
    }

    body {
      background-color: var(--bg-main);
      color: var(--text-main);
      transition: background-color 0.3s ease, color 0.3s ease;
    }

    .theme-card {
      background-color: var(--bg-card);
      border: 1px solid var(--border-color);
      box-shadow: var(--card-shadow);
      transition: all 0.3s ease;
    }

    input, select, textarea {
      background-color: var(--bg-main) !important;
      color: var(--text-main) !important;
      border-color: var(--border-color) !important;
    }

    input::placeholder, textarea::placeholder {
      color: var(--text-muted) !important;
      opacity: 0.7;
    }

    .mobile-nav-btn.active-nav {
      color: #F4C542 !important;
      font-weight: 700;
    }
    .mobile-nav-btn.muted-nav {
      color: var(--text-bottom-nav-muted) !important;
      font-weight: 500;
    }

    .touch-scroll {
      -webkit-overflow-scrolling: touch;
      touch-action: pan-y;
      overscroll-behavior-y: auto;
    }
    /* Cari Hesap Modülü Ek Stilleri */
    .cari-card {
      transition: all 0.25s ease;
    }
    .cari-card:hover {
      border-color: var(--accent);
      transform: translateY(-2px);
    }
    .badge-status-completed {
      background-color: rgba(16, 185, 129, 0.1);
      color: rgb(16, 185, 129);
      border: 1px solid rgba(16, 185, 129, 0.2);
    }
    .badge-status-partial {
      background-color: rgba(245, 158, 11, 0.1);
      color: rgb(245, 158, 11);
      border: 1px solid rgba(245, 158, 11, 0.2);
    }
    .badge-status-pending {
      background-color: rgba(239, 68, 68, 0.1);
      color: rgb(239, 68, 68);
      border: 1px solid rgba(239, 68, 68, 0.2);
    }
  </style>
</head>
<body class="font-sans antialiased">

  <div class="flex h-[100dvh] w-full overflow-hidden">
    <!-- SOL MENÜ (SIDEBAR - MASAÜSTÜ) -->
    <aside class="hidden md:flex flex-col w-64 border-r border-[var(--border-color)] bg-[var(--bg-card)] p-4 transition-all shrink-0">
      <div class="flex items-center gap-3 px-2 py-4 border-b border-[var(--border-color)]">
        <div class="w-10 h-10 rounded-xl bg-[var(--accent)] flex items-center justify-center text-white font-bold text-xl shadow-lg">
          <i class="fa-solid fa-helmet-safety"></i>
        </div>
        <div>
          <h1 class="font-bold text-lg leading-none text-[var(--text-main)]">ŞantiyePRO</h1>
          <span class="text-xs text-[var(--text-muted)]">PWA v1.2</span>
        </div>
      </div>

      <nav class="flex-1 mt-6 space-y-1">
        <button onclick="window.switchTab('home')" id="nav-home" class="nav-btn w-full flex items-center gap-3 px-4 py-3 rounded-xl font-medium transition-all text-left bg-[var(--accent)] text-white shadow-md">
          <i class="fa-solid fa-chart-pie w-5"></i> Ana Sayfa
        </button>
        <button onclick="window.switchTab('material')" id="nav-material" class="nav-btn w-full flex items-center gap-3 px-4 py-3 rounded-xl font-medium transition-all text-left text-[var(--text-muted)] hover:bg-[var(--border-color)] hover:text-[var(--text-main)]">
          <i class="fa-solid fa-boxes-stacked w-5"></i> Malzeme Takibi
        </button>
        <button onclick="window.switchTab('purchased')" id="nav-purchased" class="nav-btn w-full flex items-center gap-3 px-4 py-3 rounded-xl font-medium transition-all text-left text-[var(--text-muted)] hover:bg-[var(--border-color)] hover:text-[var(--text-main)]">
          <i class="fa-solid fa-cart-flatbed w-5"></i> Satın Alınanlar
        </button>
        <button onclick="window.switchTab('categories')" id="nav-categories" class="nav-btn w-full flex items-center gap-3 px-4 py-3 rounded-xl font-medium transition-all text-left text-[var(--text-muted)] hover:bg-[var(--border-color)] hover:text-[var(--text-main)]">
          <i class="fa-solid fa-folder-tree w-5"></i> Kategoriler
        </button>
        <button onclick="window.switchTab('budget')" id="nav-budget" class="nav-btn w-full flex items-center gap-3 px-4 py-3 rounded-xl font-medium transition-all text-left text-[var(--text-muted)] hover:bg-[var(--border-color)] hover:text-[var(--text-main)]">
          <i class="fa-solid fa-wallet w-5"></i> Bütçe & Finans
        </button>
        <button onclick="window.switchTab('settings')" id="nav-settings" class="nav-btn w-full flex items-center gap-3 px-4 py-3 rounded-xl font-medium transition-all text-left text-[var(--text-muted)] hover:bg-[var(--border-color)] hover:text-[var(--text-main)]">
          <i class="fa-solid fa-gear w-5"></i> Ayarlar
        </button>
      </nav>
    </aside>

    <!-- ANA İÇERİK ALANI -->
    <div class="flex-1 flex flex-col min-w-0 min-h-0 h-full overflow-hidden">
      <header class="h-16 border-b border-[var(--border-color)] bg-[var(--bg-header)] text-[var(--text-header)] px-6 flex items-center justify-between transition-all shrink-0">
        <h2 id="page-title" class="text-xl font-bold text-[var(--text-header)]">Ana Sayfa Panel</h2>
      </header>

      <main class="flex-1 min-h-0 overflow-y-auto p-4 sm:p-6 pb-28 touch-scroll">
        
        <!-- 🏠 SEKMESİ: ANA SAYFA -->
        <div id="tab-home" class="tab-content space-y-6">
          <div class="grid grid-cols-2 gap-4">
            <div class="p-4 sm:p-5 rounded-2xl theme-card">
              <div class="flex items-center justify-between text-xs text-[var(--text-muted)] font-medium mb-2">
                <span>TOPLAM GELİR / ANA PARA</span>
                <i class="fa-solid fa-building-columns text-emerald-500 text-base"></i>
              </div>
              <div id="homeTotalIncomeDisplay" class="text-xl sm:text-2xl font-black text-emerald-500">₺0,00</div>
              <div class="text-[10px] sm:text-xs text-emerald-500/80 mt-1 flex items-center gap-1">
                <i class="fa-solid fa-circle-check"></i> Şantiye Kasa Girişi
              </div>
            </div>

            <div class="p-4 sm:p-5 rounded-2xl theme-card">
              <div class="flex items-center justify-between text-xs font-medium mb-2">
                <span class="text-amber-400 font-semibold">PEŞİN HARCAMA</span>
                <i class="fa-solid fa-receipt text-amber-400 text-base"></i>
              </div>
              <div id="homeTotalExpenseDisplay" class="text-xl sm:text-2xl font-black text-amber-400">₺0,00</div>
              <div class="text-[10px] sm:text-xs text-[var(--text-muted)] mt-1">Nakit Çıkan Paralar</div>
            </div>

            <div class="p-4 sm:p-5 rounded-2xl theme-card">
              <div class="flex items-center justify-between text-xs text-[var(--text-muted)] font-medium mb-2">
                <span>TOPLAM BORÇ</span>
                <i class="fa-solid fa-file-invoice-dollar text-rose-500 text-base"></i>
              </div>
              <div id="homeTotalDebtDisplay" class="text-xl sm:text-2xl font-black text-rose-500">₺0,00</div>
              <div class="text-[10px] sm:text-xs text-rose-500/80 mt-1 flex items-center gap-1">
                <i class="fa-solid fa-clock"></i> Ödenecek Kalan Borç
              </div>
            </div>

            <div class="p-4 sm:p-5 rounded-2xl theme-card">
              <div class="flex items-center justify-between text-xs text-[var(--text-muted)] font-medium mb-2">
                <span>HAZIR NAKİT / KASA</span>
                <i class="fa-solid fa-wallet text-[var(--accent)] text-base"></i>
              </div>
              <div id="homeNetCashDisplay" class="text-xl sm:text-2xl font-black text-[var(--accent)]">₺0,00</div>
              <div class="text-[10px] sm:text-xs text-[var(--text-muted)] mt-1">Kullanılabilir Bütçe</div>
            </div>
          </div>
        </div>

        <!-- 📦 SEKMESİ: MALZEME EKLEME SAYFASI -->
        <div id="tab-material" class="tab-content hidden space-y-6">
          <div class="flex items-center gap-2.5 mb-1">
            <div class="w-8 h-8 rounded-lg bg-[var(--accent)]/10 flex items-center justify-center text-[var(--accent)]">
              <i class="fa-solid fa-helmet-safety text-base"></i>
            </div>
            <h2 class="text-xl font-bold text-[var(--text-main)] tracking-tight">Malzeme Yönetimi</h2>
          </div>

          <div class="theme-card rounded-2xl p-4 sm:p-5 border border-[var(--border-color)] space-y-4 max-w-lg shadow-sm">
            <div class="flex items-center justify-between pb-2.5 border-b border-[var(--border-color)]">
              <div class="flex items-center gap-2">
                <i class="fa-solid fa-circle-plus text-[var(--accent)] text-sm"></i>
                <h3 class="font-semibold text-xs sm:text-sm text-[var(--text-main)] uppercase tracking-wider">Yeni Kalem Ekle</h3>
              </div>
            </div>

            <form id="inlineMaterialForm" onsubmit="window.handleInlineMaterialSubmit(event)" class="space-y-3">
              <div>
                <label class="block text-[11px] font-medium text-[var(--text-muted)] mb-1">Kategori</label>
                <select id="inlineMatStage" onchange="updateInlineCategoryOptions()" class="w-full px-3 py-2 rounded-xl focus:outline-none focus:border-[var(--accent)] text-xs sm:text-sm transition-all"></select>
              </div>

              <div>
                <label class="block text-[11px] font-medium text-[var(--text-muted)] mb-1">Kalem Adı (Listeden Seç)</label>
                <select id="inlineMatCategory" class="w-full px-3 py-2 rounded-xl focus:outline-none focus:border-[var(--accent)] text-xs sm:text-sm transition-all"></select>
              </div>

              <div>
                <label class="block text-[11px] font-medium text-[var(--text-muted)] mb-1">İstenen Miktar & Fiyat</label>
                <div class="grid grid-cols-3 gap-2">
                  <input type="number" id="inlineMatAmount" required placeholder="Miktar" step="any" min="0" class="w-full px-3 py-2 rounded-xl focus:outline-none focus:border-[var(--accent)] text-xs sm:text-sm transition-all">
                  
                  <select id="inlineMatUnit" class="w-full px-2 py-2 rounded-xl focus:outline-none focus:border-[var(--accent)] text-xs sm:text-sm transition-all">
                    <option value="Adet">Adet</option>
                    <option value="Kova">Kova</option>
                    <option value="Metretül">Metretül</option>
                    <option value="Metre">Metre</option>
                    <option value="Set">Set</option>
                    <option value="m³">m³</option>
                    <option value="Ton">Ton</option>
                    <option value="m²">m²</option>
                    <option value="Torba">Torba</option>
                    <option value="Kg">Kg</option>
                  </select>

                  <input type="number" id="inlineMatUnitPrice" placeholder="Fiyat (₺)" step="any" min="0" class="w-full px-3 py-2 rounded-xl focus:outline-none focus:border-[var(--accent)] text-xs sm:text-sm transition-all">
                </div>
              </div>

              <div class="flex justify-end pt-2">
                <button type="submit" class="w-full sm:w-auto px-5 py-2.5 bg-[var(--accent)] hover:opacity-90 active:scale-95 text-white font-bold rounded-xl text-xs sm:text-sm shadow-md transition-all flex items-center justify-center gap-2">
                  <i class="fa-solid fa-plus text-xs"></i> Listeye Ekle
                </button>
              </div>
            </form>
          </div>

          <div class="space-y-3 max-w-lg pt-2">
            <div class="flex items-center justify-between px-1">
              <div class="flex items-center gap-2">
                <i class="fa-solid fa-boxes-stacked text-[var(--accent)] text-sm"></i>
                <h3 class="font-bold text-sm sm:text-base text-[var(--text-main)]">Eklenen Malzemeler</h3>
              </div>
              <span id="materialCountBadge" class="px-2.5 py-0.5 rounded-full text-xs font-bold bg-[var(--accent)]/10 text-[var(--accent)] border border-[var(--accent)]/20">0</span>
            </div>

            <div id="materialListContainer" class="space-y-2.5"></div>
          </div>
        </div>
        <!-- 🛒 SEKMESİ: SATIN ALINANLAR -->
        <div id="tab-purchased" class="tab-content hidden space-y-6 max-w-lg">
          <div class="flex items-center justify-between">
            <div class="flex items-center gap-2.5">
              <div class="w-8 h-8 rounded-lg bg-amber-500/10 flex items-center justify-center text-amber-500">
                <i class="fa-solid fa-cart-flatbed text-base"></i>
              </div>
              <div>
                <h2 class="text-xl font-bold text-[var(--text-main)] tracking-tight">Satın Alınanlar</h2>
                <p class="text-xs text-[var(--text-muted)]">Gerçekleşen alımlar ve irsaliye geçmişi.</p>
              </div>
            </div>
            <span id="purchasedCountBadge" class="px-2.5 py-0.5 rounded-full text-xs font-bold bg-amber-500/10 text-amber-500 border border-amber-500/20">0 Kalem</span>
          </div>

          <!-- Özet Kartları -->
          <div class="grid grid-cols-2 gap-3">
            <div class="theme-card p-3.5 rounded-2xl border border-[var(--border-color)]">
              <span class="text-[10px] uppercase font-bold text-[var(--text-muted)] block mb-1">Toplam Alım Tutarı</span>
              <span id="purchaseTotalSumDisplay" class="text-base sm:text-lg font-black text-amber-500">₺0,00</span>
            </div>
            <div class="theme-card p-3.5 rounded-2xl border border-[var(--border-color)]">
              <span class="text-[10px] uppercase font-bold text-[var(--text-muted)] block mb-1">Toplam Kalan Borç</span>
              <span id="purchaseDebtSumDisplay" class="text-base sm:text-lg font-black text-rose-500">₺0,00</span>
            </div>
          </div>

          <!-- Arama ve Filtre -->
          <div class="theme-card p-3 rounded-2xl border border-[var(--border-color)] space-y-2.5">
            <div class="relative">
              <i class="fa-solid fa-magnifying-glass absolute left-3 top-2.5 text-xs text-[var(--text-muted)]"></i>
              <input type="text" id="purchaseSearchInput" oninput="renderPurchasedList()" placeholder="Malzeme veya açıklama ara..." class="w-full pl-8 pr-3 py-1.5 rounded-xl text-xs border focus:outline-none focus:border-[var(--accent)]">
            </div>
            <div>
              <select id="purchaseCategoryFilter" onchange="renderPurchasedList()" class="w-full px-3 py-1.5 rounded-xl text-xs border focus:outline-none focus:border-[var(--accent)]">
                <option value="ALL">Tüm Kategoriler</option>
                <option value="Kaba İnşaat">Kaba İnşaat</option>
                <option value="İnce İnşaat">İnce İnşaat</option>
                <option value="İşçilik">İşçilik</option>
                <option value="Proje ve Ruhsat">Proje ve Ruhsat</option>
              </select>
            </div>
          </div>

          <!-- Liste Taşıyıcısı -->
          <div id="purchasedListContainer" class="space-y-3"></div>
        </div>
      
        <!-- 📇 SEKMESİ: CARİ HESAPLAR -->
        <div id="tab-cari" class="tab-content hidden space-y-6">
          <!-- LİSTE GÖRÜNÜMÜ -->
          <div id="cariMainView" class="space-y-4">
            <div class="flex justify-between items-center theme-card p-4 rounded-2xl border border-[var(--border-color)] shadow-sm">
              <div>
                <h2 class="text-xl font-bold text-[var(--text-main)] flex items-center gap-2.5">
                  <i class="fa-solid fa-address-book text-[var(--accent)]"></i> Cari Hesap Yönetimi
                </h2>
                <p class="text-xs text-[var(--text-muted)] mt-1">Tedarikçi ve taşeron borç takibi, ödeme planları ve cari hareketler.</p>
              </div>
              <button onclick="openNewCariModal()" class="bg-[var(--accent)] hover:opacity-90 active:scale-95 text-white px-4 py-2.5 rounded-xl font-bold shadow-md transition-all flex items-center gap-2 text-xs sm:text-sm cursor-pointer">
                <i class="fa-solid fa-plus text-xs"></i><span>Yeni Cari Ekle</span>
              </button>
            </div>

            <div id="cariListContainer" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
              <!-- Dinamik Cari Kartları -->
            </div>
          </div>

          <!-- DETAY GÖRÜNÜMÜ (Başlangıçta Gizli) -->
          <div id="cariDetailView" class="hidden space-y-4">
            <div class="theme-card p-4 rounded-2xl border border-[var(--border-color)] flex items-center justify-between">
              <button type="button" onclick="closeCariDetail()" class="px-3 py-1.5 rounded-xl bg-[var(--bg-main)] border border-[var(--border-color)] text-xs font-bold text-[var(--text-main)] flex items-center gap-1.5 cursor-pointer">
                <i class="fa-solid fa-arrow-left"></i> Cari Listesine Dön
              </button>
<button type="button" onclick="openCariDirectPaymentModal()" class="px-3 py-1.5 rounded-xl bg-emerald-500 text-slate-950 font-bold text-xs shadow-md flex items-center gap-1.5 cursor-pointer">
  <i class="fa-solid fa-cash-register"></i> Ödeme Yap
</button>
              <div class="flex items-center gap-2">
                <button type="button" onclick="openCariDebtModal()" class="px-3 py-1.5 rounded-xl bg-rose-500 text-white text-xs font-bold shadow-md flex items-center gap-1.5 cursor-pointer">
                  <i class="fa-solid fa-plus"></i> Borç Ekle
                </button>
                <button type="button" onclick="deleteCari(activeCariId)" class="px-3 py-1.5 rounded-xl bg-rose-500/10 text-rose-500 border border-rose-500/30 text-xs font-bold flex items-center gap-1.5 cursor-pointer">
                  <i class="fa-solid fa-trash-can"></i> Sil
                </button>
              </div>
            </div>

            <div class="theme-card p-5 rounded-2xl border border-[var(--border-color)] space-y-4">
              <div class="flex justify-between items-start">
                <div>
                  <span id="detailCariPhone" class="text-xs text-[var(--accent)] font-semibold"></span>
                  <h3 id="detailCariNameTitle" class="text-lg font-black text-[var(--text-main)]"></h3>
                  <p id="detailCariDesc" class="text-xs text-[var(--text-muted)] mt-0.5"></p>
                </div>
                <div class="text-right">
                  <div id="detailNetBalance" class="text-lg font-black text-rose-500">₺0,00</div>
                  <div class="text-[10px] text-[var(--text-muted)]">Kalan Bakiye</div>
                </div>
              </div>

              <div class="grid grid-cols-2 gap-3 pt-3 border-t border-[var(--border-color)] text-xs">
                <div>Toplam Borç: <span id="detailTotalDebt" class="font-bold text-[var(--text-main)]">₺0,00</span></div>
                <div class="text-right">Toplam Ödenen: <span id="detailTotalPaid" class="font-bold text-emerald-500">₺0,00</span></div>
              </div>
            </div>

            <!-- Hareketler ve Planlar -->
            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
              <div class="theme-card p-4 rounded-2xl border border-[var(--border-color)] space-y-3">
                <h4 class="font-bold text-xs uppercase tracking-wider text-[var(--text-muted)]">Borç & Ödeme Hareketleri</h4>
                <div id="detailTransactionsContainer" class="space-y-2"></div>
              </div>
              <div class="theme-card p-4 rounded-2xl border border-[var(--border-color)] space-y-3">
                <h4 class="font-bold text-xs uppercase tracking-wider text-[var(--text-muted)]">Ödeme Planları / Taksitler</h4>
                <div id="detailPaymentPlansContainer" class="space-y-2"></div>
              </div>
            </div>
          </div>
        </div>
      

        <!-- 💰 SEKMESİ: BÜTÇE & FİNANS MODÜLÜ -->
        <div id="tab-budget" class="tab-content hidden space-y-5 max-w-lg">
          <div class="flex items-center gap-2.5">
            <div class="w-8 h-8 rounded-lg bg-emerald-500/10 flex items-center justify-center text-emerald-500">
              <i class="fa-solid fa-wallet text-base"></i>
            </div>
            <h2 class="text-xl font-bold text-[var(--text-main)] tracking-tight">Bütçe & Kasa Yönetimi</h2>
          </div>

          <!-- Finansal Özet Kartları -->
          <div class="grid grid-cols-2 gap-3">
            <div class="theme-card p-3.5 rounded-2xl border border-[var(--border-color)]">
              <span class="text-[10px] uppercase font-bold text-[var(--text-muted)] block mb-1">Toplam Kasa Girişi</span>
              <span id="budgetTotalIncomeDisplay" class="text-base sm:text-lg font-black text-emerald-500">₺0,00</span>
            </div>
            <div class="theme-card p-3.5 rounded-2xl border border-[var(--border-color)]">
              <span class="text-[10px] uppercase font-bold text-[var(--text-muted)] block mb-1">Toplam Peşin Gider</span>
              <span id="budgetTotalExpenseDisplay" class="text-base sm:text-lg font-black text-amber-500">₺0,00</span>
            </div>
          </div>

          <!-- Kasa Hareketi Ekle Formu -->
          <div class="theme-card rounded-2xl p-4 border border-[var(--border-color)] space-y-3">
            <h3 class="text-xs font-bold uppercase tracking-wider text-[var(--text-main)] flex items-center gap-2">
              <i class="fa-solid fa-money-bill-transfer text-[var(--accent)]"></i> Harici Kasa Hareketi Ekle
            </h3>
            <form onsubmit="window.handleBudgetTransactionSubmit(event)" class="space-y-3">
              <div class="grid grid-cols-2 gap-2">
                <div>
                  <label class="block text-[11px] font-medium text-[var(--text-muted)] mb-1">İşlem Tipi</label>
                  <select id="budgetType" onchange="window.toggleBudgetExpenseCategorySelect()" class="w-full px-3 py-2 rounded-xl text-xs sm:text-sm focus:outline-none focus:border-[var(--accent)] transition-all">
                    <option value="INCOME">Gelir (+ Kasa Girişi)</option>
                    <option value="EXPENSE">Gider (- Kasa Çıkışı)</option>
                  </select>
                </div>
                <div>
                  <label class="block text-[11px] font-medium text-[var(--text-muted)] mb-1">Tutar (₺)</label>
                  <input type="number" id="budgetAmount" step="any" required min="0.01" placeholder="0.00" class="w-full px-3 py-2 rounded-xl text-xs sm:text-sm focus:outline-none focus:border-[var(--accent)] transition-all">
                </div>
              </div>

              <!-- Dinamik Gider Kategorisi Alanı -->
              <div id="budgetExpenseCategoryContainer" class="hidden space-y-3">
                <div>
                  <label class="block text-[11px] font-medium text-[var(--text-muted)] mb-1">Gider Kategorisi</label>
                  <select id="budgetExpenseCategory" onchange="window.updateBudgetSubItemsList()" class="w-full px-3 py-2 rounded-xl text-xs sm:text-sm focus:outline-none focus:border-[var(--accent)] transition-all">
                    <option value="Kaba İnşaat">Kaba İnşaat</option>
                    <option value="İnce İnşaat">İnce İnşaat</option>
                    <option value="İşçilik">İşçilik</option>
                    <option value="Proje ve Ruhsat">Proje ve Ruhsat</option>
                  </select>
                </div>

                <div>
                  <label class="block text-[11px] font-medium text-[var(--text-muted)] mb-1">Kalem / İş Seçimi (Kütüphaneden)</label>
                  <select id="budgetSubItemSelect" onchange="window.handleBudgetSubItemChange()" class="w-full px-3 py-2 rounded-xl text-xs sm:text-sm focus:outline-none focus:border-[var(--accent)] transition-all">
                  </select>
                </div>
              </div>

              <div>
                <label class="block text-[11px] font-medium text-[var(--text-muted)] mb-1">Açıklama / Detay Notu</label>
                <input type="text" id="budgetDesc" required placeholder="Örn: 15 Günlük Kalıpçı Avansı, Fatura No..." class="w-full px-3 py-2 rounded-xl text-xs sm:text-sm focus:outline-none focus:border-[var(--accent)] transition-all">
              </div>

              <button type="submit" class="w-full py-2.5 bg-emerald-500 hover:opacity-90 active:scale-95 text-slate-950 font-bold rounded-xl text-xs sm:text-sm transition-all shadow-md cursor-pointer">
                İşlemi Kasaya Kaydet
              </button>
            </form>
          </div>

          <!-- KASA HAREKET DÖKÜMÜ & TARİH FİLTRELİ SEKMELER -->
          <div class="space-y-3">
            <div class="flex items-center justify-between px-1">
              <div class="flex items-center gap-2">
                <h3 class="text-xs sm:text-sm font-bold text-[var(--text-main)] uppercase tracking-wider">Kasa Hareket Dökümü</h3>
                <!-- BAŞLIK HİZASINDA UFAK TARİH İKONU BUTONU -->
                <button type="button" onclick="window.toggleDateFilterPicker()" id="datePickerToggleBtn" class="p-1.5 rounded-lg border border-[var(--border-color)] bg-[var(--bg-card)] text-[var(--text-muted)] hover:text-[var(--accent)] hover:border-[var(--accent)]/40 transition-all flex items-center gap-1.5" title="Tarih Filtresi">
                  <i class="fa-solid fa-calendar-days text-xs"></i>
                  <span id="dateFilterActiveIndicator" class="w-2 h-2 rounded-full bg-[var(--accent)] hidden"></span>
                </button>
              </div>

              <span id="budgetCountBadge" class="px-2 py-0.5 rounded-full text-[10px] font-bold bg-[var(--accent)]/10 text-[var(--accent)]">0 İşlem</span>
            </div>

            <!-- UFAK AÇILIR TARİH SEÇME PANELİ -->
            <div id="datePickerContainer" class="hidden theme-card p-3 rounded-2xl border border-[var(--border-color)] space-y-2.5 transition-all">
              <div class="flex items-center justify-between text-[11px] font-bold text-[var(--text-muted)] pb-1 border-b border-[var(--border-color)]">
                <span><i class="fa-solid fa-filter mr-1 text-[var(--accent)]"></i> Tarih Aralığı Süzgeci</span>
                <button type="button" onclick="window.toggleDateFilterPicker()" class="text-xs text-[var(--text-muted)] hover:text-[var(--text-main)]"><i class="fa-solid fa-xmark"></i></button>
              </div>

              <div class="grid grid-cols-2 gap-2">
                <div>
                  <label class="block text-[10px] font-medium text-[var(--text-muted)] mb-0.5">Başlangıç</label>
                  <input type="date" id="budgetStartDate" class="w-full px-2.5 py-1.5 rounded-lg text-xs border focus:outline-none focus:border-[var(--accent)]">
                </div>
                <div>
                  <label class="block text-[10px] font-medium text-[var(--text-muted)] mb-0.5">Bitiş</label>
                  <input type="date" id="budgetEndDate" class="w-full px-2.5 py-1.5 rounded-lg text-xs border focus:outline-none focus:border-[var(--accent)]">
                </div>
              </div>

              <div class="flex items-center justify-end gap-2 pt-1">
                <button type="button" onclick="window.clearDateFilter()" class="px-3 py-1 rounded-lg border border-[var(--border-color)] text-[11px] font-semibold text-[var(--text-muted)] hover:text-rose-500 transition-all">
                  Temizle
                </button>
                <button type="button" onclick="window.applyDateFilter()" class="px-4 py-1 rounded-lg bg-[var(--accent)] text-white text-[11px] font-bold shadow-sm hover:opacity-90 transition-all">
                  Uygula
                </button>
              </div>
            </div>

            <!-- GELİR / GİDER SÜZGEÇ SEKMELERİ -->
            <div class="flex items-center gap-1.5 p-1 rounded-xl bg-[var(--bg-main)] border border-[var(--border-color)]">
              <button type="button" onclick="window.setBudgetFilter('ALL')" id="filter-btn-ALL" class="budget-filter-btn flex-1 py-1.5 rounded-lg text-xs font-bold transition-all bg-[var(--accent)] text-white shadow-sm">
                Tümü
              </button>
              <button type="button" onclick="window.setBudgetFilter('INCOME')" id="filter-btn-INCOME" class="budget-filter-btn flex-1 py-1.5 rounded-lg text-xs font-bold transition-all text-[var(--text-muted)] hover:text-emerald-500">
                <i class="fa-solid fa-arrow-down-left mr-1 text-[10px]"></i> Gelirler
              </button>
              <button type="button" onclick="window.setBudgetFilter('EXPENSE')" id="filter-btn-EXPENSE" class="budget-filter-btn flex-1 py-1.5 rounded-lg text-xs font-bold transition-all text-[var(--text-muted)] hover:text-rose-500">
                <i class="fa-solid fa-arrow-up-right mr-1 text-[10px]"></i> Giderler
              </button>
            </div>

            <div id="budgetTransactionsContainer" class="space-y-2"></div>
          </div>
        </div>
        <!-- 📁 SEKMESİ: KATEGORİLER -->
        <div id="tab-categories" class="tab-content hidden space-y-4 max-w-lg">
          <div class="flex items-center justify-between theme-card p-4 rounded-2xl border border-[var(--border-color)]">
            <div>
              <h2 class="text-xl font-bold text-[var(--text-main)] flex items-center gap-2">
                <i class="fa-solid fa-folder-tree text-[var(--accent)]"></i> Kategori & Kalemler
              </h2>
              <p class="text-xs text-[var(--text-muted)] mt-0.5">Şantiye malzeme ve işçilik kütüphanesini yönetin.</p>
            </div>
          </div>

          <!-- Yeni Ana Kategori Ekleme Formu -->
          <div class="theme-card p-4 rounded-2xl border border-[var(--border-color)] space-y-3">
            <h3 class="font-bold text-xs uppercase tracking-wider text-[var(--text-main)]">Yeni Ana Kategori Ekle</h3>
            <form onsubmit="addNewMainCategory(event)" class="flex gap-2">
              <input type="text" id="newMainCategoryInput" placeholder="Kategori adı (Örn: Çatı İşleri)..." required class="flex-1 px-3 py-2 rounded-xl text-xs sm:text-sm border focus:outline-none focus:border-[var(--accent)]">
              <button type="submit" class="px-4 py-2 bg-[var(--accent)] text-white font-bold rounded-xl text-xs sm:text-sm shadow-md cursor-pointer">Ekle</button>
            </form>
          </div>

          <!-- Kategorilerin Listelendiği Ana Alan -->
          <div id="categoriesContainer" class="space-y-3"></div>
        </div>

        <!-- ⚙️ SEKMESİ: AYARLAR -->
        <div id="tab-settings" class="tab-content hidden space-y-4 max-w-lg">
          
          <!-- MİNİMALİST GÖRÜNÜM VE TEMA KARTI (SAĞ/SOL TOGGLE BUTONLU) -->
          <div class="theme-card rounded-2xl p-4 border border-[var(--border-color)] flex items-center justify-between">
            <div class="flex items-center gap-3">
              <div class="w-8 h-8 rounded-lg bg-[var(--accent)]/10 flex items-center justify-center text-[var(--accent)]">
                <i id="settings-theme-icon" class="fa-solid fa-moon text-sm"></i>
              </div>
              <h3 class="text-xs sm:text-sm font-bold text-[var(--text-main)]">Görünüm ve Tema</h3>
            </div>

            <!-- SAĞ / SOL TOGGLE BUTONU -->
            <button type="button" onclick="window.toggleTheme()" class="relative w-20 h-8 rounded-xl bg-[var(--bg-main)] border border-[var(--border-color)] p-1 flex items-center transition-all cursor-pointer select-none">
              <div id="theme-toggle-knob" class="w-7 h-6 rounded-lg bg-[var(--accent)] text-white text-[10px] flex items-center justify-center font-bold shadow-md transition-all duration-300 transform translate-x-0">
                <i id="knob-icon" class="fa-solid fa-moon"></i>
              </div>
              <span id="knob-label-dark" class="absolute left-2.5 text-[10px] font-bold text-[var(--text-muted)] pointer-events-none">Koyu</span>
              <span id="knob-label-light" class="absolute right-2.5 text-[10px] font-bold text-[var(--text-muted)] pointer-events-none">Açık</span>
            </button>
          </div>

          <!-- SİSTEM VERİLERİNİ SIFIRLAMA KARTI -->
          <div class="theme-card rounded-2xl p-4 border border-[var(--border-color)] flex items-center justify-between">
            <div class="flex items-center gap-3">
              <div class="w-8 h-8 rounded-lg bg-rose-500/10 flex items-center justify-center text-rose-500">
                <i class="fa-solid fa-rotate-left text-sm"></i>
              </div>
              <h3 class="text-xs sm:text-sm font-bold text-[var(--text-main)]">Sistem Verilerini Sıfırla</h3>
            </div>
            <button type="button" onclick="window.resetSystemData()" class="px-3 py-1.5 bg-rose-500/10 border border-rose-500/30 text-rose-500 hover:bg-rose-500 hover:text-white font-bold text-xs rounded-xl transition-all active:scale-95 cursor-pointer flex items-center gap-1.5 shrink-0">
              <i class="fa-solid fa-trash-can text-[11px]"></i> Sıfırla
            </button>
          </div>

        </div>

      </main>
    </div>
  </div>

  <!-- MOBİL ALT GEZİNME ÇUBUĞU -->
  <div class="md:hidden fixed bottom-0 left-0 right-0 h-16 bg-[var(--bg-bottom-nav)] border-t border-[var(--border-color)] flex items-center justify-around z-40 px-1 transition-all">
    <button onclick="window.switchTab('home')" id="mobile-nav-home" class="mobile-nav-btn active-nav flex flex-col items-center justify-center w-full h-full transition-all">
      <i class="fa-solid fa-chart-pie text-lg mb-1"></i>
      <span class="text-[9px]">Ana Sayfa</span>
    </button>
    <button onclick="window.switchTab('material')" id="mobile-nav-material" class="mobile-nav-btn muted-nav flex flex-col items-center justify-center w-full h-full transition-all">
      <i class="fa-solid fa-boxes-stacked text-lg mb-1"></i>
      <span class="text-[9px]">Malzeme</span>
    </button>
    <button onclick="window.switchTab('purchased')" id="mobile-nav-purchased" class="mobile-nav-btn muted-nav flex flex-col items-center justify-center w-full h-full transition-all">
      <i class="fa-solid fa-cart-flatbed text-lg mb-1"></i>
      <span class="text-[9px]">Satın Alınanlar</span>
    </button>
    <button onclick="window.switchTab('categories')" id="mobile-nav-categories" class="mobile-nav-btn muted-nav flex flex-col items-center justify-center w-full h-full transition-all">
      <i class="fa-solid fa-folder-tree text-lg mb-1"></i>
      <span class="text-[9px]">Kategori</span>
    </button>
    <button onclick="window.switchTab('budget')" id="mobile-nav-budget" class="mobile-nav-btn muted-nav flex flex-col items-center justify-center w-full h-full transition-all">
      <i class="fa-solid fa-wallet text-lg mb-1"></i>
      <span class="text-[9px]">Bütçe</span>
    </button>
    <button onclick="window.switchTab('cari')" id="mobile-nav-cari" class="mobile-nav-btn muted-nav flex flex-col items-center justify-center w-full h-full transition-all">
      <i class="fa-solid fa-address-book text-lg mb-1"></i>
      <span class="text-[9px]">Cari</span>
    </button>

    <button onclick="window.switchTab('settings')" id="mobile-nav-settings" class="mobile-nav-btn muted-nav flex flex-col items-center justify-center w-full h-full transition-all">
      <i class="fa-solid fa-gear text-lg mb-1"></i>
      <span class="text-[9px]">Ayarlar</span>
    </button>
  </div>
  <!-- GERÇEK MALZEME ALIM / FATURA GİRİŞİ MODALI -->
  <div id="purchaseModal" onclick="window.checkPurchaseModalBackdrop(event)" class="fixed inset-0 bg-black/70 z-[85] hidden flex items-center justify-center p-3 sm:p-4 transition-opacity duration-200 backdrop-blur-sm">
      <div class="theme-card w-full max-w-lg rounded-2xl border border-[var(--border-color)] shadow-2xl overflow-hidden flex flex-col max-h-[90vh]">
          <div class="bg-[var(--bg-header)] px-4 py-3 flex justify-between items-center border-b border-[var(--border-color)] shrink-0">
              <div class="flex items-center gap-2">
                  <i class="fa-solid fa-cart-shopping text-amber-500"></i>
                  <h3 class="text-sm font-bold text-[var(--text-header)]">Malzeme Satın Alma ve Cari Eşleme</h3>
              </div>
              <button type="button" onclick="window.togglePurchaseModal(false)" class="text-[var(--text-header)]/80 hover:text-[var(--text-header)] p-1 text-sm transition-colors cursor-pointer" aria-label="Kapat">
                  <i class="fa-solid fa-xmark text-base"></i>
              </button>
          </div>

          <form onsubmit="window.submitPurchaseForm(event)" class="p-4 space-y-3.5 overflow-y-auto flex-1 touch-scroll">
              <input type="hidden" id="purchaseMaterialId">

              <div>
                  <label class="block text-xs font-semibold text-[var(--text-muted)] mb-1">Seçilen Kalem</label>
                  <input type="text" id="purchaseMaterialNameDisplay" disabled class="w-full bg-[var(--bg-main)] border border-[var(--border-color)] rounded-xl px-3 py-2 text-xs sm:text-sm font-semibold text-[var(--text-main)] cursor-not-allowed opacity-75">
              </div>

              <!-- YENİ EKLENEN TEDARİKÇİ / CARİ SEÇİMİ -->
              <div>
                  <label class="block text-xs font-semibold text-[var(--text-muted)] mb-1">Tedarikçi / Cari Firma Seç</label>
                  <select id="purchaseCariSelect" required class="w-full bg-[var(--bg-main)] border border-[var(--border-color)] rounded-xl px-3 py-2 text-xs sm:text-sm text-[var(--text-main)] focus:outline-none focus:border-[var(--accent)] transition-all">
                      <!-- Dinamik Cariler Buraya Dolacak -->
                  </select>
              </div>

              <div class="grid grid-cols-2 gap-3">
                  <div>
                      <label class="block text-xs font-semibold text-[var(--text-muted)] mb-1">Alınan Miktar</label>
                      <input type="number" step="any" min="0.001" id="purchaseQty" required oninput="window.calculatePurchaseTotal()" placeholder="0" class="w-full bg-[var(--bg-main)] border border-[var(--border-color)] rounded-xl px-3 py-2 text-xs sm:text-sm text-[var(--text-main)] focus:outline-none focus:border-[var(--accent)] transition-all">
                  </div>
                  <div>
                      <label class="block text-xs font-semibold text-[var(--text-muted)] mb-1">Gerçek Birim Fiyat (₺)</label>
                      <input type="number" step="any" min="0" id="purchaseUnitPrice" required oninput="window.calculatePurchaseTotal()" placeholder="0.00" class="w-full bg-[var(--bg-main)] border border-[var(--border-color)] rounded-xl px-3 py-2 text-xs sm:text-sm text-[var(--text-main)] focus:outline-none focus:border-[var(--accent)] transition-all">
                  </div>
              </div>

              <div>
                  <label class="block text-xs font-semibold text-[var(--text-muted)] mb-1">Peşin Ödenen / Yapılan Ödeme (₺)</label>
                  <input type="number" step="any" min="0" id="purchaseInitialPayment" value="0" placeholder="0.00" class="w-full bg-[var(--bg-main)] border border-[var(--border-color)] rounded-xl px-3 py-2 text-xs sm:text-sm text-[var(--text-main)] focus:outline-none focus:border-[var(--accent)] transition-all">
              </div>

              <div>
                  <label class="block text-xs font-semibold text-[var(--text-muted)] mb-1">Açıklama / Fatura No</label>
                  <input type="text" id="purchaseDesc" placeholder="Örn: Fatura No: 12345 / Nakit Ödeme" class="w-full bg-[var(--bg-main)] border border-[var(--border-color)] rounded-xl px-3 py-2 text-xs sm:text-sm text-[var(--text-main)] placeholder:text-[var(--text-muted)] focus:outline-none focus:border-[var(--accent)] transition-all">
              </div>

              <div class="bg-[var(--bg-main)] p-3 rounded-xl border border-[var(--border-color)] flex justify-between items-center text-xs">
                  <span class="font-semibold text-[var(--text-muted)]">Hesaplanan Toplam Tutar:</span>
                  <span id="purchaseTotalDisplay" class="font-mono text-base font-black text-amber-500">0,00 ₺</span>
              </div>

              <div class="flex justify-end gap-2.5 pt-2 shrink-0">
                  <button type="button" onclick="window.togglePurchaseModal(false)" class="px-4 py-2 rounded-xl border border-[var(--border-color)] bg-[var(--bg-main)] text-[var(--text-muted)] hover:text-[var(--text-main)] text-xs font-semibold transition-all cursor-pointer">İptal</button>
                  <button type="submit" class="px-5 py-2 rounded-xl bg-amber-500 hover:bg-amber-600 active:scale-95 text-slate-950 font-bold text-xs transition-all shadow-md cursor-pointer">Alımı ve Borcu Kaydet</button>
              </div>
          </form>
      </div>
  </div>
  
  <!-- YENİ CARİ EKLEME MODALI -->
  <div id="newCariModal" class="fixed inset-0 bg-black/70 z-[85] hidden flex items-center justify-center p-3 sm:p-4 transition-opacity duration-200 backdrop-blur-sm">
      <div class="theme-card w-full max-w-lg rounded-2xl border border-[var(--border-color)] shadow-2xl overflow-hidden flex flex-col max-h-[90vh]">
          <div class="bg-[var(--bg-header)] px-4 py-3 flex justify-between items-center border-b border-[var(--border-color)] shrink-0">
              <div class="flex items-center gap-2">
                  <i class="fa-solid fa-user-plus text-[var(--accent)]"></i>
                  <h3 class="text-sm font-bold text-[var(--text-header)]">Yeni Cari Hesap Tanımla</h3>
              </div>
              <button type="button" onclick="closeNewCariModal()" class="text-[var(--text-header)]/80 hover:text-[var(--text-header)] p-1 text-sm transition-colors cursor-pointer">
                  <i class="fa-solid fa-xmark text-base"></i>
              </button>
          </div>

          <form onsubmit="handleNewCariSubmit(event)" class="p-4 space-y-3.5 overflow-y-auto flex-1 touch-scroll">
              <div>
                  <label class="block text-xs font-semibold text-[var(--text-muted)] mb-1">Cari Adı / Firma Ünvanı</label>
                  <input type="text" id="newCariName" required placeholder="Örn: Yılmaz Demir Ticaret" class="w-full bg-[var(--bg-main)] border border-[var(--border-color)] rounded-xl px-3 py-2 text-xs sm:text-sm text-[var(--text-main)] focus:outline-none focus:border-[var(--accent)] transition-all">
              </div>
              <div>
                  <label class="block text-xs font-semibold text-[var(--text-muted)] mb-1">Cari Türü</label>
                  <select id="newCariType" class="w-full bg-[var(--bg-main)] border border-[var(--border-color)] rounded-xl px-3 py-2 text-xs sm:text-sm text-[var(--text-main)] focus:outline-none focus:border-[var(--accent)] transition-all">
                      <option value="Tedarikçi">Tedarikçi (Malzeme)</option>
                      <option value="Taşeron">Taşeron (İşçilik / Hizmet)</option>
                      <option value="Diğer">Diğer</option>
                  </select>
              </div>
              <div>
                  <label class="block text-xs font-semibold text-[var(--text-muted)] mb-1">Telefon / İletişim</label>
                  <input type="text" id="newCariPhone" placeholder="05XX XXX XX XX" class="w-full bg-[var(--bg-main)] border border-[var(--border-color)] rounded-xl px-3 py-2 text-xs sm:text-sm text-[var(--text-main)] focus:outline-none focus:border-[var(--accent)] transition-all">
              </div>
              <div class="flex justify-end gap-2.5 pt-2 shrink-0">
                  <button type="button" onclick="closeNewCariModal()" class="px-4 py-2 rounded-xl border border-[var(--border-color)] bg-[var(--bg-main)] text-[var(--text-muted)] hover:text-[var(--text-main)] text-xs font-semibold transition-all cursor-pointer">İptal</button>
                  <button type="submit" class="px-5 py-2 rounded-xl bg-[var(--accent)] hover:opacity-90 active:scale-95 text-white font-bold text-xs transition-all shadow-md cursor-pointer">Kaydet</button>
              </div>
          </form>
      </div>
  </div>
  <!-- CARİ BORÇ EKLEME MODALI -->
  <div id="cariDebtModal" class="fixed inset-0 bg-black/70 z-[85] hidden flex items-center justify-center p-3 sm:p-4 transition-opacity duration-200 backdrop-blur-sm">
      <div class="theme-card w-full max-w-lg rounded-2xl border border-[var(--border-color)] shadow-2xl overflow-hidden flex flex-col max-h-[90vh]">
          <div class="bg-[var(--bg-header)] px-4 py-3 flex justify-between items-center border-b border-[var(--border-color)] shrink-0">
              <div class="flex items-center gap-2">
                  <i class="fa-solid fa-file-invoice-dollar text-rose-500"></i>
                  <h3 class="text-sm font-bold text-[var(--text-header)]">Cari Hesaba Borç Ekle</h3>
              </div>
              <button type="button" onclick="closeCariDebtModal()" class="text-[var(--text-header)]/80 p-1 text-sm cursor-pointer"><i class="fa-solid fa-xmark text-base"></i></button>
          </div>
          <form onsubmit="handleCariDebtSubmit(event)" class="p-4 space-y-3.5">
              <div>
                  <label class="block text-xs font-semibold text-[var(--text-muted)] mb-1">Borç Tutarı (₺)</label>
                  <input type="number" step="any" min="0.01" id="cariDebtAmount" required placeholder="900" class="w-full bg-[var(--bg-main)] border border-[var(--border-color)] rounded-xl px-3 py-2 text-xs">
              </div>
              <div>
                  <label class="block text-xs font-semibold text-[var(--text-muted)] mb-1">Açıklama / Fatura</label>
                  <input type="text" id="cariDebtDesc" required placeholder="Hafriyat Alımı Kalan Borç" class="w-full bg-[var(--bg-main)] border border-[var(--border-color)] rounded-xl px-3 py-2 text-xs">
              </div>
              <div class="flex justify-end gap-2.5 pt-2">
                  <button type="button" onclick="closeCariDebtModal()" class="px-4 py-2 rounded-xl border bg-[var(--bg-main)] text-xs">İptal</button>
                  <button type="submit" class="px-5 py-2 rounded-xl bg-rose-500 text-white font-bold text-xs">Borcu Kaydet</button>
              </div>
          </form>
      </div>
  </div>

   <!-- PLANLI ÖDEME YAPMA MODALI -->
  <div id="planPaymentModal" class="fixed inset-0 bg-black/70 z-[85] hidden flex items-center justify-center p-3 sm:p-4 transition-opacity duration-200 backdrop-blur-sm">
      <div class="theme-card w-full max-w-lg rounded-2xl border border-[var(--border-color)] shadow-2xl overflow-hidden flex flex-col max-h-[90vh]">
          <div class="bg-[var(--bg-header)] px-4 py-3 flex justify-between items-center border-b border-[var(--border-color)] shrink-0">
              <div class="flex items-center gap-2">
                  <i class="fa-solid fa-cash-register text-[var(--accent)]"></i>
                  <h3 class="text-sm font-bold text-[var(--text-header)]">Planlı Ödeme Yap</h3>
              </div>
              <button type="button" onclick="closePlanPaymentModal()" class="text-[var(--text-header)]/80 hover:text-[var(--text-header)] p-1 text-sm transition-colors cursor-pointer">
                  <i class="fa-solid fa-xmark text-base"></i>
              </button>
          </div>

          <form onsubmit="handlePlanPaymentSubmit(event)" class="p-4 space-y-3.5 overflow-y-auto flex-1 touch-scroll" id="planPaymentForm">
              <!-- Eksik Gizli Alanlar Eklendi -->
              <input type="hidden" id="planPaymentCariId">
              <input type="hidden" id="planPaymentPlanId">
              <input type="hidden" id="planPaymentUniqueId">
              
              <div id="payModalInfo" class="hidden"></div>
              <div id="planPaymentMaxInfo" class="text-xs bg-[var(--bg-main)] p-3 rounded-xl border border-[var(--border-color)] text-[var(--text-muted)]"></div>

              <div>
                  <label class="block text-xs font-semibold text-[var(--text-muted)] mb-1">Ödeme Tutarı (₺)</label>
                  <input type="number" step="any" id="planPaymentAmount" required class="w-full bg-[var(--bg-main)] border border-[var(--border-color)] rounded-xl px-3 py-2 text-xs sm:text-sm text-[var(--text-main)] focus:outline-none focus:border-[var(--accent)] transition-all">
              </div>

              <div>
                  <label class="block text-xs font-semibold text-[var(--text-muted)] mb-1">Gider Kategorisi</label>
                  <select id="planPaymentCategory" required class="w-full bg-[var(--bg-main)] border border-[var(--border-color)] rounded-xl px-3 py-2 text-xs sm:text-sm text-[var(--text-main)] focus:outline-none focus:border-[var(--accent)] transition-all">
                      <!-- Dinamik Kategori Seçenekleri Buraya Dolacak -->
                  </select>
              </div>

              <div>
                  <label class="block text-xs font-semibold text-[var(--text-muted)] mb-1">Ödeme Açıklaması</label>
                  <input type="text" id="planPaymentDesc" required placeholder="Taksit Ödemesi / Fatura No..." class="w-full bg-[var(--bg-main)] border border-[var(--border-color)] rounded-xl px-3 py-2 text-xs sm:text-sm text-[var(--text-main)] focus:outline-none focus:border-[var(--accent)] transition-all">
              </div>

              <div class="flex justify-end gap-2.5 pt-2 shrink-0">
                  <button type="button" onclick="closePlanPaymentModal()" class="px-4 py-2 rounded-xl border border-[var(--border-color)] bg-[var(--bg-main)] text-[var(--text-muted)] hover:text-[var(--text-main)] text-xs font-semibold transition-all cursor-pointer">İptal</button>
                  <button type="submit" id="payModalSubmitBtn" class="px-5 py-2 rounded-xl bg-[var(--accent)] hover:opacity-90 active:scale-95 text-white font-bold text-xs transition-all shadow-md cursor-pointer">Ödemeyi Onayla</button>
              </div>
          </form>
      </div>
  </div>
  <!-- CARİ DOĞRUDAN ÖDEME YAPMA MODALI -->
  <div id="cariDirectPaymentModal" class="fixed inset-0 bg-black/70 z-[85] hidden flex items-center justify-center p-3 sm:p-4 transition-opacity duration-200 backdrop-blur-sm">
      <div class="theme-card w-full max-w-lg rounded-2xl border border-[var(--border-color)] shadow-2xl overflow-hidden flex flex-col max-h-[90vh]">
          <div class="bg-[var(--bg-header)] px-4 py-3 flex justify-between items-center border-b border-[var(--border-color)] shrink-0">
              <div class="flex items-center gap-2">
                  <i class="fa-solid fa-wallet text-emerald-500"></i>
                  <h3 class="text-sm font-bold text-[var(--text-header)]">Cari Hesaba Ödeme Yap</h3>
              </div>
              <button type="button" onclick="closeCariDirectPaymentModal()" class="text-[var(--text-header)]/80 p-1 text-sm cursor-pointer"><i class="fa-solid fa-xmark text-base"></i></button>
          </div>

          <form onsubmit="handleCariDirectPaymentSubmit(event)" class="p-4 space-y-3.5 overflow-y-auto flex-1 touch-scroll">
              <div>
                  <label class="block text-xs font-semibold text-[var(--text-muted)] mb-1">Ödeme Tutarı (₺)</label>
                  <input type="number" step="any" min="0.01" id="cariDirectPayAmount" required placeholder="0.00" class="w-full bg-[var(--bg-main)] border border-[var(--border-color)] rounded-xl px-3 py-2 text-xs sm:text-sm text-[var(--text-main)] focus:outline-none focus:border-[var(--accent)] transition-all">
              </div>

              <div>
                  <label class="block text-xs font-semibold text-[var(--text-muted)] mb-1">Gider Kategorisi</label>
                  <select id="cariDirectPayCategory" required class="w-full bg-[var(--bg-main)] border border-[var(--border-color)] rounded-xl px-3 py-2 text-xs sm:text-sm text-[var(--text-main)] focus:outline-none focus:border-[var(--accent)] transition-all">
                      <option value="Kaba İnşaat">Kaba İnşaat</option>
                      <option value="İnce İnşaat">İnce İnşaat</option>
                      <option value="İşçilik">İşçilik</option>
                      <option value="Proje ve Ruhsat">Proje ve Ruhsat</option>
                  </select>
              </div>

              <div>
                  <label class="block text-xs font-semibold text-[var(--text-muted)] mb-1">Ödeme Açıklaması / Dekont No</label>
                  <input type="text" id="cariDirectPayDesc" required placeholder="Örn: Nakit / Havale Ödemesi" class="w-full bg-[var(--bg-main)] border border-[var(--border-color)] rounded-xl px-3 py-2 text-xs sm:text-sm text-[var(--text-main)] focus:outline-none focus:border-[var(--accent)] transition-all">
              </div>

              <div class="flex justify-end gap-2.5 pt-2 shrink-0">
                  <button type="button" onclick="closeCariDirectPaymentModal()" class="px-4 py-2 rounded-xl border border-[var(--border-color)] bg-[var(--bg-main)] text-[var(--text-muted)] hover:text-[var(--text-main)] text-xs font-semibold transition-all cursor-pointer">İptal</button>
                  <button type="submit" class="px-5 py-2 rounded-xl bg-emerald-500 hover:opacity-95 active:scale-95 text-slate-950 font-bold text-xs transition-all shadow-md cursor-pointer">Ödemeyi Onayla</button>
              </div>
          </form>
      </div>
  </div>

  <!-- JAVASCRIPT KODLARI & PWA SERVICE WORKER KAYDI -->
  <script>
    // PWA Service Worker Otomatik Kaydı
    if ('serviceWorker' in navigator) {
      window.addEventListener('load', () => {
        const swCode = `
          self.addEventListener('install', (e) => {
            self.skipWaiting();
          });
          self.addEventListener('activate', (e) => {
            e.waitUntil(clients.claim());
          });
          self.addEventListener('fetch', (e) => {
            e.respondWith(
              fetch(e.request).catch(() => caches.match(e.request))
            );
          });
        `;
        const blob = new Blob([swCode], {type: 'application/javascript'});
        const swURL = URL.createObjectURL(blob);
        navigator.serviceWorker.register(swURL).catch(() => {});
      });
    }

    // Global Scope Bağlantıları
    window.switchTab = switchTab;
    window.toggleTheme = toggleTheme;
    window.togglePurchaseModal = togglePurchaseModal;
    window.openPurchaseModal = openPurchaseModal;
    window.checkPurchaseModalBackdrop = checkPurchaseModalBackdrop;
    window.calculatePurchaseTotal = calculatePurchaseTotal;
    window.submitPurchaseForm = submitPurchaseForm;
    window.handleInlineMaterialSubmit = handleInlineMaterialSubmit;
    window.handleBudgetTransactionSubmit = handleBudgetTransactionSubmit;
    window.deleteBudgetTransaction = deleteBudgetTransaction;
    window.addNewMainCategory = addNewMainCategory;
    window.deleteMainCategory = deleteMainCategory;
    window.addCustomItemToCategory = addCustomItemToCategory;
    window.removeCategoryItem = removeCategoryItem;
    window.togglePurchaseGroupDetail = togglePurchaseGroupDetail;
    window.updateInlineCategoryOptions = updateInlineCategoryOptions;
    window.renderPurchasedList = renderPurchasedList;
    window.toggleBudgetExpenseCategorySelect = toggleBudgetExpenseCategorySelect;
    window.updateBudgetSubItemsList = updateBudgetSubItemsList;
    window.handleBudgetSubItemChange = handleBudgetSubItemChange;
    window.setBudgetFilter = setBudgetFilter;
    window.toggleDateFilterPicker = toggleDateFilterPicker;
    window.applyDateFilter = applyDateFilter;
    window.clearDateFilter = clearDateFilter;
    window.resetSystemData = resetSystemData;
    window.openNewCariModal = openNewCariModal;
    window.closeNewCariModal = closeNewCariModal;
    window.handleNewCariSubmit = handleNewCariSubmit;
    window.openPlanPaymentModal = openPlanPaymentModal;
    window.closePlanPaymentModal = closePlanPaymentModal;
    window.handlePlanPaymentSubmit = handlePlanPaymentSubmit;
    window.renderCariTab = renderCariTab;
    window.selectCari = selectCari;
    window.renderCariDetail = renderCariDetail;
    window.closeCariDetail = closeCariDetail;
    window.deleteCari = deleteCari;


    const defaultCategoriesData = {
      "Kaba İnşaat": [
        "Hafriyat", "C35 Beton", "İnşaat Demiri (10mm & Karışık)", "Kalıp (Amortisman)", 
        "Ahşap Kereste", "Plywood (18mm)", "Çivi - Demir Teli", "Mıcır", 
        "Tuğla (8.5'lik İç Bölme)", "Tuğla (13.5'lik Dış Bölme)", "Bims (13.5 ' lik Dış Bölme)", 
        "XPS Köpük (3 cm)", "EPS Köpük Mantolama (5 cm)", "Sürme Su Yalıtımı (2K Set)", 
        "Membran (Su Yalıtım)(4mm)", "Sönmüş Kireç+Kum", "Kum (Şap)", "Çimento (50 kg)(Şap)", 
        "Çimento (50 kg)(Sıva-Tuğla)"
      ],
      "İnce İnşaat": [
        "Alçı Sıva (35 kg)", "Saten Alçı(25 kg)", "Alçıpan (12,5 mm)", "C Profil (Tavan/Duvar)", 
        "Duvar Boyası (İç Cephe 15L)", "Seramik (60x120 cm)", "Seramik Yapıştırıcısı (25 kg)", 
        "Seramik Derz Dolgusu (20 kg)", "Laminat Parke (8 mm)", "Oda Kapısı (Amerikan Panel)", 
        "Banyo Doğraması", "Banyo Doğraması(Ebeveyn)", "Mutfak Doğraması", "PVC Doğrama (Profil)", 
        "Cam (4+16+4 Isıcam)", "Alüminyum Korkuluk", "Giriş Çelik Kapı", "Apartman Giriş Kapısı", 
        "Elektrik Kablosu (2,5 NYA)", "Anahtar & Priz Grupları", "PPRC Boru (32 mm)", 
        "PVC Boru (50/110 mm)", "Batarya & Armatür Seti", "Batarya & Armatür Seti(Ebeveyn)", 
        "Doğalgaz Borusu (1\")", "Radyatör (600x1000 Panel)", "Radyatör Vanası (1/2 Köşe)", 
        "Gömme Rezervuar (Set)", "Vitrifiye(Klozet , Lavabo , Duş)", "Lavabo Bataryası", 
        "Sigorta Panosu&Şalt", "Havlupan (50x110 cm)"
      ],
      "İşçilik": [
        "Kalıpçı Yevmiyesi", "Demirci Yevmiyesi", "Duvarcı Yevmiyesi", "Sıvacı Yevmiyesi", 
        "Boyacı Yevmiyesi", "Fayansçı Yevmiyesi", "Tesisatçı İşçiliği", "Elektrikçi İşçiliği", "Genel Amele / Şantiye Şefi"
      ],
      "Proje ve Ruhsat": [
        "Mimari Proje Bedeli", "Statik Proje Bedeli", "Mekanik Proje Bedeli", "Elektrik Proje Bedeli", 
        "Zemin Etüdü Raporu", "Belediye Ruhsat Harçları", "Yapı Denetim Hizmet Bedeli", "Harita Mühendisliği / Kot Kesit"
      ]
    };

      let appData = {
  theme: localStorage.getItem('insaTakip_theme') || 'dark',
  categoriesData: JSON.parse(localStorage.getItem('insaTakip_categories')) || defaultCategoriesData
};

    let materials = JSON.parse(localStorage.getItem('insaTakip_materials')) || [];
    let purchases = JSON.parse(localStorage.getItem('insaTakip_purchases')) || [];
    let budgetTransactions = JSON.parse(localStorage.getItem('insaTakip_budget')) || [];

    if (typeof cariList === 'undefined') {
      var cariList = JSON.parse(localStorage.getItem('insaTakip_cariList')) || [];
    }
    if (!Array.isArray(cariList)) {
      cariList = [];
    }
    if (typeof activeCariId === 'undefined') {
      var activeCariId = null;
    }
    if (!materials) materials = [];
    if (!purchases) purchases = [];
    if (!budgetTransactions) budgetTransactions = [];
    if (!cariList) cariList = [];

    let editingCardId = null; 
    let openCategories = {};  
    let openPurchaseGroups = {}; 
    let currentBudgetFilter = 'ALL';
    let filterStartDate = null;
    let filterEndDate = null;

    function escapeHTML(str) {
      if (!str) return '';
      return String(str)
        .replace(/&/g, '&amp;')
        .replace(/</g, '&lt;')
        .replace(/>/g, '&gt;')
        .replace(/"/g, '&quot;')
        .replace(/'/g, '&#039;');
    }

    function openNewCariModal() {
      const modal = document.getElementById('newCariModal');
      if (modal) {
        modal.classList.remove('hidden');
        document.body.style.overflow = 'hidden';
      }
    }

    function closeNewCariModal() {
      const modal = document.getElementById('newCariModal');
      if (modal) {
        modal.classList.add('hidden');
        document.body.style.overflow = '';
        const form = document.getElementById('newCariForm');
        if (form) form.reset();
      }
    }

    function handleNewCariSubmit(event) {
      event.preventDefault();
      const nameInput = document.getElementById('newCariName');
      const phoneInput = document.getElementById('newCariPhone');
      const typeInput = document.getElementById('newCariType'); 
      const descInput = document.getElementById('newCariDesc'); // Güvenli kontrol (zorunlu değil)

      if (!nameInput) return;
      const name = nameInput.value.trim();
      if (!name) return;

      const newCari = {
        id: generatePaymentId(),
        name: name,
        phone: phoneInput ? phoneInput.value.trim() : '',
        type: typeInput ? typeInput.value : 'Tedarikçi',
        desc: descInput ? descInput.value.trim() : '',   
        transactions: [],
        paymentPlans: [],
        createdAt: getTurkeyLocalDate()
      };

      if (!Array.isArray(cariList)) cariList = [];
      cariList.push(newCari);
      saveData();
      closeNewCariModal();
      renderCariTab();
      showToast('Cari hesap başarıyla oluşturuldu.', 'success');
    }

    function renderCariTab() {
      const container = document.getElementById('cariListContainer');
      const countBadge = document.getElementById('cariCountBadge');
      if (!container) return;

      if (!Array.isArray(cariList)) cariList = [];
      if (countBadge) countBadge.textContent = `${cariList.length} Cari`;

      if (cariList.length === 0) {
        container.innerHTML = `
          <div class="theme-card rounded-2xl p-6 text-center border border-[var(--border-color)] space-y-2">
            <div class="w-12 h-12 rounded-full bg-[var(--accent)]/10 text-[var(--accent)] flex items-center justify-center mx-auto text-xl">
              <i class="fa-solid fa-users"></i>
            </div>
            <p class="text-xs sm:text-sm font-medium text-[var(--text-muted)]">Henüz kayıtlı cari hesap bulunmuyor.</p>
          </div>
        `;
        return;
      }

      container.innerHTML = cariList.map(cari => {
        // Ana borç: Yalnızca DEBT hareketlerinden hesaplanır (paymentPlans kesinlikle eklenmez)
        const totalDebt = (cari.transactions || [])
          .filter(t => t.type === 'DEBT')
          .reduce((sum, t) => sum + (Number(t.amount) || 0), 0);

        // Ödenen: Yalnızca PAYMENT hareketlerinden hesaplanır
        const totalPaid = (cari.transactions || [])
          .filter(t => t.type === 'PAYMENT')
          .reduce((sum, t) => sum + (Number(t.amount) || 0), 0);

        const netBalance = Math.max(0, totalDebt - totalPaid);
        const safeCariId = String(cari.id).replace(/'/g, "\\'");

        return `
          <div onclick="window.selectCari('${safeCariId}')" class="theme-card rounded-2xl p-4 border border-[var(--border-color)] hover:border-[var(--accent)] transition-all cursor-pointer space-y-3">
            <div class="flex items-start justify-between gap-2">
              <div>
                <div class="flex items-center gap-2 mb-1">
                  <span class="px-2 py-0.5 rounded text-[10px] font-semibold bg-[var(--accent)]/10 text-[var(--accent)] border border-[var(--accent)]/20">${escapeHTML(cari.type || 'Tedarikçi')}</span>
                </div>
                <h4 class="font-bold text-sm sm:text-base text-[var(--text-main)]">${escapeHTML(cari.name)}</h4>
                ${cari.phone ? `<div class="text-xs text-[var(--text-muted)] mt-0.5"><i class="fa-solid fa-phone mr-1 text-[var(--accent)]"></i>${escapeHTML(cari.phone)}</div>` : ''}
              </div>
              <div class="text-right">
                <div class="text-xs sm:text-sm font-black text-rose-500">${formatCurrency(netBalance)}</div>
                <div class="text-[10px] text-[var(--text-muted)] mt-0.5">Kalan Bakiye</div>
              </div>
            </div>

            <div class="grid grid-cols-2 gap-2 pt-2 border-t border-[var(--border-color)] text-[11px] text-[var(--text-muted)]">
              <div>Toplam Borç: <span class="font-semibold text-[var(--text-main)]">${formatCurrency(totalDebt)}</span></div>
              <div class="text-right">Ödenen: <span class="font-semibold text-emerald-500">${formatCurrency(totalPaid)}</span></div>
            </div>
          </div>
        `;
      }).join('');
    }

    function selectCari(cariId) {
      activeCariId = cariId;
      const cari = cariList.find(c => c.id == cariId);
      if (cari) {
        renderCariDetail(cari);
      }
    }

    function renderCariDetail(cari) {
      const listView = document.getElementById('cariMainView');
      const detailView = document.getElementById('cariDetailView');
      if (!listView || !detailView) return;

      listView.classList.add('hidden');
      detailView.classList.remove('hidden');

      const nameEl = document.getElementById('detailCariNameTitle');
      const phoneEl = document.getElementById('detailCariPhone');
      const descEl = document.getElementById('detailCariDesc');
      if (nameEl) nameEl.textContent = cari.name;
      if (phoneEl) phoneEl.textContent = cari.phone || 'Belirtilmemiş';
      if (descEl) descEl.textContent = cari.desc || 'Açıklama yok';

      // Finansal Özet: Yalnızca DEBT ve PAYMENT hareketleri (paymentPlans kesinlikle dahil edilmez)
      const totalDebt = (cari.transactions || [])
        .filter(t => t.type === 'DEBT')
        .reduce((sum, t) => sum + (Number(t.amount) || 0), 0);

      const totalPaid = (cari.transactions || [])
        .filter(t => t.type === 'PAYMENT')
        .reduce((sum, t) => sum + (Number(t.amount) || 0), 0);

      const netBalance = Math.max(0, totalDebt - totalPaid);

      const debtEl = document.getElementById('detailTotalDebt');
      const paidEl = document.getElementById('detailTotalPaid');
      const balanceEl = document.getElementById('detailNetBalance');
      if (debtEl) debtEl.textContent = formatCurrency(totalDebt);
      if (paidEl) paidEl.textContent = formatCurrency(totalPaid);
      if (balanceEl) balanceEl.textContent = formatCurrency(netBalance);

      // Ödeme planı durumları (PENDING, PARTIAL, COMPLETED) - Ana borca asla eklenmez
      const plansContainer = document.getElementById('detailPaymentPlansContainer');
      if (plansContainer) {
        const plans = cari.paymentPlans || [];
        if (plans.length === 0) {
          plansContainer.innerHTML = `<p class="text-xs text-[var(--text-muted)] italic">Henüz tanımlı ödeme planı / taksit bulunmuyor.</p>`;
        } else {
          plansContainer.innerHTML = plans.map(plan => {
            const rawStatus = plan.status || 'PENDING';
            let statusText = 'Bekliyor';
            let statusBadgeClass = 'bg-amber-500/10 text-amber-500 border-amber-500/20';

            if (rawStatus === 'COMPLETED' || plan.isPaid) {
              statusText = 'Tamamlandı';
              statusBadgeClass = 'bg-emerald-500/10 text-emerald-500 border-emerald-500/20';
            } else if (rawStatus === 'PARTIAL') {
              statusText = 'Kısmi Ödendi';
              statusBadgeClass = 'bg-sky-500/10 text-sky-500 border-sky-500/20';
            }

            return `
              <div class="bg-[var(--bg-main)] p-3 rounded-xl border border-[var(--border-color)] flex items-center justify-between text-xs">
                <div>
                  <div class="font-bold text-[var(--text-main)]">${formatCurrency(plan.amount)}</div>
                  <div class="text-[10px] text-[var(--text-muted)]">Vade: ${escapeHTML(plan.dueDate || 'Belirtilmemiş')}</div>
                </div>
                <span class="px-2 py-0.5 rounded-md text-[10px] font-bold border ${statusBadgeClass}">
                  ${statusText}
                </span>
              </div>
            `;
          }).join('');
        }
      }

      // Hareketler Geçmişi (Güvenli escapeHTML gösterimi)
      const txContainer = document.getElementById('detailTransactionsContainer');
      if (txContainer) {
        const txs = cari.transactions || [];
        if (txs.length === 0) {
          txContainer.innerHTML = `<p class="text-xs text-[var(--text-muted)] italic">Henüz işlem hareketi bulunmuyor.</p>`;
        } else {
          txContainer.innerHTML = [...txs].reverse().map(tx => {
            const isDebt = tx.type === 'DEBT';
            return `
              <div class="bg-[var(--bg-main)] p-3 rounded-xl border border-[var(--border-color)] flex items-center justify-between text-xs">
                <div>
                  <div class="font-bold text-[var(--text-main)]">${escapeHTML(tx.description || (isDebt ? 'Borç Kaydı' : 'Ödeme'))}</div>
                  <div class="text-[10px] text-[var(--text-muted)]">${escapeHTML(tx.date || '')}</div>
                </div>
                <span class="font-black ${isDebt ? 'text-rose-500' : 'text-emerald-500'}">
                  ${isDebt ? '+' : '-'}${formatCurrency(tx.amount)}
                </span>
              </div>
            `;
          }).join('');
        }
      }
    }

    function closeCariDetail() {
      activeCariId = null;
      const listView = document.getElementById('cariMainView');
      const detailView = document.getElementById('cariDetailView');
      if (!listView || !detailView) return;

      detailView.classList.add('hidden');
      listView.classList.remove('hidden');
      renderCariTab();
    }

    function deleteCari(cariId) {
      const cari = cariList.find(c => c.id == cariId);
      if (!cari) return;

      // Borç veya ödeme geçmişi bulunan cari hesapların silinmesini engelleyen güvenlik kuralı
      const hasHistory = (cari.transactions && cari.transactions.length > 0) || 
                         (cari.paymentPlans && cari.paymentPlans.length > 0);

      if (hasHistory) {
        alert(
          `⚠️ "${cari.name}" cari hesabı silinemez!\n\n` +
          `Bu cari hesaba ait aktif borç hareketleri, ödemeler veya ödeme planları bulunuyor.\n` +
          `Geçmişi olan cari hesaplar güvenlik nedeniyle doğrudan silinemez.`
        );
        return;
      }

      if (confirm(`"${cari.name}" cari hesabını silmek istediğinize emin misiniz?`)) {
        cariList = cariList.filter(c => c.id != cariId);
        saveData();
        closeCariDetail();
        renderCariTab();
        showToast('Cari hesap silindi.', 'error');
      }
    }

    function formatCurrency(amount) {
      const num = parseFloat(amount) || 0;
      return new Intl.NumberFormat('tr-TR', { style: 'currency', currency: 'TRY' }).format(num);
    }

    function switchTab(tabName) {
      document.querySelectorAll('.tab-content').forEach(el => el.classList.add('hidden'));
      
      document.querySelectorAll('.nav-btn').forEach(btn => {
        btn.classList.remove('bg-[var(--accent)]', 'text-white');
        btn.classList.add('text-[var(--text-muted)]');
      });

      document.querySelectorAll('.mobile-nav-btn').forEach(btn => {
        btn.classList.remove('active-nav');
        btn.classList.add('muted-nav');
      });

      const targetTab = document.getElementById('tab-' + tabName);
      if(targetTab) targetTab.classList.remove('hidden');
      
      const activeBtn = document.getElementById('nav-' + tabName);
      if(activeBtn) {
        activeBtn.classList.add('bg-[var(--accent)]', 'text-white');
        activeBtn.classList.remove('text-[var(--text-muted)]');
      }

      const activeMobileBtn = document.getElementById('mobile-nav-' + tabName);
      if(activeMobileBtn) {
        activeMobileBtn.classList.remove('muted-nav');
        activeMobileBtn.classList.add('active-nav');
      }

      const titles = {
        'home': 'Ana Sayfa Panel',
        'material': 'Malzeme & Stok Yönetimi',
        'purchased': 'Satın Alınanlar Geçmişi',
        'cari': 'Cari Hesap Yönetimi',
        'categories': 'Kategori & Kalem Kütüphanesi',
        'budget': 'Bütçe & Finans Kontrolü',
        'settings': 'Sistem Ayarları'
      };
      const titleEl = document.getElementById('page-title');
      if(titleEl) titleEl.innerText = titles[tabName] || 'ŞantiyePRO';

      if (tabName === 'home') {
        renderHomeDashboard();
      } else if(tabName === 'material') {
        editingCardId = null;
        updateInlineCategoryOptions();
        renderMaterialsList();
      } else if (tabName === 'purchased') {
        renderPurchasedList();
      } else if (tabName === 'cari') {
        if (typeof renderCariTab === 'function') renderCariTab();
      } else if (tabName === 'budget') {
        renderBudgetTab();
      } else if (tabName === 'categories') {
        renderCategoriesTab();
      } else if (tabName === 'settings') {
        renderSettingsTab();
      }
    }

    function applyTheme() {
      const html = document.documentElement;

      if (appData.theme === 'light') {
        html.classList.remove('dark');
        html.classList.add('light');
      } else {
        html.classList.remove('light');
        html.classList.add('dark');
      }

      renderSettingsTab();
    }

    function toggleTheme() {
      appData.theme = (appData.theme === 'dark') ? 'light' : 'dark';
      applyTheme();
      saveData();
      showToast(`${appData.theme === 'dark' ? 'Koyu' : 'Açık'} tema aktif.`, 'info');
    }

    function renderSettingsTab() {
      const knob = document.getElementById('theme-toggle-knob');
      const knobIcon = document.getElementById('knob-icon');
      const settingsIcon = document.getElementById('settings-theme-icon');
      const darkLabel = document.getElementById('knob-label-dark');
      const lightLabel = document.getElementById('knob-label-light');

      if (!knob) return;

      if (appData.theme === 'dark') {
        knob.style.transform = 'translateX(0px)';
        knobIcon.className = 'fa-solid fa-moon';
        if (settingsIcon) settingsIcon.className = 'fa-solid fa-moon text-sm';
        if (darkLabel) darkLabel.style.opacity = '0';
        if (lightLabel) lightLabel.style.opacity = '1';
      } else {
        knob.style.transform = 'translateX(44px)';
        knobIcon.className = 'fa-solid fa-sun';
        if (settingsIcon) settingsIcon.className = 'fa-solid fa-sun text-sm text-amber-500';
        if (darkLabel) darkLabel.style.opacity = '1';
        if (lightLabel) lightLabel.style.opacity = '0';
      }
    }

    function updateInlineCategoryOptions() {
      const stageEl = document.getElementById('inlineMatStage');
      const categorySelect = document.getElementById('inlineMatCategory');
      if (!stageEl || !categorySelect) return;

      const allowedCategories = ["Kaba İnşaat", "İnce İnşaat", "İşçilik", "Proje ve Ruhsat"];
      const availableCategories = Object.keys(appData.categoriesData);
      const mainCategories = availableCategories.length > 0 ? availableCategories : allowedCategories;

      if (stageEl.options.length !== mainCategories.length || Array.from(stageEl.options).some((opt, idx) => opt.value !== mainCategories[idx])) {
        stageEl.innerHTML = mainCategories.map(cat => `<option value="${escapeHTML(cat)}">${escapeHTML(cat)}</option>`).join('');
      }

      const currentStage = stageEl.value || mainCategories[0] || '';
      const currentCategoryVal = categorySelect.value;
      const subItems = appData.categoriesData[currentStage] || [];

      categorySelect.innerHTML = subItems.map(item => `<option value="${escapeHTML(item)}">${escapeHTML(item)}</option>`).join('');
      
      if (currentCategoryVal && subItems.includes(currentCategoryVal)) {
        categorySelect.value = currentCategoryVal;
      }
    }

    function handleInlineMaterialSubmit(e) {
      e.preventDefault();
      const stage = document.getElementById('inlineMatStage').value || 'Kaba İnşaat';
      const name = document.getElementById('inlineMatCategory').value;
      const amount = Math.max(0, parseFloat(document.getElementById('inlineMatAmount').value) || 0);
      const unit = document.getElementById('inlineMatUnit').value;
      const unitPrice = Math.max(0, parseFloat(document.getElementById('inlineMatUnitPrice').value) || 0);

      const newMaterial = {
        id: Date.now(),
        stage: stage,
        name: name,
        amount: amount,
        unit: unit,
        unitPrice: unitPrice,
        totalPrice: amount * unitPrice,
        purchased: false,
        paidAmount: 0,
        debtAmount: 0,
        note: '',
        date: new Date().toISOString().split('T')[0]
      };

      materials.push(newMaterial);
      saveData();
      renderMaterialsList();
      showToast('Malzeme listeye eklendi.', 'success');
      e.target.reset();
      updateInlineCategoryOptions();
    }

    function togglePurchaseModal(show) {
      const modal = document.getElementById('purchaseModal');
      if (!modal) return;
      if (show === false) {
        modal.classList.add('hidden');
        document.body.style.overflow = '';
      } else {
        if (modal.classList.contains('hidden')) {
          modal.classList.remove('hidden');
          document.body.style.overflow = 'hidden';
        } else {
          modal.classList.add('hidden');
          document.body.style.overflow = '';
        }
      }
    }

    function openPurchaseModal(materialId) {
      const mat = (materials || []).find(m => m.id === Number(materialId) || m.id === materialId);
      if (!mat) {
        alert("Malzeme bulunamadı!");
        return;
      }

      document.getElementById('purchaseMaterialId').value = mat.id;
      document.getElementById('purchaseMaterialNameDisplay').value = `${mat.name} (${mat.stage})`;
      document.getElementById('purchaseQty').value = mat.amount || '';
      document.getElementById('purchaseUnitPrice').value = mat.unitPrice || '';
      document.getElementById('purchaseInitialPayment').value = '0';
      document.getElementById('purchaseDesc').value = '';

      // Cari Firma Seçeneklerini Doldur
      const cariSelect = document.getElementById('purchaseCariSelect');
      if (cariSelect) {
        if (!cariList || cariList.length === 0) {
          cariSelect.innerHTML = `<option value="">-- Önce Cari Hesap Eklemelisiniz --</option>`;
        } else {
          cariSelect.innerHTML = cariList.map(c => `<option value="${c.id}">${escapeHTML(c.name)} (${escapeHTML(c.type || 'Tedarikçi')})</option>`).join('');
        }
      }

      calculatePurchaseTotal();
      togglePurchaseModal(true);
    }

    function checkPurchaseModalBackdrop(event) {
      if (event.target.id === 'purchaseModal') {
        togglePurchaseModal(false);
      }
    }

    function calculatePurchaseTotal() {
      const qty = Math.max(0, parseFloat(document.getElementById('purchaseQty').value) || 0);
      const price = Math.max(0, parseFloat(document.getElementById('purchaseUnitPrice').value) || 0);
      const total = qty * price;
      const display = document.getElementById('purchaseTotalDisplay');
      if (display) {
        display.textContent = total.toLocaleString('tr-TR', {minimumFractionDigits: 2, maximumFractionDigits: 2}) + ' ₺';
      }
    }

    function submitPurchaseForm(e) {
      e.preventDefault();
      const matId = document.getElementById('purchaseMaterialId').value;
      const qty = document.getElementById('purchaseQty').value;
      const price = document.getElementById('purchaseUnitPrice').value;
      const payment = document.getElementById('purchaseInitialPayment').value;
      const desc = document.getElementById('purchaseDesc').value;

      const success = addMaterialTransaction(matId, qty, price, payment, desc);
      if (success) {
        togglePurchaseModal(false);
        renderAll();
        showToast("Malzeme satın alma işlemi başarıyla kaydedildi!", "success");
      }
    }
       function addMaterialTransaction(matId, qty, price, payment, desc, cariId) {
      const parsedQty = Math.max(0, parseFloat(qty) || 0);
      const parsedPrice = Math.max(0, parseFloat(price) || 0);
      const parsedPayment = Math.max(0, parseFloat(payment) || 0);
      const total = parsedQty * parsedPrice;

      if (parsedQty <= 0) {
        alert('Lütfen geçerli bir miktar girin.');
        return false;
      }

      if (parsedPayment > total) {
        alert('Peşin ödenen tutar toplam alım tutarından fazla olamaz!');
        return false;
      }

      const debt = total - parsedPayment;

      // Borç varsa cariId boş olamaz ve cariList içinde gerçekten bulunmak zorundadır
      if (debt > 0) {
        if (!cariId || cariId === '' || cariId === 'null' || cariId === 'undefined') {
          alert('Vadeli (borçlu) alımlar için mutlaka bir cari hesap seçmelisiniz!');
          return false;
        }

        const selectedCariCheck = (cariList || []).find(c => c.id == cariId);
        if (!selectedCariCheck) {
          alert('Hata: Seçilen cari hesap sistemde bulunamadı! İşlem iptal edildi.');
          return false;
        }
      }

      // Dizilerin kullanılabilir olduğundan emin ol
      if (!Array.isArray(purchases)) purchases = [];
      if (!Array.isArray(budgetTransactions)) budgetTransactions = [];

      const targetMaterial = materials.find(m => m.id == matId);
      if (targetMaterial) {
        const sharedDebtId = generatePaymentId(); // Ortak borç bağlayıcı kimliği

        // materials.amount (planlanan miktar) kesinlikle değiştirilmez/ezilmez.
        targetMaterial.purchased = true;
        targetMaterial.cariId = cariId || null;
        targetMaterial.debtId = sharedDebtId;

        const newPurchase = {
          id: Date.now(),
          materialId: targetMaterial.id,
          debtId: sharedDebtId,
          name: targetMaterial.name,
          stage: targetMaterial.stage || 'Kaba İnşaat',
          unit: targetMaterial.unit || 'Adet',
          amount: parsedQty,
          unitPrice: parsedPrice,
          totalPrice: total,
          paidAmount: parsedPayment,
          debtAmount: debt,
          note: desc ? desc.trim() : '',
          cariId: cariId || null,
          date: getTurkeyLocalDate()
        };

        // CARİ HESAP ENTEGRASYONU (Doğrulanmış cariye DEBT ve PAYMENT işleme)
        if (cariId) {
          const selectedCari = cariList.find(c => c.id == cariId);
          if (selectedCari) {
            const debtTxId = generatePaymentId();
            const debtTx = {
              id: debtTxId,
              debtId: sharedDebtId,
              type: 'DEBT',
              amount: total,
              date: getTurkeyLocalDate(),
              description: `Malzeme Alımı: ${targetMaterial.name} (${parsedQty} ${targetMaterial.unit}) - ${desc || 'Fatura'}`
            };

            if (!Array.isArray(selectedCari.transactions)) selectedCari.transactions = [];
            selectedCari.transactions.push(debtTx);

            if (parsedPayment > 0) {
              const paymentTx = {
                id: generatePaymentId(),
                debtId: sharedDebtId,
                type: 'PAYMENT',
                amount: parsedPayment,
                date: getTurkeyLocalDate(),
                description: `Peşinat Ödemesi: ${targetMaterial.name}`
              };
              selectedCari.transactions.push(paymentTx);
            }
          }
        }

        purchases.push(newPurchase);

        // PEŞİNAT YALNIZCA BİR KEZ KASA GİDERİNE EKLENİR
        if (parsedPayment > 0) {
          budgetTransactions.push({
            id: generatePaymentId(),
            type: 'EXPENSE',
            category: targetMaterial.stage || 'Kaba İnşaat',
            amount: parsedPayment,
            desc: `Malzeme Peşinatı: ${targetMaterial.name} (${desc ? desc.trim() : 'Satın Alım'})`,
            date: getTurkeyLocalDate(),
            auto: true
          });
        }

        saveData();
        return true;
      }
      return false;
    }

    function submitPurchaseForm(e) {
      e.preventDefault();
      const matId = document.getElementById('purchaseMaterialId').value;
      const qty = document.getElementById('purchaseQty').value;
      const price = document.getElementById('purchaseUnitPrice').value;
      const payment = document.getElementById('purchaseInitialPayment').value;
      const desc = document.getElementById('purchaseDesc').value;
      const cariId = document.getElementById('purchaseCariSelect').value;

      const success = addMaterialTransaction(matId, qty, price, payment, desc, cariId);
      if (success) {
        togglePurchaseModal(false);
        renderAll();
        showToast("Malzeme alımı ve cari borçlandırma başarıyla kaydedildi!", "success");
      }
    }

    function startEditingMaterial(id) {
      editingCardId = id;
      renderMaterialsList();
    }

    function cancelEditingMaterial() {
      editingCardId = null;
      renderMaterialsList();
    }

    function saveEditingMaterial(id) {
      const amountInput = document.getElementById(`editAmount_${id}`);
      const priceInput = document.getElementById(`editPrice_${id}`);
      if (!amountInput || !priceInput) return;

      const newAmount = Math.max(0, parseFloat(amountInput.value) || 0);
      const newPrice = Math.max(0, parseFloat(priceInput.value) || 0);

      const targetIndex = materials.findIndex(m => m.id == id);
      if (targetIndex !== -1) {
        materials[targetIndex].amount = newAmount;
        materials[targetIndex].unitPrice = newPrice;
        materials[targetIndex].totalPrice = newAmount * newPrice;

        saveData();
        editingCardId = null;
        renderMaterialsList();
        showToast('Malzeme güncellendi.', 'success');
      }
    }

    function deleteMaterial(id) {
      const target = materials.find(m => m.id == id);
      if (!target) return;

      if (confirm(`"${target.name}" malzemesini silmek istediğinize emin misiniz?`)) {
        materials = materials.filter(m => m.id != id);
        if (editingCardId == id) editingCardId = null;
        saveData();
        renderMaterialsList();
        showToast('Malzeme silindi.', 'error');
      }
    }
    function renderMaterialsList() {
      const container = document.getElementById('materialListContainer');
      const badge = document.getElementById('materialCountBadge');
      if (!container) return;

      if (badge) badge.textContent = materials.length;

      if (!materials || materials.length === 0) {
        container.innerHTML = `
          <div class="theme-card rounded-2xl p-6 text-center border border-[var(--border-color)] space-y-2">
            <div class="w-12 h-12 rounded-full bg-[var(--accent)]/10 text-[var(--accent)] flex items-center justify-center mx-auto text-xl">
              <i class="fa-solid fa-box-open"></i>
            </div>
            <p class="text-xs sm:text-sm font-medium text-[var(--text-muted)]">Henüz malzeme eklenmedi.</p>
          </div>
        `;
        return;
      }

      const sortedMaterials = [...materials].reverse();

      container.innerHTML = sortedMaterials.map(mat => {
        const matStage = mat.stage || 'Kaba İnşaat';
        const matName = escapeHTML(mat.name || 'İsimsiz Malzeme');
        const matUnit = escapeHTML(mat.unit || 'Adet');
        const plannedAmount = mat.amount || 0;
        const matUnitPrice = mat.unitPrice || 0;

        // Purchases irsaliye kayıtlarını kontrol et
        const relatedPurchases = (purchases || []).filter(p => p.materialId == mat.id);
        const hasPurchases = relatedPurchases.length > 0;

        let purchasedAmount = 0;
        let totalPurchasedPrice = 0;
        let totalPaidAmount = 0;
        let totalDebtAmount = 0;

        if (hasPurchases) {
          purchasedAmount = relatedPurchases.reduce((sum, p) => sum + (Number(p.amount) || 0), 0);
          totalPurchasedPrice = relatedPurchases.reduce((sum, p) => sum + (Number(p.totalPrice) || 0), 0);
          
          let calculatedPaid = 0;
          let calculatedDebt = 0;

          relatedPurchases.forEach(p => {
            const pTotal = Number(p.totalPrice) || 0;
            let remainingForThisRecord = pTotal;

            const targetKey = String(p.debtId || p.id).trim();
            let foundCari = null;
            if (p.cariId) {
              foundCari = (cariList || []).find(c => c.id == p.cariId);
            }
            if (!foundCari && cariList && cariList.length > 0) {
              foundCari = cariList.find(c => (c.transactions || []).some(t => t.debtId && String(t.debtId).trim() === targetKey));
            }

            // Cari ve gerçek DEBT kaydı kontrolü
            let hasValidDebtTx = false;
            if (foundCari && Array.isArray(foundCari.transactions)) {
              hasValidDebtTx = foundCari.transactions.some(t => {
                if (t.type !== 'DEBT') return false;
                const hasId = t.debtId !== null && t.debtId !== undefined && String(t.debtId).trim() !== '';
                const checkKey = hasId ? String(t.debtId).trim() : String(t.id).trim();
                return checkKey === targetKey;
              });
            }

            if (foundCari && hasValidDebtTx) {
              remainingForThisRecord = calculateRemainingDebtForRecord(foundCari, p.debtId || p.id);
            } else {
              remainingForThisRecord = Number(p.debtAmount) || Math.max(0, pTotal - (Number(p.paidAmount) || 0));
            }

            const paidForThisRecord = Math.max(0, pTotal - remainingForThisRecord);

            calculatedPaid += paidForThisRecord;
            calculatedDebt += remainingForThisRecord;
          });

          totalPaidAmount = calculatedPaid;
          totalDebtAmount = calculatedDebt;
        } else {
          // Fallback: Hiç purchases kaydı bulunmayan eski malzemeler
          purchasedAmount = mat.purchased ? (mat.amount || 0) : 0;
          totalPurchasedPrice = mat.purchased ? (Number(mat.totalPrice) || (plannedAmount * matUnitPrice)) : 0;
          totalPaidAmount = mat.purchased ? (Number(mat.paidAmount) || 0) : 0;
          totalDebtAmount = mat.purchased ? (Number(mat.debtAmount) || 0) : 0;
        }

        const isPurchased = purchasedAmount > 0 || Boolean(mat.purchased);
        const isEditing = (editingCardId == mat.id);

        const badgeBg = 'bg-[var(--accent)]/10 text-[var(--accent)] border-[var(--accent)]/20';

        return `
          <div class="theme-card rounded-2xl p-3.5 sm:p-4 border border-[var(--border-color)] space-y-3 transition-all hover:border-[var(--accent)]/40">
            <div class="flex items-start justify-between gap-2">
              <div class="min-w-0 flex-1">
                <div class="flex items-center gap-1.5 flex-wrap mb-1">
                  <span class="inline-block px-2 py-0.5 rounded-md text-[10px] font-semibold border ${badgeBg}">
                    ${escapeHTML(matStage)}
                  </span>
                  ${isPurchased ? `
                    <span class="inline-flex items-center gap-1 px-2 py-0.5 rounded-md text-[10px] font-bold bg-emerald-500/10 text-emerald-500 border border-emerald-500/20">
                      <i class="fa-solid fa-circle-check text-[9px]"></i> Satın Alındı (${purchasedAmount} ${matUnit})
                    </span>
                  ` : ''}
                </div>
                <h4 class="font-bold text-xs sm:text-sm text-[var(--text-main)] truncate">${matName}</h4>
              </div>

              <div class="text-right shrink-0">
                <div class="text-xs sm:text-sm font-black text-[var(--accent)]">${formatCurrency(isPurchased ? totalPurchasedPrice : (plannedAmount * matUnitPrice))}</div>
                <div class="text-[10px] text-[var(--text-muted)] mt-0.5">Planlanan: ${plannedAmount} ${matUnit}</div>
              </div>
            </div>

            ${isEditing ? `
              <div class="grid grid-cols-2 gap-2 pt-2 border-t border-[var(--border-color)] text-[11px]">
                <div>
                  <label class="block text-[10px] uppercase font-semibold text-[var(--text-muted)] opacity-70 mb-1">Planlanan Miktar (${matUnit})</label>
                  <input type="number" id="editAmount_${mat.id}" value="${plannedAmount}" step="any" min="0" class="w-full px-2.5 py-1.5 rounded-lg text-xs focus:outline-none focus:border-[var(--accent)]">
                </div>
                <div>
                  <label class="block text-[10px] uppercase font-semibold text-[var(--text-muted)] opacity-70 mb-1">Birim Fiyat (₺)</label>
                  <input type="number" id="editPrice_${mat.id}" value="${matUnitPrice}" step="any" min="0" class="w-full px-2.5 py-1.5 rounded-lg text-xs focus:outline-none focus:border-[var(--accent)]">
                </div>
              </div>
            ` : `
              <div class="grid grid-cols-2 gap-2 pt-2 border-t border-[var(--border-color)] text-[11px] text-[var(--text-muted)]">
                <div>
                  <span class="block text-[10px] uppercase font-semibold text-[var(--text-muted)] opacity-70">Planlanan Miktar</span>
                  <span class="font-medium text-[var(--text-main)]">${plannedAmount} ${matUnit}</span>
                </div>
                <div class="text-right">
                  <span class="block text-[10px] uppercase font-semibold text-[var(--text-muted)] opacity-70">Birim Fiyat</span>
                  <span class="font-medium text-[var(--text-main)]">${formatCurrency(matUnitPrice)}</span>
                </div>
              </div>

              ${isPurchased ? `
                <div class="grid grid-cols-2 gap-2 pt-2 border-t border-[var(--border-color)] text-[10px] bg-[var(--bg-main)] p-2 rounded-xl">
                  <div>
                    <span class="block text-[var(--text-muted)]">Toplam Ödenen:</span>
                    <span class="font-bold text-emerald-500">${formatCurrency(totalPaidAmount)}</span>
                  </div>
                  <div class="text-right">
                    <span class="block text-[var(--text-muted)]">Toplam Kalan Borç:</span>
                    <span class="font-bold text-rose-500">${formatCurrency(totalDebtAmount)}</span>
                  </div>
                </div>
              ` : ''}
            `}

            <div class="flex items-center justify-between pt-2 border-t border-[var(--border-color)] gap-2">
              <div>
                <button type="button" onclick="window.openPurchaseModal(${mat.id})" class="px-3 py-1.5 rounded-lg bg-[var(--accent)] hover:opacity-90 active:scale-95 text-white text-xs font-bold shadow-sm transition-all flex items-center gap-1.5">
                  <i class="fa-solid fa-cart-shopping"></i> ${isPurchased ? 'Tekrar Satın Al' : 'Satın Al'}
                </button>
              </div>

              <div class="flex items-center gap-1.5">
                ${isEditing ? `
                  <button type="button" onclick="saveEditingMaterial(${mat.id})" title="Kaydet" aria-label="Kaydet" class="w-8 h-8 rounded-lg border border-emerald-500/30 text-emerald-500 bg-emerald-500/10 flex items-center justify-center text-xs transition-all active:scale-95">
                    <i class="fa-solid fa-check"></i>
                  </button>
                  <button type="button" onclick="cancelEditingMaterial()" title="İptal" aria-label="İptal" class="w-8 h-8 rounded-lg border border-rose-500/30 text-rose-500 bg-rose-500/10 flex items-center justify-center text-xs transition-all active:scale-95">
                    <i class="fa-solid fa-xmark"></i>
                  </button>
                ` : `
                  <button type="button" onclick="startEditingMaterial(${mat.id})" title="Düzenle" aria-label="Düzenle" class="w-8 h-8 rounded-lg border border-[var(--border-color)] text-[var(--text-muted)] hover:text-sky-500 hover:border-sky-500/40 bg-[var(--bg-main)] flex items-center justify-center text-xs transition-all active:scale-95">
                    <i class="fa-solid fa-pen"></i>
                  </button>
                  <button type="button" onclick="deleteMaterial(${mat.id})" title="Sil" aria-label="Sil" class="w-8 h-8 rounded-lg border border-[var(--border-color)] text-[var(--text-muted)] hover:text-rose-500 hover:border-rose-500/40 bg-[var(--bg-main)] flex items-center justify-center text-xs transition-all active:scale-95">
                    <i class="fa-solid fa-trash-can"></i>
                  </button>
                `}
              </div>
            </div>
          </div>
        `;
      }).join('');
    }
  

    function togglePurchaseGroupDetail(groupKey) {
      openPurchaseGroups[groupKey] = !openPurchaseGroups[groupKey];
      renderPurchasedList();
    }
       function getTurkeyLocalDate() {
      try {
        const options = { timeZone: 'Europe/Istanbul', year: 'numeric', month: '2-digit', day: '2-digit' };
        const formatter = new Intl.DateTimeFormat('en-CA', options);
        return formatter.format(new Date());
      } catch (e) {
        const d = new Date();
        const year = d.getFullYear();
        const month = String(d.getMonth() + 1).padStart(2, '0');
        const day = String(d.getDate()).padStart(2, '0');
        return `${year}-${month}-${day}`;
      }
}
    
      function generatePaymentId() {
      return 'pay_' + Date.now() + '_' + Math.random().toString(36).substr(2, 9);
    }

     function calculateRemainingDebtForRecord(cari, debtKey) {
      if (!cari || !cari.transactions || !Array.isArray(cari.transactions)) return 0;
      if (debtKey === null || debtKey === undefined || String(debtKey).trim() === '') return 0;
      
      const keyStr = String(debtKey).trim();
      
      // 1. Ana Borç (DEBT): Geçerli debtId varsa onu kullan; yoksa/geçersizse eski kayıtlar için id eşleşmesini güvenle kullan
      const totalDebtForTarget = cari.transactions
        .filter(t => {
          if (t.type !== 'DEBT') return false;
          const hasValidDebtId = t.debtId !== null && t.debtId !== undefined && String(t.debtId).trim() !== '';
          if (hasValidDebtId) {
            return String(t.debtId).trim() === keyStr;
          }
          return String(t.id).trim() === keyStr;
        })
        .reduce((sum, t) => sum + (Number(t.amount) || 0), 0);

      // 2. Gerçek Ödemeler (PAYMENT): Yalnızca geçerli ve eşleşen debtId alanına sahip ödemeleri topla (Asla PAYMENT.id kullanılmaz)
      const totalPaidForTarget = cari.transactions
        .filter(t => {
          if (t.type !== 'PAYMENT') return false;
          const hasValidDebtId = t.debtId !== null && t.debtId !== undefined && String(t.debtId).trim() !== '';
          return hasValidDebtId && String(t.debtId).trim() === keyStr;
        })
        .reduce((sum, t) => sum + (Number(t.amount) || 0), 0);

      // 3. Kalan Net Borç: Asla negatif değer döndürmez
      return Math.max(0, totalDebtForTarget - totalPaidForTarget);
    }

    function renderPurchasedList() {
      const container = document.getElementById('purchasedListContainer');
      const badge = document.getElementById('purchasedCountBadge');
      const totalSumEl = document.getElementById('purchaseTotalSumDisplay');
      const debtSumEl = document.getElementById('purchaseDebtSumDisplay');
      
      const searchInput = document.getElementById('purchaseSearchInput');
      const categoryFilter = document.getElementById('purchaseCategoryFilter');

      if (!container) return;

      const query = searchInput ? searchInput.value.trim().toLowerCase() : '';
      const selectedCategory = categoryFilter ? categoryFilter.value : 'ALL';

      if (!purchases || purchases.length === 0) {
        if (badge) badge.textContent = '0 Kalem';
        if (totalSumEl) totalSumEl.textContent = '₺0,00';
        if (debtSumEl) debtSumEl.textContent = '₺0,00';

        container.innerHTML = `
          <div class="theme-card rounded-2xl p-6 text-center border border-[var(--border-color)] space-y-2">
            <div class="w-12 h-12 rounded-full bg-amber-500/10 text-amber-500 flex items-center justify-center mx-auto text-xl">
              <i class="fa-solid fa-receipt"></i>
            </div>
            <p class="text-xs sm:text-sm font-medium text-[var(--text-muted)]">Henüz satın alınan malzeme bulunmuyor.</p>
          </div>
        `;
        return;
      }

      const filteredPurchases = purchases.filter(item => {
        const matchesCategory = (selectedCategory === 'ALL' || item.stage === selectedCategory);
        const matchesSearch = !query || 
          (item.name && item.name.toLowerCase().includes(query)) ||
          (item.note && item.note.toLowerCase().includes(query)) ||
          (item.stage && item.stage.toLowerCase().includes(query));

        return matchesCategory && matchesSearch;
      });

      // Her irsaliyenin güncel kalan borcunu calculateRemainingDebtForRecord ile dinamik hesapla
      const processedPurchases = filteredPurchases.map(item => {
        let currentDebt = Number(item.debtAmount) || 0;
        const targetKey = String(item.debtId || item.id).trim();

        if (item.debtId || item.id) {
          let foundCari = null;

          if (item.cariId) {
            const directCari = (cariList || []).find(c => c.id == item.cariId);
            if (directCari && Array.isArray(directCari.transactions)) {
              const hasValidDebt = directCari.transactions.some(t => {
                if (t.type !== 'DEBT') return false;
                const hasValidDebtId = t.debtId !== null && t.debtId !== undefined && String(t.debtId).trim() !== '';
                const checkKey = hasValidDebtId ? String(t.debtId).trim() : String(t.id).trim();
                return checkKey === targetKey;
              });
              if (hasValidDebt) foundCari = directCari;
            }
          }

          if (!foundCari && cariList && cariList.length > 0) {
            foundCari = cariList.find(c => {
              if (!Array.isArray(c.transactions)) return false;
              return c.transactions.some(t => {
                if (t.type !== 'DEBT') return false;
                const hasValidDebtId = t.debtId !== null && t.debtId !== undefined && String(t.debtId).trim() !== '';
                const checkKey = hasValidDebtId ? String(t.debtId).trim() : String(t.id).trim();
                return checkKey === targetKey;
              });
            });
          }

          let hasValidDebtTx = false;
          if (foundCari && Array.isArray(foundCari.transactions)) {
            hasValidDebtTx = foundCari.transactions.some(t => {
              if (t.type !== 'DEBT') return false;
              const hasValidDebtId = t.debtId !== null && t.debtId !== undefined && String(t.debtId).trim() !== '';
              const checkKey = hasValidDebtId ? String(t.debtId).trim() : String(t.id).trim();
              return checkKey === targetKey;
            });
          }

          if (foundCari && hasValidDebtTx) {
            currentDebt = calculateRemainingDebtForRecord(foundCari, item.debtId || item.id);
          }
        }

        return {
          ...item,
          dynamicDebtAmount: currentDebt,
          dynamicPaidAmount: Math.max(0, (item.totalPrice || 0) - currentDebt)
        };
      });

      const sumTotal = processedPurchases.reduce((acc, curr) => acc + (curr.totalPrice || 0), 0);
      const sumDebt = processedPurchases.reduce((acc, curr) => acc + (curr.dynamicDebtAmount || 0), 0);

      if (totalSumEl) totalSumEl.textContent = formatCurrency(sumTotal);
      if (debtSumEl) debtSumEl.textContent = formatCurrency(sumDebt);

      if (processedPurchases.length === 0) {
        if (badge) badge.textContent = '0 Kalem';
        container.innerHTML = `
          <div class="theme-card rounded-2xl p-6 text-center border border-[var(--border-color)] space-y-2">
            <div class="w-10 h-10 rounded-full bg-amber-500/10 text-amber-500 flex items-center justify-center mx-auto text-lg">
              <i class="fa-solid fa-filter-circle-xmark"></i>
            </div>
            <p class="text-xs sm:text-sm font-medium text-[var(--text-muted)]">Aranan kriterlere uygun alım kaydı bulunamadı.</p>
          </div>
        `;
        return;
      }

      const groupedMap = {};
      processedPurchases.forEach(item => {
        const groupKey = `${item.stage}_${item.name}`;
        if (!groupedMap[groupKey]) {
          groupedMap[groupKey] = {
            groupKey: groupKey,
            name: item.name,
            stage: item.stage,
            unit: item.unit || 'Adet',
            records: []
          };
        }
        groupedMap[groupKey].records.push(item);
      });

      const groupKeys = Object.keys(groupedMap);
      if (badge) badge.textContent = `${groupKeys.length} Kalem (${processedPurchases.length} İrsaliye)`;

      container.innerHTML = groupKeys.map(key => {
        const group = groupedMap[key];
        const records = group.records.sort((a, b) => b.id - a.id);
        
        const groupTotalAmount = records.reduce((acc, r) => acc + (r.amount || 0), 0);
        const groupTotalPrice = records.reduce((acc, r) => acc + (r.totalPrice || 0), 0);
        const groupTotalDebt = records.reduce((acc, r) => acc + (r.dynamicDebtAmount || 0), 0);

        const isOpen = Boolean(openPurchaseGroups[key]);
        const escapedGroupKey = key.replace(/'/g, "\\'");

        return `
          <div class="theme-card rounded-2xl border border-[var(--border-color)] overflow-hidden transition-all duration-300">
            <div onclick="window.togglePurchaseGroupDetail('${escapedGroupKey}')" class="p-3.5 sm:p-4 flex items-center justify-between cursor-pointer select-none hover:bg-[var(--bg-main)]/50 transition-colors">
              <div class="min-w-0 flex-1 pr-2">
                <div class="flex items-center gap-1.5 flex-wrap mb-1">
                  <span class="inline-block px-2 py-0.5 rounded-md text-[10px] font-semibold bg-amber-500/10 text-amber-500 border border-amber-500/20">
                    ${escapeHTML(group.stage)}
                  </span>
                  <span class="inline-block px-2 py-0.5 rounded-md text-[10px] font-bold bg-[var(--accent)]/10 text-[var(--accent)] border border-[var(--accent)]/20">
                    ${records.length} Alım Kaydı
                  </span>
                </div>
                <h4 class="font-bold text-xs sm:text-sm text-[var(--text-main)] truncate">${escapeHTML(group.name)}</h4>
                <div class="text-[11px] text-[var(--text-muted)] mt-1">
                  Toplam Alım: <span class="font-semibold text-[var(--text-main)]">${groupTotalAmount} ${escapeHTML(group.unit)}</span>
                </div>
              </div>

              <div class="flex items-center gap-3 shrink-0">
                <div class="text-right">
                  <div class="text-xs sm:text-sm font-black text-amber-500">${formatCurrency(groupTotalPrice)}</div>
                  ${groupTotalDebt > 0 ? `
                    <div class="text-[10px] font-bold text-rose-500">Borç: ${formatCurrency(groupTotalDebt)}</div>
                  ` : `
                    <div class="text-[10px] font-bold text-emerald-500">Kapandı</div>
                  `}
                </div>
                <i class="fa-solid fa-chevron-${isOpen ? 'up' : 'down'} text-xs text-[var(--text-muted)]"></i>
              </div>
            </div>

            ${isOpen ? `
              <div class="border-t border-[var(--border-color)] bg-[var(--bg-main)] p-3 space-y-2.5">
                <div class="text-[10px] uppercase font-bold text-[var(--text-muted)] tracking-wider px-1">Alım ve İrsaliye Detayları (${records.length})</div>
                ${records.map((r, idx) => `
                  <div class="bg-[var(--bg-card)] border border-[var(--border-color)] p-3 rounded-xl space-y-2">
                    <div class="flex items-center justify-between text-xs font-semibold">
                      <span class="text-amber-500 flex items-center gap-1">
                        <i class="fa-solid fa-receipt text-[10px]"></i> ${records.length - idx}. Alım
                      </span>
                      <span class="text-[10px] text-[var(--text-muted)]">${r.date || ''}</span>
                    </div>

                    <div class="grid grid-cols-2 gap-2 text-[11px]">
                      <div>
                        <span class="block text-[10px] text-[var(--text-muted)]">Alınan Miktar:</span>
                        <span class="font-medium text-[var(--text-main)]">${r.amount} ${escapeHTML(r.unit || group.unit)}</span>
                      </div>
                      <div class="text-right">
                        <span class="block text-[10px] text-[var(--text-muted)]">Gerçek Birim Fiyat:</span>
                        <span class="font-medium text-[var(--text-main)]">${formatCurrency(r.unitPrice)}</span>
                      </div>
                    </div>

                    <div class="grid grid-cols-2 gap-2 pt-1.5 border-t border-[var(--border-color)] text-[10px]">
                      <div>
                        <span class="text-[var(--text-muted)]">Toplam Ödenen:</span>
                        <span class="font-bold text-emerald-500 ml-1">${formatCurrency(r.dynamicPaidAmount)}</span>
                      </div>
                      <div class="text-right">
                        <span class="text-[var(--text-muted)]">Kalan Borç:</span>
                        <span class="font-bold ${r.dynamicDebtAmount > 0 ? 'text-rose-500' : 'text-emerald-500'} ml-1">
                          ${r.dynamicDebtAmount > 0 ? formatCurrency(r.dynamicDebtAmount) : 'Kapandı'}
                        </span>
                      </div>
                      ${r.note ? `
                        <div class="col-span-2 text-[10px] text-[var(--text-muted)] italic pt-1 border-t border-[var(--border-color)]">
                          <i class="fa-solid fa-circle-info text-[var(--accent)] mr-1"></i> ${escapeHTML(r.note)}
                        </div>
                      ` : ''}
                    </div>
                  </div>
                `).join('')}
              </div>
            ` : ''}
          </div>
        `;
      }).join('');
    }
  

    // BÜTÇE KATEGORİ VE ALT KALEM YÖNETİMİ
    function toggleBudgetExpenseCategorySelect() {
      const typeSelect = document.getElementById('budgetType');
      const catContainer = document.getElementById('budgetExpenseCategoryContainer');
      if (typeSelect && catContainer) {
        if (typeSelect.value === 'EXPENSE') {
          catContainer.classList.remove('hidden');
          updateBudgetSubItemsList();
        } else {
          catContainer.classList.add('hidden');
        }
      }
    }

    function updateBudgetSubItemsList() {
      const catSelect = document.getElementById('budgetExpenseCategory');
      const subSelect = document.getElementById('budgetSubItemSelect');
      if (!catSelect || !subSelect) return;

      const selectedCat = catSelect.value;
      const subItems = appData.categoriesData[selectedCat] || [];

      let optionsHTML = `<option value="">-- Kütüphaneden Kalem Seç (Opsiyonel) --</option>`;
      subItems.forEach(item => {
        optionsHTML += `<option value="${escapeHTML(item)}">${escapeHTML(item)}</option>`;
      });
      optionsHTML += `<option value="CUSTOM">+ Diğer / Serbest Metin Gir</option>`;

      subSelect.innerHTML = optionsHTML;
    }

    function handleBudgetSubItemChange() {
      const subSelect = document.getElementById('budgetSubItemSelect');
      const descInput = document.getElementById('budgetDesc');
      if (!subSelect || !descInput) return;

      const val = subSelect.value;
      if (val && val !== 'CUSTOM') {
        descInput.value = val;
      } else if (val === 'CUSTOM') {
        descInput.value = '';
        descInput.focus();
      }
    }

    function handleBudgetTransactionSubmit(e) {
      e.preventDefault();
      const type = document.getElementById('budgetType').value;
      const amount = Math.max(0, parseFloat(document.getElementById('budgetAmount').value) || 0);
      const desc = document.getElementById('budgetDesc').value.trim();
      const category = (type === 'EXPENSE') ? document.getElementById('budgetExpenseCategory').value : 'Gelir';

      if (amount <= 0 || !desc) {
        showToast('Lütfen geçerli tutar ve açıklama girin.', 'error');
        return;
      }

      const newTx = {
        id: Date.now(),
        type: type,
        category: category,
        amount: amount,
        desc: desc,
        date: new Date().toISOString().split('T')[0],
        auto: false
      };

      budgetTransactions.push(newTx);
      saveData();
      renderAll();
      showToast('Kasa hareketi başarıyla işlendi.', 'success');
      
      document.getElementById('budgetAmount').value = '';
      document.getElementById('budgetDesc').value = '';
      const subSelect = document.getElementById('budgetSubItemSelect');
      if(subSelect) subSelect.value = '';
    }

    function deleteBudgetTransaction(id) {
      if (confirm('Bu kasa hareketini silmek istediğinize emin misiniz?')) {
        budgetTransactions = budgetTransactions.filter(t => t.id != id);
        saveData();
        renderAll();
        showToast('Kasa hareketi silindi.', 'error');
      }
    }

    // TARİH FİLTRESİ FONKSİYONLARI
    function toggleDateFilterPicker() {
      const container = document.getElementById('datePickerContainer');
      if (!container) return;
      container.classList.toggle('hidden');
    }

    function applyDateFilter() {
      const startVal = document.getElementById('budgetStartDate').value;
      const endVal = document.getElementById('budgetEndDate').value;

      if (!startVal && !endVal) {
        showToast('Lütfen en az bir tarih seçin.', 'error');
        return;
      }

      filterStartDate = startVal ? startVal : null;
      filterEndDate = endVal ? endVal : null;

      const indicator = document.getElementById('dateFilterActiveIndicator');
      const toggleBtn = document.getElementById('datePickerToggleBtn');
      if (indicator) indicator.classList.remove('hidden');
      if (toggleBtn) {
        toggleBtn.classList.add('text-[var(--accent)]', 'border-[var(--accent)]');
      }

      toggleDateFilterPicker();
      renderBudgetTab();
      showToast('Tarih filtresi uygulandı.', 'success');
    }

    function clearDateFilter() {
      filterStartDate = null;
      filterEndDate = null;

      document.getElementById('budgetStartDate').value = '';
      document.getElementById('budgetEndDate').value = '';

      const indicator = document.getElementById('dateFilterActiveIndicator');
      const toggleBtn = document.getElementById('datePickerToggleBtn');
      if (indicator) indicator.classList.add('hidden');
      if (toggleBtn) {
        toggleBtn.classList.remove('text-[var(--accent)]', 'border-[var(--accent)]');
      }

      toggleDateFilterPicker();
      renderBudgetTab();
      showToast('Tarih filtresi kaldırıldı.', 'info');
    }

    function setBudgetFilter(filterType) {
      currentBudgetFilter = filterType;

      document.querySelectorAll('.budget-filter-btn').forEach(btn => {
        btn.classList.remove('bg-[var(--accent)]', 'text-white', 'shadow-sm');
        btn.classList.add('text-[var(--text-muted)]');
      });

      const activeBtn = document.getElementById('filter-btn-' + filterType);
      if (activeBtn) {
        activeBtn.classList.add('bg-[var(--accent)]', 'text-white', 'shadow-sm');
        activeBtn.classList.remove('text-[var(--text-muted)]');
      }

      renderBudgetTab();
    }

    function renderBudgetTab() {
      const container = document.getElementById('budgetTransactionsContainer');
      const badge = document.getElementById('budgetCountBadge');
      const totalIncomeEl = document.getElementById('budgetTotalIncomeDisplay');
      const totalExpenseEl = document.getElementById('budgetTotalExpenseDisplay');

      if (!container) return;

      const totalIncome = budgetTransactions.filter(t => t.type === 'INCOME').reduce((acc, t) => acc + t.amount, 0);
      const totalExpense = budgetTransactions.filter(t => t.type === 'EXPENSE').reduce((acc, t) => acc + t.amount, 0);

      if (totalIncomeEl) totalIncomeEl.textContent = formatCurrency(totalIncome);
      if (totalExpenseEl) totalExpenseEl.textContent = formatCurrency(totalExpense);

      // GELİR/GİDER VE TARİH SÜZGEÇLERİ
      let filteredTransactions = budgetTransactions.filter(tx => {
        if (currentBudgetFilter === 'INCOME' && tx.type !== 'INCOME') return false;
        if (currentBudgetFilter === 'EXPENSE' && tx.type !== 'EXPENSE') return false;

        if (filterStartDate && tx.date < filterStartDate) return false;
        if (filterEndDate && tx.date > filterEndDate) return false;

        return true;
      });

      if (badge) badge.textContent = `${filteredTransactions.length} İşlem`;

      if (!filteredTransactions || filteredTransactions.length === 0) {
        container.innerHTML = `
          <div class="theme-card rounded-2xl p-6 text-center border border-[var(--border-color)] space-y-2">
            <p class="text-xs sm:text-sm font-medium text-[var(--text-muted)]">Bu filtrelere uygun kasa hareketi bulunmuyor.</p>
          </div>
        `;
        return;
      }

      const sortedTxs = [...filteredTransactions].reverse();

      container.innerHTML = sortedTxs.map(tx => {
        const isIncome = (tx.type === 'INCOME');
        const txCategory = tx.category || (isIncome ? 'Gelir' : 'Kaba İnşaat');

        return `
          <div class="theme-card rounded-xl p-3 border border-[var(--border-color)] flex items-center justify-between gap-2">
            <div class="flex items-center gap-2.5 min-w-0">
              <div class="w-8 h-8 rounded-lg ${isIncome ? 'bg-emerald-500/10 text-emerald-500' : 'bg-rose-500/10 text-rose-500'} flex items-center justify-center shrink-0">
                <i class="fa-solid ${isIncome ? 'fa-arrow-down-left' : 'fa-arrow-up-right'} text-xs"></i>
              </div>
              <div class="min-w-0">
                <div class="font-bold text-xs text-[var(--text-main)] truncate">${escapeHTML(tx.desc)}</div>
                <div class="text-[10px] text-[var(--text-muted)] flex items-center gap-1.5 flex-wrap">
                  <span>${tx.date || ''}</span>
                  <span class="px-1.5 py-0.2 rounded text-[9px] bg-[var(--accent)]/10 text-[var(--accent)] border border-[var(--accent)]/20 font-medium">${escapeHTML(txCategory)}</span>
                  ${tx.auto ? `<span class="px-1.5 py-0.2 rounded text-[9px] bg-sky-500/10 text-sky-500 border border-sky-500/20">Otomatik Peşinat</span>` : ''}
                </div>
              </div>
            </div>

            <div class="flex items-center gap-2 shrink-0">
              <span class="font-black text-xs sm:text-sm ${isIncome ? 'text-emerald-500' : 'text-rose-500'}">
                ${isIncome ? '+' : '-'}${formatCurrency(tx.amount)}
              </span>
              ${!tx.auto ? `
                <button type="button" onclick="window.deleteBudgetTransaction(${tx.id})" title="Sil" class="text-[var(--text-muted)] hover:text-rose-500 p-1 text-xs transition">
                  <i class="fa-solid fa-trash-can"></i>
                </button>
              ` : ''}
            </div>
          </div>
        `;
      }).join('');
    }
    function renderHomeDashboard() {
      const homeIncomeEl = document.getElementById('homeTotalIncomeDisplay');
      const homeExpenseEl = document.getElementById('homeTotalExpenseDisplay');
      const homeDebtEl = document.getElementById('homeTotalDebtDisplay');
      const homeCashEl = document.getElementById('homeNetCashDisplay');

      const totalIncome = budgetTransactions.filter(t => t.type === 'INCOME').reduce((acc, t) => acc + t.amount, 0);
      const totalExpense = budgetTransactions.filter(t => t.type === 'EXPENSE').reduce((acc, t) => acc + t.amount, 0);
      
      // Toplam Borç: Yalnızca purchases kayıtlarına ait kalan borçlar calculateRemainingDebtForRecord ile dinamik toplanır
      let totalDebt = 0;
      if (purchases && purchases.length > 0) {
        purchases.forEach(p => {
          let remainingForThisPurchase = Number(p.debtAmount) || 0;
          const targetKey = String(p.debtId || p.id).trim();

          if (p.debtId || p.id) {
            let foundCari = null;

            // 1. Önce p.cariId ile doğrudan eşleşen cariyi kontrol et
            if (p.cariId) {
              const directCari = (cariList || []).find(c => c.id == p.cariId);
              if (directCari && Array.isArray(directCari.transactions)) {
                const hasValidDebt = directCari.transactions.some(t => {
                  if (t.type !== 'DEBT') return false;
                  const hasValidDebtId = t.debtId !== null && t.debtId !== undefined && String(t.debtId).trim() !== '';
                  const checkKey = hasValidDebtId ? String(t.debtId).trim() : String(t.id).trim();
                  return checkKey === targetKey;
                });
                if (hasValidDebt) {
                  foundCari = directCari;
                }
              }
            }

            // 2. Doğrudan bulunamadıysa tüm carilerde DEBT kaydı (debtId veya id) eşleşmesi ara
            if (!foundCari && cariList && cariList.length > 0) {
              foundCari = cariList.find(c => {
                if (!Array.isArray(c.transactions)) return false;
                return c.transactions.some(t => {
                  if (t.type !== 'DEBT') return false;
                  const hasValidDebtId = t.debtId !== null && t.debtId !== undefined && String(t.debtId).trim() !== '';
                  const checkKey = hasValidDebtId ? String(t.debtId).trim() : String(t.id).trim();
                  return checkKey === targetKey;
                });
              });
            }

            // 3. Gerçek DEBT kaydının bulunduğu cari doğrulandıysa calculateRemainingDebtForRecord kullan
            let hasValidDebtTx = false;
            if (foundCari && Array.isArray(foundCari.transactions)) {
              hasValidDebtTx = foundCari.transactions.some(t => {
                if (t.type !== 'DEBT') return false;
                const hasValidDebtId = t.debtId !== null && t.debtId !== undefined && String(t.debtId).trim() !== '';
                const checkKey = hasValidDebtId ? String(t.debtId).trim() : String(t.id).trim();
                return checkKey === targetKey;
              });
            }

            if (foundCari && hasValidDebtTx) {
              remainingForThisPurchase = calculateRemainingDebtForRecord(foundCari, p.debtId || p.id);
            }
          }
          totalDebt += remainingForThisPurchase;
        });
      }

      const netCash = totalIncome - totalExpense;

      if (homeIncomeEl) homeIncomeEl.textContent = formatCurrency(totalIncome);
      if (homeExpenseEl) homeExpenseEl.textContent = formatCurrency(totalExpense);
      if (homeDebtEl) homeDebtEl.textContent = formatCurrency(totalDebt);
      if (homeCashEl) homeCashEl.textContent = formatCurrency(netCash);
    }
 
    function toggleCategoryAccordion(catName) {
      openCategories[catName] = !openCategories[catName];
      renderCategoriesTab();
    }

    function renderCategoriesTab() {
      const container = document.getElementById('categoriesContainer');
      if (!container) return;

      const categories = appData.categoriesData || defaultCategoriesData;

      container.innerHTML = Object.keys(categories).map(cat => {
        const catMaterials = categories[cat] || [];
        const isOpen = !!openCategories[cat];
        
        const safeCatId = cat.replace(/[^a-zA-Z0-9_-]/g, '_');
        const escapedCatName = cat.replace(/'/g, "\\'");

        return `
          <div class="theme-card border border-[var(--border-color)] rounded-xl overflow-hidden transition-all duration-300">
            <div class="p-4 flex justify-between items-center transition select-none">
              <div onclick="toggleCategoryAccordion('${escapedCatName}')" class="flex items-center gap-3 cursor-pointer flex-1">
                <i class="fa-solid fa-folder text-[var(--accent)] text-lg"></i>
                <div>
                  <h4 class="font-bold text-sm sm:text-base text-[var(--text-main)]">${escapeHTML(cat)}</h4>
                  <span class="text-xs text-[var(--text-muted)]">${catMaterials.length} Örnek Kalem</span>
                </div>
              </div>
              <div class="flex items-center gap-3">
                <button type="button" onclick="window.deleteMainCategory('${escapedCatName}')" title="Kategoriyi Sil" class="text-rose-500 hover:text-rose-400 p-1.5 text-xs transition">
                  <i class="fa-solid fa-trash-can"></i>
                </button>
                <div onclick="toggleCategoryAccordion('${escapedCatName}')" class="cursor-pointer">
                  <i class="fa-solid fa-chevron-${isOpen ? 'up' : 'down'} text-sm text-[var(--text-muted)]"></i>
                </div>
              </div>
            </div>
            ${isOpen ? `
              <div class="border-t border-[var(--border-color)] bg-[var(--bg-main)] p-4 space-y-3 transition-colors duration-300">
                <form onsubmit="window.addCustomItemToCategory(event, '${escapedCatName}')" class="flex gap-2 mb-3">
                  <input type="text" id="newCatItemInput_${safeCatId}" placeholder="Yeni örnek ürün/hizmet ekle..." required class="flex-1 rounded-lg px-3 py-1.5 text-xs focus:outline-none transition-colors duration-300">
                  <button type="submit" class="bg-[var(--accent)] hover:opacity-90 text-white font-bold px-3 py-1.5 rounded-lg text-xs transition shadow cursor-pointer">
                    <i class="fa-solid fa-plus mr-1"></i> Ekle
                  </button>
                </form>

                <div class="space-y-2">
                  ${catMaterials.map(matName => {
                    const escapedMatName = matName.replace(/'/g, "\\'");
                    return `
                      <div class="bg-[var(--bg-card)] border border-[var(--border-color)] p-2.5 sm:p-3 rounded-lg flex justify-between items-center transition-colors duration-300">
                        <div class="font-medium text-xs sm:text-sm flex items-center gap-2 text-[var(--text-main)]">
                          <i class="fa-solid fa-circle-dot text-[8px] text-[var(--accent)]"></i> ${escapeHTML(matName)}
                        </div>
                        <button type="button" onclick="window.removeCategoryItem('${escapedCatName}', '${escapedMatName}')" class="text-rose-500 hover:text-rose-400 p-1 text-xs cursor-pointer" title="Kalemi Sil">
                          <i class="fa-solid fa-trash"></i>
                        </button>
                      </div>
                    `;
                  }).join('')}
                </div>
              </div>
            ` : ''}
          </div>
        `;
      }).join('');
    }

    function addNewMainCategory(e) {
      e.preventDefault();
      const input = document.getElementById('newMainCategoryInput');
      if (!input) return;

      const catName = input.value.trim();
      if (!catName) return;

      if (appData.categoriesData[catName]) {
        showToast('Bu isimde bir kategori zaten mevcut!', 'error');
        return;
      }

      appData.categoriesData[catName] = [];
      openCategories[catName] = true;
      saveData();
      renderAll();
      input.value = '';
      showToast(`"${catName}" kategorisi başarıyla eklendi.`, 'success');
    }

    function deleteMainCategory(catName) {
      const linkedMaterials = (materials || []).filter(m => m.stage === catName);
      const linkedPurchases = (purchases || []).filter(p => p.stage === catName);
      const linkedBudget = (budgetTransactions || []).filter(b => b.category === catName);
      const totalDependencies = linkedMaterials.length + linkedPurchases.length + linkedBudget.length;

      if (totalDependencies > 0) {
        alert(
          `⚠️ "${catName}" kategorisi silinemez!\n\n` +
          `Bu kategoriye bağlı;\n` +
          `- ${linkedMaterials.length} adet malzeme kartı\n` +
          `- ${linkedPurchases.length} adet satın alma kaydı\n` +
          `- ${linkedBudget.length} adet bütçe/kasa hareketi bulunuyor.\n\n` +
          `Lütfen önce bu kayıtları silin.`
        );
        return;
      }

      if (!confirm(`"${catName}" ana kategorisini silmek istediğinize emin misiniz?`)) return;

      delete appData.categoriesData[catName];
      delete openCategories[catName];
      
      saveData();
      renderAll();
      showToast(`"${catName}" kategorisi silindi.`, 'error');
    }

    function addCustomItemToCategory(event, catName) {
      event.preventDefault();
      
      const safeCatId = catName.replace(/[^a-zA-Z0-9_-]/g, '_');
      const inputEl = document.getElementById(`newCatItemInput_${safeCatId}`);
      if (!inputEl) return;

      const itemName = inputEl.value.trim();
      if (!itemName) return;

      if (!appData.categoriesData[catName]) {
        appData.categoriesData[catName] = [];
      }

      const exists = appData.categoriesData[catName].some(
        item => item.toLowerCase() === itemName.toLowerCase()
      );

      if (!exists) {
        appData.categoriesData[catName].push(itemName);
        saveData();
        renderAll();
        showToast(`"${itemName}" kalemi eklendi.`, 'success');
      } else {
        showToast("Bu ürün veya hizmet bu kategoride zaten mevcut!", 'error');
      }
    }

    function removeCategoryItem(catName, itemName) {
      if (!confirm(`"${itemName}" ürününü süzgeçten silmek istiyor musunuz?`)) return;

      if (appData.categoriesData[catName]) {
        appData.categoriesData[catName] = appData.categoriesData[catName].filter(i => i !== itemName);
        saveData();
        renderAll();
        showToast('Kalem silindi.', 'error');
      }
    }
    function resetSystemData() {
      if (!confirm("Tüm sistem verileri (malzemeler, satın almalar, bütçe hareketleri ve cari hesaplar) sıfırlanacak. Onaylıyor musunuz?")) return;

      localStorage.removeItem('insaTakip_materials');
      localStorage.removeItem('insaTakip_purchases');
      localStorage.removeItem('insaTakip_budget');
      localStorage.removeItem('insaTakip_categories');
      localStorage.removeItem('insaTakip_cariList'); // <-- Cari listesini de siliyoruz
      
      materials = [];
      purchases = [];
      budgetTransactions = [];
      cariList = []; // <-- Cari dizisini sıfırlıyoruz
      activeCariId = null;
      appData.categoriesData = defaultCategoriesData;

      saveData();
      renderAll();
      showToast('Tüm sistem verileri ve cari hesaplar sıfırlandı.', 'error');
    }


    function saveData() {
      localStorage.setItem('insaTakip_materials', JSON.stringify(materials));
      localStorage.setItem('insaTakip_purchases', JSON.stringify(purchases));
      localStorage.setItem('insaTakip_budget', JSON.stringify(budgetTransactions));
      localStorage.setItem('insaTakip_cariList', JSON.stringify(cariList)); 
      localStorage.setItem('insaTakip_categories', JSON.stringify(appData.categoriesData));
      localStorage.setItem('insaTakip_theme', appData.theme);
    }

    function renderAll() {
      applyTheme();
      renderHomeDashboard();
      renderCategoriesTab();
      updateInlineCategoryOptions();
      renderMaterialsList();
      renderPurchasedList();
      renderBudgetTab();
      renderSettingsTab();
    }

    function showToast(message, type = 'info') {
      const toast = document.createElement('div');
      toast.className = `fixed bottom-20 right-4 z-50 px-4 py-2.5 rounded-xl text-white text-sm font-medium shadow-lg transition-all duration-300 transform translate-y-2 opacity-0 ${
        type === 'error' ? 'bg-rose-500' : type === 'success' ? 'bg-emerald-500' : 'bg-[var(--accent)]'
      }`;
      toast.textContent = message;
      document.body.appendChild(toast);
      setTimeout(() => toast.classList.remove('translate-y-2', 'opacity-0'), 10);
      setTimeout(() => {
        toast.classList.add('opacity-0', 'translate-y-2');
        setTimeout(() => toast.remove(), 300);
      }, 3000);
    }
    function openPlanPaymentModal(cariId, planId) {
      const cari = (cariList || []).find(c => c.id == cariId);
      if (!cari) return;

      const plan = (cari.paymentPlans || []).find(p => p.id == planId);
      if (!plan) return;

      if (!plan.debtId) {
        showToast('Hata: Ödeme planına ait geçerli bir borç ID bulunamadı!', 'error');
        return;
      }

      const matchingDebt = (cari.transactions || []).find(t => t.type === 'DEBT' && t.id === plan.debtId);
      if (!matchingDebt) {
        showToast('Hata: Ödeme planı sistemde gerçek bir DEBT kaydıyla eşleşmiyor!', 'error');
        return;
      }

      const planAmount = Number(plan.amount);
      const planPaid = Number(plan.paidAmount || 0);
      const planRemaining = Math.max(0, planAmount - planPaid);
      const debtRemaining = calculateRemainingDebtForRecord(cari, plan.debtId);
      const maxAllowed = Math.min(planRemaining, debtRemaining);

      const modal = document.getElementById('planPaymentModal');
      const cariIdInput = document.getElementById('planPaymentCariId');
      const planIdInput = document.getElementById('planPaymentPlanId');
      const uniqueIdInput = document.getElementById('planPaymentUniqueId');
      const amountInput = document.getElementById('planPaymentAmount');
      const categorySelect = document.getElementById('planPaymentCategory');
      const maxInfoEl = document.getElementById('planPaymentMaxInfo');

      if (cariIdInput) cariIdInput.value = cari.id;
      if (planIdInput) planIdInput.value = plan.id;
      if (uniqueIdInput) uniqueIdInput.value = generatePaymentId();

      // Kategori Seçeneklerini Sistem Kategorileriyle Doldur
      if (categorySelect) {
        const validCategories = (typeof defaultCategoriesData !== 'undefined' && defaultCategoriesData) 
          ? Object.keys(defaultCategoriesData) 
          : ["Kaba İnşaat", "İnce İnşaat", "İşçilik", "Proje ve Ruhsat"];
        
        categorySelect.innerHTML = validCategories.map(cat => `<option value="${escapeHTML(cat)}">${escapeHTML(cat)}</option>`).join('');
      }

      if (amountInput) {
        amountInput.value = maxAllowed > 0 ? maxAllowed : 0;
        amountInput.max = maxAllowed;
        amountInput.min = 0.01;
      }

      if (maxInfoEl) {
        maxInfoEl.textContent = `Maksimum Ödenecek: ${formatCurrency(maxAllowed)} (Taksit Kalan: ${formatCurrency(planRemaining)}, Borç Kalan: ${formatCurrency(debtRemaining)})`;
      }

      if (modal) {
        modal.classList.remove('hidden');
        document.body.style.overflow = 'hidden';
      }
    }
       
    function closePlanPaymentModal() {
      const modal = document.getElementById('planPaymentModal');
      if (modal) {
        modal.classList.add('hidden');
        document.body.style.overflow = '';
        const form = document.getElementById('planPaymentForm');
        if (form) form.reset();
        const uniqueIdInput = document.getElementById('planPaymentUniqueId');
        if (uniqueIdInput) uniqueIdInput.value = '';
      }
    }

    function handlePlanPaymentSubmit(event) {
      event.preventDefault();

      const cariIdField = document.getElementById('planPaymentCariId');
      const planIdField = document.getElementById('planPaymentPlanId');
      const uniqueIdField = document.getElementById('planPaymentUniqueId');
      const amountField = document.getElementById('planPaymentAmount');
      const categoryField = document.getElementById('planPaymentCategory');
      const descField = document.getElementById('planPaymentDesc');

      if (!cariIdField || !planIdField || !amountField) return;

      if (!uniqueIdField || !uniqueIdField.value) {
        showToast('Güvenlik Hatası: Geçersiz veya eksik işlem kimliği (paymentId).', 'error');
        return;
      }

      if (!categoryField || !categoryField.value || categoryField.value.trim() === '') {
        showToast('Hata: Gider kategorisi seçilmedi.', 'error');
        return;
      }

      const selectedCategory = categoryField.value.trim();

      // Madde 7 & 8: Gerçek kategori kaynağı (defaultCategoriesData anahtarları)
      const validCategories = (typeof defaultCategoriesData !== 'undefined' && defaultCategoriesData) 
        ? Object.keys(defaultCategoriesData) 
        : [];

      // Boş liste nedeniyle kontrolün atlanmasına izin verilmez; liste boşsa durdurulur
      if (validCategories.length === 0 || !validCategories.includes(selectedCategory)) {
        showToast('Hata: Seçilen kategori sistemdeki gerçek kategorilerle eşleşmiyor veya liste boş!', 'error');
        return;
      }

      const cariId = cariIdField.value;
      const planId = planIdField.value;
      const paymentId = uniqueIdField.value;
      
      const rawInputAmount = Number(amountField.value);
      if (!Number.isFinite(rawInputAmount)) {
        showToast('Lütfen geçerli bir ödeme tutarı girin.', 'error');
        return;
      }

      const amount = rawInputAmount;
      const description = descField ? descField.value.trim() : 'Taksit Ödemesi';

      const cari = (cariList || []).find(c => c.id == cariId);
      if (!cari) {
        showToast('Cari hesap bulunamadı.', 'error');
        return;
      }

      const plan = (cari.paymentPlans || []).find(p => p.id == planId);
      if (!plan) {
        showToast('Ödeme planı bulunamadı.', 'error');
        return;
      }

      if (!plan.debtId) {
        showToast('Hata: Ödeme planı borç ID bilgisi içermiyor.', 'error');
        return;
      }

      // Madde 9: Kesin DEBT eşleşmesi (t.id üzerinden)
      const matchingDebt = (cari.transactions || []).find(t => t.type === 'DEBT' && t.id === plan.debtId);
      if (!matchingDebt) {
        showToast('Geçersiz borç eşleşmesi! Ödeme yapılamaz.', 'error');
        return;
      }

      const planAmount = Number(plan.amount);
      const planPaid = Number(plan.paidAmount);

      if (!Number.isFinite(planAmount) || planAmount <= 0 || !Number.isFinite(planPaid) || planPaid < 0 || planPaid > planAmount) {
        showToast('Hata: Plan sayısal verileri kural dışı veya geçersiz.', 'error');
        return;
      }

      const planRemaining = Math.max(0, planAmount - planPaid);
      
      const rawDebtRemaining = calculateRemainingDebtForRecord(cari, plan.debtId);
      if (!Number.isFinite(rawDebtRemaining) || rawDebtRemaining < 0) {
        showToast('Hata: Kalan borç miktarı geçersiz veya negatif. Ödeme durduruldu!', 'error');
        return;
      }
      const debtRemaining = rawDebtRemaining;
      
      const maxAllowed = Math.min(planRemaining, debtRemaining);

      if (amount <= 0) {
        showToast('Lütfen pozitif bir ödeme tutarı girin.', 'error');
        return;
      }

      if (amount > maxAllowed) {
        showToast(`Ödeme tutarı izin verilen maksimum (${formatCurrency(maxAllowed)}) tutarı aşamaz!`, 'error');
        return;
      }

      const alreadyPaidTx = (cari.transactions || []).some(t => t.id === paymentId);
      const alreadyPaidBudget = (budgetTransactions || []).some(b => b.id === paymentId);
      if (alreadyPaidTx || alreadyPaidBudget) {
        showToast('Bu ödeme işlemi daha önce gerçekleştirilmiş.', 'error');
        closePlanPaymentModal();
        return;
      }

      const memCariBackup = JSON.parse(JSON.stringify(cariList));
      const memBudgetBackup = JSON.parse(JSON.stringify(budgetTransactions));
      
      const lsKeys = {
        cariList: localStorage.getItem('insaTakip_cariList'),
        budget: localStorage.getItem('insaTakip_budget'),
        materials: localStorage.getItem('insaTakip_materials'),
        purchases: localStorage.getItem('insaTakip_purchases'),
        categories: localStorage.getItem('insaTakip_categories'),
        theme: localStorage.getItem('insaTakip_theme')
      };

      try {
        const paymentTx = {
          id: paymentId,
          type: 'PAYMENT',
          debtId: plan.debtId,
          amount: amount,
          date: getTurkeyLocalDate(),
          description: description
        };

        if (!Array.isArray(cari.transactions)) cari.transactions = [];
        cari.transactions.push(paymentTx);

        const newPaidAmount = planPaid + amount;
        plan.paidAmount = newPaidAmount;

        if (newPaidAmount <= 0) {
          plan.status = 'PENDING';
          plan.isPaid = false;
        } else if (newPaidAmount < planAmount) {
          plan.status = 'PARTIAL';
          plan.isPaid = false;
        } else {
          plan.status = 'COMPLETED';
          plan.isPaid = true;
          plan.paidAmount = planAmount;
        }

        const budgetExpense = {
          id: paymentId,
          type: 'EXPENSE',
          category: selectedCategory,
          amount: amount,
          desc: `Cari Ödemesi: ${cari.name} - ${description}`,
          date: getTurkeyLocalDate(),
          auto: true
        };

        if (!Array.isArray(budgetTransactions)) budgetTransactions = [];
        budgetTransactions.push(budgetExpense);

        localStorage.setItem('insaTakip_cariList', JSON.stringify(cariList));
        saveData();

        closePlanPaymentModal();

        renderCariTab();
        if (activeCariId == cari.id) {
          renderCariDetail(cari);
        }
        renderBudgetTab();

        showToast('Ödeme başarıyla kaydedildi.', 'success');

      } catch (error) {
        try {
          cariList = memCariBackup;
          budgetTransactions = memBudgetBackup;

          // Madde 11: Rollback sonrasında tekrar saveData() çağrılmaz; localStorage doğrudan eski değerlerine set edilir
          Object.keys(lsKeys).forEach(key => {
            const lsName = `insaTakip_${key}`;
            if (lsKeys[key] !== null) {
              localStorage.setItem(lsName, lsKeys[key]);
            } else {
              localStorage.removeItem(lsName);
            }
          });

          showToast('Ödeme başarısız oldu, sistem güvenli şekilde önceki durumuna geri döndürüldü.', 'error');
        } catch (rollbackError) {
          console.error('Kritik Rollback Hatası:', rollbackError);
          showToast('Kritik Hata: İşlem başarısız oldu ve sistem eski haline getirilemedi!', 'error');
        }

        console.error('Ödeme işlemi hatası:', error);
      }
    }
    function openCariDebtModal() {
      const modal = document.getElementById('cariDebtModal');
      if (modal) {
        modal.classList.remove('hidden');
        document.body.style.overflow = 'hidden';
      }
    }

    function closeCariDebtModal() {
      const modal = document.getElementById('cariDebtModal');
      if (modal) {
        modal.classList.add('hidden');
        document.body.style.overflow = '';
        const form = document.querySelector('#cariDebtModal form');
        if (form) form.reset();
      }
    }

    function handleCariDebtSubmit(event) {
      event.preventDefault();
      if (!activeCariId) return;

      const cari = cariList.find(c => c.id == activeCariId);
      if (!cari) return;

      const amount = Number(document.getElementById('cariDebtAmount').value);
      const desc = document.getElementById('cariDebtDesc').value.trim();

      if (amount <= 0 || !desc) {
        showToast('Lütfen geçerli tutar ve açıklama girin.', 'error');
        return;
      }

      const debtTx = {
        id: generatePaymentId(),
        type: 'DEBT',
        amount: amount,
        date: getTurkeyLocalDate(),
        description: desc
      };

      if (!Array.isArray(cari.transactions)) cari.transactions = [];
      cari.transactions.push(debtTx);

      saveData();
      closeCariDebtModal();
      renderCariDetail(cari);
      renderCariTab();
      showToast('Borç hareketi başarıyla eklendi.', 'success');
    }

    // Global köprülere ekle:
    window.openCariDebtModal = openCariDebtModal;
    window.closeCariDebtModal = closeCariDebtModal;
    window.handleCariDebtSubmit = handleCariDebtSubmit;
    function openCariDirectPaymentModal() {
      if (!activeCariId) {
        showToast('Önce bir cari hesap seçmelisiniz.', 'error');
        return;
      }
      const modal = document.getElementById('cariDirectPaymentModal');
      if (modal) {
        modal.classList.remove('hidden');
        document.body.style.overflow = 'hidden';
      }
    }

    function closeCariDirectPaymentModal() {
      const modal = document.getElementById('cariDirectPaymentModal');
      if (modal) {
        modal.classList.add('hidden');
        document.body.style.overflow = '';
        const form = document.querySelector('#cariDirectPaymentModal form');
        if (form) form.reset();
      }
    }
    function handleCariDirectPaymentSubmit(event) {
      event.preventDefault();
      if (!activeCariId) return;

      const cari = (cariList || []).find(c => c.id == activeCariId);
      if (!cari) return;

      const amountField = document.getElementById('cariDirectPayAmount');
      const catField = document.getElementById('cariDirectPayCategory');
      const descField = document.getElementById('cariDirectPayDesc');

      const totalPaymentAmount = Number(amountField ? amountField.value : 0);
      const category = catField ? catField.value : 'Kaba İnşaat';
      const description = descField ? descField.value.trim() : 'Cari Ödemesi';

      if (!Number.isFinite(totalPaymentAmount) || totalPaymentAmount <= 0) {
        showToast('Lütfen geçerli bir ödeme tutarı girin.', 'error');
        return;
      }

      const transactions = cari.transactions || [];

      // calculateRemainingDebtForRecord ile birebir aynı kimlik doğrulama mantığı
      const debtMap = new Map();
      transactions.forEach(t => {
        if (t.type === 'DEBT') {
          const hasValidDebtId = t.debtId !== null && t.debtId !== undefined && String(t.debtId).trim() !== '';
          const rawId = hasValidDebtId ? t.debtId : t.id;
          
          if (rawId !== null && rawId !== undefined && String(rawId).trim() !== '') {
            const normalizedId = String(rawId).trim();
            if (!debtMap.has(normalizedId)) {
              debtMap.set(normalizedId, {
                debtId: normalizedId,
                date: t.date,
                amount: Number(t.amount) || 0
              });
            } else {
              const existing = debtMap.get(normalizedId);
              existing.amount += Number(t.amount) || 0;
              if (t.date && (!existing.date || new Date(t.date) < new Date(existing.date))) {
                existing.date = t.date;
              }
            }
          }
        }
      });
      const debtRecords = Array.from(debtMap.values());

      debtRecords.sort((a, b) => {
        const timeA = a.date ? new Date(a.date).getTime() : 0;
        const timeB = b.date ? new Date(b.date).getTime() : 0;
        const validA = Number.isNaN(timeA) ? 0 : timeA;
        const validB = Number.isNaN(timeB) ? 0 : timeB;
        return validA - validB;
      });

      let totalRemainingDebt = 0;
      const debtDetails = debtRecords.map(debt => {
        const targetDebtId = debt.debtId;
        const remaining = calculateRemainingDebtForRecord(cari, targetDebtId);
        totalRemainingDebt += remaining;
        return { debtId: targetDebtId, remaining };
      }).filter(d => d.remaining > 0);

      if (debtDetails.length > 0 && totalPaymentAmount > totalRemainingDebt) {
        showToast(`Ödeme tutarı (${formatCurrency(totalPaymentAmount)}), carinin toplam kalan borcunu (${formatCurrency(totalRemainingDebt)}) aşamaz!`, 'error');
        return;
      }

      try {
        const masterPaymentGroupId = generatePaymentId();
        let remainingToDistribute = totalPaymentAmount;

        if (!Array.isArray(cari.transactions)) cari.transactions = [];

        for (const debt of debtDetails) {
          if (remainingToDistribute <= 0) break;

          const paymentForThisDebt = Math.min(remainingToDistribute, debt.remaining);

          const splitPaymentTx = {
            id: generatePaymentId(),
            debtId: debt.debtId,
            type: 'PAYMENT',
            amount: paymentForThisDebt,
            date: getTurkeyLocalDate(),
            description: `${description} (Borç Ödemesi)`
          };

          cari.transactions.push(splitPaymentTx);
          remainingToDistribute -= paymentForThisDebt;
        }

        if (remainingToDistribute > 0) {
          const generalPaymentTx = {
            id: generatePaymentId(),
            debtId: null,
            type: 'PAYMENT',
            amount: remainingToDistribute,
            date: getTurkeyLocalDate(),
            description: description
          };
          cari.transactions.push(generalPaymentTx);
        }

        const budgetExpense = {
          id: masterPaymentGroupId,
          type: 'EXPENSE',
          category: category,
          amount: totalPaymentAmount,
          desc: `Cari Ödemesi: ${cari.name} - ${description}`,
          date: getTurkeyLocalDate(),
          auto: true
        };

        if (!Array.isArray(budgetTransactions)) budgetTransactions = [];
        budgetTransactions.push(budgetExpense);

        saveData();
        closeCariDirectPaymentModal();
        renderCariDetail(cari);
        renderCariTab();
        renderBudgetTab();
        renderHomeDashboard();

        showToast('Ödeme kasadan düşüldü ve ilgili borçlara güvenli FIFO sırasıyla işlendi.', 'success');
      } catch (error) {
        showToast('Ödeme kaydedilemedi!', 'error');
        console.error(error);
      }
    }

    // Global Scope Köprülerine ekle:
    window.openCariDirectPaymentModal = openCariDirectPaymentModal;
    window.closeCariDirectPaymentModal = closeCariDirectPaymentModal;
    window.handleCariDirectPaymentSubmit = handleCariDirectPaymentSubmit;

    renderAll();
  </script>
</body>
</html>
