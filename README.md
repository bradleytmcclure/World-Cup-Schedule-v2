<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>FIFA World Cup 2026 - Interactive Match Schedule</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://unpkg.com/lucide@latest"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700;800&display=swap');
        
        :root {
            --color-green: #3CAC3B;
            --color-blue: #2A398D;
            --color-red: #E61D25;
            --color-light-gray: #D1D4D1;
            --color-dark-slate: #474A4A;
        }

        body {
            font-family: 'Plus Jakarta Sans', sans-serif;
            background: 
                radial-gradient(circle at 10% 20%, rgba(42, 57, 141, 0.45) 0%, transparent 45%),
                radial-gradient(circle at 90% 80%, rgba(230, 29, 37, 0.25) 0%, transparent 50%),
                radial-gradient(circle at 50% 50%, rgba(60, 172, 59, 0.08) 0%, transparent 60%),
                #1b1c1c; /* Rich dark slate base */
            background-attachment: fixed;
        }

        .custom-scrollbar::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        .custom-scrollbar::-webkit-scrollbar-track {
            background: rgba(71, 74, 74, 0.3);
            border-radius: 3px;
        }
        .custom-scrollbar::-webkit-scrollbar-thumb {
            background: rgba(60, 172, 59, 0.3);
            border-radius: 3px;
        }
        .custom-scrollbar::-webkit-scrollbar-thumb:hover {
            background: rgba(60, 172, 59, 0.6);
        }

        .card-glow:hover {
            box-shadow: 0 0 25px rgba(60, 172, 59, 0.2);
            transform: translateY(-2px);
        }
    </style>
