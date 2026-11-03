<!DOCTYPE html>
<html lang="tr" class="h-full bg-slate-100">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Akıllı Masa Standı & Dijital Menü Hub</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700;800&family=Playfair+Display:ital,wght@0,600;0,700;1,600&display=swap" rel="stylesheet">
    <!-- QR Code Generator JS -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>

    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        brand: {
                            50: '#fffbeb',
                            100: '#fef3c7',
                            500: '#f59e0b',
                            600: '#d97706',
                            700: '#b45309',
                            800: '#92400e',
                            900: '#78350f',
                            dark: '#1e1b18'
                        }
                    },
                    fontFamily: {
                        sans: ['"Plus Jakarta Sans"', 'sans-serif'],
                        serif: ['"Playfair Display"', 'serif']
                    }
                }
            }
        }
    </script>
    <style>
        /* Custom Scrollbar */
        ::-webkit-scrollbar {
            width: 6px;
        }
        ::-webkit-scrollbar-track {
            background: rgba(0,0,0,0.05);
        }
        ::-webkit-scrollbar-thumb {
            background: rgba(0,0,0,0.2);
            border-radius: 10px;
        }
        
        .glass-card {
            background: rgba(255, 255, 255, 0.85);
            backdrop-filter: blur(12px);
            border: 1px solid rgba(255, 255, 255, 0.4);
        }
        
        .pulse-nfc {
            box-shadow: 0 0 0 0 rgba(217, 119, 6, 0.7);
            animation: pulse-animation 2s infinite;
        }
        
        @keyframes pulse-animation {
            0% {
                box-shadow: 0 0 0 0 rgba(217, 119, 6, 0.5);
            }
            70% {
                box-shadow: 0 0 0 15px rgba(217, 119, 6, 0);
            }
            100% {
                box-shadow: 0 0 0 0 rgba(217, 119, 6, 0);
            }
        }
    </style>
