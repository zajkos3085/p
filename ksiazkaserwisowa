<!DOCTYPE html>
<html lang="pl" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AutoLog - Cyfrowa Książka Serwisowa</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Font Awesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">
    <!-- Biblioteka html2pdf.js do bezpośredniego generowania plików PDF -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>
    <!-- Google Fonts: Inter & JetBrains Mono -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&family=JetBrains+Mono:wght@400;600;700&display=swap" rel="stylesheet">
    
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                        mono: ['JetBrains Mono', 'monospace'],
                    },
                    colors: {
                        garage: {
                            950: '#090d16',
                            900: '#0f172a',
                            800: '#1e293b',
                            700: '#334155',
                            accent: '#3b82f6',
                            mblue: '#0066b1',
                            mred: '#e22718'
                        }
                    }
                }
            }
        }
    </script>

    <style>
        /* Stylizacja pasków przewijania i detali wizualnych */
        ::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #0f172a;
        }
        ::-webkit-scrollbar-thumb {
            background: #334155;
            border-radius: 3px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #475569;
        }

        .m-stripe {
            background: linear-gradient(90deg, #0066b1 0%, #0066b1 33%, #171c54 33%, #171c54 66%, #e22718 66%, #e22718 100%);
        }

        /* Reguły dla wydruku pojedynczego samochodu */
        @media print {
            body {
                background: #ffffff !important;
                color: #000000 !important;
                font-size: 12pt;
            }
            .no-print {
                display: none !important;
            }
            .print-only {
                display: block !important;
            }
            .print-container {
                width: 100% !important;
                max-width: 100% !important;
                margin: 0 !important;
                padding: 0 !important;
                box-shadow: none !important;
                background: transparent !important;
                border: none !important;
            }
            .print-card {
                background: #ffffff !important;
                border: 1px solid #cbd5e1 !important;
                color: #000000 !important;
                box-shadow: none !important;
                break-inside: avoid;
                margin-bottom: 12px !important;
            }
            .print-text-dark {
                color: #0f172a !important;
            }
            .print-text-muted {
                color: #475569 !important;
            }
            .print-badge {
                border: 1px solid #94a3b8 !important;
                background: #f8fafc !important;
                color: #0f172a !important;
            }
        }
        .print-only {
            display: none;
        }
    </style>
</head>
<body class="bg-garage-950 text-slate-100 font-sans min-h-screen flex flex-col selection:bg-blue-600 selection:text-white">

    <!-- Dekoracyjny pasek na samej górze -->
    <div class="h-1.5 w-full m-stripe no-print"></div>

    <!-- Pasek nawigacyjny -->
    <header class="bg-garage-900/90 backdrop-blur-md border-b border-slate-800 sticky top-0 z-30 no-print">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex items-center justify-between">
            <button type="button" onclick="closeCarDetails()" class="flex items-center space-x-3 text-left group focus:outline-none cursor-pointer" title="Wróć do pulpitu głównego">
                <div class="w-10 h-10 rounded-xl bg-gradient-to-br from-blue-600 to-indigo-700 group-hover:from-blue-500 group-hover:to-indigo-600 flex items-center justify-center shadow-lg shadow-blue-900/40 transition transform group-hover:scale-105">
                    <i class="fa-solid fa-car-wrench text-white text-lg"></i>
                </div>
                <div>
                    <h1 class="font-bold text-lg tracking-tight leading-none text-white group-hover:text-blue-400 transition flex items-center gap-2">
                        AutoLog <span class="text-xs font-mono uppercase bg-blue-500/20 text-blue-400 px-2 py-0.5 rounded-full border border-blue-500/30">Cloud</span>
                    </h1>
                    <p class="text-xs text-slate-400 group-hover:text-slate-300 transition">Cyfrowa Książka Serwisowa</p>
                </div>
            </button>

            <div class="flex items-center space-x-2 sm:space-x-3">
                <!-- Przycisk statusu i ustawień połączenia z chmurą (Zsynchronizowano) -->
                <button onclick="openConnectionModal()" id="sync-status" class="flex items-center space-x-2 text-xs bg-slate-800/90 hover:bg-slate-700 border border-slate-700 hover:border-blue-500/50 px-3 py-2 rounded-xl text-slate-200 transition shadow-sm cursor-pointer" title="Kliknij, aby zarządzać połączeniem i synchronizacją Firebase">
                    <span class="w-2 h-2 rounded-full bg-amber-400 animate-pulse" id="sync-dot"></span>
                    <span id="sync-text" class="font-medium">Łączenie...</span>
                    <i class="fa-solid fa-sliders text-[10px] text-slate-400 ml-1"></i>
                </button>

                <!-- Przycisk dodawania nowego auta -->
                <button onclick="openCarModal()" class="bg-blue-600 hover:bg-blue-500 active:bg-blue-700 text-white text-sm font-medium px-3.5 py-2 rounded-xl shadow-lg shadow-blue-600/25 transition flex items-center space-x-2">
                    <i class="fa-solid fa-plus"></i>
                    <span class="hidden sm:inline">Dodaj Pojazd</span>
                </button>
            </div>
        </div>
    </header>

    <!-- Główny kontener aplikacji -->
    <main class="flex-1 max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-6 grid grid-cols-1 lg:grid-cols-12 gap-6 print-container">
        
        <!-- LEWA KOLUMNA: Lista pojazdów (Garaż) -->
        <aside class="lg:col-span-4 xl:col-span-3 space-y-4 no-print">
            <div class="bg-garage-900 border border-slate-800 rounded-2xl p-4 shadow-xl">
                <div class="flex items-center justify-between mb-3">
                    <h2 class="text-xs font-bold uppercase tracking-wider text-slate-400 flex items-center gap-2">
                        <i class="fa-solid fa-warehouse text-blue-400"></i> Twój Garaż
                    </h2>
                    <span id="cars-count" class="text-xs font-mono bg-slate-800 text-slate-300 px-2 py-0.5 rounded-md">0</span>
                </div>

                <!-- Wyszukiwarka samochodów w garażu -->
                <div class="relative mb-3">
                    <i class="fa-solid fa-magnifying-glass absolute left-3 top-1/2 -translate-y-1/2 text-slate-500 text-xs"></i>
                    <input type="text" id="search-car-input" oninput="renderCarsSidebar()" placeholder="Szukaj (auto, właściciel, VIN)..." class="w-full bg-slate-950 border border-slate-800 focus:border-blue-500 rounded-xl pl-8 pr-3 py-2 text-xs text-slate-200 placeholder-slate-500 focus:outline-none transition">
                </div>

                <!-- Lista kart pojazdów (mobilnie przewijana poziomo lub pionowo) -->
                <div id="cars-list" class="space-y-2.5">
                    <!-- Stan ładowania -->
                    <div class="animate-pulse space-y-2">
                        <div class="h-16 bg-slate-800/70 rounded-xl"></div>
                        <div class="h-16 bg-slate-800/40 rounded-xl"></div>
                    </div>
                </div>

                <!-- Szybki import danych ze zdjęcia (BMW F10) -->
                <div id="f10-import-banner" class="mt-4 pt-4 border-t border-slate-800/80 hidden">
                    <button onclick="importF10FromScreenshot()" class="w-full py-2.5 px-3 rounded-xl bg-gradient-to-r from-slate-800 to-slate-800/60 hover:from-blue-950/60 hover:to-slate-800 border border-slate-700 hover:border-blue-500/40 transition flex items-center justify-between group text-left">
                        <div class="flex items-center space-x-2.5">
                            <div class="w-8 h-8 rounded-lg bg-blue-500/10 border border-blue-500/30 flex items-center justify-center text-blue-400 group-hover:scale-105 transition">
                                <i class="fa-solid fa-file-import text-sm"></i>
                            </div>
                            <div>
                                <div class="text-xs font-semibold text-slate-200">Wczytaj dane F10 ze zdjęcia</div>
                                <div class="text-[11px] text-slate-400 font-mono">WBA5C71050D054064</div>
                            </div>
                        </div>
                        <i class="fa-solid fa-chevron-right text-xs text-slate-500 group-hover:text-blue-400 transition"></i>
                    </button>
                </div>

                <!-- Szybki Eksport / Import kopii zapasowej JSON -->
                <div class="mt-4 pt-3 border-t border-slate-800/80 flex items-center gap-2">
                    <button onclick="exportDatabaseJson()" class="flex-1 py-2 px-2.5 rounded-xl bg-slate-950 hover:bg-slate-800 border border-slate-800 hover:border-blue-500/40 text-[11px] font-medium text-slate-300 transition flex items-center justify-center gap-1.5" title="Pobierz kopię zapasową wszystkich aut do pliku JSON">
                        <i class="fa-solid fa-download text-blue-400"></i> Eksportuj bazę
                    </button>
                    <label class="flex-1 py-2 px-2.5 rounded-xl bg-slate-950 hover:bg-slate-800 border border-slate-800 hover:border-emerald-500/40 text-[11px] font-medium text-slate-300 transition flex items-center justify-center gap-1.5 cursor-pointer" title="Wczytaj bazę pojazdów z pliku JSON">
                        <i class="fa-solid fa-upload text-emerald-400"></i> Importuj
                        <input type="file" id="import-json-input" accept=".json,application/json" onchange="importDatabaseJson(event)" class="hidden">
                    </label>
                </div>
            </div>
        </aside>

        <!-- PRAWA KOLUMNA: Szczegóły wybranego auta, części oraz historia napraw -->
        <section class="lg:col-span-8 xl:col-span-9 print-container">
            
            <!-- Widok, gdy nie wybrano żadnego auta -->
            <div id="empty-garage-state" class="space-y-6 no-print">
                
                <!-- PANEL PRZYPOMNIEŃ O PRZEGLĄDACH (2 tygodnie przed upływem roku od ostatniego przeglądu) -->
                <div id="reminders-panel" class="bg-garage-900 border border-slate-800 rounded-2xl p-5 sm:p-6 shadow-xl">
                    <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-3 pb-4 border-b border-slate-800">
                        <div class="flex items-center space-x-3">
                            <div class="w-10 h-10 rounded-xl bg-amber-500/10 border border-amber-500/30 text-amber-400 flex items-center justify-center shrink-0">
                                <i class="fa-solid fa-bell text-lg"></i>
                            </div>
                            <div>
                                <div class="flex items-center gap-2">
                                    <h3 class="text-base font-bold text-white">Przypomnienia o przeglądach i serwisach</h3>
                                    <span id="reminders-badge-count" class="text-xs font-mono font-bold px-2 py-0.5 rounded-full bg-amber-500/20 text-amber-300 border border-amber-500/30">0</span>
                                </div>
                                <p class="text-xs text-slate-400">Automatyczny alert na 14 dni przed upływem roku (wyliczany wyłącznie z najnowszego przeglądu / serwisu danego auta)</p>
                            </div>
                        </div>
                        <div class="flex items-center gap-2 self-start sm:self-auto">
                            <button onclick="toggleShowAllReminders()" id="toggle-all-reminders-btn" class="text-xs px-3 py-1.5 rounded-xl bg-slate-800 hover:bg-slate-700 text-slate-300 border border-slate-700 transition flex items-center gap-1.5">
                                <i class="fa-regular fa-calendar-check text-blue-400"></i>
                                <span id="toggle-all-reminders-text">Pokaż wszystkie terminy</span>
                            </button>
                        </div>
                    </div>

                    <div id="reminders-list" class="mt-4 space-y-3">
                        <!-- Generowane dynamicznie w JS -->
                    </div>
                </div>

                <!-- Karta powitalna zachęcająca do wyboru pojazdu -->
                <div class="bg-garage-900 border border-slate-800 rounded-2xl p-8 sm:p-10 text-center shadow-xl">
                    <div class="w-14 h-14 rounded-2xl bg-blue-500/10 border border-blue-500/20 text-blue-400 flex items-center justify-center mx-auto mb-4 text-xl">
                        <i class="fa-solid fa-hand-pointer"></i>
                    </div>
                    <h2 id="empty-state-title" class="text-xl font-bold text-white mb-2">Wybierz pojazd z garażu</h2>
                    <p id="empty-state-desc" class="text-slate-400 text-sm max-w-md mx-auto mb-6">
                        Kliknij na konkretny samochód na liście po lewej stronie (lub powyżej na telefonie), aby otworzyć jego kartę serwisową, sprawdzić numery filtrów oraz historię napraw.
                    </p>
                    <div class="flex flex-col sm:flex-row items-center justify-center gap-3">
                        <button id="empty-state-f10-btn" onclick="importF10FromScreenshot()" class="w-full sm:w-auto px-5 py-2.5 rounded-xl bg-blue-600 hover:bg-blue-500 text-white font-medium text-sm shadow-lg shadow-blue-600/30 transition flex items-center justify-center gap-2">
                            <i class="fa-solid fa-bolt"></i> Załaduj moje F10 ze zdjęcia
                        </button>
                        <button onclick="openCarModal()" class="w-full sm:w-auto px-5 py-2.5 rounded-xl bg-slate-800 hover:bg-slate-700 text-slate-200 font-medium text-sm border border-slate-700 transition flex items-center justify-center gap-2">
                            <i class="fa-solid fa-plus"></i> Dodaj nowy pojazd
                        </button>
                    </div>
                </div>
            </div>

            <!-- Główny widok wybranego pojazdu -->
            <div id="car-details-view" class="space-y-6 hidden">
                
                <!-- NAGŁÓWEK DO DRUKU (Widoczny wyłącznie na wydruku / PDF) -->
                <div class="print-only border-b-2 border-slate-800 pb-4 mb-6">
                    <div class="flex justify-between items-start">
                        <div>
                            <div class="text-xs uppercase tracking-widest text-slate-500 font-bold">Karta Historii Pojazdu / Książka Serwisowa</div>
                            <h1 id="print-car-name" class="text-3xl font-extrabold text-slate-900 mt-1">BMW F10</h1>
                            <div class="mt-2 font-mono text-sm text-slate-700">
                                <span>VIN: <strong id="print-car-vin">WBA5C71050D054064</strong></span>
                                <span class="mx-3">|</span>
                                <span>Właściciel: <strong id="print-car-owner">-</strong></span>
                                <span class="mx-3">|</span>
                                <span>Aktualny przebieg: <strong id="print-car-mileage">340 000 km</strong></span>
                            </div>
                        </div>
                        <div class="text-right text-xs text-slate-500">
                            <div>Wygenerowano z systemu AutoLog</div>
                            <div id="print-date" class="font-mono mt-1"></div>
                        </div>
                    </div>
                </div>

                <!-- KARTA GŁÓWNA POJAZDU -->
                <div class="bg-gradient-to-br from-garage-900 via-garage-900 to-slate-900 border border-slate-800 rounded-2xl p-5 sm:p-6 shadow-xl relative overflow-hidden no-print">
                    <div class="absolute top-0 right-0 w-64 h-64 bg-blue-600/5 rounded-full blur-3xl pointer-events-none"></div>
                    
                    <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-4">
                        <div class="space-y-1.5">
                            <div class="flex items-center flex-wrap gap-2.5">
                                <button onclick="closeCarDetails()" class="px-2.5 py-0.5 text-xs font-semibold bg-slate-800 hover:bg-slate-700 text-slate-300 border border-slate-700 rounded-md transition flex items-center gap-1.5" title="Zamknij kartę pojazdu">
                                    <i class="fa-solid fa-arrow-left text-[10px]"></i> Powrót
                                </button>
                                <span id="detail-car-badge" class="px-2.5 py-0.5 text-xs font-bold uppercase tracking-wider bg-blue-500/20 text-blue-400 border border-blue-500/30 rounded-md">
                                    Karta Pojazdu
                                </span>
                                <span id="detail-car-owner" class="hidden text-xs font-semibold bg-indigo-500/15 border border-indigo-500/30 text-indigo-300 px-2.5 py-0.5 rounded-md flex items-center gap-1.5">
                                    <i class="fa-solid fa-user text-[10px]"></i> <span id="detail-car-owner-text"></span>
                                </span>
                                <span id="detail-car-plate" class="hidden font-mono text-xs bg-slate-800 border border-slate-700 text-slate-200 px-2.5 py-0.5 rounded-md"></span>
                            </div>
                            <h2 id="detail-car-name" class="text-2xl sm:text-3xl font-extrabold text-white tracking-tight">BMW F10</h2>
                            
                            <!-- Numer VIN z przyciskiem kopiowania -->
                            <div class="flex items-center space-x-2 pt-1">
                                <span class="text-xs text-slate-400">VIN:</span>
                                <code id="detail-car-vin" class="font-mono text-xs sm:text-sm bg-slate-950/90 border border-slate-800 px-2.5 py-1 rounded-lg text-amber-400 tracking-wider select-all">WBA5C71050D054064</code>
                                <button onclick="copyVin()" class="p-1.5 text-slate-400 hover:text-white bg-slate-800/80 hover:bg-slate-700 rounded-lg transition text-xs" title="Skopiuj VIN">
                                    <i class="fa-regular fa-copy"></i>
                                </button>
                            </div>
                        </div>

                        <!-- Statystyki i przyciski akcji pojazdu -->
                        <div class="flex flex-wrap sm:flex-col items-start sm:items-end justify-between gap-3 pt-3 sm:pt-0 border-t sm:border-t-0 border-slate-800/80">
                            <div class="bg-slate-950/70 border border-slate-800/90 px-4 py-2 rounded-xl text-left sm:text-right">
                                <div class="text-[11px] uppercase tracking-wider text-slate-400">Ostatni odnotowany przebieg</div>
                                <div id="detail-car-mileage" class="text-lg sm:text-xl font-mono font-bold text-emerald-400">340 000 km</div>
                            </div>

                            <div class="flex items-center flex-wrap gap-2">
                                <button onclick="downloadVehiclePdf()" id="btn-download-pdf" class="px-3.5 py-2 rounded-xl bg-blue-600 hover:bg-blue-500 text-white text-xs font-semibold transition flex items-center gap-2 shadow-lg shadow-blue-600/25">
                                    <i class="fa-solid fa-file-pdf"></i>
                                    <span>Pobierz PDF</span>
                                </button>
                                <button onclick="printVehicleHistory()" class="px-3.5 py-2 rounded-xl bg-slate-800 hover:bg-slate-700 text-slate-200 border border-slate-700 text-xs font-semibold transition flex items-center gap-2 shadow-sm">
                                    <i class="fa-solid fa-print text-blue-400"></i>
                                    <span>Drukuj</span>
                                </button>
                                <button onclick="openCarModal(true)" class="p-2 rounded-xl bg-slate-800 hover:bg-slate-700 text-slate-300 border border-slate-700 text-xs transition" title="Edytuj dane auta i numery porównawcze filtrów">
                                    <i class="fa-solid fa-pen-to-square"></i>
                                </button>
                                <button onclick="confirmDeleteCar()" class="p-2 rounded-xl bg-slate-800 hover:bg-red-950/60 text-slate-400 hover:text-red-400 border border-slate-700 hover:border-red-800/50 text-xs transition" title="Usuń pojazd">
                                    <i class="fa-solid fa-trash-can"></i>
                                </button>
                            </div>
                        </div>
                    </div>

                    <!-- Dodatkowa notatka ogólna do pojazdu -->
                    <p id="detail-car-notes" class="mt-3 text-xs text-slate-400 hidden border-t border-slate-800/70 pt-3"></p>

                    <!-- PASEK INTERWAŁU WYMIANY OLEJU (LICZNIK KILOMETRÓW) -->
                    <div id="oil-interval-card" class="mt-4 pt-4 border-t border-slate-800/80">
                        <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-2 mb-2">
                            <div class="flex items-center gap-2">
                                <span class="w-7 h-7 rounded-lg bg-amber-500/10 border border-amber-500/30 text-amber-400 flex items-center justify-center text-xs">
                                    <i class="fa-solid fa-oil-can"></i>
                                </span>
                                <div>
                                    <div class="text-xs font-bold uppercase tracking-wider text-slate-200 flex items-center gap-2">
                                        <span>Interwał wymiany oleju</span>
                                        <span id="oil-interval-badge" class="px-2 py-0.5 text-[10px] font-mono rounded-md bg-slate-800 text-slate-300 border border-slate-700">co 15 000 km</span>
                                    </div>
                                    <p id="oil-interval-subtext" class="text-[11px] text-slate-400">Ostatnia wymiana przy: - • Następna wymiana przy: -</p>
                                </div>
                            </div>
                            <div class="flex items-center gap-2">
                                <div id="oil-interval-remaining" class="text-xs font-mono font-bold px-3 py-1 rounded-xl bg-emerald-500/15 text-emerald-300 border border-emerald-500/30">
                                    Pozostało: 15 000 km
                                </div>
                                <button type="button" onclick="promptUpdateMileage()" class="px-2.5 py-1 rounded-xl bg-slate-800 hover:bg-slate-700 text-slate-200 border border-slate-700 text-[11px] font-medium transition flex items-center gap-1" title="Szybko zaktualizuj aktualny przebieg auta">
                                    <i class="fa-solid fa-gauge-high text-blue-400"></i> Zmień przebieg
                                </button>
                            </div>
                        </div>
                        <div class="w-full h-2.5 bg-slate-950 rounded-full overflow-hidden border border-slate-800 p-0.5">
                            <div id="oil-interval-bar" class="h-full rounded-full bg-gradient-to-r from-emerald-500 to-teal-400 transition-all duration-500" style="width: 0%"></div>
                        </div>
                    </div>
                </div>

                <!-- SEKCJA NUMERÓW PORÓWNAWCZYCH FILTRÓW (Do doboru zamienników) -->
                <div class="bg-garage-900 border border-slate-800 rounded-2xl p-5 shadow-xl print-card">
                    <div class="flex items-center justify-between mb-4">
                        <div class="flex items-center space-x-2.5">
                            <div class="w-8 h-8 rounded-lg bg-amber-500/10 border border-amber-500/20 text-amber-400 flex items-center justify-center no-print">
                                <i class="fa-solid fa-gears text-sm"></i>
                            </div>
                            <div>
                                <h3 class="text-sm font-bold text-white print-text-dark uppercase tracking-wider">Numery Porównawcze Filtrów (OEM / Katalog M-MOT)</h3>
                                <p class="text-xs text-slate-400 print-text-muted no-print">Kliknij na kafelek z numerem filtra, aby od razu otworzyć tę część w sklepie <strong class="text-blue-400">WebTerminal M-MOT</strong></p>
                            </div>
                        </div>
                        <button onclick="openCarModal(true)" class="text-xs text-blue-400 hover:text-blue-300 font-medium flex items-center gap-1.5 no-print">
                            <i class="fa-solid fa-pen text-[10px]"></i> Edytuj numery
                        </button>
                    </div>

                    <div class="grid grid-cols-2 sm:grid-cols-4 gap-3">
                        <!-- Filtr oleju -->
                        <div onclick="searchPartInShop(event, 'part-oil')" class="cursor-pointer group bg-slate-950/60 hover:bg-blue-950/30 border border-slate-800 hover:border-blue-500/60 rounded-xl p-3.5 transition print-badge" title="Kliknij, aby otworzyć w sklepie M-MOT">
                            <div class="flex items-center justify-between text-xs text-slate-400 print-text-muted mb-1">
                                <span><i class="fa-solid fa-oil-can text-amber-400 mr-1.5 no-print"></i>Filtr oleju</span>
                                <div class="flex items-center gap-1.5 no-print">
                                    <button type="button" onclick="event.stopPropagation(); copyPartNumber('part-oil', 'Filtr oleju (nr porównawczy)')" class="p-1 rounded hover:bg-slate-800 text-slate-500 hover:text-slate-200 transition" title="Skopiuj sam numer do schowka">
                                        <i class="fa-regular fa-copy text-[11px]"></i>
                                    </button>
                                    <i class="fa-solid fa-arrow-up-right-from-square text-[11px] opacity-75 group-hover:opacity-100 transition text-blue-400" title="Otwórz w M-MOT"></i>
                                </div>
                            </div>
                            <div id="part-oil" class="font-mono font-bold text-sm sm:text-base text-slate-100 group-hover:text-blue-300 print-text-dark tracking-wide transition">8507683</div>
                            <div class="text-[10px] text-blue-400/80 print-text-muted mt-0.5 flex items-center gap-1">
                                <i class="fa-solid fa-cart-shopping text-[9px] no-print"></i> Szukaj w M-MOT
                            </div>
                        </div>

                        <!-- Filtr powietrza -->
                        <div onclick="searchPartInShop(event, 'part-air')" class="cursor-pointer group bg-slate-950/60 hover:bg-blue-950/30 border border-slate-800 hover:border-blue-500/60 rounded-xl p-3.5 transition print-badge" title="Kliknij, aby otworzyć w sklepie M-MOT">
                            <div class="flex items-center justify-between text-xs text-slate-400 print-text-muted mb-1">
                                <span><i class="fa-solid fa-wind text-sky-400 mr-1.5 no-print"></i>Filtr powietrza</span>
                                <div class="flex items-center gap-1.5 no-print">
                                    <button type="button" onclick="event.stopPropagation(); copyPartNumber('part-air', 'Filtr powietrza (nr porównawczy)')" class="p-1 rounded hover:bg-slate-800 text-slate-500 hover:text-slate-200 transition" title="Skopiuj sam numer do schowka">
                                        <i class="fa-regular fa-copy text-[11px]"></i>
                                    </button>
                                    <i class="fa-solid fa-arrow-up-right-from-square text-[11px] opacity-75 group-hover:opacity-100 transition text-blue-400" title="Otwórz w M-MOT"></i>
                                </div>
                            </div>
                            <div id="part-air" class="font-mono font-bold text-sm sm:text-base text-slate-100 group-hover:text-blue-300 print-text-dark tracking-wide transition">7800151</div>
                            <div class="text-[10px] text-blue-400/80 print-text-muted mt-0.5 flex items-center gap-1">
                                <i class="fa-solid fa-cart-shopping text-[9px] no-print"></i> Szukaj w M-MOT
                            </div>
                        </div>

                        <!-- Filtr kabinowy -->
                        <div onclick="searchPartInShop(event, 'part-cabin')" class="cursor-pointer group bg-slate-950/60 hover:bg-blue-950/30 border border-slate-800 hover:border-blue-500/60 rounded-xl p-3.5 transition print-badge" title="Kliknij, aby otworzyć w sklepie M-MOT">
                            <div class="flex items-center justify-between text-xs text-slate-400 print-text-muted mb-1">
                                <span><i class="fa-solid fa-fan text-emerald-400 mr-1.5 no-print"></i>Filtr kabinowy</span>
                                <div class="flex items-center gap-1.5 no-print">
                                    <button type="button" onclick="event.stopPropagation(); copyPartNumber('part-cabin', 'Filtr kabinowy (nr porównawczy)')" class="p-1 rounded hover:bg-slate-800 text-slate-500 hover:text-slate-200 transition" title="Skopiuj sam numer do schowka">
                                        <i class="fa-regular fa-copy text-[11px]"></i>
                                    </button>
                                    <i class="fa-solid fa-arrow-up-right-from-square text-[11px] opacity-75 group-hover:opacity-100 transition text-blue-400" title="Otwórz w M-MOT"></i>
                                </div>
                            </div>
                            <div id="part-cabin" class="font-mono font-bold text-sm sm:text-base text-slate-100 group-hover:text-blue-300 print-text-dark tracking-wide transition">9272642</div>
                            <div class="text-[10px] text-blue-400/80 print-text-muted mt-0.5 flex items-center gap-1">
                                <i class="fa-solid fa-cart-shopping text-[9px] no-print"></i> Szukaj w M-MOT
                            </div>
                        </div>

                        <!-- Filtr paliwa -->
                        <div onclick="searchPartInShop(event, 'part-fuel')" class="cursor-pointer group bg-slate-950/60 hover:bg-blue-950/30 border border-slate-800 hover:border-blue-500/60 rounded-xl p-3.5 transition print-badge" title="Kliknij, aby otworzyć w sklepie M-MOT">
                            <div class="flex items-center justify-between text-xs text-slate-400 print-text-muted mb-1">
                                <span><i class="fa-solid fa-gas-pump text-rose-400 mr-1.5 no-print"></i>Filtr paliwa</span>
                                <div class="flex items-center gap-1.5 no-print">
                                    <button type="button" onclick="event.stopPropagation(); copyPartNumber('part-fuel', 'Filtr paliwa (nr porównawczy)')" class="p-1 rounded hover:bg-slate-800 text-slate-500 hover:text-slate-200 transition" title="Skopiuj sam numer do schowka">
                                        <i class="fa-regular fa-copy text-[11px]"></i>
                                    </button>
                                    <i class="fa-solid fa-arrow-up-right-from-square text-[11px] opacity-75 group-hover:opacity-100 transition text-blue-400" title="Otwórz w M-MOT"></i>
                                </div>
                            </div>
                            <div id="part-fuel" class="font-mono font-bold text-sm sm:text-base text-slate-100 group-hover:text-blue-300 print-text-dark tracking-wide transition">7822499</div>
                            <div class="text-[10px] text-blue-400/80 print-text-muted mt-0.5 flex items-center gap-1">
                                <i class="fa-solid fa-cart-shopping text-[9px] no-print"></i> Szukaj w M-MOT
                            </div>
                        </div>
                    </div>

                    <!-- Dodatkowe części (np. olej, klocki, rozrząd) jeśli wpisane -->
                    <div id="part-extra-container" class="mt-3 pt-3 border-t border-slate-800/80 hidden">
                        <div class="text-xs text-slate-400 print-text-muted">Inne części / specyfikacja: <span id="part-extra" class="font-mono text-slate-200 print-text-dark"></span></div>
                    </div>
                </div>

                <!-- SEKCJA 5: LISTA "DO ZROBIENIA PRZY NASTĘPNYM SERWISIE" (SCHOWEK USTEREK I ZAKUPÓW) -->
                <div class="bg-garage-900 border border-slate-800 rounded-2xl p-5 shadow-xl no-print">
                    <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-3 mb-4">
                        <div class="flex items-center space-x-2.5">
                            <div class="w-8 h-8 rounded-lg bg-purple-500/10 border border-purple-500/30 text-purple-400 flex items-center justify-center shrink-0">
                                <i class="fa-solid fa-list-check text-sm"></i>
                            </div>
                            <div>
                                <div class="flex items-center gap-2">
                                    <h3 class="text-sm font-bold text-white uppercase tracking-wider">Do zrobienia przy następnym serwisie (Schowek usterek)</h3>
                                    <span id="todo-count-badge" class="text-[11px] font-mono font-bold px-2 py-0.5 rounded-full bg-purple-500/20 text-purple-300 border border-purple-500/30">0</span>
                                </div>
                                <p class="text-xs text-slate-400">Zapisuj zauważone usterki lub części do kupienia przed kolejną wizytą w warsztacie</p>
                            </div>
                        </div>

                        <!-- Wyszukiwarka na liście Do Zrobienia -->
                        <div class="relative w-full sm:w-52">
                            <i class="fa-solid fa-magnifying-glass absolute left-3 top-1/2 -translate-y-1/2 text-slate-500 text-xs"></i>
                            <input type="text" id="search-todo-input" oninput="renderTodoList()" placeholder="Szukaj na liście usterek..." class="w-full bg-slate-950 border border-slate-800 focus:border-purple-500 rounded-xl pl-8 pr-3 py-1.5 text-xs text-slate-200 placeholder-slate-500 focus:outline-none transition">
                        </div>
                    </div>

                    <!-- Formularz szybkiego dodawania usterki / zadania -->
                    <form onsubmit="addTodoItem(event)" class="flex flex-col sm:flex-row gap-2 mb-4">
                        <input type="text" id="input-todo-text" required placeholder="np. Słychać łącznik stabilizatora prawy przód, kupić pióra wycieraczek..." class="flex-1 bg-slate-950 border border-slate-700 focus:border-purple-500 rounded-xl px-3.5 py-2 text-xs sm:text-sm text-white placeholder-slate-500 focus:outline-none transition">
                        <select id="input-todo-priority" class="bg-slate-950 border border-slate-700 text-slate-200 text-xs rounded-xl px-3 py-2 focus:outline-none focus:border-purple-500">
                            <option value="NORMAL">Standardowe</option>
                            <option value="URGENT">Pilne!</option>
                            <option value="BUY">Do kupienia</option>
                        </select>
                        <button type="submit" class="bg-purple-600 hover:bg-purple-500 text-white text-xs font-semibold px-4 py-2 rounded-xl shadow-lg shadow-purple-600/20 transition flex items-center justify-center gap-1.5 shrink-0">
                            <i class="fa-solid fa-plus"></i> Dodaj do listy
                        </button>
                    </form>

                    <!-- Lista zadań / usterek -->
                    <div id="todo-list-container" class="space-y-2">
                        <!-- Generowane dynamicznie w JS -->
                    </div>
                </div>

                <!-- SEKCJA HISTORII SERWISOWEJ I NAPRAW -->
                <div class="bg-garage-900 border border-slate-800 rounded-2xl p-5 sm:p-6 shadow-xl print-card">
                    <div class="flex flex-col lg:flex-row lg:items-center justify-between gap-4 mb-5">
                        <div>
                            <h3 class="text-base sm:text-lg font-bold text-white print-text-dark flex items-center gap-2">
                                <i class="fa-solid fa-clipboard-list text-blue-400 no-print"></i>
                                Historia Serwisowa i Naprawy
                            </h3>
                            <p class="text-xs text-slate-400 print-text-muted">Pełny dziennik wymian oleju, przeglądów oraz napraw mechanicznych</p>
                        </div>

                        <div class="flex flex-col sm:flex-row items-stretch sm:items-center gap-2 no-print">
                            <!-- Wyszukiwarka w historii przeglądów i napraw -->
                            <div class="relative flex-1 sm:w-56">
                                <i class="fa-solid fa-magnifying-glass absolute left-3 top-1/2 -translate-y-1/2 text-slate-500 text-xs"></i>
                                <input type="text" id="search-record-input" oninput="renderRecords()" placeholder="Szukaj w historii (np. filtr, olej)..." class="w-full bg-slate-950 border border-slate-700 focus:border-blue-500 rounded-xl pl-8 pr-7 py-2 text-xs text-slate-200 placeholder-slate-500 focus:outline-none transition">
                                <button id="clear-record-search" onclick="clearRecordSearch()" class="hidden absolute right-2.5 top-1/2 -translate-y-1/2 text-slate-500 hover:text-slate-300 text-xs">
                                    <i class="fa-solid fa-xmark"></i>
                                </button>
                            </div>

                            <!-- Filtr kategorii wpisów -->
                            <select id="filter-record-type" onchange="renderRecords()" class="bg-slate-950 border border-slate-700 text-slate-200 text-xs rounded-xl px-3 py-2 focus:outline-none focus:border-blue-500">
                                <option value="ALL">Wszystkie wpisy</option>
                                <option value="OIL">Wymiany oleju</option>
                                <option value="REPAIR">Naprawy</option>
                                <option value="INSPECTION">Przeglądy / Inne</option>
                            </select>

                            <!-- Przycisk dodania nowego wpisu serwisowego -->
                            <button onclick="openRecordModal()" class="bg-emerald-600 hover:bg-emerald-500 active:bg-emerald-700 text-white text-xs sm:text-sm font-semibold px-4 py-2 rounded-xl shadow-lg shadow-emerald-600/20 transition flex items-center justify-center gap-2 shrink-0">
                                <i class="fa-solid fa-screwdriver-wrench"></i>
                                <span>Dodaj wpis / naprawę</span>
                            </button>
                        </div>
                    </div>

                    <!-- Podsumowanie kosztów i liczby wpisów -->
                    <div class="grid grid-cols-2 sm:grid-cols-3 gap-3 mb-6">
                        <div class="bg-slate-950/60 border border-slate-800/80 rounded-xl p-3 print-badge">
                            <div class="text-[11px] text-slate-400 print-text-muted uppercase">Liczba wpisów</div>
                            <div id="stat-records-count" class="text-base sm:text-lg font-bold font-mono text-white print-text-dark">0</div>
                        </div>
                        <div class="bg-slate-950/60 border border-slate-800/80 rounded-xl p-3 print-badge">
                            <div class="text-[11px] text-slate-400 print-text-muted uppercase">Ostatni serwis olejowy</div>
                            <div id="stat-last-oil" class="text-sm sm:text-base font-bold font-mono text-amber-400 print-text-dark">Brak danych</div>
                        </div>
                        <div class="col-span-2 sm:col-span-1 bg-slate-950/60 border border-slate-800/80 rounded-xl p-3 print-badge">
                            <div class="text-[11px] text-slate-400 print-text-muted uppercase">Suma wpisanych kosztów</div>
                            <div id="stat-total-cost" class="text-base sm:text-lg font-bold font-mono text-emerald-400 print-text-dark">0 PLN</div>
                        </div>
                    </div>

                    <!-- Lista wpisów serwisowych -->
                    <div id="records-list" class="space-y-3">
                        <!-- Generowane dynamicznie -->
                    </div>
                </div>
            </div>
        </section>
    </main>

    <!-- MODAL: DODAWANIE / EDYCJA POJAZDU I NUMERÓW FILTRÓW -->
    <div id="car-modal" class="fixed inset-0 z-50 bg-black/75 backdrop-blur-sm hidden flex items-center justify-center p-4 overflow-y-auto no-print">
        <div class="bg-garage-900 border border-slate-700 rounded-2xl max-w-xl w-full p-5 sm:p-6 shadow-2xl my-8">
            <div class="flex items-center justify-between pb-3 border-b border-slate-800">
                <h3 id="car-modal-title" class="text-lg font-bold text-white flex items-center gap-2">
                    <i class="fa-solid fa-car text-blue-400"></i> Dodaj Pojazd
                </h3>
                <button onclick="closeCarModal()" class="text-slate-400 hover:text-white p-1">
                    <i class="fa-solid fa-xmark text-lg"></i>
                </button>
            </div>

            <form id="car-form" onsubmit="saveCar(event)" class="mt-4 space-y-4">
                <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                    <div>
                        <label class="block text-xs font-semibold uppercase text-indigo-400 mb-1"><i class="fa-solid fa-user mr-1"></i>Właściciel pojazdu</label>
                        <input type="text" id="input-car-owner" placeholder="np. Jan Kowalski / tel." class="w-full bg-slate-950 border border-slate-700 rounded-xl px-3.5 py-2.5 text-sm text-white focus:outline-none focus:border-blue-500">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold uppercase text-slate-400 mb-1">Nr rej. / Rocznik</label>
                        <input type="text" id="input-car-plate" placeholder="np. PO 12345 / 2012" class="w-full bg-slate-950 border border-slate-700 rounded-xl px-3.5 py-2.5 text-sm font-mono text-white focus:outline-none focus:border-blue-500">
                    </div>
                </div>

                <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                    <div>
                        <label class="block text-xs font-semibold uppercase text-slate-400 mb-1">Aktualny przebieg (km)</label>
                        <input type="number" id="input-car-mileage" placeholder="np. 340000" class="w-full bg-slate-950 border border-slate-700 rounded-xl px-3.5 py-2.5 text-sm font-mono text-white focus:outline-none focus:border-blue-500">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold uppercase text-amber-400 mb-1"><i class="fa-solid fa-oil-can mr-1"></i>Interwał wymiany oleju (co ile km)</label>
                        <input type="number" id="input-car-oil-interval" placeholder="np. 15000 lub 10000" value="15000" class="w-full bg-slate-950 border border-slate-700 rounded-xl px-3.5 py-2.5 text-sm font-mono text-amber-300 focus:outline-none focus:border-blue-500">
                    </div>
                </div>

                <!-- Numery katalogowe filtrów -->
                <div class="pt-2 border-t border-slate-800">
                    <div class="text-xs font-bold uppercase tracking-wider text-blue-400 mb-3 flex items-center gap-1.5">
                        <i class="fa-solid fa-box-open"></i> Numery Porównawcze Filtrów (OEM / Do doboru zamienników)
                    </div>
                    <div class="grid grid-cols-2 gap-3">
                        <div>
                            <label class="block text-[11px] text-slate-400 mb-1">Filtr oleju</label>
                            <input type="text" id="input-part-oil" placeholder="np. 8507683" class="w-full bg-slate-950 border border-slate-700 rounded-xl px-3 py-2 text-xs font-mono text-white focus:outline-none focus:border-blue-500">
                        </div>
                        <div>
                            <label class="block text-[11px] text-slate-400 mb-1">Filtr powietrza</label>
                            <input type="text" id="input-part-air" placeholder="np. 7800151" class="w-full bg-slate-950 border border-slate-700 rounded-xl px-3 py-2 text-xs font-mono text-white focus:outline-none focus:border-blue-500">
                        </div>
                        <div>
                            <label class="block text-[11px] text-slate-400 mb-1">Filtr kabinowy</label>
                            <input type="text" id="input-part-cabin" placeholder="np. 9272642" class="w-full bg-slate-950 border border-slate-700 rounded-xl px-3 py-2 text-xs font-mono text-white focus:outline-none focus:border-blue-500">
                        </div>
                        <div>
                            <label class="block text-[11px] text-slate-400 mb-1">Filtr paliwa</label>
                            <input type="text" id="input-part-fuel" placeholder="np. 7822499" class="w-full bg-slate-950 border border-slate-700 rounded-xl px-3 py-2 text-xs font-mono text-white focus:outline-none focus:border-blue-500">
                        </div>
                    </div>
                    <div class="mt-3">
                        <label class="block text-[11px] text-slate-400 mb-1">Dodatkowe części / specyfikacja oleju (opcjonalnie)</label>
                        <input type="text" id="input-part-extra" placeholder="np. Olej 5W30 LL-04 5.2L, Klocki przód..." class="w-full bg-slate-950 border border-slate-700 rounded-xl px-3 py-2 text-xs text-white focus:outline-none focus:border-blue-500">
                    </div>
                </div>

                <div class="flex items-center justify-end space-x-3 pt-4 border-t border-slate-800">
                    <button type="button" onclick="closeCarModal()" class="px-4 py-2.5 rounded-xl bg-slate-800 hover:bg-slate-700 text-slate-300 text-xs font-semibold transition">
                        Anuluj
                    </button>
                    <button type="submit" id="save-car-btn" class="px-5 py-2.5 rounded-xl bg-blue-600 hover:bg-blue-500 text-white text-xs font-semibold shadow-lg shadow-blue-600/30 transition flex items-center gap-2">
                        <i class="fa-solid fa-floppy-disk"></i> Zapisz pojazd
                    </button>
                </div>
            </form>
        </div>
    </div>

    <!-- MODAL SZYBKIEJ AKTUALIZACJI PRZEBIEGU -->
    <div id="mileage-modal" class="fixed inset-0 z-50 bg-black/75 backdrop-blur-sm hidden flex items-center justify-center p-4 no-print">
        <div class="bg-garage-900 border border-slate-700 rounded-2xl max-w-sm w-full p-5 shadow-2xl">
            <div class="flex items-center justify-between pb-3 border-b border-slate-800">
                <h3 class="text-base font-bold text-white flex items-center gap-2">
                    <i class="fa-solid fa-gauge-high text-blue-400"></i> Aktualizuj przebieg pojazdu
                </h3>
                <button onclick="closeMileageModal()" class="text-slate-400 hover:text-white p-1">
                    <i class="fa-solid fa-xmark text-lg"></i>
                </button>
            </div>
            <form onsubmit="saveQuickMileage(event)" class="mt-4 space-y-4">
                <div>
                    <label class="block text-xs font-semibold uppercase text-slate-400 mb-1.5">Aktualny stan licznika (w km lub np. 348kkm) *</label>
                    <input type="text" id="input-quick-mileage" required placeholder="np. 348500 lub 348kkm" class="w-full bg-slate-950 border border-slate-700 rounded-xl px-3.5 py-2.5 text-sm font-mono text-emerald-400 focus:outline-none focus:border-blue-500">
                    <p class="text-[11px] text-slate-400 mt-1">Wpisanie aktualnego przebiegu pozwoli dokładnie wyliczyć ile kilometrów zostało do kolejnej wymiany oleju.</p>
                </div>
                <div class="flex items-center justify-end space-x-2 pt-2 border-t border-slate-800">
                    <button type="button" onclick="closeMileageModal()" class="px-4 py-2 rounded-xl bg-slate-800 hover:bg-slate-700 text-slate-300 text-xs font-semibold transition">
                        Anuluj
                    </button>
                    <button type="submit" class="px-4 py-2 rounded-xl bg-blue-600 hover:bg-blue-500 text-white text-xs font-semibold shadow-lg shadow-blue-600/30 transition">
                        Zapisz przebieg
                    </button>
                </div>
            </form>
        </div>
    </div>

    <!-- MODAL USTAWIEŃ POŁĄCZENIA I SYNCHRONIZACJI FIREBASE -->
    <div id="connection-modal" class="fixed inset-0 z-50 bg-black/75 backdrop-blur-sm hidden flex items-center justify-center p-4 overflow-y-auto no-print">
        <div class="bg-garage-900 border border-slate-700 rounded-2xl max-w-md w-full p-5 sm:p-6 shadow-2xl">
            <div class="flex items-center justify-between pb-3 border-b border-slate-800">
                <h3 class="text-base font-bold text-white flex items-center gap-2">
                    <i class="fa-solid fa-cloud-bolt text-blue-400"></i> Ustawienia Łącza i Synchronizacji
                </h3>
                <button onclick="closeConnectionModal()" class="text-slate-400 hover:text-white p-1">
                    <i class="fa-solid fa-xmark text-lg"></i>
                </button>
            </div>

            <div class="mt-4 space-y-4 text-xs">
                <div class="bg-slate-950/90 border border-slate-800 rounded-xl p-3.5 flex items-center justify-between">
                    <div>
                        <div class="text-slate-400 text-[11px]">Aktualny status połączenia</div>
                        <div id="modal-sync-status-text" class="font-bold text-sm text-emerald-400 mt-0.5">Zsynchronizowano z Firebase</div>
                    </div>
                    <button onclick="forceManualSync()" class="px-3 py-2 rounded-xl bg-blue-600/20 hover:bg-blue-600/30 text-blue-300 border border-blue-500/30 font-semibold transition flex items-center gap-1.5">
                        <i class="fa-solid fa-rotate"></i> Synchronizuj teraz
                    </button>
                </div>

                <div class="bg-slate-950/60 border border-slate-800/80 rounded-xl p-3.5 space-y-1.5">
                    <div class="text-slate-400 text-[11px] uppercase font-semibold">Identyfikator sesji chmury (UID)</div>
                    <div id="user-id-display" class="font-mono text-xs text-slate-200 break-all select-all">Ładowanie...</div>
                </div>

                <!-- Konfiguracja linku do ulubionego sklepu motoryzacyjnego -->
                <div class="bg-slate-950/80 border border-slate-800 rounded-xl p-3.5 space-y-2">
                    <label class="block text-xs font-semibold uppercase text-blue-400">
                        <i class="fa-solid fa-cart-shopping mr-1"></i> Twój sklep motoryzacyjny (WebTerminal M-MOT)
                    </label>
                    <input type="text" id="input-shop-url" placeholder="https://webterminal.m-mot.pl/search/v2/product?searchOption=-1&frg={NR}&closelyMatching=false" class="w-full bg-slate-950 border border-slate-700 rounded-xl px-3 py-2 text-xs font-mono text-slate-200 focus:outline-none focus:border-blue-500">
                    <p class="text-[11px] text-slate-400 leading-relaxed">
                        Domyślnie skonfigurowano Twoją hurtownię <strong class="text-slate-200">M-MOT WebTerminal</strong>. W miejsce <code class="text-amber-400">{NR}</code> aplikacja automatycznie podstawia numer klikniętego filtra z kafelka.
                    </p>
                </div>

                <!-- Kopia zapasowa Eksport / Import JSON -->
                <div class="bg-slate-950/80 border border-slate-800 rounded-xl p-3.5 space-y-2">
                    <div class="text-xs font-semibold uppercase text-emerald-400">
                        <i class="fa-solid fa-database mr-1"></i> Kopia Zapasowa Danych (Eksport / Import pliku)
                    </div>
                    <div class="flex items-center gap-2">
                        <button type="button" onclick="exportDatabaseJson()" class="flex-1 py-2 px-3 rounded-xl bg-slate-800 hover:bg-slate-700 text-slate-200 font-semibold transition flex items-center justify-center gap-1.5">
                            <i class="fa-solid fa-download text-blue-400"></i> Pobierz plik .JSON
                        </button>
                        <label class="flex-1 py-2 px-3 rounded-xl bg-slate-800 hover:bg-slate-700 text-slate-200 font-semibold transition flex items-center justify-center gap-1.5 cursor-pointer">
                            <i class="fa-solid fa-upload text-emerald-400"></i> Wgraj plik .JSON
                            <input type="file" accept=".json,application/json" onchange="importDatabaseJson(event)" class="hidden">
                        </label>
                    </div>
                </div>

                <div>
                    <label class="block text-xs font-semibold uppercase text-slate-400 mb-1.5">Własna konfiguracja Firebase JSON (opcjonalnie)</label>
                    <textarea id="input-custom-firebase-config" rows="4" placeholder='{"apiKey": "...", "projectId": "...", "appId": "..."}' class="w-full bg-slate-950 border border-slate-700 rounded-xl px-3 py-2 text-xs font-mono text-slate-200 focus:outline-none focus:border-blue-500"></textarea>
                    <p class="text-[11px] text-slate-500 mt-1">Pozostaw puste, aby korzystać z domyślnego, automatycznego połączenia chmurowego.</p>
                </div>

                <div class="flex items-center justify-end space-x-2 pt-3 border-t border-slate-800">
                    <button type="button" onclick="closeConnectionModal()" class="px-4 py-2 rounded-xl bg-slate-800 hover:bg-slate-700 text-slate-300 font-semibold transition">
                        Zamknij
                    </button>
                    <button type="button" onclick="applyCustomFirebaseConnection()" class="px-4 py-2 rounded-xl bg-blue-600 hover:bg-blue-500 text-white font-semibold shadow-lg shadow-blue-600/30 transition flex items-center gap-1.5">
                        <i class="fa-solid fa-plug-circle-check"></i> Połącz / Zapisz łącze
                    </button>
                </div>
            </div>
        </div>
    </div>

    <!-- POWIADOMIENIE TOAST -->
    <div id="toast" class="fixed bottom-5 right-5 z-50 transform translate-y-20 opacity-0 transition-all duration-300 pointer-events-none no-print">
        <div class="bg-slate-900 border border-slate-700 text-white px-4 py-3 rounded-xl shadow-2xl flex items-center space-x-3 text-xs font-medium">
            <i id="toast-icon" class="fa-solid fa-circle-check text-emerald-400 text-base"></i>
            <span id="toast-msg">Zapisano pomyślnie</span>
        </div>
    </div>

    <!-- SKRYPT FIREBASE ORAZ LOGIKA APLIKACJI -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInAnonymously, signInWithCustomToken, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, setDoc, updateDoc, deleteDoc, onSnapshot, collection } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // Stan aplikacji w pamięci
        let db = null;
        let auth = null;
        let currentUser = null;
        let cars = [];
        let selectedCarId = null;
        let isEditingCar = false;
        let editingRecordId = null;
        let initialSeedDone = false;
        let showAllUpcomingReminders = false;
        let activeRescheduleContext = null;
        let customShopUrlTemplate = 'https://webterminal.m-mot.pl/search/v2/product?searchOption=-1&frg={NR}&closelyMatching=false';

        const appId = typeof __app_id !== 'undefined' ? __app_id : 'autolog-service-book';

        // Parsowanie daty wpisu (obsługuje DD.MM.RRRR, MM.RRRR, RRRR-MM-DD)
        function parseRecordDateString(dateStr) {
            if (!dateStr) return null;
            const clean = String(dateStr).trim();

            // Format RRRR-MM-DD
            const isoMatch = clean.match(/^(\d{4})-(\d{1,2})-(\d{1,2})$/);
            if (isoMatch) {
                return new Date(parseInt(isoMatch[1], 10), parseInt(isoMatch[2], 10) - 1, parseInt(isoMatch[3], 10));
            }

            // Format DD.MM.RRRR lub D.M.RRRR
            const dmyMatch = clean.match(/^(\d{1,2})[./-](\d{1,2})[./-](\d{4})$/);
            if (dmyMatch) {
                return new Date(parseInt(dmyMatch[3], 10), parseInt(dmyMatch[2], 10) - 1, parseInt(dmyMatch[1], 10));
            }

            // Format MM.RRRR (np. 04.2025) - przyjmujemy 1 dzień miesiąca
            const myMatch = clean.match(/^(\d{1,2})[./-](\d{4})$/);
            if (myMatch) {
                return new Date(parseInt(myMatch[2], 10), parseInt(myMatch[1], 10) - 1, 1);
            }

            return null;
        }

        function formatDateToPl(dateObj) {
            if (!dateObj || isNaN(dateObj.getTime())) return '-';
            const d = String(dateObj.getDate()).padStart(2, '0');
            const m = String(dateObj.getMonth() + 1).padStart(2, '0');
            const y = dateObj.getFullYear();
            return `${d}.${m}.${y}`;
        }

        function formatDateToInputIso(dateObj) {
            if (!dateObj || isNaN(dateObj.getTime())) return '';
            const d = String(dateObj.getDate()).padStart(2, '0');
            const m = String(dateObj.getMonth() + 1).padStart(2, '0');
            const y = dateObj.getFullYear();
            return `${y}-${m}-${d}`;
        }

        // Funkcja pomocnicza do konwersji przebiegu np. "325kkm" -> 325000
        function parseMileageValue(raw) {
            if (!raw) return 0;
            const str = String(raw).toLowerCase().trim().replace(/\s+/g, '');
            if (str.includes('kkm') || str.endsWith('k')) {
                const num = parseFloat(str.replace(/[^0-9.,]/g, '').replace(',', '.'));
                return isNaN(num) ? 0 : Math.round(num * 1000);
            }
            const digits = parseInt(str.replace(/[^0-9]/g, ''), 10);
            return isNaN(digits) ? 0 : digits;
        }

        // Formatowanie przebiegu do czytelnej postaci np. 340 000 km (340kkm)
        function formatMileage(km) {
            const num = parseMileageValue(km);
            if (!num) return 'Brak danych';
            const kkm = (num / 1000).toFixed(num % 1000 === 0 ? 0 : 1);
            return `${num.toLocaleString('pl-PL')} km (${kkm}kkm)`;
        }

        // Powiadomienia Toast
        window.showToast = function(message, isError = false) {
            const toast = document.getElementById('toast');
            const msgEl = document.getElementById('toast-msg');
            const iconEl = document.getElementById('toast-icon');
            msgEl.textContent = message;
            iconEl.className = isError 
                ? 'fa-solid fa-circle-exclamation text-red-400 text-base' 
                : 'fa-solid fa-circle-check text-emerald-400 text-base';
            toast.classList.remove('translate-y-20', 'opacity-0');
            setTimeout(() => {
                toast.classList.add('translate-y-20', 'opacity-0');
            }, 3000);
        };

        // Aktualizacja wskaźnika synchronizacji
        function setSyncIndicator(status, text) {
            const dot = document.getElementById('sync-dot');
            const label = document.getElementById('sync-text');
            const modalStatus = document.getElementById('modal-sync-status-text');
            if (!dot || !label) return;
            label.textContent = text;
            dot.className = 'w-2 h-2 rounded-full ';
            if (status === 'online') {
                dot.className += 'bg-emerald-400';
                if (modalStatus) {
                    modalStatus.textContent = 'Połączono – ' + text;
                    modalStatus.className = 'font-bold text-sm text-emerald-400 mt-0.5';
                }
            } else if (status === 'syncing') {
                dot.className += 'bg-amber-400 animate-pulse';
                if (modalStatus) {
                    modalStatus.textContent = text;
                    modalStatus.className = 'font-bold text-sm text-amber-400 mt-0.5';
                }
            } else {
                dot.className += 'bg-red-500';
                if (modalStatus) {
                    modalStatus.textContent = text;
                    modalStatus.className = 'font-bold text-sm text-red-400 mt-0.5';
                }
            }
        }

        // Otwieranie i zamykanie modala ustawień łącza pod przyciskiem "Zsynchronizowano"
        window.openConnectionModal = function() {
            const shopInput = document.getElementById('input-shop-url');
            if (shopInput) shopInput.value = customShopUrlTemplate;
            document.getElementById('connection-modal').classList.remove('hidden');
        };

        window.closeConnectionModal = function() {
            document.getElementById('connection-modal').classList.add('hidden');
        };

        // Wyszukiwanie numeru porównawczego bezpośrednio w ulubionym sklepie użytkownika
        window.searchPartInShop = function(event, elementId) {
            if (event) event.stopPropagation();
            const partNumber = document.getElementById(elementId)?.textContent?.trim();
            if (!partNumber || partNumber === 'Brak') {
                showToast('Brak numeru części w tym kafelku – uzupełnij go w edycji pojazdu', true);
                return;
            }
            const cleanNumber = partNumber.replace(/\s+/g, '');
            let url = customShopUrlTemplate || 'https://webterminal.m-mot.pl/search/v2/product?searchOption=-1&frg={NR}&closelyMatching=false';
            if (url.includes('{NR}')) {
                url = url.replace('{NR}', encodeURIComponent(cleanNumber));
            } else if (url.includes('frg=')) {
                url = url.replace(/frg=[^&]+/, 'frg=' + encodeURIComponent(cleanNumber));
            } else {
                url = url + encodeURIComponent(cleanNumber);
            }
            window.open(url, '_blank', 'noopener,noreferrer');
        };

        // Eksport wszystkich samochodów i historii do pliku JSON
        window.exportDatabaseJson = function() {
            try {
                const exportPayload = {
                    app: 'AutoLog - Cyfrowa Książka Serwisowa',
                    exportedAt: new Date().toISOString(),
                    shopUrlTemplate: customShopUrlTemplate,
                    cars: cars
                };
                const dataStr = JSON.stringify(exportPayload, null, 2);
                const blob = new Blob([dataStr], { type: 'application/json;charset=utf-8' });
                const url = URL.createObjectURL(blob);
                const link = document.createElement('a');
                const dateStamp = new Date().toISOString().slice(0, 10);
                link.href = url;
                link.download = `AutoLog_Kopia_Zapasowa_${dateStamp}.json`;
                document.body.appendChild(link);
                link.click();
                document.body.removeChild(link);
                URL.revokeObjectURL(url);
                showToast(`Wyeksportowano bazę (${cars.length} pojazdów) do pliku JSON!`);
            } catch (e) {
                console.error('Błąd eksportu JSON:', e);
                showToast('Wystąpił błąd podczas eksportu bazy', true);
            }
        };

        // Import bazy pojazdów z pliku JSON
        window.importDatabaseJson = function(event) {
            const file = event.target.files?.[0];
            if (!file) return;

            const reader = new FileReader();
            reader.onload = async function(e) {
                try {
                    const parsed = JSON.parse(e.target.result);
                    const importedCars = Array.isArray(parsed) ? parsed : (Array.isArray(parsed.cars) ? parsed.cars : null);

                    if (!importedCars) {
                        showToast('Nieprawidłowy plik kopii zapasowej JSON', true);
                        return;
                    }

                    if (parsed.shopUrlTemplate) {
                        customShopUrlTemplate = parsed.shopUrlTemplate;
                    }

                    for (const carItem of importedCars) {
                        if (!carItem.id) carItem.id = 'car-' + Date.now() + '-' + Math.random().toString(36).slice(2, 6);
                        const existingIdx = cars.findIndex(c => c.id === carItem.id);
                        if (existingIdx >= 0) {
                            cars[existingIdx] = { ...carItem, updatedAt: Date.now() };
                        } else {
                            cars.push({ ...carItem, updatedAt: Date.now() });
                        }
                        if (currentUser && db) {
                            await persistCarToFirestore(carItem);
                        }
                    }

                    renderAll();
                    showToast(`Zaimportowano pomyślnie ${importedCars.length} pojazd(ów)!`);
                } catch (err) {
                    console.error('Błąd importu JSON:', err);
                    showToast('Błąd odczytu pliku JSON', true);
                } finally {
                    event.target.value = '';
                }
            };
            reader.readAsText(file);
        };

        // Ręczne wymuszenie synchronizacji wszystkich aut z chmurą Firebase
        window.forceManualSync = async function() {
            setSyncIndicator('syncing', 'Synchronizacja...');
            try {
                if (!currentUser || !db) {
                    await initFirebase();
                } else {
                    for (const car of cars) {
                        await persistCarToFirestore(car);
                    }
                }
                setSyncIndicator('online', 'Zsynchronizowano');
                showToast('Dane zostały pomyślnie zsynchronizowane z chmurą!');
            } catch (e) {
                console.error(e);
                setSyncIndicator('offline', 'Błąd łącza');
                showToast('Wystąpił błąd podczas synchronizacji', true);
            }
        };

        // Zastosowanie własnej konfiguracji łącza Firebase lub ponowne połączenie
        window.applyCustomFirebaseConnection = async function() {
            const shopInput = document.getElementById('input-shop-url');
            if (shopInput && shopInput.value.trim() !== '') {
                customShopUrlTemplate = shopInput.value.trim();
            }

            const rawConfig = document.getElementById('input-custom-firebase-config').value.trim();
            if (!rawConfig) {
                await window.forceManualSync();
                closeConnectionModal();
                return;
            }
            try {
                const parsedConfig = JSON.parse(rawConfig);
                setSyncIndicator('syncing', 'Łączenie...');
                const customApp = initializeApp(parsedConfig, 'custom-app-' + Date.now());
                auth = getAuth(customApp);
                db = getFirestore(customApp);
                await signInAnonymously(auth);
                onAuthStateChanged(auth, (user) => {
                    if (user) {
                        currentUser = user;
                        const uidEl = document.getElementById('user-id-display');
                        if (uidEl) uidEl.textContent = user.uid;
                        setupRealtimeListener();
                        showToast('Połączono z nowym łączem Firebase!');
                        closeConnectionModal();
                    }
                });
            } catch (err) {
                console.error(err);
                showToast('Nieprawidłowy format JSON konfiguracji Firebase', true);
            }
        };

        // Domyślny obiekt samochodu ze zdjęcia użytkownika (BMW F10)
        function getF10TemplateData() {
            return {
                id: 'bmw-f10-wba5c71050d054064',
                name: 'BMW F10',
                vin: 'WBA5C71050D054064',
                owner: '',
                mileage: 340000,
                oilIntervalKm: 15000,
                plate: 'Seria 5 (F10)',
                parts: {
                    oilFilter: '8507683',
                    airFilter: '7800151',
                    cabinFilter: '9272642',
                    fuelFilter: '7822499',
                    extra: ''
                },
                todos: [],
                records: [
                    {
                        id: 'rec-oil-2026-06',
                        type: 'OIL',
                        title: 'Wymiana oleju',
                        date: '06.2026',
                        mileage: 340000,
                        rawMileageLabel: '340kkm',
                        cost: 0,
                        filtersReplaced: {
                            oil: true,
                            air: false,
                            cabin: false,
                            fuel: false
                        },
                        partsUsed: 'Wymiana oleju + filtr oleju (zamiennik wg nr porównawczego 8507683)',
                        description: 'Serwis olejowy przy przebiegu 340kkm (zgodnie z notatką serwisową).'
                    },
                    {
                        id: 'rec-oil-2025-04',
                        type: 'OIL',
                        title: 'Wymiana oleju',
                        date: '04.2025',
                        mileage: 325000,
                        rawMileageLabel: '325kkm',
                        cost: 0,
                        filtersReplaced: {
                            oil: true,
                            air: true,
                            cabin: true,
                            fuel: true
                        },
                        partsUsed: 'Komplet filtrów (zamienniki wg nr porównawczych: oleju 8507683, powietrza 7800151, kabinowy 9272642, paliwa 7822499)',
                        description: 'Serwis olejowy przy przebiegu 325kkm (zgodnie z notatką serwisową).'
                    }
                ],
                updatedAt: Date.now()
            };
        }

        // Inicjalizacja Firebase i uwierzytelniania (zgodnie z wymogami RULE 1, 2, 3)
        async function initFirebase() {
            try {
                if (typeof __firebase_config === 'undefined' || !__firebase_config) {
                    setSyncIndicator('offline', 'Tryb lokalny');
                    cars = [getF10TemplateData()];
                    selectedCarId = null; // Karta pojazdu wyświetli się dopiero po kliknięciu
                    renderAll();
                    return;
                }

                const firebaseConfig = JSON.parse(__firebase_config);
                const app = initializeApp(firebaseConfig);
                auth = getAuth(app);
                db = getFirestore(app);

                setSyncIndicator('syncing', 'Autoryzacja...');

                // RULE 3: Auth Before Queries
                if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
                    await signInWithCustomToken(auth, __initial_auth_token);
                } else {
                    await signInAnonymously(auth);
                }

                onAuthStateChanged(auth, (user) => {
                    if (user) {
                        currentUser = user;
                        document.getElementById('user-id-display').textContent = `Użytkownik: ${user.uid}`;
                        setupRealtimeListener();
                    }
                });
            } catch (err) {
                console.error('Błąd inicjalizacji Firebase:', err);
                setSyncIndicator('offline', 'Błąd chmury');
                cars = [getF10TemplateData()];
                selectedCarId = null;
                renderAll();
            }
        }

        // Nasłuchiwanie kolekcji samochodów użytkownika w Firestore
        function setupRealtimeListener() {
            if (!currentUser || !db) return;

            // RULE 1: Strict Path
            const carsColRef = collection(db, 'artifacts', appId, 'users', currentUser.uid, 'cars');

            onSnapshot(carsColRef, async (snapshot) => {
                const loadedCars = [];
                snapshot.forEach((docSnap) => {
                    const data = docSnap.data();
                    // Deserializacja tablicy napraw jeśli była zapisana jako string lub tablica
                    let parsedRecords = [];
                    if (Array.isArray(data.records)) {
                        parsedRecords = data.records;
                    } else if (typeof data.records === 'string') {
                        try { parsedRecords = JSON.parse(data.records); } catch (e) { parsedRecords = []; }
                    }
                    loadedCars.push({
                        ...data,
                        id: docSnap.id,
                        records: parsedRecords
                    });
                });

                // Jeśli baza jest całkowicie pusta przy pierwszym uruchomieniu, automatycznie dodajemy auto F10 ze zdjęcia
                if (loadedCars.length === 0 && !initialSeedDone) {
                    initialSeedDone = true;
                    await window.importF10FromScreenshot(true);
                    return;
                }

                initialSeedDone = true;
                // Sortowanie w pamięci JS (RULE 2)
                loadedCars.sort((a, b) => (b.updatedAt || 0) - (a.updatedAt || 0));
                cars = loadedCars;

                // Jeśli wybrane wcześniej auto zostało usunięte, odznaczamy wybór
                if (selectedCarId && !cars.some(c => c.id === selectedCarId)) {
                    selectedCarId = null;
                }

                setSyncIndicator('online', 'Zsynchronizowano');
                renderAll();
            }, (error) => {
                console.error('Błąd odczytu Firestore:', error);
                setSyncIndicator('offline', 'Błąd synchronizacji');
            });
        }

        // Zapis pojedynczego auta do Firestore
        async function persistCarToFirestore(carObj) {
            if (!currentUser || !db) {
                renderAll();
                return;
            }
            setSyncIndicator('syncing', 'Zapisywanie...');
            const carDocRef = doc(db, 'artifacts', appId, 'users', currentUser.uid, 'cars', carObj.id);
            await setDoc(carDocRef, {
                ...carObj,
                updatedAt: Date.now()
            });
            setSyncIndicator('online', 'Zsynchronizowano');
        }

        // Import danych F10 bezpośrednio ze zrzutu ekranu
        window.importF10FromScreenshot = async function(silent = false) {
            const f10Data = getF10TemplateData();
            try {
                if (currentUser && db) {
                    await persistCarToFirestore(f10Data);
                } else {
                    const existingIndex = cars.findIndex(c => c.id === f10Data.id);
                    if (existingIndex >= 0) cars[existingIndex] = f10Data;
                    else cars.unshift(f10Data);
                }
                if (!silent) {
                    selectedCarId = f10Data.id;
                    showToast('Wczytano dane BMW F10 (WBA5C71050D054064) wraz z filtrami i historią!');
                }
                renderAll();
            } catch (e) {
                console.error(e);
                showToast('Nie udało się zaimportować pojazdu', true);
            }
        };

        // Główne renderowanie całego interfejsu
        function renderAll() {
            renderCarsSidebar();
            renderSelectedCarDetails();
        }

        // Renderowanie listy aut w lewym panelu (z uwzględnieniem wyszukiwarki)
        window.renderCarsSidebar = function() {
            const listEl = document.getElementById('cars-list');
            const countEl = document.getElementById('cars-count');
            const f10Banner = document.getElementById('f10-import-banner');
            const searchQuery = (document.getElementById('search-car-input')?.value || '').toLowerCase().trim();

            countEl.textContent = cars.length;

            // Pokaż przycisk przywrócenia F10 jeśli nie ma auta o tym VINie na liście
            const hasF10 = cars.some(c => (c.vin || '').toUpperCase() === 'WBA5C71050D054064');
            if (hasF10) {
                f10Banner.classList.add('hidden');
            } else {
                f10Banner.classList.remove('hidden');
            }

            if (cars.length === 0) {
                listEl.innerHTML = `
                    <div class="text-center py-6 px-3 text-xs text-slate-500 border border-dashed border-slate-800 rounded-xl">
                        Brak zapisanych pojazdów. Kliknij "Dodaj Pojazd".
                    </div>
                `;
                return;
            }

            const filteredCars = cars.filter(car => {
                if (!searchQuery) return true;
                const nameMatch = (car.name || '').toLowerCase().includes(searchQuery);
                const vinMatch = (car.vin || '').toLowerCase().includes(searchQuery);
                const plateMatch = (car.plate || '').toLowerCase().includes(searchQuery);
                const ownerMatch = (car.owner || '').toLowerCase().includes(searchQuery);
                return nameMatch || vinMatch || plateMatch || ownerMatch;
            });

            if (filteredCars.length === 0) {
                listEl.innerHTML = `
                    <div class="text-center py-5 px-3 text-xs text-slate-500 border border-dashed border-slate-800 rounded-xl">
                        Brak aut pasujących do wyszukiwania "${escapeHtml(searchQuery)}".
                    </div>
                `;
                return;
            }

            listEl.innerHTML = filteredCars.map(car => {
                const isSelected = car.id === selectedCarId;
                const recordsCount = Array.isArray(car.records) ? car.records.length : 0;
                const activeTodosCount = Array.isArray(car.todos) ? car.todos.filter(t => !t.done).length : 0;
                return `
                    <div onclick="selectCar('${car.id}')" class="cursor-pointer rounded-xl p-3.5 transition border ${isSelected ? 'bg-blue-600/15 border-blue-500/60 shadow-lg shadow-blue-950/50' : 'bg-slate-950/60 hover:bg-slate-800/70 border-slate-800/90'}">
                        <div class="flex items-center justify-between">
                            <div class="font-bold text-sm ${isSelected ? 'text-white' : 'text-slate-200'} flex items-center gap-2">
                                <i class="fa-solid fa-car ${isSelected ? 'text-blue-400' : 'text-slate-500'}"></i>
                                <span>${escapeHtml(car.name)}</span>
                            </div>
                            <span class="text-[11px] font-mono px-2 py-0.5 rounded-md ${isSelected ? 'bg-blue-500/20 text-blue-300' : 'bg-slate-800 text-slate-400'}">
                                ${(parseMileageValue(car.mileage) / 1000).toFixed(0)}kkm
                            </span>
                        </div>
                        ${car.owner ? `
                            <div class="mt-1 text-[11px] text-indigo-300 flex items-center gap-1.5">
                                <i class="fa-solid fa-user text-[9px]"></i>
                                <span class="truncate">${escapeHtml(car.owner)}</span>
                            </div>
                        ` : ''}
                        <div class="mt-1.5 flex items-center justify-between text-[11px] text-slate-400 font-mono">
                            <span class="truncate max-w-[140px]">${escapeHtml(car.vin || 'Brak VIN')}</span>
                            <span class="text-slate-500 font-sans flex items-center gap-1.5">
                                ${activeTodosCount > 0 ? `<span class="px-1.5 py-0.2 rounded bg-purple-500/20 text-purple-300 text-[10px] font-semibold" title="Pozycje do zrobienia">${activeTodosCount} ust.</span>` : ''}
                                <span>${recordsCount} wpisów</span>
                                <i class="fa-solid fa-chevron-right text-[9px] ${isSelected ? 'text-blue-400' : 'text-slate-600'}"></i>
                            </span>
                        </div>
                    </div>
                `;
            }).join('');
        };

        // Wybór samochodu z listy (kliknięcie otwiera kartę pojazdu)
        window.selectCar = function(carId) {
            selectedCarId = carId;
            renderAll();
            // Na ekranach mobilnych płynnie przewiń do karty wybranego pojazdu
            if (window.innerWidth < 1024) {
                const detailsEl = document.getElementById('car-details-view');
                if (detailsEl) {
                    setTimeout(() => detailsEl.scrollIntoView({ behavior: 'smooth', block: 'start' }), 50);
                }
            }
        };
        // Zamknięcie widoku karty pojazdu i powrót do pulpitu głównego
        window.closeCarDetails = function() {
            selectedCarId = null;
            renderAll();
            window.scrollTo({ top: 0, behavior: 'smooth' });
        };

        // Renderowanie szczegółów wybranego samochodu
        function renderSelectedCarDetails() {
            const emptyState = document.getElementById('empty-garage-state');
            const detailsView = document.getElementById('car-details-view');
            const emptyTitle = document.getElementById('empty-state-title');
            const emptyDesc = document.getElementById('empty-state-desc');
            const emptyF10Btn = document.getElementById('empty-state-f10-btn');

            const car = cars.find(c => c.id === selectedCarId);
            if (!car) {
                emptyState.classList.remove('hidden');
                detailsView.classList.add('hidden');

                const hasF10 = cars.some(c => (c.vin || '').toUpperCase() === 'WBA5C71050D054064');
                if (cars.length > 0) {
                    emptyTitle.textContent = 'Wybierz samochód ze swojego garażu';
                    emptyDesc.textContent = 'Kliknij na wybrane auto na liście w sekcji "Twój Garaż", aby wyświetlić jego kartę pojazdu, numery filtrów oraz pełną historię serwisową.';
                } else {
                    emptyTitle.textContent = 'Twój garaż jest pusty';
                    emptyDesc.textContent = 'Dodaj swój pierwszy samochód ręcznie lub załaduj dane swojego BMW F10 ze zdjęcia.';
                }
                if (hasF10) {
                    emptyF10Btn.classList.add('hidden');
                } else {
                    emptyF10Btn.classList.remove('hidden');
                }
                renderRemindersPanel();
                return;
            }

            emptyState.classList.add('hidden');
            detailsView.classList.remove('hidden');

            // Obliczenie najwyższego przebiegu (z karty auta lub z najnowszego wpisu serwisowego)
            let maxMileage = parseMileageValue(car.mileage);
            if (Array.isArray(car.records)) {
                car.records.forEach(r => {
                    const rm = parseMileageValue(r.mileage);
                    if (rm > maxMileage) maxMileage = rm;
                });
            }

            // Ustawienie danych głównych i danych do druku
            document.getElementById('detail-car-name').textContent = car.name || 'Pojazd';
            document.getElementById('detail-car-vin').textContent = car.vin || 'Brak numeru VIN';
            document.getElementById('detail-car-mileage').textContent = formatMileage(maxMileage);

            const ownerBadge = document.getElementById('detail-car-owner');
            const ownerText = document.getElementById('detail-car-owner-text');
            if (car.owner && car.owner.trim() !== '') {
                ownerText.textContent = car.owner;
                ownerBadge.classList.remove('hidden');
            } else {
                ownerBadge.classList.add('hidden');
            }

            const plateEl = document.getElementById('detail-car-plate');
            if (car.plate && car.plate.trim() !== '') {
                plateEl.textContent = car.plate;
                plateEl.classList.remove('hidden');
            } else {
                plateEl.classList.add('hidden');
            }

            // Dane nagłówka do wydruku
            document.getElementById('print-car-name').textContent = car.name || 'Pojazd';
            document.getElementById('print-car-vin').textContent = car.vin || '-';
            document.getElementById('print-car-owner').textContent = car.owner || 'Nie wpisano';
            document.getElementById('print-car-mileage').textContent = formatMileage(maxMileage);
            document.getElementById('print-date').textContent = new Date().toLocaleDateString('pl-PL');

            // Numery części / filtrów
            const parts = car.parts || {};
            document.getElementById('part-oil').textContent = parts.oilFilter || 'Brak';
            document.getElementById('part-air').textContent = parts.airFilter || 'Brak';
            document.getElementById('part-cabin').textContent = parts.cabinFilter || 'Brak';
            document.getElementById('part-fuel').textContent = parts.fuelFilter || 'Brak';

            const extraContainer = document.getElementById('part-extra-container');
            if (parts.extra && parts.extra.trim() !== '') {
                document.getElementById('part-extra').textContent = parts.extra;
                extraContainer.classList.remove('hidden');
            } else {
                extraContainer.classList.add('hidden');
            }

            renderOilIntervalStatus(car, maxMileage);
            renderTodoList();
            renderRecords();
        }

        // Obliczanie i wyświetlanie paska interwału kilometrowego wymiany oleju
        function computeOilIntervalInfo(car, currentMaxMileage) {
            const intervalKm = parseInt(car.oilIntervalKm, 10) || 15000;
            const records = Array.isArray(car.records) ? car.records : [];
            const oilRecords = records
                .filter(r => r.type === 'OIL' && parseMileageValue(r.mileage) > 0)
                .sort((a, b) => parseMileageValue(b.mileage) - parseMileageValue(a.mileage));

            if (oilRecords.length === 0) {
                return {
                    hasOilRecord: false,
                    intervalKm
                };
            }

            const lastOilRec = oilRecords[0];
            const lastOilMileage = parseMileageValue(lastOilRec.mileage);
            const nextOilMileage = lastOilMileage + intervalKm;
            const drivenSinceLast = Math.max(0, currentMaxMileage - lastOilMileage);
            const remainingKm = nextOilMileage - currentMaxMileage;
            const percentUsed = Math.min(100, Math.max(4, Math.round((drivenSinceLast / intervalKm) * 100)));

            return {
                hasOilRecord: true,
                intervalKm,
                lastOilMileage,
                lastOilDate: lastOilRec.date || '-',
                nextOilMileage,
                drivenSinceLast,
                remainingKm,
                percentUsed
            };
        }

        function renderOilIntervalStatus(car, currentMaxMileage) {
            const badgeEl = document.getElementById('oil-interval-badge');
            const subtextEl = document.getElementById('oil-interval-subtext');
            const remainingEl = document.getElementById('oil-interval-remaining');
            const barEl = document.getElementById('oil-interval-bar');
            if (!badgeEl || !subtextEl || !remainingEl || !barEl) return;

            const info = computeOilIntervalInfo(car, currentMaxMileage);
            badgeEl.textContent = `co ${info.intervalKm.toLocaleString('pl-PL')} km`;

            if (!info.hasOilRecord) {
                subtextEl.textContent = 'Brak zarejestrowanego serwisu olejowego – dodaj wpis "Serwis Olejowy", aby rozpocząć odliczanie.';
                remainingEl.textContent = 'Brak danych';
                remainingEl.className = 'text-xs font-mono font-bold px-3 py-1 rounded-xl bg-slate-800 text-slate-400 border border-slate-700';
                barEl.style.width = '0%';
                return;
            }

            const lastKkm = (info.lastOilMileage / 1000).toFixed(info.lastOilMileage % 1000 === 0 ? 0 : 1);
            const nextKkm = (info.nextOilMileage / 1000).toFixed(info.nextOilMileage % 1000 === 0 ? 0 : 1);

            subtextEl.innerHTML = `Ostatni serwis olejowy: <strong class="text-slate-200 font-mono">${lastKkm}kkm</strong> (${escapeHtml(info.lastOilDate)}) • Następna wymiana przy: <strong class="text-amber-400 font-mono">${nextKkm}kkm (${info.nextOilMileage.toLocaleString('pl-PL')} km)</strong> • Przejechano: <strong class="text-slate-200 font-mono">${info.drivenSinceLast.toLocaleString('pl-PL')} km</strong>`;

            barEl.style.width = `${info.percentUsed}%`;

            if (info.remainingKm <= 0) {
                remainingEl.innerHTML = `<i class="fa-solid fa-triangle-exclamation mr-1"></i>Przekroczono o ${Math.abs(info.remainingKm).toLocaleString('pl-PL')} km!`;
                remainingEl.className = 'text-xs font-mono font-bold px-3 py-1 rounded-xl bg-red-500/20 text-red-300 border border-red-500/40';
                barEl.className = 'h-full rounded-full bg-gradient-to-r from-red-600 to-rose-500 transition-all duration-500';
            } else if (info.remainingKm <= 1500) {
                remainingEl.innerHTML = `<i class="fa-solid fa-clock mr-1"></i>Zostało tylko: ${info.remainingKm.toLocaleString('pl-PL')} km`;
                remainingEl.className = 'text-xs font-mono font-bold px-3 py-1 rounded-xl bg-amber-500/20 text-amber-300 border border-amber-500/40';
                barEl.className = 'h-full rounded-full bg-gradient-to-r from-amber-500 to-orange-500 transition-all duration-500';
            } else {
                remainingEl.innerHTML = `Zostało: ${info.remainingKm.toLocaleString('pl-PL')} km`;
                remainingEl.className = 'text-xs font-mono font-bold px-3 py-1 rounded-xl bg-emerald-500/15 text-emerald-300 border border-emerald-500/30';
                barEl.className = 'h-full rounded-full bg-gradient-to-r from-emerald-500 to-teal-400 transition-all duration-500';
            }
        }

        // Szybka aktualizacja przebiegu auta z poziomu paska interwału
        window.promptUpdateMileage = function() {
            const car = cars.find(c => c.id === selectedCarId);
            if (!car) return;
            document.getElementById('input-quick-mileage').value = car.mileage || '';
            document.getElementById('mileage-modal').classList.remove('hidden');
        };

        window.closeMileageModal = function() {
            document.getElementById('mileage-modal').classList.add('hidden');
        };

        window.saveQuickMileage = async function(e) {
            e.preventDefault();
            const car = cars.find(c => c.id === selectedCarId);
            if (!car) return;
            const newKm = parseMileageValue(document.getElementById('input-quick-mileage').value);
            if (!newKm) {
                showToast('Wpisz prawidłowy przebieg', true);
                return;
            }
            const updatedCar = { ...car, mileage: newKm, updatedAt: Date.now() };
            if (!currentUser || !db) {
                const idx = cars.findIndex(c => c.id === car.id);
                if (idx >= 0) cars[idx] = updatedCar;
            }
            await persistCarToFirestore(updatedCar);
            closeMileageModal();
            renderAll();
            showToast(`Zaktualizowano przebieg do ${formatMileage(newKm)}`);
        };

        // OBSŁUGA LISTY "DO ZROBIENIA PRZY NASTĘPNYM SERWISIE" (SCHOWEK USTEREK)
        window.renderTodoList = function() {
            const car = cars.find(c => c.id === selectedCarId);
            if (!car) return;

            const container = document.getElementById('todo-list-container');
            const badge = document.getElementById('todo-count-badge');
            const searchQuery = (document.getElementById('search-todo-input')?.value || '').toLowerCase().trim();

            const todos = Array.isArray(car.todos) ? car.todos : [];
            const activeCount = todos.filter(t => !t.done).length;
            if (badge) badge.textContent = activeCount;

            const filteredTodos = todos.filter(t => {
                if (!searchQuery) return true;
                return (t.text || '').toLowerCase().includes(searchQuery);
            });

            if (filteredTodos.length === 0) {
                container.innerHTML = `
                    <div class="text-center py-4 px-3 bg-slate-950/40 border border-dashed border-slate-800 rounded-xl text-xs text-slate-500">
                        ${searchQuery ? `Brak pozycji pasujących do "${escapeHtml(searchQuery)}".` : 'Brak zapisanych usterek lub zadań na kolejny serwis.'}
                    </div>
                `;
                return;
            }

            container.innerHTML = filteredTodos.map(item => {
                let priorityBadge = '';
                if (item.priority === 'URGENT') {
                    priorityBadge = `<span class="px-2 py-0.5 rounded text-[10px] font-bold bg-red-500/20 text-red-300 border border-red-500/30">Pilne</span>`;
                } else if (item.priority === 'BUY') {
                    priorityBadge = `<span class="px-2 py-0.5 rounded text-[10px] font-bold bg-amber-500/20 text-amber-300 border border-amber-500/30">Do kupienia</span>`;
                } else {
                    priorityBadge = `<span class="px-2 py-0.5 rounded text-[10px] font-medium bg-slate-800 text-slate-400">Serwis</span>`;
                }

                return `
                    <div class="flex items-center justify-between gap-3 p-3 rounded-xl border ${item.done ? 'bg-slate-950/30 border-slate-800/50 opacity-60' : 'bg-slate-950/80 border-slate-800'} transition">
                        <label class="flex items-center gap-3 cursor-pointer flex-1 min-w-0">
                            <input type="checkbox" ${item.done ? 'checked' : ''} onchange="toggleTodoItem('${item.id}')" class="w-4 h-4 rounded accent-purple-600 bg-slate-900 border-slate-700 shrink-0">
                            <div class="flex items-center flex-wrap gap-2 min-w-0">
                                ${priorityBadge}
                                <span class="text-xs sm:text-sm ${item.done ? 'line-through text-slate-500' : 'text-slate-200 font-medium'} break-words">${escapeHtml(item.text)}</span>
                                ${item.createdAt ? `<span class="text-[10px] font-mono text-slate-500">(${escapeHtml(item.createdAt)})</span>` : ''}
                            </div>
                        </label>
                        <div class="flex items-center gap-1.5 shrink-0">
                            <button type="button" onclick="convertTodoToRecord('${item.id}')" class="px-2.5 py-1 rounded-lg bg-emerald-600/15 hover:bg-emerald-600/30 text-emerald-300 border border-emerald-500/30 text-[11px] font-medium transition flex items-center gap-1" title="Utwórz wpis serwisowy z tej pozycji">
                                <i class="fa-solid fa-screwdriver-wrench text-[10px]"></i>
                                <span class="hidden sm:inline">Przenieś do wpisu</span>
                            </button>
                            <button type="button" onclick="deleteTodoItem('${item.id}')" class="p-1.5 text-slate-500 hover:text-red-400 rounded-lg hover:bg-slate-800 transition text-xs" title="Usuń z listy">
                                <i class="fa-solid fa-trash-can"></i>
                            </button>
                        </div>
                    </div>
                `;
            }).join('');
        };

        window.addTodoItem = async function(e) {
            e.preventDefault();
            const car = cars.find(c => c.id === selectedCarId);
            if (!car) return;

            const input = document.getElementById('input-todo-text');
            const prioritySelect = document.getElementById('input-todo-priority');
            const text = input.value.trim();
            if (!text) return;

            const newTodo = {
                id: 'todo-' + Date.now(),
                text,
                priority: prioritySelect.value || 'NORMAL',
                done: false,
                createdAt: new Date().toLocaleDateString('pl-PL')
            };

            const updatedTodos = [newTodo, ...(Array.isArray(car.todos) ? car.todos : [])];
            const updatedCar = { ...car, todos: updatedTodos, updatedAt: Date.now() };

            if (!currentUser || !db) {
                const idx = cars.findIndex(c => c.id === car.id);
                if (idx >= 0) cars[idx] = updatedCar;
            }

            input.value = '';
            await persistCarToFirestore(updatedCar);
            renderAll();
            showToast('Dodano pozycję do listy "Do zrobienia"');
        };

        window.toggleTodoItem = async function(todoId) {
            const car = cars.find(c => c.id === selectedCarId);
            if (!car) return;

            const updatedTodos = (car.todos || []).map(t => t.id === todoId ? { ...t, done: !t.done } : t);
            const updatedCar = { ...car, todos: updatedTodos, updatedAt: Date.now() };

            if (!currentUser || !db) {
                const idx = cars.findIndex(c => c.id === car.id);
                if (idx >= 0) cars[idx] = updatedCar;
            }

            await persistCarToFirestore(updatedCar);
            renderAll();
        };

        window.deleteTodoItem = async function(todoId) {
            const car = cars.find(c => c.id === selectedCarId);
            if (!car) return;

            const updatedTodos = (car.todos || []).filter(t => t.id !== todoId);
            const updatedCar = { ...car, todos: updatedTodos, updatedAt: Date.now() };

            if (!currentUser || !db) {
                const idx = cars.findIndex(c => c.id === car.id);
                if (idx >= 0) cars[idx] = updatedCar;
            }

            await persistCarToFirestore(updatedCar);
            renderAll();
            showToast('Usunięto pozycję ze schowka usterek');
        };

        window.convertTodoToRecord = function(todoId) {
            const car = cars.find(c => c.id === selectedCarId);
            if (!car) return;
            const todo = (car.todos || []).find(t => t.id === todoId);
            if (!todo) return;

            openRecordModal();
            const repairRadio = document.querySelector('input[name="record-type"][value="REPAIR"]');
            if (repairRadio) repairRadio.checked = true;
            toggleAllFilters(false);
            document.getElementById('input-record-title').value = todo.text;
        };

        // Wyliczanie przypomnień o przeglądach dla wszystkich aut:
        // Dla każdego auta bierzemy WYŁĄCZNIE JEDEN NAJNOWSZY przegląd / serwis olejowy i od niego liczymy rok (-14 dni)
        function computeVehicleReminders() {
            const now = new Date();
            now.setHours(0, 0, 0, 0);
            const reminders = [];

            cars.forEach(car => {
                const records = Array.isArray(car.records) ? car.records : [];
                const overrides = car.reminderOverrides || {};

                // Bierzemy pod uwagę wszystkie przeglądy / serwisy olejowe (OIL lub INSPECTION) w danym aucie
                // i sortujemy je od najnowszego (po dacie, a w razie remisu po przebiegu)
                const serviceRecords = records
                    .filter(r => (r.type === 'OIL' || r.type === 'INSPECTION') && parseRecordDateString(r.date))
                    .sort((a, b) => {
                        const dateDiff = parseRecordDateString(b.date) - parseRecordDateString(a.date);
                        if (dateDiff !== 0) return dateDiff;
                        return parseMileageValue(b.mileage) - parseMileageValue(a.mileage);
                    });

                if (serviceRecords.length === 0) return;

                // Wybieramy TYLKO JEDEN NAJNOWSZY przegląd / serwis w tym aucie
                const latestRec = serviceRecords[0];
                const remKey = `LATEST_${latestRec.id}`;
                const override = overrides[remKey] || {};

                // Jeśli użytkownik usunął / odrzucił przypomnienie dla tego najnowszego przeglądu, pomijamy je
                if (override.dismissed) return;

                const lastDate = parseRecordDateString(latestRec.date);
                if (!lastDate) return;

                // Domyślny termin kolejnego przeglądu: +1 rok od ostatniego (najnowszego) przeglądu
                let dueDate = new Date(lastDate.getFullYear() + 1, lastDate.getMonth(), lastDate.getDate());
                let isRescheduled = false;

                if (override.customDueDate) {
                    const customParsed = parseRecordDateString(override.customDueDate);
                    if (customParsed) {
                        dueDate = customParsed;
                        isRescheduled = true;
                    }
                }

                dueDate.setHours(0, 0, 0, 0);
                const diffMs = dueDate.getTime() - now.getTime();
                const daysLeft = Math.round(diffMs / (1000 * 60 * 60 * 24));

                // Aktywne przypomnienie pojawia się na 14 dni (2 tygodnie) przed upływem roku (lub jeśli jest już po terminie / przeniesione)
                const isWithinTwoWeeks = daysLeft <= 14;

                // Sprawdzenie interwału kilometrowego dla tego auta
                let maxKm = parseMileageValue(car.mileage);
                records.forEach(r => {
                    const rm = parseMileageValue(r.mileage);
                    if (rm > maxKm) maxKm = rm;
                });
                const oilInfo = computeOilIntervalInfo(car, maxKm);
                const isMileageDueSoon = oilInfo.hasOilRecord && oilInfo.remainingKm <= 1500;

                reminders.push({
                    carId: car.id,
                    carName: car.name,
                    carVin: car.vin,
                    remKey,
                    categoryType: latestRec.type,
                    categoryLabel: latestRec.type === 'INSPECTION' ? 'Przegląd okresowy' : 'Przegląd / Serwis olejowy',
                    lastRecordTitle: latestRec.title,
                    lastRecordDateStr: latestRec.date,
                    lastRecordMileageStr: `${(parseMileageValue(latestRec.mileage) / 1000).toFixed(0)}kkm`,
                    lastDate,
                    dueDate,
                    dueDateStr: formatDateToPl(dueDate),
                    dueDateIso: formatDateToInputIso(dueDate),
                    daysLeft,
                    isWithinTwoWeeks: isWithinTwoWeeks || isMileageDueSoon,
                    isMileageDueSoon,
                    oilRemainingKm: oilInfo.hasOilRecord ? oilInfo.remainingKm : null,
                    oilNextMileageKm: oilInfo.hasOilRecord ? oilInfo.nextOilMileage : null,
                    isRescheduled,
                    note: override.note || ''
                });
            });

            // Sortuj od najpilniejszych (najmniej dni pozostało)
            reminders.sort((a, b) => a.daysLeft - b.daysLeft);
            return reminders;
        }

        window.toggleShowAllReminders = function() {
            showAllUpcomingReminders = !showAllUpcomingReminders;
            renderRemindersPanel();
        };

        window.renderRemindersPanel = function() {
            const listEl = document.getElementById('reminders-list');
            const badgeCountEl = document.getElementById('reminders-badge-count');
            const toggleTextEl = document.getElementById('toggle-all-reminders-text');
            if (!listEl) return;

            const allReminders = computeVehicleReminders();
            // Przypomnienia spełniające warunek <= 14 dni przed upływem roku (lub ręcznie przełożone przez użytkownika)
            const activeAlerts = allReminders.filter(r => r.isWithinTwoWeeks || r.isRescheduled);

            if (badgeCountEl) {
                badgeCountEl.textContent = activeAlerts.length;
                badgeCountEl.className = activeAlerts.length > 0
                    ? 'text-xs font-mono font-bold px-2 py-0.5 rounded-full bg-amber-500/20 text-amber-300 border border-amber-500/30'
                    : 'text-xs font-mono font-bold px-2 py-0.5 rounded-full bg-emerald-500/20 text-emerald-300 border border-emerald-500/30';
            }

            if (toggleTextEl) {
                toggleTextEl.textContent = showAllUpcomingReminders
                    ? 'Pokaż tylko pilne (≤ 14 dni)'
                    : `Pokaż wszystkie terminy (${allReminders.length})`;
            }

            const displayed = showAllUpcomingReminders ? allReminders : activeAlerts;

            if (displayed.length === 0) {
                listEl.innerHTML = `
                    <div class="bg-slate-950/60 border border-slate-800/80 rounded-xl p-4 flex flex-col sm:flex-row items-center justify-between gap-3 text-xs text-slate-400">
                        <div class="flex items-center space-x-3">
                            <i class="fa-solid fa-circle-check text-emerald-400 text-lg"></i>
                            <div>
                                <div class="font-semibold text-slate-200">Brak pilnych przeglądów w najbliższych 2 tygodniach</div>
                                <div>Wszystkie Twoje samochody mają aktualne badania i serwisy (powiadomienie pojawi się automatycznie na 14 dni przed upływem roku od ostatniego przeglądu).</div>
                            </div>
                        </div>
                        ${allReminders.length > 0 ? `
                            <button onclick="toggleShowAllReminders()" class="shrink-0 px-3 py-1.5 rounded-lg bg-slate-800 hover:bg-slate-700 text-blue-400 font-medium transition">
                                Zobacz zaplanowane (${allReminders.length})
                            </button>
                        ` : ''}
                    </div>
                `;
                return;
            }

            listEl.innerHTML = displayed.map(rem => {
                let statusBadge = '';
                let cardBorder = 'border-slate-800';

                if (rem.daysLeft < 0) {
                    statusBadge = `<span class="px-2.5 py-0.5 rounded-md text-[11px] font-bold bg-red-500/20 text-red-300 border border-red-500/40"><i class="fa-solid fa-triangle-exclamation mr-1"></i>Po terminie (${Math.abs(rem.daysLeft)} dni temu)</span>`;
                    cardBorder = 'border-red-500/40 bg-red-950/10';
                } else if (rem.daysLeft === 0) {
                    statusBadge = `<span class="px-2.5 py-0.5 rounded-md text-[11px] font-bold bg-amber-500/20 text-amber-300 border border-amber-500/40"><i class="fa-solid fa-bell mr-1"></i>Termin mija dzisiaj!</span>`;
                    cardBorder = 'border-amber-500/50 bg-amber-950/10';
                } else if (rem.daysLeft <= 14) {
                    statusBadge = `<span class="px-2.5 py-0.5 rounded-md text-[11px] font-bold bg-amber-500/20 text-amber-300 border border-amber-500/40"><i class="fa-solid fa-clock mr-1"></i>Za ${rem.daysLeft} dni (mniej niż 2 tyg.)</span>`;
                    cardBorder = 'border-amber-500/40 bg-amber-950/10';
                } else {
                    statusBadge = `<span class="px-2.5 py-0.5 rounded-md text-[11px] font-medium bg-blue-500/15 text-blue-300 border border-blue-500/30"><i class="fa-regular fa-calendar mr-1"></i>Za ${rem.daysLeft} dni</span>`;
                }

                const rescheduledTag = rem.isRescheduled
                    ? `<span class="px-2 py-0.5 rounded text-[10px] font-mono bg-purple-500/20 text-purple-300 border border-purple-500/30"><i class="fa-solid fa-calendar-day mr-1"></i>Przełożony termin</span>`
                    : '';

                const mileageAlertTag = rem.oilRemainingKm !== null
                    ? (rem.oilRemainingKm <= 0
                        ? `<span class="px-2 py-0.5 rounded text-[10px] font-mono font-bold bg-red-500/20 text-red-300 border border-red-500/40"><i class="fa-solid fa-gauge-high mr-1"></i>Interwał oleju przekroczony o ${Math.abs(rem.oilRemainingKm).toLocaleString('pl-PL')} km!</span>`
                        : (rem.oilRemainingKm <= 1500
                            ? `<span class="px-2 py-0.5 rounded text-[10px] font-mono font-bold bg-amber-500/20 text-amber-300 border border-amber-500/40"><i class="fa-solid fa-oil-can mr-1"></i>Wymiana oleju za ${rem.oilRemainingKm.toLocaleString('pl-PL')} km</span>`
                            : `<span class="px-2 py-0.5 rounded text-[10px] font-mono bg-slate-800 text-slate-300 border border-slate-700">Do wymiany oleju: ${rem.oilRemainingKm.toLocaleString('pl-PL')} km</span>`))
                    : '';

                return `
                    <div class="rounded-xl p-4 border ${cardBorder} bg-slate-950/80 transition flex flex-col sm:flex-row sm:items-center justify-between gap-4">
                        <div class="space-y-1.5">
                            <div class="flex items-center flex-wrap gap-2">
                                <span onclick="selectCar('${rem.carId}')" class="cursor-pointer font-extrabold text-sm text-white hover:text-blue-400 transition flex items-center gap-1.5">
                                    <i class="fa-solid fa-car text-blue-400"></i> ${escapeHtml(rem.carName)}
                                </span>
                                <span class="text-xs text-slate-400">•</span>
                                <span class="text-xs font-semibold text-slate-200">${escapeHtml(rem.categoryLabel)}</span>
                                ${statusBadge}
                                ${mileageAlertTag}
                                ${rescheduledTag}
                            </div>

                            <div class="text-xs text-slate-400 flex flex-wrap items-center gap-x-4 gap-y-1">
                                <span>Ostatni przegląd: <strong class="text-slate-300 font-mono">${escapeHtml(rem.lastRecordDateStr)}</strong> przy <strong class="text-slate-300 font-mono">${escapeHtml(rem.lastRecordMileageStr || '')}</strong> (${escapeHtml(rem.lastRecordTitle)})</span>
                                <span>Termin kolejnego przeglądu: <strong class="text-amber-400 font-mono">${escapeHtml(rem.dueDateStr)}</strong></span>
                            </div>

                            ${rem.note ? `<div class="text-[11px] text-purple-300 italic"><i class="fa-regular fa-comment-dots mr-1"></i>Notatka: ${escapeHtml(rem.note)}</div>` : ''}
                        </div>

                        <div class="flex items-center flex-wrap gap-2 shrink-0 pt-2 sm:pt-0 border-t sm:border-t-0 border-slate-800/80">
                            <button onclick="openRescheduleModal('${rem.carId}', '${rem.remKey}', '${rem.dueDateIso}', '${escapeHtml(rem.carName)} - ${escapeHtml(rem.categoryLabel)}', '${escapeHtml(rem.note)}')" class="px-3 py-1.5 rounded-xl bg-slate-800 hover:bg-amber-600/20 text-slate-200 hover:text-amber-300 border border-slate-700 hover:border-amber-500/40 text-xs font-medium transition flex items-center gap-1.5" title="Przenieś przypomnienie na inny termin">
                                <i class="fa-regular fa-calendar-plus text-amber-400"></i>
                                <span>Przenieś termin</span>
                            </button>
                            <button onclick="dismissReminder('${rem.carId}', '${rem.remKey}')" class="px-3 py-1.5 rounded-xl bg-slate-800 hover:bg-red-950/60 text-slate-300 hover:text-red-300 border border-slate-700 hover:border-red-800/50 text-xs font-medium transition flex items-center gap-1.5" title="Usuń przypomnienie">
                                <i class="fa-solid fa-trash-can text-red-400"></i>
                                <span>Usuń</span>
                            </button>
                        </div>
                    </div>
                `;
            }).join('');
        };

        // Otwieranie modala przeniesienia przypomnienia na inny termin
        window.openRescheduleModal = function(carId, remKey, currentDueIso, label, currentNote = '') {
            activeRescheduleContext = { carId, remKey };
            document.getElementById('reschedule-car-label').textContent = label;
            document.getElementById('reschedule-current-due').textContent = `Obecny wyznaczony termin: ${currentDueIso.split('-').reverse().join('.')}`;
            document.getElementById('input-reschedule-date').value = currentDueIso;
            document.getElementById('input-reschedule-note').value = currentNote || '';
            document.getElementById('reschedule-modal').classList.remove('hidden');
        };

        window.closeRescheduleModal = function() {
            document.getElementById('reschedule-modal').classList.add('hidden');
            activeRescheduleContext = null;
        };

        // Szybkie dodanie dni (+7, +14, +30) w modalu zmiany terminu
        window.applyQuickPostpone = function(daysToAdd) {
            const dateInput = document.getElementById('input-reschedule-date');
            let baseDate = dateInput.value ? new Date(dateInput.value) : new Date();
            const today = new Date();
            if (baseDate < today) baseDate = today;
            baseDate.setDate(baseDate.getDate() + daysToAdd);
            dateInput.value = formatDateToInputIso(baseDate);
        };

        // Zapisanie nowego terminu przypomnienia
        window.saveRescheduledReminder = async function(e) {
            e.preventDefault();
            if (!activeRescheduleContext) return;
            const { carId, remKey } = activeRescheduleContext;
            const newDateIso = document.getElementById('input-reschedule-date').value;
            const note = document.getElementById('input-reschedule-note').value.trim();

            const car = cars.find(c => c.id === carId);
            if (!car || !newDateIso) return;

            const updatedOverrides = { ...(car.reminderOverrides || {}) };
            updatedOverrides[remKey] = {
                ...(updatedOverrides[remKey] || {}),
                customDueDate: newDateIso,
                note,
                dismissed: false,
                updatedAt: Date.now()
            };

            const updatedCar = {
                ...car,
                reminderOverrides: updatedOverrides,
                updatedAt: Date.now()
            };

            if (!currentUser || !db) {
                const idx = cars.findIndex(c => c.id === carId);
                if (idx >= 0) cars[idx] = updatedCar;
            }

            await persistCarToFirestore(updatedCar);
            closeRescheduleModal();
            renderAll();
            showToast(`Przeniesiono przypomnienie na ${newDateIso.split('-').reverse().join('.')}`);
        };

        // Usunięcie / odrzucenie przypomnienia z potwierdzeniem
        window.dismissReminder = function(carId, remKey) {
            const car = cars.find(c => c.id === carId);
            if (!car) return;

            document.getElementById('confirm-title').textContent = 'Usunąć to przypomnienie?';
            document.getElementById('confirm-desc').textContent = 'To przypomnienie o przeglądzie zostanie usunięte z listy powiadomień (wpis w historii pojazdu pozostanie bez zmian).';
            const btn = document.getElementById('confirm-action-btn');
            btn.onclick = async () => {
                const updatedOverrides = { ...(car.reminderOverrides || {}) };
                updatedOverrides[remKey] = {
                    ...(updatedOverrides[remKey] || {}),
                    dismissed: true,
                    updatedAt: Date.now()
                };

                const updatedCar = {
                    ...car,
                    reminderOverrides: updatedOverrides,
                    updatedAt: Date.now()
                };

                if (!currentUser || !db) {
                    const idx = cars.findIndex(c => c.id === carId);
                    if (idx >= 0) cars[idx] = updatedCar;
                }

                await persistCarToFirestore(updatedCar);
                closeConfirmModal();
                renderAll();
                showToast('Przypomnienie zostało usunięte');
            };
            document.getElementById('confirm-modal').classList.remove('hidden');
        };

        // Czyszczenie wyszukiwarki w historii napraw
        window.clearRecordSearch = function() {
            const input = document.getElementById('search-record-input');
            if (input) {
                input.value = '';
                renderRecords();
            }
        };

        // Pomocnicza funkcja sprawdzająca czy dany filtr był wymieniony w danym wpisie
        function getReplacedFiltersForRecord(rec, carParts) {
            const result = [];
            const fr = rec.filtersReplaced || null;
            const textCombined = `${rec.partsUsed || ''} ${rec.description || ''} ${rec.title || ''}`.toLowerCase();

            const oilChecked = fr ? !!fr.oil : (textCombined.includes('filtr oleju') || (carParts.oilFilter && textCombined.includes(carParts.oilFilter.toLowerCase())));
            const airChecked = fr ? !!fr.air : (textCombined.includes('filtr powietrza') || (carParts.airFilter && textCombined.includes(carParts.airFilter.toLowerCase())));
            const cabinChecked = fr ? !!fr.cabin : (textCombined.includes('filtr kabinowy') || (carParts.cabinFilter && textCombined.includes(carParts.cabinFilter.toLowerCase())));
            const fuelChecked = fr ? !!fr.fuel : (textCombined.includes('filtr paliwa') || (carParts.fuelFilter && textCombined.includes(carParts.fuelFilter.toLowerCase())));

            if (oilChecked) result.push({ label: 'Filtr oleju', code: carParts.oilFilter || '', color: 'amber' });
            if (airChecked) result.push({ label: 'Filtr powietrza', code: carParts.airFilter || '', color: 'sky' });
            if (cabinChecked) result.push({ label: 'Filtr kabinowy', code: carParts.cabinFilter || '', color: 'emerald' });
            if (fuelChecked) result.push({ label: 'Filtr paliwa', code: carParts.fuelFilter || '', color: 'rose' });

            return result;
        }

        // Renderowanie historii serwisowej i napraw dla wybranego pojazdu
        window.renderRecords = function() {
            const car = cars.find(c => c.id === selectedCarId);
            if (!car) return;

            const filterType = document.getElementById('filter-record-type').value;
            const searchInput = document.getElementById('search-record-input');
            const clearBtn = document.getElementById('clear-record-search');
            const searchQuery = (searchInput?.value || '').toLowerCase().trim();

            if (clearBtn) {
                if (searchQuery.length > 0) clearBtn.classList.remove('hidden');
                else clearBtn.classList.add('hidden');
            }

            const listEl = document.getElementById('records-list');

            const allRecords = Array.isArray(car.records) ? [...car.records] : [];
            // Sortowanie po przebiegu malejąco (najnowsze na górze)
            allRecords.sort((a, b) => parseMileageValue(b.mileage) - parseMileageValue(a.mileage));

            // Obliczanie statystyk
            document.getElementById('stat-records-count').textContent = allRecords.length;

            const oilRecords = allRecords.filter(r => r.type === 'OIL');
            if (oilRecords.length > 0) {
                const lastOil = oilRecords[0];
                const kkm = (parseMileageValue(lastOil.mileage) / 1000).toFixed(0);
                document.getElementById('stat-last-oil').textContent = `${lastOil.date} (${kkm}kkm)`;
            } else {
                document.getElementById('stat-last-oil').textContent = 'Brak wpisów';
            }

            const totalCost = allRecords.reduce((acc, r) => acc + (parseFloat(r.cost) || 0), 0);
            document.getElementById('stat-total-cost').textContent = `${totalCost.toLocaleString('pl-PL')} PLN`;

            // Filtrowanie po kategorii oraz wyszukiwarce tekstowej
            const filtered = allRecords.filter(r => {
                if (filterType !== 'ALL' && r.type !== filterType) return false;
                if (!searchQuery) return true;

                const replacedList = getReplacedFiltersForRecord(r, car.parts || {}).map(f => `${f.label} ${f.code}`).join(' ');
                const searchable = [
                    r.title || '',
                    r.date || '',
                    String(r.mileage || ''),
                    r.rawMileageLabel || '',
                    r.partsUsed || '',
                    r.description || '',
                    replacedList
                ].join(' ').toLowerCase();

                return searchable.includes(searchQuery);
            });

            if (filtered.length === 0) {
                listEl.innerHTML = `
                    <div class="text-center py-8 px-4 bg-slate-950/40 border border-dashed border-slate-800 rounded-xl text-slate-400 text-xs">
                        ${searchQuery ? `Brak wpisów pasujących do frazy <strong>"${escapeHtml(searchQuery)}"</strong>.` : `Brak wpisów w tej kategorii. Kliknij <strong>"Dodaj wpis / naprawę"</strong>, aby zarejestrować czynność serwisową.`}
                    </div>
                `;
                return;
            }

            const carParts = car.parts || {};

            listEl.innerHTML = filtered.map(rec => {
                const kmNum = parseMileageValue(rec.mileage);
                const kkmFormatted = kmNum ? `${(kmNum / 1000).toFixed(kmNum % 1000 === 0 ? 0 : 1)}kkm` : rec.mileage;
                const fullKmFormatted = kmNum ? `${kmNum.toLocaleString('pl-PL')} km` : '';

                let badgeHtml = '';
                if (rec.type === 'OIL') {
                    badgeHtml = `<span class="px-2.5 py-0.5 rounded-md text-[11px] font-semibold bg-amber-500/15 text-amber-400 border border-amber-500/30 print-badge"><i class="fa-solid fa-oil-can mr-1 no-print"></i>Serwis Olejowy</span>`;
                } else if (rec.type === 'REPAIR') {
                    badgeHtml = `<span class="px-2.5 py-0.5 rounded-md text-[11px] font-semibold bg-blue-500/15 text-blue-400 border border-blue-500/30 print-badge"><i class="fa-solid fa-wrench mr-1 no-print"></i>Naprawa</span>`;
                } else {
                    badgeHtml = `<span class="px-2.5 py-0.5 rounded-md text-[11px] font-semibold bg-emerald-500/15 text-emerald-400 border border-emerald-500/30 print-badge"><i class="fa-solid fa-clipboard-check mr-1 no-print"></i>Przegląd / Inne</span>`;
                }

                const costHtml = rec.cost && parseFloat(rec.cost) > 0 
                    ? `<span class="font-mono text-xs font-bold text-emerald-400 print-text-dark bg-emerald-500/10 border border-emerald-500/20 px-2.5 py-1 rounded-lg print-badge">${parseFloat(rec.cost).toLocaleString('pl-PL')} PLN</span>` 
                    : '';

                // Wyświetlenie odhaczonych filtrów ze znacznikami ("ptaszkami")
                const replacedFilters = getReplacedFiltersForRecord(rec, carParts);
                const replacedFiltersHtml = replacedFilters.length > 0 ? `
                    <div class="flex flex-wrap items-center gap-1.5 pt-1">
                        <span class="text-[11px] text-slate-400 print-text-muted mr-1">Wymienione filtry (nr porównawcze):</span>
                        ${replacedFilters.map(f => `
                            <span class="inline-flex items-center gap-1 px-2 py-0.5 rounded-md text-[11px] font-medium bg-emerald-500/10 text-emerald-300 border border-emerald-500/30 print-badge">
                                <i class="fa-solid fa-check text-emerald-400 text-[10px]"></i>
                                <span>${escapeHtml(f.label)}</span>
                                ${f.code ? `<strong class="font-mono text-[10px] text-slate-300 print-text-dark">(porównawczy: ${escapeHtml(f.code)})</strong>` : ''}
                            </span>
                        `).join('')}
                    </div>
                ` : '';

                return `
                    <div class="bg-slate-950/70 hover:bg-slate-950 border border-slate-800/90 rounded-xl p-4 transition print-card">
                        <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-2 pb-2.5 border-b border-slate-800/70">
                            <div class="flex items-center flex-wrap gap-2">
                                ${badgeHtml}
                                <h4 class="font-bold text-sm sm:text-base text-white print-text-dark">${escapeHtml(rec.title)}</h4>
                            </div>

                            <div class="flex items-center flex-wrap gap-2 sm:gap-3">
                                <div class="font-mono text-xs bg-slate-900 border border-slate-800 px-2.5 py-1 rounded-lg text-slate-300 print-badge">
                                    <i class="fa-regular fa-calendar text-blue-400 mr-1.5 no-print"></i>${escapeHtml(rec.date)}
                                </div>
                                <div class="font-mono text-xs bg-slate-900 border border-slate-800 px-2.5 py-1 rounded-lg text-amber-400 font-bold print-badge" title="${fullKmFormatted}">
                                    <i class="fa-solid fa-gauge-high mr-1.5 no-print"></i>${escapeHtml(kkmFormatted)} <span class="text-[10px] text-slate-400 font-normal">(${fullKmFormatted})</span>
                                </div>
                                ${costHtml}
                                <div class="flex items-center space-x-1 no-print">
                                    <button onclick="openRecordModal('${rec.id}')" class="p-1.5 text-slate-400 hover:text-blue-400 rounded-lg hover:bg-slate-800 transition text-xs" title="Edytuj wpis">
                                        <i class="fa-solid fa-pen"></i>
                                    </button>
                                    <button onclick="confirmDeleteRecord('${rec.id}')" class="p-1.5 text-slate-400 hover:text-red-400 rounded-lg hover:bg-slate-800 transition text-xs" title="Usuń wpis">
                                        <i class="fa-solid fa-trash"></i>
                                    </button>
                                </div>
                            </div>
                        </div>

                        <div class="pt-2.5 space-y-2 text-xs">
                            ${replacedFiltersHtml}
                            ${rec.partsUsed ? `
                                <div class="bg-slate-900/70 border border-slate-800/80 rounded-lg p-2.5 text-slate-300 print-text-dark print-badge">
                                    <div class="text-[10px] uppercase tracking-wider text-slate-400 print-text-muted font-semibold mb-1"><i class="fa-solid fa-boxes-stacked text-blue-400 mr-1 no-print"></i>Użyte części / zamontowane zamienniki:</div>
                                    <div class="font-mono text-xs text-slate-200 print-text-dark whitespace-pre-line leading-relaxed">${escapeHtml(rec.partsUsed)}</div>
                                </div>
                            ` : ''}
                            ${rec.description ? `
                                <p class="text-slate-400 print-text-muted leading-relaxed whitespace-pre-line">${escapeHtml(rec.description)}</p>
                            ` : ''}
                        </div>
                    </div>
                `;
            }).join('');
        };

        // Kopiowanie numeru VIN oraz numerów części do schowka
        function copyTextToClipboard(text, label) {
            const tempInput = document.createElement('textarea');
            tempInput.value = text;
            document.body.appendChild(tempInput);
            tempInput.select();
            document.execCommand('copy');
            document.body.removeChild(tempInput);
            showToast(`Skopiowano ${label}: ${text}`);
        }

        window.copyVin = function() {
            const car = cars.find(c => c.id === selectedCarId);
            if (car && car.vin) {
                copyTextToClipboard(car.vin, 'VIN');
            }
        };

        window.copyPartNumber = function(elementId, partName) {
            const text = document.getElementById(elementId)?.textContent?.trim();
            if (text && text !== 'Brak') {
                copyTextToClipboard(text, partName);
            }
        };

        // Drukowanie historii pojedynczego samochodu
        window.printVehicleHistory = function() {
            window.print();
        };

        // Generowanie i pobieranie pliku PDF z historią wybranego samochodu
        window.downloadVehiclePdf = async function() {
            const car = cars.find(c => c.id === selectedCarId);
            if (!car) return;

            const btn = document.getElementById('btn-download-pdf');
            const originalHtml = btn ? btn.innerHTML : '';
            if (btn) {
                btn.disabled = true;
                btn.innerHTML = `<i class="fa-solid fa-spinner fa-spin"></i><span>Generowanie PDF...</span>`;
            }

            try {
                let maxMileage = parseMileageValue(car.mileage);
                const allRecords = Array.isArray(car.records) ? [...car.records] : [];
                allRecords.sort((a, b) => parseMileageValue(b.mileage) - parseMileageValue(a.mileage));
                allRecords.forEach(r => {
                    const rm = parseMileageValue(r.mileage);
                    if (rm > maxMileage) maxMileage = rm;
                });

                const parts = car.parts || {};
                const totalCost = allRecords.reduce((acc, r) => acc + (parseFloat(r.cost) || 0), 0);

                // Budujemy czysty, elegancki kontener dokumentu PDF
                const pdfWrapper = document.createElement('div');
                pdfWrapper.style.padding = '24px';
                pdfWrapper.style.fontFamily = 'Inter, Arial, sans-serif';
                pdfWrapper.style.color = '#0f172a';
                pdfWrapper.style.backgroundColor = '#ffffff';
                pdfWrapper.style.width = '750px';

                const recordsHtml = allRecords.length === 0
                    ? `<div style="padding: 16px; border: 1px dashed #cbd5e1; border-radius: 8px; text-align: center; color: #64748b; font-size: 12px;">Brak zarejestrowanych wpisów serwisowych.</div>`
                    : allRecords.map(rec => {
                        const kmNum = parseMileageValue(rec.mileage);
                        const kkmFormatted = kmNum ? `${(kmNum / 1000).toFixed(kmNum % 1000 === 0 ? 0 : 1)}kkm (${kmNum.toLocaleString('pl-PL')} km)` : rec.mileage;
                        const typeLabel = rec.type === 'OIL' ? 'Serwis Olejowy' : (rec.type === 'REPAIR' ? 'Naprawa' : 'Przegląd / Inne');
                        const replacedFilters = getReplacedFiltersForRecord(rec, parts);

                        return `
                            <div style="border: 1px solid #cbd5e1; border-radius: 10px; padding: 12px 14px; margin-bottom: 10px; page-break-inside: avoid; background: #ffffff;">
                                <div style="display: flex; justify-content: space-between; align-items: center; border-bottom: 1px solid #e2e8f0; padding-bottom: 6px; margin-bottom: 8px;">
                                    <div>
                                        <span style="font-size: 10px; font-weight: 700; text-transform: uppercase; background: #f1f5f9; color: #334155; padding: 2px 6px; border-radius: 4px; border: 1px solid #cbd5e1; margin-right: 6px;">${escapeHtml(typeLabel)}</span>
                                        <strong style="font-size: 14px; color: #0f172a;">${escapeHtml(rec.title)}</strong>
                                    </div>
                                    <div style="font-size: 11px; font-family: monospace; color: #334155;">
                                        Data: <strong>${escapeHtml(rec.date)}</strong> | Przebieg: <strong>${escapeHtml(kkmFormatted)}</strong>
                                        ${rec.cost && parseFloat(rec.cost) > 0 ? ` | Koszt: <strong>${parseFloat(rec.cost).toLocaleString('pl-PL')} PLN</strong>` : ''}
                                    </div>
                                </div>
                                ${replacedFilters.length > 0 ? `
                                    <div style="font-size: 11px; margin-bottom: 6px; color: #334155;">
                                        <strong>Wymienione filtry:</strong>
                                        ${replacedFilters.map(f => `<span style="display: inline-block; background: #ecfdf5; color: #065f46; border: 1px solid #a7f3d0; padding: 1px 6px; border-radius: 4px; margin-left: 4px;">✓ ${escapeHtml(f.label)} ${f.code ? `(nr por.: ${escapeHtml(f.code)})` : ''}</span>`).join(' ')}
                                    </div>
                                ` : ''}
                                ${rec.partsUsed ? `
                                    <div style="font-size: 11px; background: #f8fafc; border: 1px solid #e2e8f0; padding: 6px 8px; border-radius: 6px; margin-bottom: 6px; white-space: pre-line;">
                                        <strong style="color: #475569;">Użyte części / zamienniki:</strong> ${escapeHtml(rec.partsUsed)}
                                    </div>
                                ` : ''}
                                ${rec.description ? `
                                    <div style="font-size: 11px; color: #475569; white-space: pre-line;">${escapeHtml(rec.description)}</div>
                                ` : ''}
                            </div>
                        `;
                    }).join('');

                pdfWrapper.innerHTML = `
                    <div style="border-bottom: 2px solid #0f172a; padding-bottom: 12px; margin-bottom: 16px; display: flex; justify-content: space-between; align-items: flex-start;">
                        <div>
                            <div style="font-size: 10px; text-transform: uppercase; letter-spacing: 1px; color: #64748b; font-weight: 700;">Cyfrowa Książka Serwisowa AutoLog</div>
                            <h1 style="font-size: 24px; font-weight: 800; margin: 4px 0; color: #0f172a;">${escapeHtml(car.name)}</h1>
                            <div style="font-size: 12px; font-family: monospace; color: #334155;">
                                VIN: <strong>${escapeHtml(car.vin || '-')}</strong> | Właściciel: <strong>${escapeHtml(car.owner || '-')}</strong> | Przebieg: <strong>${formatMileage(maxMileage)}</strong>
                                ${car.plate ? ` | Oznaczenie: <strong>${escapeHtml(car.plate)}</strong>` : ''}
                            </div>
                        </div>
                        <div style="text-align: right; font-size: 10px; color: #64748b;">
                            <div>Data wygenerowania PDF:</div>
                            <div style="font-family: monospace; font-weight: 700; color: #0f172a;">${new Date().toLocaleDateString('pl-PL')}</div>
                        </div>
                    </div>

                    <div style="margin-bottom: 18px; border: 1px solid #cbd5e1; border-radius: 10px; padding: 12px; background: #f8fafc;">
                        <div style="font-size: 11px; font-weight: 700; text-transform: uppercase; color: #334155; margin-bottom: 8px;">
                            Numery Porównawcze Filtrów (OEM / Referencyjne do doboru zamienników)
                        </div>
                        <div style="display: grid; grid-template-columns: repeat(4, 1fr); gap: 8px; font-size: 11px;">
                            <div style="background: #ffffff; border: 1px solid #cbd5e1; padding: 6px 8px; border-radius: 6px;">
                                <div style="color: #64748b; font-size: 10px;">Filtr oleju (porównawczy)</div>
                                <div style="font-family: monospace; font-weight: 700; font-size: 13px;">${escapeHtml(parts.oilFilter || 'Brak')}</div>
                            </div>
                            <div style="background: #ffffff; border: 1px solid #cbd5e1; padding: 6px 8px; border-radius: 6px;">
                                <div style="color: #64748b; font-size: 10px;">Filtr powietrza (porównawczy)</div>
                                <div style="font-family: monospace; font-weight: 700; font-size: 13px;">${escapeHtml(parts.airFilter || 'Brak')}</div>
                            </div>
                            <div style="background: #ffffff; border: 1px solid #cbd5e1; padding: 6px 8px; border-radius: 6px;">
                                <div style="color: #64748b; font-size: 10px;">Filtr kabinowy (porównawczy)</div>
                                <div style="font-family: monospace; font-weight: 700; font-size: 13px;">${escapeHtml(parts.cabinFilter || 'Brak')}</div>
                            </div>
                            <div style="background: #ffffff; border: 1px solid #cbd5e1; padding: 6px 8px; border-radius: 6px;">
                                <div style="color: #64748b; font-size: 10px;">Filtr paliwa (porównawczy)</div>
                                <div style="font-family: monospace; font-weight: 700; font-size: 13px;">${escapeHtml(parts.fuelFilter || 'Brak')}</div>
                            </div>
                        </div>
                        ${parts.extra ? `<div style="margin-top: 8px; font-size: 11px; color: #334155;"><strong>Inne części / specyfikacja:</strong> ${escapeHtml(parts.extra)}</div>` : ''}
                    </div>

                    <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 10px;">
                        <h2 style="font-size: 15px; font-weight: 700; color: #0f172a; margin: 0;">Historia Serwisowa i Naprawy (${allRecords.length} wpisów)</h2>
                        <div style="font-size: 12px; font-family: monospace; color: #334155;">Łączne wpisane koszty: <strong>${totalCost.toLocaleString('pl-PL')} PLN</strong></div>
                    </div>

                    <div>${recordsHtml}</div>
                `;

                const safeFileName = `Ksiazka_Serwisowa_${(car.name || 'Pojazd').replace(/[^a-zA-Z0-9_-]/g, '_')}_${car.vin || ''}.pdf`;

                const opt = {
                    margin: [10, 10, 10, 10],
                    filename: safeFileName,
                    image: { type: 'jpeg', quality: 0.98 },
                    html2canvas: { scale: 2, useCORS: true },
                    jsPDF: { unit: 'mm', format: 'a4', orientation: 'portrait' }
                };

                await html2pdf().set(opt).from(pdfWrapper).save();
                showToast('Pobrano plik PDF z historią pojazdu!');
            } catch (err) {
                console.error('Błąd generowania PDF:', err);
                showToast('Nie udało się wygenerować PDF – otwieram okno druku', true);
                window.print();
            } finally {
                if (btn) {
                    btn.disabled = false;
                    btn.innerHTML = originalHtml;
                }
            }
        };

        // Obsługa modala Dodawania / Edycji samochodu
        window.openCarModal = function(editMode = false) {
            isEditingCar = editMode;
            const modal = document.getElementById('car-modal');
            const titleEl = document.getElementById('car-modal-title');

            if (editMode) {
                const car = cars.find(c => c.id === selectedCarId);
                if (!car) return;
                titleEl.innerHTML = `<i class="fa-solid fa-pen-to-square text-blue-400"></i> Edytuj Pojazd i Filtry`;
                document.getElementById('input-car-name').value = car.name || '';
                document.getElementById('input-car-vin').value = car.vin || '';
                document.getElementById('input-car-owner').value = car.owner || '';
                document.getElementById('input-car-mileage').value = parseMileageValue(car.mileage) || '';
                document.getElementById('input-car-oil-interval').value = parseInt(car.oilIntervalKm, 10) || 15000;
                document.getElementById('input-car-plate').value = car.plate || '';
                document.getElementById('input-part-oil').value = car.parts?.oilFilter || '';
                document.getElementById('input-part-air').value = car.parts?.airFilter || '';
                document.getElementById('input-part-cabin').value = car.parts?.cabinFilter || '';
                document.getElementById('input-part-fuel').value = car.parts?.fuelFilter || '';
                document.getElementById('input-part-extra').value = car.parts?.extra || '';
            } else {
                titleEl.innerHTML = `<i class="fa-solid fa-car text-blue-400"></i> Dodaj Nowy Pojazd`;
                document.getElementById('car-form').reset();
                document.getElementById('input-car-oil-interval').value = 15000;
            }

            modal.classList.remove('hidden');
        };

        window.closeCarModal = function() {
            document.getElementById('car-modal').classList.add('hidden');
        };

        window.saveCar = async function(e) {
            e.preventDefault();
            const name = document.getElementById('input-car-name').value.trim();
            const vin = document.getElementById('input-car-vin').value.trim().toUpperCase();
            const owner = document.getElementById('input-car-owner').value.trim();
            const mileage = parseMileageValue(document.getElementById('input-car-mileage').value);
            const oilIntervalKm = parseInt(document.getElementById('input-car-oil-interval').value, 10) || 15000;
            const plate = document.getElementById('input-car-plate').value.trim();

            const parts = {
                oilFilter: document.getElementById('input-part-oil').value.trim(),
                airFilter: document.getElementById('input-part-air').value.trim(),
                cabinFilter: document.getElementById('input-part-cabin').value.trim(),
                fuelFilter: document.getElementById('input-part-fuel').value.trim(),
                extra: document.getElementById('input-part-extra').value.trim()
            };

            if (isEditingCar && selectedCarId) {
                const existing = cars.find(c => c.id === selectedCarId);
                if (!existing) return;
                const updatedCar = {
                    ...existing,
                    name,
                    vin,
                    owner,
                    mileage: mileage || existing.mileage,
                    oilIntervalKm,
                    plate,
                    parts,
                    updatedAt: Date.now()
                };
                await persistCarToFirestore(updatedCar);
                showToast('Zaktualizowano kartę pojazdu');
            } else {
                const newCar = {
                    id: 'car-' + Date.now(),
                    name,
                    vin,
                    owner,
                    mileage: mileage || 0,
                    oilIntervalKm,
                    plate,
                    parts,
                    todos: [],
                    records: [],
                    updatedAt: Date.now()
                };
                selectedCarId = newCar.id;
                if (!currentUser || !db) {
                    cars.unshift(newCar);
                }
                await persistCarToFirestore(newCar);
                showToast('Dodano nowy pojazd do garażu');
            }

            closeCarModal();
        };

        // Obsługa modala Dodawania / Edycji wpisu serwisowego lub naprawy
        window.toggleAllFilters = function(state = true) {
            document.getElementById('chk-filter-oil').checked = state;
            document.getElementById('chk-filter-air').checked = state;
            document.getElementById('chk-filter-cabin').checked = state;
            document.getElementById('chk-filter-fuel').checked = state;
            syncFilterCheckboxesToParts();
        };

        window.syncFilterCheckboxesToParts = function() {
            // Funkcja może opcjonalnie aktualizować podpowiedź lub zostawić pole na dodatkowe części
        };

        window.prefillRecordTitle = function(type) {
            const titleInput = document.getElementById('input-record-title');

            if (type === 'OIL') {
                if (!titleInput.value || titleInput.value === 'Naprawa mechaniczna' || titleInput.value === 'Przegląd techniczny / serwisowy') {
                    titleInput.value = 'Wymiana oleju';
                }
                if (!editingRecordId) {
                    document.getElementById('chk-filter-oil').checked = true;
                }
            } else if (type === 'REPAIR') {
                if (titleInput.value === 'Wymiana oleju' || titleInput.value === 'Wymiana oleju i filtrów' || titleInput.value === 'Przegląd techniczny / serwisowy') {
                    titleInput.value = '';
                }
                if (!editingRecordId) {
                    toggleAllFilters(false);
                }
            } else if (type === 'INSPECTION') {
                if (titleInput.value === 'Wymiana oleju' || titleInput.value === 'Wymiana oleju i filtrów' || !titleInput.value) {
                    titleInput.value = 'Przegląd techniczny / serwisowy';
                }
            }
        };

        window.openRecordModal = function(recordId = null) {
            editingRecordId = recordId;
            const car = cars.find(c => c.id === selectedCarId);
            if (!car) return;

            const modal = document.getElementById('record-modal');
            const titleEl = document.getElementById('record-modal-title');
            document.getElementById('record-form').reset();

            // Ustawienie numerów katalogowych filtrów przy checkboxach dla wybranego auta
            const parts = car.parts || {};
            document.getElementById('chk-label-oil').textContent = parts.oilFilter ? `Nr por.: ${parts.oilFilter}` : 'Brak nr w karcie';
            document.getElementById('chk-label-air').textContent = parts.airFilter ? `Nr por.: ${parts.airFilter}` : 'Brak nr w karcie';
            document.getElementById('chk-label-cabin').textContent = parts.cabinFilter ? `Nr por.: ${parts.cabinFilter}` : 'Brak nr w karcie';
            document.getElementById('chk-label-fuel').textContent = parts.fuelFilter ? `Nr por.: ${parts.fuelFilter}` : 'Brak nr w karcie';

            if (recordId) {
                const rec = (car.records || []).find(r => r.id === recordId);
                if (!rec) return;
                titleEl.innerHTML = `<i class="fa-solid fa-pen text-blue-400"></i> Edytuj Wpis Serwisowy`;
                const radio = document.querySelector(`input[name="record-type"][value="${rec.type || 'REPAIR'}"]`);
                if (radio) radio.checked = true;
                document.getElementById('input-record-title').value = rec.title || '';
                document.getElementById('input-record-date').value = rec.date || '';
                document.getElementById('input-record-mileage').value = rec.mileage || '';
                document.getElementById('input-record-cost').value = rec.cost || '';
                document.getElementById('input-record-parts').value = rec.partsUsed || '';
                document.getElementById('input-record-desc').value = rec.description || '';

                // Odtworzenie zaznaczonych ptaszków filtrów
                const fr = rec.filtersReplaced;
                if (fr) {
                    document.getElementById('chk-filter-oil').checked = !!fr.oil;
                    document.getElementById('chk-filter-air').checked = !!fr.air;
                    document.getElementById('chk-filter-cabin').checked = !!fr.cabin;
                    document.getElementById('chk-filter-fuel').checked = !!fr.fuel;
                } else {
                    const detected = getReplacedFiltersForRecord(rec, parts);
                    document.getElementById('chk-filter-oil').checked = detected.some(d => d.label === 'Filtr oleju');
                    document.getElementById('chk-filter-air').checked = detected.some(d => d.label === 'Filtr powietrza');
                    document.getElementById('chk-filter-cabin').checked = detected.some(d => d.label === 'Filtr kabinowy');
                    document.getElementById('chk-filter-fuel').checked = detected.some(d => d.label === 'Filtr paliwa');
                }
            } else {
                titleEl.innerHTML = `<i class="fa-solid fa-screwdriver-wrench text-emerald-400"></i> Dodaj Wpis Serwisowy / Naprawę`;
                const now = new Date();
                const month = String(now.getMonth() + 1).padStart(2, '0');
                const year = now.getFullYear();
                document.getElementById('input-record-date').value = `${month}.${year}`;
                document.getElementById('input-record-mileage').value = car.mileage || '';
                toggleAllFilters(false);
                document.getElementById('chk-filter-oil').checked = true;
                prefillRecordTitle('OIL');
            }

            modal.classList.remove('hidden');
        };

        window.closeRecordModal = function() {
            document.getElementById('record-modal').classList.add('hidden');
            editingRecordId = null;
        };

        window.saveRecord = async function(e) {
            e.preventDefault();
            const car = cars.find(c => c.id === selectedCarId);
            if (!car) return;

            const type = document.querySelector('input[name="record-type"]:checked')?.value || 'REPAIR';
            const title = document.getElementById('input-record-title').value.trim();
            const date = document.getElementById('input-record-date').value.trim();
            const rawMileage = document.getElementById('input-record-mileage').value.trim();
            const parsedMileage = parseMileageValue(rawMileage);
            const cost = parseFloat(document.getElementById('input-record-cost').value) || 0;
            const partsUsed = document.getElementById('input-record-parts').value.trim();
            const description = document.getElementById('input-record-desc').value.trim();

            const filtersReplaced = {
                oil: document.getElementById('chk-filter-oil').checked,
                air: document.getElementById('chk-filter-air').checked,
                cabin: document.getElementById('chk-filter-cabin').checked,
                fuel: document.getElementById('chk-filter-fuel').checked
            };

            const updatedRecords = Array.isArray(car.records) ? [...car.records] : [];

            if (editingRecordId) {
                const idx = updatedRecords.findIndex(r => r.id === editingRecordId);
                if (idx >= 0) {
                    updatedRecords[idx] = {
                        ...updatedRecords[idx],
                        type,
                        title,
                        date,
                        mileage: parsedMileage,
                        rawMileageLabel: rawMileage,
                        cost,
                        filtersReplaced,
                        partsUsed,
                        description
                    };
                }
            } else {
                updatedRecords.push({
                    id: 'rec-' + Date.now(),
                    type,
                    title,
                    date,
                    mileage: parsedMileage,
                    rawMileageLabel: rawMileage,
                    cost,
                    filtersReplaced,
                    partsUsed,
                    description
                });
            }

            // Aktualizacja przebiegu głównego auta jeśli nowy wpis ma wyższy przebieg
            const newCarMileage = Math.max(parseMileageValue(car.mileage), parsedMileage);

            const updatedCar = {
                ...car,
                mileage: newCarMileage,
                records: updatedRecords,
                updatedAt: Date.now()
            };

            if (!currentUser || !db) {
                const carIdx = cars.findIndex(c => c.id === car.id);
                if (carIdx >= 0) cars[carIdx] = updatedCar;
            }

            await persistCarToFirestore(updatedCar);
            closeRecordModal();
            showToast(editingRecordId ? 'Zaktualizowano wpis serwisowy' : 'Dodano nowy wpis do książki serwisowej');
        };

        // Obsługa usuwania auta lub pojedynczego wpisu
        window.confirmDeleteCar = function() {
            const car = cars.find(c => c.id === selectedCarId);
            if (!car) return;

            document.getElementById('confirm-title').textContent = `Usunąć pojazd ${car.name}?`;
            document.getElementById('confirm-desc').textContent = 'Wszystkie przypisane wpisy serwisowe oraz numery części tego pojazdu zostaną bezpowrotnie usunięte.';
            const btn = document.getElementById('confirm-action-btn');
            btn.onclick = async () => {
                if (currentUser && db) {
                    const carDocRef = doc(db, 'artifacts', appId, 'users', currentUser.uid, 'cars', car.id);
                    await deleteDoc(carDocRef);
                } else {
                    cars = cars.filter(c => c.id !== car.id);
                    selectedCarId = cars[0]?.id || null;
                    renderAll();
                }
                closeConfirmModal();
                showToast('Pojazd został usunięty');
            };
            document.getElementById('confirm-modal').classList.remove('hidden');
        };

        window.confirmDeleteRecord = function(recordId) {
            const car = cars.find(c => c.id === selectedCarId);
            if (!car) return;

            document.getElementById('confirm-title').textContent = 'Usunąć ten wpis serwisowy?';
            document.getElementById('confirm-desc').textContent = 'Wpis zostanie trwale usunięty z historii tego samochodu.';
            const btn = document.getElementById('confirm-action-btn');
            btn.onclick = async () => {
                const updatedRecords = (car.records || []).filter(r => r.id !== recordId);
                const updatedCar = { ...car, records: updatedRecords, updatedAt: Date.now() };
                if (!currentUser || !db) {
                    const idx = cars.findIndex(c => c.id === car.id);
                    if (idx >= 0) cars[idx] = updatedCar;
                }
                await persistCarToFirestore(updatedCar);
                closeConfirmModal();
                showToast('Wpis został usunięty');
            };
            document.getElementById('confirm-modal').classList.remove('hidden');
        };

        window.closeConfirmModal = function() {
            document.getElementById('confirm-modal').classList.add('hidden');
        };

        // Zabezpieczenie przed XSS przy generowaniu HTML
        function escapeHtml(str) {
            if (!str) return '';
            return String(str)
                .replace(/&/g, '&amp;')
                .replace(/</g, '&lt;')
                .replace(/>/g, '&gt;')
                .replace(/"/g, '&quot;')
                .replace(/'/g, '&#039;');
        }

        // Start aplikacji po załadowaniu okna
        window.onload = function() {
            initFirebase();
        };
    </script>
</body>
</html>