</head>
<body class="text-[#D1D4D1] min-h-screen flex flex-col selection:bg-[#3CAC3B] selection:text-[#1b1c1c]">

    <!-- Header Section -->
    <header class="border-b border-[#474A4A]/50 bg-[#474A4A]/20 backdrop-blur-md sticky top-0 z-50 px-4 lg:px-8 py-4">
        <div class="max-w-7xl mx-auto flex flex-col lg:flex-row items-center justify-between gap-4">
            <div class="flex items-center gap-3">
                <div class="bg-gradient-to-tr from-[#3CAC3B] to-[#54c953] p-2.5 rounded-xl shadow-lg shadow-[#3CAC3B]/20">
                    <i data-lucide="trophy" class="w-6 h-6 text-[#1b1c1c] stroke-[2.5]"></i>
                </div>
                <div>
                    <div class="flex items-center gap-2">
                        <h1 class="text-xl font-extrabold tracking-tight text-white">
                            FIFA WORLD CUP 2026
                        </h1>
                        <span class="bg-[#E61D25]/15 text-[#E61D25] text-[10px] font-bold px-2 py-0.5 rounded-full border border-[#E61D25]/30 flex items-center gap-1 animate-pulse">
                            <span class="w-1.5 h-1.5 bg-[#E61D25] rounded-full"></span> LIVE CALENDAR
                        </span>
                    </div>
                    <p class="text-xs font-semibold text-[#3CAC3B] tracking-wider uppercase">Interactive Match Center & Scheduler</p>
                </div>
            </div>

            <div class="flex flex-wrap bg-[#474A4A]/40 p-1 rounded-xl border border-[#474A4A]/60 w-full lg:w-auto gap-1">
                <button onclick="switchView('groups')" id="tab-groups" class="flex-1 lg:flex-none flex items-center justify-center gap-2 px-3.5 py-2 rounded-lg text-xs font-semibold transition-all duration-200 bg-gradient-to-r from-[#3CAC3B] to-[#54c953] text-[#1b1c1c] shadow-md">
                    <i data-lucide="layers" class="w-3.5 h-3.5"></i> Groups
                </button>
                <button onclick="switchView('teams')" id="tab-teams" class="flex-1 lg:flex-none flex items-center justify-center gap-2 px-3.5 py-2 rounded-lg text-xs font-semibold transition-all duration-200 text-[#D1D4D1]/80 hover:text-white">
                    <i data-lucide="flag" class="w-3.5 h-3.5"></i> Teams
                </button>
                <button onclick="switchView('calendar')" id="tab-calendar" class="flex-1 lg:flex-none flex items-center justify-center gap-2 px-3.5 py-2 rounded-lg text-xs font-semibold transition-all duration-200 text-[#D1D4D1]/80 hover:text-white">
                    <i data-lucide="calendar" class="w-3.5 h-3.5"></i> Timeline
                </button>
                <button onclick="switchView('favorites')" id="tab-favorites" class="flex-1 lg:flex-none flex items-center justify-center gap-2 px-3.5 py-2 rounded-lg text-xs font-semibold transition-all duration-200 text-[#D1D4D1]/80 hover:text-white">
                    <i data-lucide="star" class="w-3.5 h-3.5 text-[#E61D25]"></i> My Schedule (<span id="fav-count">0</span>)
                </button>
            </div>
        </div>
    </header>

    <!-- Main Container -->
    <main class="flex-1 max-w-7xl w-full mx-auto p-4 lg:p-8 grid grid-cols-1 lg:grid-cols-3 gap-8">
        
        <!-- Left Column: Filters, Timezone, and Sidebar Navigation Panels -->
        <section class="lg:col-span-1 space-y-6 flex flex-col justify-between h-full">
            <div class="space-y-6">
                <!-- Opening Match Countdown Widget -->
                <div class="bg-[#474A4A]/25 border border-[#3CAC3B]/30 rounded-2xl p-5 shadow-xl backdrop-blur-sm">
                    <div class="flex items-center gap-2 text-[#3CAC3B] mb-2">
                        <i data-lucide="timer" class="w-4 h-4 animate-spin-slow"></i>
                        <h3 class="text-xs font-bold uppercase tracking-wider">Opening Match Countdown</h3>
                    </div>
                    <div class="grid grid-cols-4 gap-2 text-center" id="countdown-timer">
                        <div class="bg-[#474A4A]/40 p-2 rounded-xl border border-[#474A4A]/60">
                            <span id="days" class="text-lg font-black text-white">00</span>
                            <p class="text-[9px] text-[#D1D4D1]/60 uppercase">Days</p>
                        </div>
                        <div class="bg-[#474A4A]/40 p-2 rounded-xl border border-[#474A4A]/60">
                            <span id="hours" class="text-lg font-black text-white">00</span>
                            <p class="text-[9px] text-[#D1D4D1]/60 uppercase">Hours</p>
                        </div>
                        <div class="bg-[#474A4A]/40 p-2 rounded-xl border border-[#474A4A]/60">
                            <span id="mins" class="text-lg font-black text-white">00</span>
                            <p class="text-[9px] text-[#D1D4D1]/60 uppercase">Mins</p>
                        </div>
                        <div class="bg-[#474A4A]/40 p-2 rounded-xl border border-[#3CAC3B]/30">
                            <span id="secs" class="text-lg font-black text-[#3CAC3B]">00</span>
                            <p class="text-[9px] text-[#D1D4D1]/60 uppercase">Secs</p>
                        </div>
                    </div>
                    <div class="text-[11px] text-[#D1D4D1]/90 text-center mt-2.5">
                        ⚽ <strong>Mexico vs. South Africa</strong> at Mexico City Stadium
                    </div>
                </div>

                <!-- Active Filters Indicator & Search -->
                <div class="bg-[#474A4A]/15 border border-[#474A4A]/50 rounded-2xl p-5 shadow-xl backdrop-blur-sm">
                    <div class="flex items-center justify-between mb-2">
                        <h2 class="text-sm font-bold uppercase tracking-wider text-[#D1D4D1]/75 flex items-center gap-2">
                            <i data-lucide="sliders-horizontal" class="w-4 h-4 text-[#3CAC3B]"></i> Composite Filters
                        </h2>
                        <button onclick="resetFilters()" id="clear-btn" class="text-xs font-semibold text-[#D1D4D1]/70 hover:text-[#E61D25] transition flex items-center gap-1 opacity-0 pointer-events-none">
                            Reset <i data-lucide="rotate-ccw" class="w-3 h-3"></i>
                        </button>
                    </div>
                    <div id="filter-status" class="text-base font-bold text-white mb-2">
                        Showing All Matches
                    </div>
                    <!-- Container for active filter chips -->
                    <div id="filter-chips-container" class="mb-4"></div>
                    
                    <p id="filter-desc" class="text-xs text-[#D1D4D1]/60 mt-1">
                        Select multiple groups, teams, or timeline nodes in the sidebar explorer below to mix and match active layers.
                    </p>
                    
                    <div class="mt-4 relative">
                        <i data-lucide="search" class="absolute left-3.5 top-1/2 -translate-y-1/2 w-4 h-4 text-[#D1D4D1]/50"></i>
                        <input type="text" id="global-search" oninput="handleSearch(this.value)" placeholder="Search teams, stadiums, or cities..." class="w-full bg-[#1b1c1c]/90 border border-[#474A4A]/60 rounded-xl pl-10 pr-4 py-2.5 text-sm text-[#D1D4D1] placeholder:text-[#D1D4D1]/40 focus:outline-none focus:border-[#3CAC3B]/50 focus:ring-1 focus:ring-[#3CAC3B]/30 transition">
                    </div>
                </div>

                <!-- Timezone Selector Widget -->
                <div class="bg-[#474A4A]/15 border border-[#474A4A]/50 rounded-2xl p-5 shadow-xl backdrop-blur-sm">
                    <div class="flex items-center gap-2 text-[#3CAC3B] mb-3">
                        <i data-lucide="globe" class="w-4 h-4 text-[#3CAC3B]"></i>
                        <h3 class="text-xs font-bold uppercase tracking-wider">Kickoff Timezone Conversion</h3>
                    </div>
                    <div class="relative">
                        <select id="timezone-select" onchange="handleTimezoneChange(this.value)" class="w-full bg-[#1b1c1c] border border-[#474A4A]/60 rounded-xl px-4 py-2.5 text-sm font-semibold text-[#D1D4D1] focus:outline-none focus:border-[#3CAC3B]/50 focus:ring-1 focus:ring-[#3CAC3B]/30 transition cursor-pointer">
                            <option value="venue" selected>🏟️ Venue Local Time</option>
                            <option value="America/New_York">🗽 Eastern Time (EDT)</option>
                            <option value="America/Chicago">🤠 Central Time (CDT)</option>
                            <option value="America/Denver">🏔️ Mountain Time (MDT)</option>
                            <option value="America/Los_Angeles">🌲 Pacific Time (PDT)</option>
                            <option value="UTC">🌐 Coordinated Universal Time (UTC)</option>
                            <option id="user-tz-option" value="user">📱 My Local Time</option>
                        </select>
                    </div>
                    <p class="text-[11px] text-[#D1D4D1]/50 mt-2">
                        Dates and kickoff times convert dynamically based on stadium geographic offsets.
                    </p>
                </div>

                <!-- Dynamic Explorer Sidebar Feed -->
                <div id="view-panel-container" class="bg-[#474A4A]/10 border border-[#474A4A]/40 rounded-2xl p-5 min-h-[300px] backdrop-blur-sm">
                    <!-- Filled via JS -->
                </div>
            </div>

            <!-- Official Host Nations Poster Card -->
            <div class="bg-[#474A4A]/15 border border-[#474A4A]/50 rounded-2xl p-4 shadow-xl backdrop-blur-sm overflow-hidden flex flex-col items-center mt-6">
                <div class="flex items-center gap-2 text-[#3CAC3B] mb-3 self-start w-full">
                    <i data-lucide="image" class="w-4 h-4"></i>
                    <h3 class="text-xs font-bold uppercase tracking-wider">Official Tournament Poster</h3>
                </div>
                <div class="relative w-full rounded-xl overflow-hidden border border-[#474A4A]/60 aspect-[14.725/22.375] bg-[#1b1c1c] shadow-inner group">
                    <img src="FIFA-World-Cup-2026-Logo-Wall-Poster-14-725-x-22-375_1ef86397-20f8-4be4-aa55-9c7c98fd960f.102cfb827f1d32bd5036f07ed0420e29_2.avif" 
                         alt="FIFA World Cup 2026 Poster" 
                         class="w-full h-full object-cover group-hover:scale-105 transition-transform duration-500"
                         onerror="this.onerror=null; this.src='https://placehold.co/725x1118/1b1c1c/3CAC3B?text=FIFA+World+Cup+2026'">
                    <div class="absolute inset-0 bg-gradient-to-t from-black/80 via-transparent to-transparent opacity-0 group-hover:opacity-100 transition-opacity duration-300 flex items-end p-3">
                        <p class="text-[10px] text-white font-bold uppercase tracking-wide">CAN • MEX • USA 2026</p>
                    </div>
                </div>
            </div>
        </section>

        <!-- Right Column: Matches Feed Grid -->
        <section class="lg:col-span-2 flex flex-col space-y-4">
            <div class="flex items-center justify-between">
                <div class="flex items-center gap-2.5">
                    <h2 class="text-lg font-bold text-white">Scheduled Match Matrix</h2>
                    <span id="match-count" class="bg-[#474A4A]/30 text-[#3CAC3B] text-xs px-3 py-1 font-bold rounded-full border border-[#3CAC3B]/20 shadow-inner">0 matches</span>
                </div>
                <div class="flex items-center gap-2">
                    <span class="text-xs text-[#D1D4D1]/60 font-medium">Sort Grid:</span>
                    <select id="match-sort" onchange="handleSortChange(this.value)" class="bg-[#1b1c1c] border border-[#474A4A]/60 rounded-lg text-xs font-semibold text-[#D1D4D1] px-3 py-1.5 focus:outline-none focus:border-[#3CAC3B]">
                        <option value="chronological">Chronological Order</option>
                        <option value="stadium">By Venue Location</option>
                        <option value="group">By Group / Stage</option>
                    </select>
                </div>
            </div>

            <!-- Main Match Grid Container (Frictionless responsive heights prevent double scrollbars) -->
            <div id="match-feed-container" class="grid grid-cols-1 md:grid-cols-2 gap-4 lg:overflow-y-auto lg:max-h-[calc(100vh-220px)] pr-2 custom-scrollbar overflow-visible h-auto">
                <!-- Populated programmatically -->
            </div>
        </section>
    </main>

    <!-- Footer Status -->
    <footer class="border-t border-[#474A4A]/40 bg-[#1b1c1c]/90 py-4 text-center text-xs text-[#D1D4D1]/40 font-medium mt-auto">
        ⚽ FIFA World Cup 2026 Interactive Match Hub | Fully Responsive Dynamic Blueprint Matrix
    </footer>

    <script>
        // Compiled directly to match finalized 104-match structures
        const matchesDataset = [
            {"Date":"2026-06-11","Time":"13:00","Home Team":"Mexico","Away Team":"South Africa","Group":"Group A","Location":"Mexico City Stadium"},
            {"Date":"2026-06-11","Time":"20:00","Home Team":"South Korea","Away Team":"Czechia","Group":"Group A","Location":"Estadio Guadalajara"},
            {"Date":"2026-06-12","Time":"15:00","Home Team":"Canada","Away Team":"Bosnia and Herzegovina","Group":"Group B","Location":"Toronto Stadium"},
            {"Date":"2026-06-12","Time":"18:00","Home Team":"USA","Away Team":"Paraguay","Group":"Group D","Location":"Los Angeles Stadium"},
            {"Date":"2026-06-13","Time":"12:00","Home Team":"Qatar","Away Team":"Switzerland","Group":"Group B","Location":"San Francisco Bay Area Stadium"},
            {"Date":"2026-06-13","Time":"18:00","Home Team":"Brazil","Away Team":"Morocco","Group":"Group C","Location":"New York New Jersey Stadium"},
            {"Date":"2026-06-13","Time":"21:00","Home Team":"Haiti","Away Team":"Scotland","Group":"Group C","Location":"Boston Stadium"},
            {"Date":"2026-06-13","Time":"21:00","Home Team":"Australia","Away Team":"Türkiye","Group":"Group D","Location":"BC Place Vancouver"},
            {"Date":"2026-06-14","Time":"12:00","Home Team":"Germany","Away Team":"Curaçao","Group":"Group E","Location":"Houston Stadium"},
            {"Date":"2026-06-14","Time":"15:00","Home Team":"Netherlands","Away Team":"Japan","Group":"Group F","Location":"Dallas Stadium"},
            {"Date":"2026-06-14","Time":"19:00","Home Team":"Ivory Coast","Away Team":"Ecuador","Group":"Group E","Location":"Philadelphia Stadium"},
            {"Date":"2026-06-14","Time":"20:00","Home Team":"Sweden","Away Team":"Tunisia","Group":"Group F","Location":"Monterrey Stadium"},
            {"Date":"2026-06-15","Time":"12:00","Home Team":"Spain","Away Team":"Cape Verde","Group":"Group H","Location":"Atlanta Stadium"},
            {"Date":"2026-06-15","Time":"12:00","Home Team":"Belgium","Away Team":"Egypt","Group":"Group G","Location":"Seattle Stadium"},
            {"Date":"2026-06-15","Time":"18:00","Home Team":"Saudi Arabia","Away Team":"Uruguay","Group":"Group H","Location":"Miami Stadium"},
            {"Date":"2026-06-15","Time":"18:00","Home Team":"Iran","Away Team":"New Zealand","Group":"Group G","Location":"Los Angeles Stadium"},
            {"Date":"2026-06-16","Time":"15:00","Home Team":"France","Away Team":"Senegal","Group":"Group I","Location":"New York New Jersey Stadium"},
            {"Date":"2026-06-16","Time":"18:00","Home Team":"Iraq","Away Team":"Norway","Group":"Group I","Location":"Boston Stadium"},
            {"Date":"2026-06-16","Time":"20:00","Home Team":"Argentina","Away Team":"Algeria","Group":"Group J","Location":"Kansas City Stadium"},
            {"Date":"2026-06-16","Time":"21:00","Home Team":"Austria","Away Team":"Jordan","Group":"Group J","Location":"San Francisco Bay Area Stadium"},
            {"Date":"2026-06-17","Time":"12:00","Home Team":"Portugal","Away Team":"DR Congo","Group":"Group K","Location":"Houston Stadium"},
            {"Date":"2026-06-17","Time":"15:00","Home Team":"England","Away Team":"Croatia","Group":"Group L","Location":"Dallas Stadium"},
            {"Date":"2026-06-17","Time":"19:00","Home Team":"Ghana","Away Team":"Panama","Group":"Group L","Location":"Toronto Stadium"},
            {"Date":"2026-06-17","Time":"20:00","Home Team":"Uzbekistan","Away Team":"Colombia","Group":"Group K","Location":"Mexico City Stadium"},
            {"Date":"2026-06-18","Time":"12:00","Home Team":"Czechia","Away Team":"South Africa","Group":"Group A","Location":"Atlanta Stadium"},
            {"Date":"2026-06-18","Time":"12:00","Home Team":"Switzerland","Away Team":"Bosnia and Herzegovina","Group":"Group B","Location":"Los Angeles Stadium"},
            {"Date":"2026-06-18","Time":"15:00","Home Team":"Canada","Away Team":"Qatar","Group":"Group B","Location":"BC Place Vancouver"},
            {"Date":"2026-06-18","Time":"19:00","Home Team":"Mexico","Away Team":"South Korea","Group":"Group A","Location":"Estadio Guadalajara"},
            {"Date":"2026-06-19","Time":"12:00","Home Team":"USA","Away Team":"Australia","Group":"Group D","Location":"Seattle Stadium"},
            {"Date":"2026-06-19","Time":"18:00","Home Team":"Scotland","Away Team":"Morocco","Group":"Group C","Location":"Boston Stadium"},
            {"Date":"2026-06-19","Time":"20:00","Home Team":"Türkiye","Away Team":"Paraguay","Group":"Group D","Location":"San Francisco Bay Area Stadium"},
            {"Date":"2026-06-19","Time":"20:30","Home Team":"Brazil","Away Team":"Haiti","Group":"Group C","Location":"Philadelphia Stadium"},
            {"Date":"2026-06-20","Time":"12:00","Home Team":"Netherlands","Away Team":"Sweden","Group":"Group F","Location":"Houston Stadium"},
            {"Date":"2026-06-20","Time":"16:00","Home Team":"Germany","Away Team":"Ivory Coast","Group":"Group E","Location":"Toronto Stadium"},
            {"Date":"2026-06-20","Time":"19:00","Home Team":"Ecuador","Away Team":"Curaçao","Group":"Group E","Location":"Kansas City Stadium"},
            {"Date":"2026-06-20","Time":"22:00","Home Team":"Tunisia","Away Team":"Japan","Group":"Group F","Location":"Monterrey Stadium"},
            {"Date":"2026-06-21","Time":"12:00","Home Team":"Spain","Away Team":"Saudi Arabia","Group":"Group H","Location":"Atlanta Stadium"},
            {"Date":"2026-06-21","Time":"12:00","Home Team":"Belgium","Away Team":"Iran","Group":"Group G","Location":"Los Angeles Stadium"},
            {"Date":"2026-06-21","Time":"18:00","Home Team":"Uruguay","Away Team":"Cape Verde","Group":"Group H","Location":"Miami Stadium"},
            {"Date":"2026-06-21","Time":"18:00","Home Team":"New Zealand","Away Team":"Egypt","Group":"Group G","Location":"BC Place Vancouver"},
            {"Date":"2026-06-22","Time":"12:00","Home Team":"Argentina","Away Team":"Austria","Group":"Group J","Location":"Dallas Stadium"},
            {"Date":"2026-06-22","Time":"17:00","Home Team":"France","Away Team":"Iraq","Group":"Group I","Location":"Philadelphia Stadium"},
            {"Date":"2026-06-22","Time":"20:00","Home Team":"Norway","Away Team":"Senegal","Group":"Group I","Location":"New York New Jersey Stadium"},
            {"Date":"2026-06-22","Time":"20:00","Home Team":"Jordan","Away Team":"Algeria","Group":"Group J","Location":"San Francisco Bay Area Stadium"},
            {"Date":"2026-06-23","Time":"16:00","Home Team":"England","Away Team":"Ghana","Group":"Group L","Location":"Boston Stadium"},
            {"Date":"2026-06-23","Time":"19:00","Home Team":"Panama","Away Team":"Croatia","Group":"Group L","Location":"Toronto Stadium"},
            {"Date":"2026-06-23","Time":"13:00","Home Team":"Portugal","Away Team":"Uzbekistan","Group":"Group K","Location":"Houston Stadium"},
            {"Date":"2026-06-23","Time":"20:00","Home Team":"Colombia","Away Team":"DR Congo","Group":"Group K","Location":"Estadio Guadalajara"},
            {"Date":"2026-06-24","Time":"18:00","Home Team":"Scotland","Away Team":"Brazil","Group":"Group C","Location":"Miami Stadium"},
            {"Date":"2026-06-24","Time":"18:00","Home Team":"Morocco","Away Team":"Haiti","Group":"Group C","Location":"Atlanta Stadium"},
            {"Date":"2026-06-24","Time":"15:00","Home Team":"Switzerland","Away Team":"Canada","Group":"Group B","Location":"BC Place Vancouver"},
            {"Date":"2026-06-24","Time":"15:00","Home Team":"Bosnia and Herzegovina","Away Team":"Qatar","Group":"Group B","Location":"Seattle Stadium"},
            {"Date":"2026-06-24","Time":"21:00","Home Team":"Czechia","Away Team":"Mexico","Group":"Group A","Location":"Mexico City Stadium"},
            {"Date":"2026-06-24","Time":"21:00","Home Team":"South Africa","Away Team":"South Korea","Group":"Group A","Location":"Monterrey Stadium"},
            {"Date":"2026-06-25","Time":"16:00","Home Team":"Ecuador","Away Team":"Germany","Group":"Group E","Location":"New York New Jersey Stadium"},
            {"Date":"2026-06-25","Time":"16:00","Home Team":"Curaçao","Away Team":"Ivory Coast","Group":"Group E","Location":"Philadelphia Stadium"},
            {"Date":"2026-06-25","Time":"19:00","Home Team":"Tunisia","Away Team":"Netherlands","Group":"Group F","Location":"Kansas City Stadium"},
            {"Date":"2026-06-25","Time":"19:00","Home Team":"Japan","Away Team":"Sweden","Group":"Group F","Location":"Dallas Stadium"},
            {"Date":"2026-06-25","Time":"22:00","Home Team":"USA","Away Team":"Türkiye","Group":"Group D","Location":"Los Angeles Stadium"},
            {"Date":"2026-06-25","Time":"22:00","Home Team":"Paraguay","Away Team":"Australia","Group":"Group D","Location":"San Francisco Bay Area Stadium"},
            {"Date":"2026-06-26","Time":"15:00","Home Team":"Norway","Away Team":"France","Group":"Group I","Location":"Boston Stadium"},
            {"Date":"2026-06-26","Time":"15:00","Home Team":"Senegal","Away Team":"Iraq","Group":"Group I","Location":"Toronto Stadium"},
            {"Date":"2026-06-26","Time":"20:00","Home Team":"Uruguay","Away Team":"Spain","Group":"Group H","Location":"Estadio Guadalajara"},
            {"Date":"2026-06-26","Time":"20:00","Home Team":"Cape Verde","Away Team":"Saudi Arabia","Group":"Group H","Location":"Houston Stadium"},
            {"Date":"2026-06-26","Time":"21:00","Home Team":"New Zealand","Away Team":"Belgium","Group":"Group G","Location":"BC Place Vancouver"},
            {"Date":"2026-06-26","Time":"21:00","Home Team":"Egypt","Away Team":"Iran","Group":"Group G","Location":"Seattle Stadium"},
            {"Date":"2026-06-27","Time":"17:00","Home Team":"Panama","Away Team":"England","Group":"Group L","Location":"New York New Jersey Stadium"},
            {"Date":"2026-06-27","Time":"17:00","Home Team":"Croatia","Away Team":"Ghana","Group":"Group L","Location":"Philadelphia Stadium"},
            {"Date":"2026-06-27","Time":"19:30","Home Team":"Colombia","Away Team":"Portugal","Group":"Group K","Location":"Miami Stadium"},
            {"Date":"2026-06-27","Time":"19:30","Home Team":"DR Congo","Away Team":"Uzbekistan","Group":"Group K","Location":"Atlanta Stadium"},
            {"Date":"2026-06-27","Time":"20:00","Home Team":"Algeria","Away Team":"Austria","Group":"Group J","Location":"Kansas City Stadium"},
            {"Date":"2026-06-27","Time":"20:00","Home Team":"Jordan","Away Team":"Argentina","Group":"Group J","Location":"Dallas Stadium"},
            
            // Knockout stages
            {"Date":"2026-06-28","Time":"17:00","Home Team":"Group A Runner-Up","Away Team":"Group B Runner-Up","Group":"Round of 32","Location":"Los Angeles Stadium"},
            {"Date":"2026-06-29","Time":"17:00","Home Team":"Group C Winner","Away Team":"Group F Runner-Up","Group":"Round of 32","Location":"Houston Stadium"},
            {"Date":"2026-06-29","Time":"19:30","Home Team":"Group E Winner","Away Team":"Group A/B/C/D/F Third Place","Group":"Round of 32","Location":"Boston Stadium"},
            {"Date":"2026-06-30","Time":"20:00","Home Team":"Group F Winner","Away Team":"Group C Runner-Up","Group":"Round of 32","Location":"Monterrey Stadium"},
            {"Date":"2026-06-30","Time":"20:00","Home Team":"Group I Winner","Away Team":"Group C/D/F/G/H Third Place","Group":"Round of 32","Location":"New York New Jersey Stadium"},
            {"Date":"2026-06-30","Time":"17:00","Home Team":"Group E Runner-Up","Away Team":"Group I Runner-Up","Group":"Round of 32","Location":"Dallas Stadium"},
            {"Date":"2026-07-01","Time":"20:00","Home Team":"Group A Winner","Away Team":"Group C/D/E/F/H/I/J Third Place","Group":"Round of 32","Location":"Mexico City Stadium"},
            {"Date":"2026-07-01","Time":"12:00","Home Team":"Group L Winner","Away Team":"Group E/H/I/J/K Third Place","Group":"Round of 32","Location":"Atlanta Stadium"},
            {"Date":"2026-07-01","Time":"17:00","Home Team":"Group D Winner","Away Team":"Group B/E/F/I/J Third Place","Group":"Round of 32","Location":"San Francisco Bay Area Stadium"},
            {"Date":"2026-07-01","Time":"16:00","Home Team":"Group G Winner","Away Team":"Group A/E/H/I/J Third Place","Group":"Round of 32","Location":"Seattle Stadium"},
            {"Date":"2026-07-02","Time":"15:00","Home Team":"Group B Winner","Away Team":"Group D/E/I/J/L Third Place","Group":"Round of 32","Location":"BC Place Vancouver"},
            {"Date":"2026-07-02","Time":"15:00","Home Team":"Group H Winner","Away Team":"Group J Runner-Up","Group":"Round of 32","Location":"Los Angeles Stadium"},
            {"Date":"2026-07-02","Time":"19:00","Home Team":"Group K Runner-Up","Away Team":"Group L Runner-Up","Group":"Round of 32","Location":"Toronto Stadium"},
            {"Date":"2026-07-03","Time":"19:30","Home Team":"Group K Winner","Away Team":"Group D/E/I/J/L Third Place","Group":"Round of 32","Location":"Kansas City Stadium"},
            {"Date":"2026-07-03","Time":"15:00","Home Team":"Group J Winner","Away Team":"Group H Runner-Up","Group":"Round of 32","Location":"Miami Stadium"},
            {"Date":"2026-07-03","Time":"17:00","Home Team":"Group D Runner-Up","Away Team":"Group G Runner-Up","Group":"Round of 32","Location":"Dallas Stadium"},
            {"Date":"2026-07-04","Time":"21:00","Home Team":"Winner Match 74","Away Team":"Winner Match 77","Group":"Round of 16","Location":"Philadelphia Stadium"},
            {"Date":"2026-07-04","Time":"17:00","Home Team":"Winner Match 73","Away Team":"Winner Match 75","Group":"Round of 16","Location":"Houston Stadium"},
            {"Date":"2026-07-05","Time":"17:00","Home Team":"Winner Match 76","Away Team":"Winner Match 78","Group":"Round of 16","Location":"New York New Jersey Stadium"},
            {"Date":"2026-07-05","Time":"19:30","Home Team":"Winner Match 79","Away Team":"Winner Match 80","Group":"Round of 16","Location":"Mexico City Stadium"},
            {"Date":"2026-07-06","Time":"17:00","Home Team":"Winner Match 82","Away Team":"Winner Match 83","Group":"Round of 16","Location":"Dallas Stadium"},
            {"Date":"2026-07-06","Time":"11:00","Home Team":"Winner Match 81","Away Team":"Winner Match 84","Group":"Round of 16","Location":"Seattle Stadium"},
            {"Date":"2026-07-07","Time":"17:00","Home Team":"Winner Match 86","Away Team":"Winner Match 88","Group":"Round of 16","Location":"Atlanta Stadium"},
            {"Date":"2026-07-07","Time":"14:00","Home Team":"Winner Match 85","Away Team":"Winner Match 87","Group":"Round of 16","Location":"BC Place Vancouver"}
        ];

        // Dictionary of flags for competing nations
        const countryFlags = {
            "Mexico": "🇲🇽", "South Africa": "🇿🇦", "South Korea": "🇰🇷", "Czechia": "🇨🇿", "Czech Republic": "🇨🇿",
            "Canada": "🇨🇦", "Bosnia and Herzegovina": "🇧🇦", "USA": "🇺🇸", "Paraguay": "🇵🇾",
            "Qatar": "🇶🇦", "Switzerland": "🇨🇭", "Brazil": "🇧🇷", "Morocco": "🇲🇦",
            "Haiti": "🇭🇹", "Scotland": "🏴\u200D󠁢󠁳󠁣󠁴󠁿", "Australia": "🇦🇺", "Türkiye": "🇹🇷", "Turkey": "🇹🇷",
            "Germany": "🇩🇪", "Curaçao": "🇨🇼", "Netherlands": "🇳🇱", "Japan": "🇯🇵",
            "Ivory Coast": "🇨🇮", "Côte d'Ivoire": "🇨🇮", "Ecuador": "🇪🇨", "Sweden": "🇸🇪", "Tunisia": "🇹🇳",
            "Argentina": "🇦🇷", "Angola": "🇦🇴", "Spain": "🇪🇸", "Slovenia": "🇸🇮", "Slovakia": "🇸🇰",
            "Ghana": "🇬🇭", "New Zealand": "🇳🇿", "Iran": "🇮🇷", "IR Iran": "🇮🇷", "Mali": "🇲🇱",
            "France": "🇫🇷", "Jamaica": "🇯🇲", "Italy": "🇮🇹", "Honduras": "🇭🇳",
            "Denmark": "🇩🇰", "Oman": "🇴🇲", "Colombia": "🇨🇴", "Uzbekistan": "🇺🇿",
            "England": "🏴\u200D󠁢󠁥󠁮󠁧󠁿", "United Arab Emirates": "🇦🇪", "Belgium": "🇧🇪", "Zambia": "🇿🇲",
            "Uruguay": "🇺🇾", "China": "🇨🇳", "Panama": "🇵🇦", "Cape Verde": "🇨🇻", "Cabo Verde": "🇨🇻",
            "Egypt": "🇪🇬", "Saudi Arabia": "🇸🇦", "Algeria": "🇩🇿", "Jordan": "🇯🇴", "Austria": "🇦🇹",
            "Portugal": "🇵🇹", "DR Congo": "🇨🇩", "Congo DR": "🇨🇩", "Croatia": "🇭🇷", "Iraq": "🇮🇶", "Norway": "🇳🇴", "Senegal": "🇸🇳"
        };

        // Standard UTC Offset database (Summer Daylight Saving Time 2026 limits)
        const venueOffsets = {
            "Mexico City Stadium": -6, // Standard Mexico Central Time (No DST)
            "Estadio Guadalajara": -6,
            "Monterrey Stadium": -6,
            "Houston Stadium": -5, // US Central Daylight Time (CDT)
            "Dallas Stadium": -5,
            "Kansas City Stadium": -5,
            "Toronto Stadium": -4, // US/CAN Eastern Daylight Time (EDT)
            "New York New Jersey Stadium": -4,
            "Boston Stadium": -4,
            "Philadelphia Stadium": -4,
            "Miami Stadium": -4,
            "Atlanta Stadium": -4,
            "Cincinnati Stadium": -4,
            "Los Angeles Stadium": -7, // US Pacific Daylight Time (PDT)
            "San Francisco Bay Area Stadium": -7,
            "BC Place Vancouver": -7,
            "Seattle Stadium": -7,
            "San Diego Stadium": -7
        };

        function getTeamFlag(teamName) {
            return countryFlags[teamName] || "🏳️";
        }

        // Convert 24-hour time format (e.g., "13:00") to clean 12-hour format with AM/PM
        function convertTo12Hour(timeStr) {
            if (!timeStr) return '';
            let [hours, minutes] = timeStr.split(':').map(Number);
            const ampm = hours >= 12 ? 'PM' : 'AM';
            hours = hours % 12;
            hours = hours ? hours : 12; // Convert hour '0' to '12'
            const minutesStr = minutes < 10 ? '0' + minutes : minutes;
            return `${hours}:${minutesStr} ${ampm}`;
        }

        let currentView = 'groups'; // 'groups' | 'teams' | 'calendar' | 'favorites'
        
        let selectedFilters = {
            groups: [],
            teams: [],
            dates: []
        };
        
        let searchString = '';
        let sortMethod = 'chronological';
        let selectedTimezone = 'venue'; // Selected timezone tracker
        let starredMatches = []; 

        const groupsList = [...new Set(matchesDataset.map(m => m.Group))].sort((a,b) => {
            if (a.includes('Group') && b.includes('Group')) return a.localeCompare(b);
            return a.includes('Group') ? -1 : 1; 
        });

        const teamsList = [...new Set(matchesDataset.flatMap(m => {
            if(m.Group.includes('Group') && !m['Home Team'].includes('Runner-Up') && !m['Home Team'].includes('Winner')) {
                return [m['Home Team'], m['Away Team']];
            }
            return [];
        }))].sort();

        const timelineDays = [...new Set(matchesDataset.map(m => m.Date))].sort();

        // Switch Viewport panel sections
        function switchView(viewName) {
            currentView = viewName;
            
            ['groups', 'teams', 'calendar', 'favorites'].forEach(tab => {
                const button = document.getElementById(`tab-${tab}`);
                if(tab === viewName) {
                    button.className = "flex-1 lg:flex-none flex items-center justify-center gap-2 px-3.5 py-2 rounded-lg text-xs font-semibold transition-all duration-200 bg-gradient-to-r from-[#3CAC3B] to-[#54c953] text-[#1b1c1c] shadow-md";
                } else {
                    button.className = "flex-1 lg:flex-none flex items-center justify-center gap-2 px-3.5 py-2 rounded-lg text-xs font-semibold transition-all duration-200 text-[#D1D4D1]/80 hover:text-white";
                }
            });

            renderSidebarPanel();
            processMatchesPipeline();
        }

        function renderSidebarPanel() {
            const container = document.getElementById('view-panel-container');
            
            if (currentView === 'groups') {
                container.innerHTML = `
                    <div class="flex items-center gap-2 mb-3">
                        <i data-lucide="layers" class="w-4 h-4 text-[#3CAC3B]"></i>
                        <h3 class="text-xs font-bold uppercase tracking-wider text-[#D1D4D1]/80">Filter by Group Phase</h3>
                    </div>
                    <div class="grid grid-cols-2 gap-2 max-h-[420px] overflow-y-auto pr-1 custom-scrollbar">
                        ${groupsList.map(group => {
                            const isAct = selectedFilters.groups.includes(group);
                            return `
                                <button onclick="toggleFilter('group', '${group}')" class="text-left p-3 rounded-xl border text-xs font-semibold transition-all relative ${isAct ? 'bg-[#3CAC3B]/10 border-[#3CAC3B] text-[#3CAC3B] font-bold shadow-[0_0_12px_rgba(60,172,59,0.15)]' : 'bg-[#1b1c1c]/80 border-[#474A4A]/50 text-[#D1D4D1] hover:border-[#3CAC3B]/50 hover:bg-[#474A4A]/20'}">
                                    <div class="flex items-center justify-between gap-1">
                                        <span class="truncate">${group}</span>
                                        ${isAct ? '<i data-lucide="check" class="w-3.5 h-3.5 text-[#3CAC3B] flex-shrink-0"></i>' : ''}
                                    </div>
                                    <div class="text-[10px] text-[#D1D4D1]/50 font-normal mt-0.5">${matchesDataset.filter(m => m.Group === group).length} Matches</div>
                                </button>
                            `;
                        }).join('')}
                    </div>
                `;
            } 
            else if (currentView === 'teams') {
                container.innerHTML = `
                    <div class="flex items-center gap-2 mb-3">
                        <i data-lucide="flag" class="w-4 h-4 text-[#3CAC3B]"></i>
                        <h3 class="text-xs font-bold uppercase tracking-wider text-[#D1D4D1]/80">Filter by Nation</h3>
                    </div>
                    <div class="grid grid-cols-2 gap-2 max-h-[420px] overflow-y-auto pr-1 custom-scrollbar">
                        ${teamsList.map(team => {
                            const isAct = selectedFilters.teams.includes(team);
                            const flag = getTeamFlag(team);
                            return `
                                <button onclick="toggleFilter('team', '${team}')" class="text-left p-2.5 rounded-xl border text-xs font-semibold transition-all flex items-center justify-between gap-1 truncate ${isAct ? 'bg-[#3CAC3B]/10 border-[#3CAC3B] text-[#3CAC3B] font-bold shadow-[0_0_12px_rgba(60,172,59,0.15)]' : 'bg-[#1b1c1c]/80 border-[#474A4A]/50 text-[#D1D4D1] hover:border-[#3CAC3B]/50 hover:bg-[#474A4A]/20'}">
                                    <div class="flex items-center gap-2 truncate">
                                        <span class="text-base leading-none">${flag}</span>
                                        <span class="truncate">${team}</span>
                                    </div>
                                    ${isAct ? '<i data-lucide="check" class="w-3.5 h-3.5 text-[#3CAC3B] flex-shrink-0"></i>' : ''}
                                </button>
                            `;
                        }).join('')}
                    </div>
                `;
            } 
            else if (currentView === 'calendar') {
                container.innerHTML = `
                    <div class="flex items-center gap-2 mb-3">
                        <i data-lucide="calendar" class="w-4 h-4 text-[#3CAC3B]"></i>
                        <h3 class="text-xs font-bold uppercase tracking-wider text-[#D1D4D1]/80">Tournament Timeline Nodes</h3>
                    </div>
                    <div class="space-y-1.5 max-h-[420px] overflow-y-auto pr-1 custom-scrollbar">
                        ${timelineDays.map(dateStr => {
                            const isAct = selectedFilters.dates.includes(dateStr);
                            const d = new Date(dateStr + 'T00:00:00');
                            const formattedDate = d.toLocaleDateString('en-US', { month: 'short', day: 'numeric', weekday: 'short' });
                            const count = matchesDataset.filter(m => m.Date === dateStr).length;

                            return `
                                <button onclick="toggleFilter('date', '${dateStr}')" class="w-full text-left px-3.5 py-2.5 rounded-xl border text-xs font-bold transition-all flex items-center justify-between ${isAct ? 'bg-[#3CAC3B]/10 border-[#3CAC3B] text-[#3CAC3B] font-bold shadow-[0_0_12px_rgba(60,172,59,0.15)]' : 'bg-[#1b1c1c]/80 border-[#474A4A]/50 text-[#D1D4D1] hover:border-[#3CAC3B]/50 hover:bg-[#474A4A]/20'}">
                                    <div class="flex items-center gap-2">
                                        ${isAct ? '<i data-lucide="check-square" class="w-3.5 h-3.5 text-[#3CAC3B]"></i>' : '<i data-lucide="square" class="w-3.5 h-3.5 text-[#D1D4D1]/30"></i>'}
                                        <span>${formattedDate}</span>
                                    </div>
                                    <span class="text-[10px] bg-[#1b1c1c] text-[#D1D4D1]/60 px-2 py-0.5 rounded-full border border-[#474A4A]/60">${count} matches</span>
                                </button>
                            `;
                        }).join('')}
                    </div>
                `;
            }
            else if (currentView === 'favorites') {
                container.innerHTML = `
                    <div class="flex items-center gap-2 mb-3">
                        <i data-lucide="star" class="w-4 h-4 text-[#E61D25]"></i>
                        <h3 class="text-xs font-bold uppercase tracking-wider text-[#D1D4D1]/80">My Custom Feed Tracker</h3>
                    </div>
                    <div class="space-y-3">
                        <p class="text-xs text-[#D1D4D1]/70 leading-relaxed">
                            Matches you star on the schedule feed will appear here instantly. Toggle filters simultaneously to track closely!
                        </p>
                        <div class="p-4 bg-[#1b1c1c]/90 border border-[#474A4A]/60 rounded-xl">
                            <span class="text-[#D1D4D1]/60 text-xs">Starred Count:</span>
                            <span class="text-[#3CAC3B] font-extrabold text-sm float-right">${starredMatches.length} Matches</span>
                        </div>
                    </div>
                `;
            }
            lucide.createIcons();
        }

        // Toggle item within multi-select composite state
        function toggleFilter(type, value) {
            if (type === 'group') {
                const idx = selectedFilters.groups.indexOf(value);
                if (idx > -1) {
                    selectedFilters.groups.splice(idx, 1);
                } else {
                    selectedFilters.groups.push(value);
                }
            } else if (type === 'team') {
                const idx = selectedFilters.teams.indexOf(value);
                if (idx > -1) {
                    selectedFilters.teams.splice(idx, 1);
                } else {
                    selectedFilters.teams.push(value);
                }
            } else if (type === 'date') {
                const idx = selectedFilters.dates.indexOf(value);
                if (idx > -1) {
                    selectedFilters.dates.splice(idx, 1);
                } else {
                    selectedFilters.dates.push(value);
                }
            }
            
            renderActiveChips();
            renderSidebarPanel(); 
            processMatchesPipeline();
        }

        function renderActiveChips() {
            const container = document.getElementById('filter-chips-container');
            const clearBtn = document.getElementById('clear-btn');
            const statusText = document.getElementById('filter-status');
            
            let html = '';
            let totalActive = selectedFilters.groups.length + selectedFilters.teams.length + selectedFilters.dates.length;
            
            if (totalActive === 0) {
                statusText.innerText = 'Showing All Matches';
                clearBtn.classList.add('opacity-0', 'pointer-events-none');
                container.innerHTML = '<p class="text-xs text-[#D1D4D1]/40 italic">No filters selected. Click explorer items in the sidebar to accumulate filters.</p>';
                return;
            }
            
            statusText.innerText = `Active Filters (${totalActive})`;
            clearBtn.classList.remove('opacity-0', 'pointer-events-none');
            
            // Render active group chips
            selectedFilters.groups.forEach(g => {
                html += `
                    <span class="inline-flex items-center gap-1 bg-[#2A398D]/20 text-white text-[10px] font-extrabold pl-2.5 pr-1.5 py-1 rounded-full border border-[#2A398D]/40 shadow-inner">
                        📁 ${g}
                        <button onclick="toggleFilter('group', '${g}')" class="hover:text-[#E61D25] ml-1 focus:outline-none">
                            <i data-lucide="x" class="w-3 h-3"></i>
                        </button>
                    </span>
                `;
            });
            
            // Render active team chips
            selectedFilters.teams.forEach(t => {
                const flag = getTeamFlag(t);
                html += `
                    <span class="inline-flex items-center gap-1 bg-[#3CAC3B]/10 text-white text-[10px] font-extrabold pl-2.5 pr-1.5 py-1 rounded-full border border-[#3CAC3B]/30 shadow-inner">
                        ${flag} ${t}
                        <button onclick="toggleFilter('team', '${t}')" class="hover:text-[#E61D25] ml-1 focus:outline-none">
                            <i data-lucide="x" class="w-3 h-3"></i>
                        </button>
                    </span>
                `;
            });
            
            // Render active date chips
            selectedFilters.dates.forEach(dt => {
                const d = new Date(dt + 'T00:00:00');
                const formattedDate = d.toLocaleDateString('en-US', { month: 'short', day: 'numeric' });
                html += `
                    <span class="inline-flex items-center gap-1 bg-[#474A4A]/30 text-white text-[10px] font-extrabold pl-2.5 pr-1.5 py-1 rounded-full border border-[#474A4A]/50 shadow-inner">
                        📅 ${formattedDate}
                        <button onclick="toggleFilter('date', '${dt}')" class="hover:text-[#E61D25] ml-1 focus:outline-none">
                            <i data-lucide="x" class="w-3 h-3"></i>
                        </button>
                    </span>
                `;
            });
            
            container.innerHTML = `<div class="flex flex-wrap gap-1.5 max-h-[120px] overflow-y-auto custom-scrollbar p-0.5">${html}</div>`;
            lucide.createIcons();
        }

        // Full Reset of Filters
        function resetFilters() {
            selectedFilters = {
                groups: [],
                teams: [],
                dates: []
            };
            document.getElementById('global-search').value = '';
            searchString = '';
            
            renderActiveChips();
            renderSidebarPanel();
            processMatchesPipeline();
        }

        function handleSearch(val) {
            searchString = val.toLowerCase().trim();
            processMatchesPipeline();
        }

        function handleSortChange(val) {
            sortMethod = val;
            processMatchesPipeline();
        }

        function handleTimezoneChange(val) {
            selectedTimezone = val;
            processMatchesPipeline();
        }

        // Toggle tracked matches inside Session Memory
        function toggleFavorite(matchDate, matchTime, homeTeam, awayTeam) {
            const matchId = `${matchDate}_${matchTime}_${homeTeam}_${awayTeam}`;
            const matchIndex = starredMatches.indexOf(matchId);
            
            if (matchIndex === -1) {
                starredMatches.push(matchId);
            } else {
                starredMatches.splice(matchIndex, 1);
            }
            
            document.getElementById('fav-count').innerText = starredMatches.length;
            
            if (currentView === 'favorites') {
                renderSidebarPanel();
            }
            processMatchesPipeline();
        }

        function processMatchesPipeline() {
            let matches = [...matchesDataset];

            // 1. Group Filters (OR within Groups)
            if (selectedFilters.groups.length > 0) {
                matches = matches.filter(m => selectedFilters.groups.includes(m.Group));
            }

            // 2. Team Filters (OR within Teams - Home or Away matches)
            if (selectedFilters.teams.length > 0) {
                matches = matches.filter(m => selectedFilters.teams.includes(m['Home Team']) || selectedFilters.teams.includes(m['Away Team']));
            }

            // 3. Date Filters (OR within Dates)
            if (selectedFilters.dates.length > 0) {
                matches = matches.filter(m => selectedFilters.dates.includes(m.Date));
            }

            // 4. Star Tracked View Filter
            if (currentView === 'favorites') {
                matches = matches.filter(m => {
                    const matchId = `${m.Date}_${m.Time}_${m['Home Team']}_${m['Away Team']}`;
                    return starredMatches.includes(matchId);
                });
            }

            // 5. Global Search String Filter
            if (searchString) {
                matches = matches.filter(m => 
                    m['Home Team'].toLowerCase().includes(searchString) ||
                    m['Away Team'].toLowerCase().includes(searchString) ||
                    m.Location.toLowerCase().includes(searchString) ||
                    m.Group.toLowerCase().includes(searchString)
                );
            }

            // 6. Grid Sorters
            if (sortMethod === 'chronological') {
                matches.sort((a, b) => {
                    const comp = a.Date.localeCompare(b.Date);
                    return comp !== 0 ? comp : a.Time.localeCompare(b.Time);
                });
            } else if (sortMethod === 'stadium') {
                matches.sort((a, b) => a.Location.localeCompare(b.Location));
            } else if (sortMethod === 'group') {
                matches.sort((a, b) => a.Group.localeCompare(b.Group));
            }

            renderMatchCards(matches);
        }

        function renderMatchCards(matches) {
            const container = document.getElementById('match-feed-container');
            document.getElementById('match-count').innerText = `${matches.length} match${matches.length === 1 ? '' : 'es'}`;

            if(matches.length === 0) {
                container.innerHTML = `
                    <div class="col-span-full py-16 px-4 text-center border border-dashed border-[#474A4A]/50 rounded-2xl bg-[#1b1c1c]/40">
                        <i data-lucide="calendar-x" class="w-10 h-10 text-[#D1D4D1]/40 mx-auto mb-3 stroke-[1.5]"></i>
                        <h4 class="text-sm font-bold text-[#D1D4D1]">No Match Event Records Found</h4>
                        <p class="text-xs text-[#D1D4D1]/50 max-w-xs mx-auto mt-1">Adjust active filters or clear search query to view scheduled match records.</p>
                    </div>
                `;
                lucide.createIcons();
                return;
            }

            let htmlString = '';
            let lastSeparatorKey = null;

            matches.forEach(match => {
                // Determine display date and display time depending on chosen timezone conversion
                let displayTime = convertTo12Hour(match.Time);
                let displayDate = match.Date;

                if (selectedTimezone !== 'venue') {
                    const localOffsetHours = venueOffsets[match.Location] || -4; // EDT standard fallback
                    const [year, month, day] = match.Date.split('-').map(Number);
                    const [hour, minute] = match.Time.split(':').map(Number);
                    
                    // Create raw representation in Coordinated Universal Time
                    const utcDate = new Date(Date.UTC(year, month - 1, day, hour - localOffsetHours, minute));
                    
                    let targetTz = selectedTimezone;
                    if (targetTz === 'user') {
                        targetTz = Intl.DateTimeFormat().resolvedOptions().timeZone || 'UTC';
                    }
                    
                    try {
                        displayTime = utcDate.toLocaleTimeString('en-US', {
                            timeZone: targetTz,
                            hour: 'numeric',
                            minute: '2-digit',
                            hour12: true
                        });
                        
                        const formatter = new Intl.DateTimeFormat('en-US', {
                            timeZone: targetTz,
                            year: 'numeric',
                            month: '2-digit',
                            day: '2-digit'
                        });
                        const parts = formatter.formatToParts(utcDate);
                        const y = parts.find(p => p.type === 'year').value;
                        const m = parts.find(p => p.type === 'month').value;
                        const d = parts.find(p => p.type === 'day').value;
                        
                        displayDate = `${y}-${m}-${d}`;
                    } catch (e) {
                        console.error("Timezone shifting pipeline exception: ", e);
                    }
                }

                const dateObj = new Date(displayDate + 'T00:00:00');
                const cleanDate = dateObj.toLocaleDateString('en-US', { month: 'long', day: 'numeric', weekday: 'long' });
                
                // Track separation breaks based on active sort options
                let currentSeparatorKey = '';
                let separatorMarkup = '';

                if (sortMethod === 'chronological') {
                    currentSeparatorKey = displayDate;
                    if (currentSeparatorKey !== lastSeparatorKey) {
                        separatorMarkup = `
                            <div class="col-span-full flex items-center gap-4 mt-8 mb-4 first:mt-2">
                                <div class="h-[3px] bg-gradient-to-r from-transparent via-[#2A398D]/85 to-transparent flex-1 rounded-full"></div>
                                <div class="bg-[#1b1c1c] border border-[#2A398D]/60 px-5 py-2.5 rounded-2xl text-xs font-black text-white tracking-widest uppercase flex items-center gap-2.5 shadow-xl backdrop-blur-sm">
                                    <i data-lucide="calendar" class="w-4 h-4 text-[#3CAC3B]"></i>
                                    <span>${cleanDate}</span>
                                </div>
                                <div class="h-[3px] bg-gradient-to-r from-transparent via-[#2A398D]/85 to-transparent flex-1 rounded-full"></div>
                            </div>
                        `;
                    }
                } else if (sortMethod === 'stadium') {
                    currentSeparatorKey = match.Location;
                    if (currentSeparatorKey !== lastSeparatorKey) {
                        separatorMarkup = `
                            <div class="col-span-full flex items-center gap-4 mt-8 mb-4 first:mt-2">
                                <div class="h-[3px] bg-gradient-to-r from-transparent via-[#3CAC3B]/75 to-transparent flex-1 rounded-full"></div>
                                <div class="bg-[#1b1c1c] border border-[#3CAC3B]/60 px-5 py-2.5 rounded-2xl text-xs font-black text-[#3CAC3B] tracking-widest uppercase flex items-center gap-2.5 shadow-xl backdrop-blur-sm">
                                    <i data-lucide="map-pin" class="w-4 h-4"></i>
                                    <span>${match.Location}</span>
                                </div>
                                <div class="h-[3px] bg-gradient-to-r from-transparent via-[#3CAC3B]/75 to-transparent flex-1 rounded-full"></div>
                            </div>
                        `;
                    }
                } else if (sortMethod === 'group') {
                    currentSeparatorKey = match.Group;
                    if (currentSeparatorKey !== lastSeparatorKey) {
                        separatorMarkup = `
                            <div class="col-span-full flex items-center gap-4 mt-8 mb-4 first:mt-2">
                                <div class="h-[3px] bg-gradient-to-r from-transparent via-[#E61D25]/75 to-transparent flex-1 rounded-full"></div>
                                <div class="bg-[#1b1c1c] border border-[#E61D25]/60 px-5 py-2.5 rounded-2xl text-xs font-black text-[#E61D25] tracking-widest uppercase flex items-center gap-2.5 shadow-xl backdrop-blur-sm">
                                    <i data-lucide="layers" class="w-4 h-4"></i>
                                    <span>${match.Group}</span>
                                </div>
                                <div class="h-[3px] bg-gradient-to-r from-transparent via-[#E61D25]/75 to-transparent flex-1 rounded-full"></div>
                            </div>
                        `;
                    }
                }

                if (separatorMarkup) {
                    htmlString += separatorMarkup;
                    lastSeparatorKey = currentSeparatorKey;
                }

                const matchId = `${match.Date}_${match.Time}_${match['Home Team']}_${match['Away Team']}`;
                const isStarred = starredMatches.includes(matchId);

                const isHomeFilterAct = selectedFilters.teams.includes(match['Home Team']);
                const isAwayFilterAct = selectedFilters.teams.includes(match['Away Team']);

                const dateLabelShort = dateObj.toLocaleDateString('en-US', { month: 'short', day: 'numeric', weekday: 'short' });

                htmlString += `
                    <div class="bg-[#474A4A]/25 border ${isStarred ? 'border-[#E61D25]/50 shadow-[0_0_15px_rgba(230,29,37,0.15)]' : 'border-[#474A4A]/60'} rounded-2xl p-4 flex flex-col justify-between hover:border-[#3CAC3B]/50 transition-all duration-200 card-glow group relative backdrop-blur-sm">
                        <!-- Top Header Area -->
                        <div>
                            <div class="flex items-center justify-between gap-2 mb-3">
                                <span class="text-[10px] font-extrabold px-2.5 py-1 rounded-md bg-[#1b1c1c] text-[#D1D4D1]/70 tracking-wider uppercase border border-[#474A4A]/50">
                                    ${match.Group}
                                </span>
                                <div class="flex items-center gap-2">
                                    <span class="text-[11px] text-[#D1D4D1]/80 font-medium flex items-center gap-1 truncate max-w-[150px]">
                                        <i data-lucide="map-pin" class="w-3.5 h-3.5 text-[#D1D4D1]/50 flex-shrink-0"></i> ${match.Location.replace(' Stadium', '')}
                                    </span>
                                    <button onclick="toggleFavorite('${match.Date}', '${match.Time}', '${match['Home Team']}', '${match['Away Team']}')" class="text-[#D1D4D1]/60 hover:text-[#E61D25] transition" title="Toggle Star Tracked Status">
                                        <i data-lucide="star" class="w-4.5 h-4.5 ${isStarred ? 'fill-[#E61D25] text-[#E61D25]' : 'text-[#D1D4D1]/40 hover:text-[#E61D25]'}"></i>
                                    </button>
                                </div>
                            </div>

                            <!-- Competing Teams -->
                            <div class="space-y-3 my-3">
                                <div class="flex items-center justify-between bg-[#1b1c1c]/60 p-2 rounded-xl border border-[#474A4A]/40 transition ${isHomeFilterAct ? 'ring-2 ring-[#3CAC3B]/50 bg-[#3CAC3B]/5' : ''}">
                                    <div class="flex items-center gap-2.5">
                                        <span class="text-lg leading-none">${getTeamFlag(match['Home Team'])}</span>
                                        <span class="text-sm font-semibold tracking-wide ${isHomeFilterAct ? 'text-[#3CAC3B] font-black' : 'text-white'} group-hover:text-white transition">
                                            ${match['Home Team']}
                                        </span>
                                    </div>
                                    <span class="text-[9px] font-bold text-[#D1D4D1]/40 tracking-widest bg-[#1b1c1c] px-1.5 py-0.5 rounded-md border border-[#474A4A]/40">HOME</span>
                                </div>
                                <div class="flex items-center justify-between bg-[#1b1c1c]/60 p-2 rounded-xl border border-[#474A4A]/40 transition ${isAwayFilterAct ? 'ring-2 ring-[#3CAC3B]/50 bg-[#3CAC3B]/5' : ''}">
                                    <div class="flex items-center gap-2.5">
                                        <span class="text-lg leading-none">${getTeamFlag(match['Away Team'])}</span>
                                        <span class="text-sm font-semibold tracking-wide ${isAwayFilterAct ? 'text-[#3CAC3B] font-black' : 'text-white'} group-hover:text-white transition">
                                            ${match['Away Team']}
                                        </span>
                                    </div>
                                    <span class="text-[9px] font-bold text-[#D1D4D1]/40 tracking-widest bg-[#1b1c1c] px-1.5 py-0.5 rounded-md border border-[#474A4A]/40">AWAY</span>
                                </div>
                            </div>
                        </div>

                        <!-- Date/Time Footer Area -->
                        <div class="mt-4 pt-3 border-t border-[#474A4A]/40 flex items-center justify-between text-xs text-[#D1D4D1]/70">
                            <span class="flex items-center gap-1.5 font-semibold text-[#D1D4D1]/80">
                                <i data-lucide="calendar" class="w-4 h-4 text-[#D1D4D1]/45"></i> ${dateLabelShort}
                            </span>
                            <span class="bg-[#3CAC3B]/10 text-[#3CAC3B] border border-[#3CAC3B]/30 font-bold px-2.5 py-1 rounded-lg flex items-center gap-1 shadow-inner">
                                <i data-lucide="clock" class="w-3.5 h-3.5"></i> ${displayTime}
                            </span>
                        </div>
                    </div>
                `;
            });

            container.innerHTML = htmlString;
            lucide.createIcons();
        }

        function updateOpeningCountdown() {
            // Opening match kickoff absolute time standard: June 11, 2026 at 13:00 Central Standard Time (UTC-6)
            const kickOffDate = new Date("2026-06-11T13:00:00-06:00").getTime();
            
            function tick() {
                const now = new Date().getTime();
                const distance = kickOffDate - now;

                if (distance < 0) {
                    document.getElementById("countdown-timer").innerHTML = `
                        <div class="col-span-full py-2 bg-gradient-to-r from-[#3CAC3B] to-[#54c953] text-[#1b1c1c] font-black tracking-wide text-xs uppercase rounded-xl">
                            🎉 Tournament Kickoff Initiated!
                        </div>
                    `;
                    return;
                }

                const days = Math.floor(distance / (1000 * 60 * 60 * 24));
                const hours = Math.floor((distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
                const mins = Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60));
                const secs = Math.floor((distance % (1000 * 60)) / 1000);

                document.getElementById("days").innerText = String(days).padStart(2, '0');
                document.getElementById("hours").innerText = String(hours).padStart(2, '0');
                document.getElementById("mins").innerText = String(mins).padStart(2, '0');
                document.getElementById("secs").innerText = String(secs).padStart(2, '0');
            }

            tick();
            setInterval(tick, 1000);
        }

        // Bootstrap Lifecycle init
        window.addEventListener('DOMContentLoaded', () => {
            // Populate and label device timezone dynamically
            const userTz = Intl.DateTimeFormat().resolvedOptions().timeZone || 'UTC';
            const userOption = document.getElementById('user-tz-option');
            if (userOption) {
                userOption.innerText = `📱 My Time (${userTz})`;
            }

            renderActiveChips();
            switchView('groups');
            updateOpeningCountdown();
        });
    </script>
</body>
</html>