</head>
<body class="h-full text-slate-800 antialiased flex flex-col justify-between selection:bg-brand-500 selection:text-white">

    <!-- Header Control Bar for Demo / Admin Toggle -->
    <header class="bg-brand-dark text-white px-4 py-3 shadow-md flex justify-between items-center z-30">
        <div class="flex items-center space-x-2">
            <span class="inline-block w-3 h-3 bg-emerald-500 rounded-full animate-ping"></span>
            <h1 class="font-bold text-sm sm:text-base tracking-wide flex items-center gap-2">
                <i class="fa-solid fa-qrcode text-brand-500"></i> Akıllı Masa Standı & Dijital Bağlantı Paneli
            </h1>
        </div>
        <div class="flex items-center gap-2">
            <button onclick="toggleAdminModal()" class="bg-brand-600 hover:bg-brand-700 text-white text-xs sm:text-sm px-3 py-1.5 rounded-lg transition-all flex items-center gap-1.5 shadow">
                <i class="fa-solid fa-gear"></i> <span class="hidden sm:inline">Restoran & QR </span>
            </button>
        </div>
    </header>

    <!-- Main Container Layout -->
    <main class="flex-1 flex items-center justify-center p-2 sm:p-6 overflow-y-auto bg-gradient-to-br from-amber-50 via-slate-100 to-amber-100/50">
        
        <!-- Mobile Device Frame Container -->
        <div class="w-full max-w-sm sm:max-w-md h-[780px] max-h-[92vh] bg-slate-900 rounded-[45px] p-3 sm:p-4 shadow-2xl shadow-slate-900/40 border-[6px] border-slate-800 relative flex flex-col transition-all">
            
            <!-- Phone Top Speaker & Camera Bar -->
            <div class="absolute top-0 left-1/2 -translate-x-1/2 w-36 h-6 bg-slate-800 rounded-b-2xl z-40 flex justify-center items-center gap-2">
                <div class="w-12 h-1 bg-slate-600 rounded-full"></div>
                <div class="w-2.5 h-2.5 bg-slate-700 rounded-full"></div>
            </div>

            <!-- Mobile Inner Screen -->
            <div class="w-full h-full bg-slate-50 rounded-[35px] overflow-hidden flex flex-col relative z-10 pt-4">

                <!-- Dynamic Restaurant Top Banner -->
                <div class="relative bg-brand-dark text-white pt-8 pb-10 px-6 text-center rounded-b-[35px] shadow-lg overflow-hidden">
                    <!-- Background Pattern Decor -->
                    <div class="absolute inset-0 opacity-10 bg-[radial-gradient(#fff_1px,transparent_1px)] [background-size:16px_16px]"></div>
                    
                    <div class="relative z-10 flex flex-col items-center">
                        <div class="w-20 h-20 rounded-full bg-white p-1 shadow-md mb-2 relative group">
                            <img id="display-logo" src="https://images.unsplash.com/photo-1555396273-367ea4eb4db5?auto=format&fit=crop&w=200&q=80" alt="Restoran Logosu" class="w-full h-full object-cover rounded-full">
                            <span class="absolute -bottom-1 -right-1 bg-brand-500 text-white p-1 rounded-full text-xs shadow">
                                <i class="fa-solid fa-utensils"></i>
                            </span>
                        </div>
                        <h2 id="display-name" class="font-serif text-2xl font-bold tracking-tight text-amber-100">Yılmaz Pide</h2>
                        <p id="display-tagline" class="text-xs text-slate-300 mt-0.5 font-light">Lezzetin Dijital & Modern Adresi</p>
                        
                        <!-- NFC Badge -->
                        <div class="mt-3 inline-flex items-center gap-1.5 px-3 py-0.5 rounded-full bg-amber-500/20 text-amber-300 text-[11px] font-medium border border-amber-500/30">
                            <i class="fa-solid fa-wifi rotate-90 text-xs"></i> Akıllı Masa NFC/QR Destekli
                        </div>
                    </div>
                </div>

                <!-- Scrollable Main Content / Hub Links -->
                <div id="hub-view" class="flex-1 overflow-y-auto p-4 space-y-3.5 -mt-4 z-20">
                    
                    <!-- Callout Banner -->
                    <div class="glass-card p-3 rounded-2xl text-center shadow-sm border border-amber-200/60 mb-2">
                        <p class="text-xs text-slate-600 font-medium">
                            <i class="fa-solid fa-hand-pointer text-brand-600 animate-bounce mr-1"></i> Masanıza hoş geldiniz! İstediğiniz seçeneğe dokunun.
                        </p>
                    </div>

                    <!-- 1. DİJİTAL MENÜ BUTTON -->
                    <button onclick="switchTab('menu')" class="w-full group bg-gradient-to-r from-brand-600 to-brand-700 hover:from-brand-700 hover:to-brand-800 text-white p-4 rounded-2xl shadow-md hover:shadow-lg transition-all duration-200 flex items-center justify-between border border-amber-500/20 active:scale-[0.98]">
                        <div class="flex items-center space-x-3.5">
                            <div class="w-12 h-12 rounded-xl bg-white/10 flex items-center justify-center text-xl text-amber-200 group-hover:scale-110 transition-transform">
                                <i class="fa-solid fa-book-open"></i>
                            </div>
                            <div class="text-left">
                                <span class="block font-bold text-base tracking-wide">Dijital Menü</span>
                                <span class="block text-xs text-amber-100/80 font-light">Yemekler, Tatlılar & İçecekler</span>
                            </div>
                        </div>
                        <i class="fa-solid fa-chevron-right text-amber-200/70 group-hover:translate-x-1 transition-transform"></i>
                    </button>

                    <!-- 2. GOOGLE REVIEWS BUTTON -->
                    <a id="display-google-link" href="https://search.google.com/local/writereview?placeid=ChIJyUyKKbchY0ARAgpXrk8YfV8" target="_blank" class="w-full group bg-white hover:bg-slate-50 text-slate-800 p-4 rounded-2xl shadow-sm hover:shadow-md transition-all duration-200 flex items-center justify-between border border-slate-200/80 active:scale-[0.98]">
                        <div class="flex items-center space-x-3.5">
                            <div class="w-12 h-12 rounded-xl bg-amber-50 text-amber-500 flex items-center justify-center text-xl group-hover:scale-110 transition-transform">
                                <i class="fa-solid fa-star"></i>
                            </div>
                            <div class="text-left">
                                <div class="flex items-center gap-1.5">
                                    <span class="font-bold text-sm sm:text-base">Google'da Değerlendir</span>
                                    <span class="flex text-amber-400 text-xs">
                                        <i class="fa-solid fa-star"></i>
                                        <i class="fa-solid fa-star"></i>
                                        <i class="fa-solid fa-star"></i>
                                        <i class="fa-solid fa-star"></i>
                                        <i class="fa-solid fa-star"></i>
                                    </span>
                                </div>
                                <span class="block text-xs text-slate-500 font-light">Bizim için 1 dakikanızı ayırın ⭐</span>
                            </div>
                        </div>
                        <i class="fa-solid fa-arrow-up-right-from-square text-slate-400 group-hover:text-brand-600 transition-colors text-sm"></i>
                    </a>

                    <!-- 3. INSTAGRAM BUTTON -->
                    <a id="display-instagram-link" href="https://www.instagram.com/yilmazrestorantordu?utm_source=ig_web_button_share_sheet&srtk=ZDNlZDc0MzIxNw==" target="_blank" class="w-full group bg-white hover:bg-slate-50 text-slate-800 p-4 rounded-2xl shadow-sm hover:shadow-md transition-all duration-200 flex items-center justify-between border border-slate-200/80 active:scale-[0.98]">
                        <div class="flex items-center space-x-3.5">
                            <div class="w-12 h-12 rounded-xl bg-pink-50 text-pink-600 flex items-center justify-center text-2xl group-hover:scale-110 transition-transform">
                                <i class="fa-brands fa-instagram"></i>
                            </div>
                            <div class="text-left">
                                <span id="display-instagram-handle" class="block font-bold text-sm sm:text-base">@yilmazrestorantordu</span>
                                <span class="block text-xs text-slate-500 font-light">Instagram'da bizi takip edin & Etiketleyin</span>
                            </div>
                        </div>
                        <i class="fa-solid fa-arrow-up-right-from-square text-slate-400 group-hover:text-pink-600 transition-colors text-sm"></i>
                    </a>

                    <!-- 4. WI-FI & HIZLI İLETİŞİM / KONUM -->
                    <div class="grid grid-cols-2 gap-3 pt-1">
                        <button onclick="showWifiModal()" class="bg-white p-3 rounded-2xl shadow-sm border border-slate-200/80 flex flex-col items-center justify-center text-center hover:bg-slate-50 transition-colors">
                            <i class="fa-solid fa-wifi text-emerald-600 text-lg mb-1"></i>
                            <span class="font-semibold text-xs text-slate-700">Ücretsiz Wi-Fi</span>
                            <span class="text-[10px] text-slate-400">Şifreyi Gör</span>
                        </button>
                        <a id="display-phone-link" href="tel:05555555555" class="bg-white p-3 rounded-2xl shadow-sm border border-slate-200/80 flex flex-col items-center justify-center text-center hover:bg-slate-50 transition-colors">
                            <i class="fa-solid fa-phone text-blue-600 text-lg mb-1"></i>
                            <span class="font-semibold text-xs text-slate-700">Garson / İletişim</span>
                            <span class="text-[10px] text-slate-400">Hemen Ara</span>
                        </a>
                    </div>

                    <!-- Footer Note in Mobile -->
                    <div class="text-center pt-3 pb-2">
                        <p class="text-[10px] text-slate-400 flex items-center justify-center gap-1">
                            <i class="fa-solid fa-shield-halved"></i> Güvenli Temassız Masa Sistemi
                        </p>
                    </div>
                </div>

                <!-- MENU VIEW TAB (Modal inside mobile screen) -->
                <div id="menu-view" class="hidden flex-1 flex-col overflow-hidden bg-slate-50 z-30 -mt-4 rounded-t-[30px]">
                    <!-- Menu Header Bar -->
                    <div class="bg-white p-3 px-4 border-b border-slate-200 flex justify-between items-center shadow-sm">
                        <button onclick="switchTab('hub')" class="text-slate-600 hover:text-brand-600 text-sm font-semibold flex items-center gap-1">
                            <i class="fa-solid fa-arrow-left"></i> Geri
                        </button>
                        <span class="font-serif font-bold text-slate-800 text-base">Lezzet Menümüz</span>
                        <div class="w-10"></div><!-- spacer -->
                    </div>

                    <!-- Category Pills Horizontal Scroll -->
                    <div class="bg-white px-3 py-2.5 border-b border-slate-100 flex gap-2 overflow-x-auto no-scrollbar">
                        <button onclick="filterMenu('all')" class="menu-cat-btn active px-3.5 py-1.5 rounded-full text-xs font-semibold whitespace-nowrap bg-brand-600 text-white shadow-sm">Tümü</button>
                        <button onclick="filterMenu('pide')" class="menu-cat-btn px-3.5 py-1.5 rounded-full text-xs font-semibold whitespace-nowrap bg-slate-100 text-slate-600 hover:bg-slate-200">Pideler & Lahmacun</button>
                        <button onclick="filterMenu('ana-yemek')" class="menu-cat-btn px-3.5 py-1.5 rounded-full text-xs font-semibold whitespace-nowrap bg-slate-100 text-slate-600 hover:bg-slate-200">Ana Yemekler</button>
                        <button onclick="filterMenu('tatli')" class="menu-cat-btn px-3.5 py-1.5 rounded-full text-xs font-semibold whitespace-nowrap bg-slate-100 text-slate-600 hover:bg-slate-200">Tatlılar</button>
                        <button onclick="filterMenu('icecek')" class="menu-cat-btn px-3.5 py-1.5 rounded-full text-xs font-semibold whitespace-nowrap bg-slate-100 text-slate-600 hover:bg-slate-200">İçecekler</button>
                    </div>

                    <!-- Menu Items List -->
                    <div id="menu-items-container" class="flex-1 overflow-y-auto p-4 space-y-3">
                        <!-- Dynamic items will be rendered here -->
                    </div>
                </div>

                <!-- Wi-Fi Modal Inside Mobile -->
                <div id="wifi-modal" class="hidden absolute inset-0 bg-slate-900/60 backdrop-blur-sm z-50 flex items-center justify-center p-4">
                    <div class="bg-white rounded-2xl p-5 w-full max-w-xs text-center shadow-xl animate-fade-in">
                        <div class="w-12 h-12 bg-emerald-100 text-emerald-600 rounded-full flex items-center justify-center mx-auto mb-3 text-xl">
                            <i class="fa-solid fa-wifi"></i>
                        </div>
                        <h3 class="font-bold text-slate-800 text-lg">Müşteri Wi-Fi Ağı</h3>
                        <p class="text-xs text-slate-500 mt-1">Ağ Adı: <span class="font-semibold text-slate-700">Aura_Guest_Free</span></p>
                        <div class="bg-slate-100 p-3 rounded-xl my-3 flex items-center justify-between border border-slate-200">
                            <span id="wifi-pass-text" class="font-mono font-bold text-brand-700 text-sm">aura2026*</span>
                            <button onclick="copyWifiPassword()" class="text-xs bg-white px-2.5 py-1 rounded-lg border text-slate-600 hover:bg-slate-50">
                                <i class="fa-solid fa-copy"></i> Kopyala
                            </button>
                        </div>
                        <button onclick="hideWifiModal()" class="w-full bg-slate-800 text-white text-xs py-2.5 rounded-xl font-semibold">Kapat</button>
                    </div>
                </div>

            </div>

            <!-- Phone Bottom Indicator Bar -->
            <div class="absolute bottom-1 left-1/2 -translate-x-1/2 w-32 h-1 bg-slate-700 rounded-full z-40"></div>
        </div>

    </main>

    <!-- ADMIN & QR SETUP MODAL -->
    <div id="admin-modal" class="hidden fixed inset-0 bg-slate-900/70 backdrop-blur-md z-50 flex items-center justify-center p-4 overflow-y-auto">
        <div class="bg-white rounded-3xl max-w-2xl w-full shadow-2xl overflow-hidden my-auto border border-slate-100">
            <!-- Modal Header -->
            <div class="bg-brand-dark text-white p-5 flex justify-between items-center">
                <div class="flex items-center gap-2">
                    <i class="fa-solid fa-sliders text-brand-500 text-lg"></i>
                    <h3 class="font-bold text-lg">Restoran Ayarları & QR Kod Oluşturucu</h3>
                </div>
                <button onclick="toggleAdminModal()" class="text-slate-400 hover:text-white text-xl">
                    <i class="fa-solid fa-xmark"></i>
                </button>
            </div>

            <!-- Modal Content Tabs -->
            <div class="p-6 space-y-6 max-h-[80vh] overflow-y-auto">
                
                <!-- Section 1: General Info -->
                <div class="space-y-4">
                    <h4 class="font-bold text-sm text-brand-700 uppercase tracking-wider flex items-center gap-2 border-b pb-2">
                        <i class="fa-solid fa-store"></i> Restoran Bilgileri
                    </h4>
                    <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                        <div>
                            <label class="block text-xs font-semibold text-slate-700 mb-1">Yılmaz Pide</label>
                            <input type="text" id="input-name" class="w-full px-3 py-2 border border-slate-300 rounded-xl text-sm focus:ring-2 focus:ring-brand-500 outline-none" value="Aura Kitchen & Bar">
                        </div>
                        <div>
                            <label class="block text-xs font-semibold text-slate-700 mb-1">Slogan</label>
                            <input type="text" id="input-tagline" class="w-full px-3 py-2 border border-slate-300 rounded-xl text-sm focus:ring-2 focus:ring-brand-500 outline-none" value="Lezzetin Dijital & Modern Adresi">
                        </div>
                        <div>
                            <label class="block text-xs font-semibold text-slate-700 mb-1">Instagram Kullanıcı Adı</label>
                            <input type="text" id="input-instagram" class="w-full px-3 py-2 border border-slate-300 rounded-xl text-sm focus:ring-2 focus:ring-brand-500 outline-none" value="@aurakitchen">
                        </div>
                        <div>
                            <label class="block text-xs font-semibold text-slate-700 mb-1">Google Yorum Linki</label>
                            <input type="text" id="input-google" class="w-full px-3 py-2 border border-slate-300 rounded-xl text-sm focus:ring-2 focus:ring-brand-500 outline-none" value="https://maps.google.com">
                        </div>
                    </div>
                </div>

                <!-- Section 2: QR Generator & Printable Stand View -->
                <div class="space-y-4 pt-2">
                    <h4 class="font-bold text-sm text-brand-700 uppercase tracking-wider flex items-center gap-2 border-b pb-2">
                        <i class="fa-solid fa-print"></i> Masa Standı İçin QR Kod İndir / Yazdır
                    </h4>
                    <div class="bg-amber-50/60 p-4 rounded-2xl border border-amber-200/80 flex flex-col sm:flex-row items-center gap-6">
                        <!-- QR Code Container -->
                        <div class="bg-white p-3 rounded-2xl shadow-sm border border-slate-200 flex flex-col items-center">
                            <div id="qrcode" class="p-2"></div>
                            <span class="text-[10px] text-slate-500 font-mono mt-1">Önizleme QR</span>
                        </div>
                        
                        <div class="flex-1 text-center sm:text-left space-y-2">
                            <h5 class="font-bold text-slate-800 text-sm">Masaya Koyacağınız QR Kod</h5>
                            <p class="text-xs text-slate-600">
                                Bu QR kodu telefon kamerasından okutulduğunda doğrudan bu hazırladığınız menü ve yönlendirme ekranı açılır.
                            </p>
                            <div class="pt-2 flex flex-wrap gap-2 justify-center sm:justify-start">
                                <button onclick="printStand()" class="bg-brand-600 hover:bg-brand-700 text-white text-xs px-4 py-2 rounded-xl font-semibold transition-all flex items-center gap-1.5 shadow">
                                    <i class="fa-solid fa-print"></i> Stand Tasarımını Yazdır / PDF Al
                                </button>
                            </div>
                        </div>
                    </div>
                </div>

            </div>

            <!-- Modal Footer -->
            <div class="bg-slate-50 p-4 border-t border-slate-200 flex justify-end gap-2">
                <button onclick="saveAdminSettings()" class="bg-emerald-600 hover:bg-emerald-700 text-white text-sm px-5 py-2 rounded-xl font-semibold transition-all">
                    Değişiklikleri Kaydet
                </button>
            </div>
        </div>
    </div>

    <!-- Printable Hidden Stand Element (For Print Preview) -->
    <div id="printable-stand" class="hidden print:block print:fixed print:inset-0 print:bg-white print:p-8">
        <div class="max-w-md mx-auto border-4 border-amber-700 p-8 rounded-3xl text-center space-y-6">
            <h1 class="text-3xl font-serif font-bold text-slate-900" id="print-name">Yılmaz Pide</h1>
            <p class="text-sm text-slate-600" id="print-tagline">Lezzetin Dijital Adresi</p>
            <div class="my-6 flex justify-center">
                <div id="qrcode-print" class="p-4 border-2 border-slate-200 rounded-2xl inline-block"></div>
            </div>
            <div class="space-y-1">
                <p class="font-bold text-lg text-amber-800">MENÜ, YORUM & TAKİP İÇİN OKUTUN</p>
                <p class="text-xs text-slate-500">Kameranızı açıp QR Kodu taratın veya Telefonunuzu Yaklaştırın (NFC)</p>
            </div>
            <div class="flex justify-center gap-4 text-xs font-semibold text-slate-700 pt-4 border-t">
                <span>📜 Menü</span>
                <span>⭐ Google Yorum</span>
                <span>📸 Instagram</span>
            </div>
        </div>
    </div>

    <script>
        // Default Sample Menu Data
        const menuItems = [
            {
                id: 1,
                name: "Kuşbaşılı Kaşarlı Pide",
                category: "pide",
                price: "280 ₺",
                desc: "Özel marine edilmiş dana eti, közlenmiş biber ve erimiş özel kaşar peyniri ile.",
                img: "https://images.unsplash.com/photo-1628840042765-356cda07504e?auto=format&fit=crop&w=300&q=80"
            },
            {
                id: 2,
                name: "Taş Fırın Lahmacun",
                category: "pide",
                price: "110 ₺",
                desc: "Çıtır ince hamur, zırh kıyması, taze yeşillikler ve limon ile servis edilir.",
                img: "https://images.unsplash.com/photo-1565299624946-b28f40a0ae38?auto=format&fit=crop&w=300&q=80"
            },
            {
                id: 3,
                name: "Izgara Izgara Köfte",
                category: "ana-yemek",
                price: "340 ₺",
                desc: "100% dana kıymadan el yapımı köfteler, ızgara domates, biber ve pirinç pilavı ile.",
                img: "https://images.unsplash.com/photo-1529042410759-befb1204b468?auto=format&fit=crop&w=300&q=80"
            },
            {
                id: 4,
                name: "Fıstıklı Ev Baklavası",
                category: "tatli",
                price: "190 ₺",
                desc: "Antep fıstıklı, ince açılmış yufka ve dondurma ikramı ile.",
                img: "https://images.unsplash.com/photo-1519676867240-f03562e64548?auto=format&fit=crop&w=300&q=80"
            },
            {
                id: 5,
                name: "Taze Yayık Ayranı",
                category: "icecek",
                price: "45 ₺",
                desc: "Ev yapımı bol köpüklü geleneksel ayran.",
                img: "https://images.unsplash.com/photo-1551024709-8f23befc6f87?auto=format&fit=crop&w=300&q=80"
            }
        ];

        // Initialize UI Elements & QR
        let qrCodeInstance = null;

        window.onload = function() {
            renderMenu('all');
            generateQR();
        };

        // Render Menu Items
        function renderMenu(category) {
            const container = document.getElementById('menu-items-container');
            container.innerHTML = '';

            const filtered = category === 'all' 
                ? menuItems 
                : menuItems.filter(item => item.category === category);

            filtered.forEach(item => {
                const card = document.createElement('div');
                card.className = "bg-white p-3 rounded-2xl border border-slate-100 shadow-sm flex gap-3 items-center hover:shadow-md transition-shadow";
                card.innerHTML = `
                    <img src="${item.img}" alt="${item.name}" class="w-20 h-20 rounded-xl object-cover flex-shrink-0 bg-slate-100">
                    <div class="flex-1 min-w-0">
                        <div class="flex justify-between items-start">
                            <h4 class="font-bold text-slate-800 text-sm truncate">${item.name}</h4>
                            <span class="font-extrabold text-brand-700 text-sm ml-2">${item.price}</span>
                        </div>
                        <p class="text-[11px] text-slate-500 line-clamp-2 mt-1 leading-snug">${item.desc}</p>
                    </div>
                `;
                container.appendChild(card);
            });
        }

        // Filter Category Pills
        function filterMenu(category) {
            document.querySelectorAll('.menu-cat-btn').forEach(btn => {
                btn.classList.remove('bg-brand-600', 'text-white', 'shadow-sm');
                btn.classList.add('bg-slate-100', 'text-slate-600');
            });
            event.target.classList.remove('bg-slate-100', 'text-slate-600');
            event.target.classList.add('bg-brand-600', 'text-white', 'shadow-sm');

            renderMenu(category);
        }

        // Switch Mobile View Tabs
        function switchTab(tabName) {
            const hubView = document.getElementById('hub-view');
            const menuView = document.getElementById('menu-view');

            if (tabName === 'menu') {
                hubView.classList.add('hidden');
                menuView.classList.remove('hidden');
                menuView.classList.add('flex');
            } else {
                menuView.classList.add('hidden');
                menuView.classList.remove('flex');
                hubView.classList.remove('hidden');
            }
        }

        // Wi-Fi Modal Helpers
        function showWifiModal() {
            document.getElementById('wifi-modal').classList.remove('hidden');
        }
        function hideWifiModal() {
            document.getElementById('wifi-modal').classList.add('hidden');
        }
        function copyWifiPassword() {
            const pass = document.getElementById('wifi-pass-text').innerText;
            navigator.clipboard.writeText(pass);
            alert('Wi-Fi şifresi kopyalandı: ' + pass);
        }

        // Admin Modal Controls
        function toggleAdminModal() {
            const modal = document.getElementById('admin-modal');
            modal.classList.toggle('hidden');
        }

        function saveAdminSettings() {
            const name = document.getElementById('input-name').value;
            const tagline = document.getElementById('input-tagline').value;
            const insta = document.getElementById('input-instagram').value;
            const google = document.getElementById('input-google').value;

            document.getElementById('display-name').innerText = name;
            document.getElementById('display-tagline').innerText = tagline;
            document.getElementById('display-instagram-handle').innerText = insta;
            document.getElementById('display-google-link').href = google;
            
            // Print update
            document.getElementById('print-name').innerText = name;
            document.getElementById('print-tagline').innerText = tagline;

            generateQR();
            toggleAdminModal();
        }

        // QR Code Generator
        function generateQR() {
            const qrContainer = document.getElementById("qrcode");
            const qrPrintContainer = document.getElementById("qrcode-print");
            
            qrContainer.innerHTML = "";
            qrPrintContainer.innerHTML = "";

            const currentUrl = window.location.href;

            new QRCode(qrContainer, {
                text: currentUrl,
                width: 110,
                height: 110,
                colorDark: "#1e1b18",
                colorLight: "#ffffff",
                correctLevel: QRCode.CorrectLevel.H
            });

            new QRCode(qrPrintContainer, {
                text: currentUrl,
                width: 160,
                height: 160,
                colorDark: "#1e1b18",
                colorLight: "#ffffff",
                correctLevel: QRCode.CorrectLevel.H
            });
        }

        // Print Stand Function
        function printStand() {
            window.print();
        }
    </script>
</body>
</html>
