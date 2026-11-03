# Kout-
Kout for friends 
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Kout Bo Arbaa (كوت بو أربعة) - Gulf Partnership Card Game</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Cinzel:wght@600;700;800&family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <!-- Font Awesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">

    <style>
        :root {
            --felt-dark: #07291e;
            --felt-mid: #0c4130;
            --felt-light: #135d45;
            --gold-light: #fde68a;
            --gold-mid: #d97706;
            --gold-border: #f59e0b;
        }

        body {
            font-family: 'Inter', sans-serif;
            background-color: #041711;
            color: #f8fafc;
            overflow: hidden;
            height: 100dvh;
            max-height: 100dvh;
            user-select: none;
            -webkit-user-select: none;
            -webkit-tap-highlight-color: transparent;
        }

        .font-cinzel {
            font-family: 'Cinzel', serif;
        }

        /* Rich Green Felt Table with Arabesque Overlay */
        .felt-table {
            background: radial-gradient(ellipse at center, var(--felt-light) 0%, var(--felt-mid) 55%, var(--felt-dark) 100%);
            position: relative;
            box-shadow: inset 0 0 90px rgba(0, 0, 0, 0.75);
        }

        .felt-table::before {
            content: "";
            position: absolute;
            inset: 0;
            opacity: 0.06;
            pointer-events: none;
            background-image: url("data:image/svg+xml,%3Csvg width='60' height='60' viewBox='0 0 60 60' xmlns='http://www.w3.org/2000/svg'%3E%3Cg fill='none' stroke='%23fbbf24' stroke-width='1'%3E%3Cpath d='M30 0L60 30L30 60L0 30Z'/%3E%3Ccircle cx='30' cy='30' r='12'/%3E%3Cpath d='M30 10L50 30L30 50L10 30Z'/%3E%3C/g%3E%3C/svg%3E");
        }

        /* Ornate Gold Frame */
        .gold-frame {
            border: 2px solid rgba(245, 158, 11, 0.35);
            box-shadow: 0 0 25px rgba(0, 0, 0, 0.6), inset 0 0 15px rgba(245, 158, 11, 0.12);
        }

        /* Playing Card Styling - Mobile First + Laptop Scaling */
        .playing-card {
            width: 58px;
            height: 86px;
            border-radius: 9px;
            background: linear-gradient(145deg, #ffffff 0%, #f8fafc 100%);
            box-shadow: 0 5px 12px rgba(0, 0, 0, 0.5), inset 0 0 0 1px rgba(0, 0, 0, 0.12);
            transition: transform 0.22s cubic-bezier(0.34, 1.56, 0.64, 1), box-shadow 0.22s ease, filter 0.2s ease, opacity 0.2s ease;
            position: relative;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            padding: 5px 6px;
            cursor: default;
            flex-shrink: 0;
            touch-action: manipulation;
        }

        @media (min-width: 640px) {
            .playing-card {
                width: 78px;
                height: 112px;
                padding: 6px 7px;
                border-radius: 10px;
            }
        }

        @media (min-width: 1024px) {
            .playing-card {
                width: 92px;
                height: 132px;
                padding: 8px 9px;
            }
        }

        /* Center Arena Played Cards */
        .arena-card .playing-card {
            width: 52px;
            height: 76px;
            padding: 3px 4px;
        }

        @media (min-width: 640px) {
            .arena-card .playing-card {
                width: 74px;
                height: 106px;
                padding: 6px 7px;
            }
        }

        @media (min-width: 1024px) {
            .arena-card .playing-card {
                width: 86px;
                height: 124px;
                padding: 7px 8px;
            }
        }

        /* Mobile Overlapping Fan for South Hand */
        @media (max-width: 639px) {
            #southHandContainer .playing-card:not(:first-child) {
                margin-left: -12px;
            }
            #southHandContainer .playing-card.playable:hover,
            #southHandContainer .playing-card.playable:active {
                transform: translateY(-14px) scale(1.06);
            }
        }

        /* Playable vs Unplayable states in South Hand (No aggressive pre-glow) */
        .playing-card.playable {
            cursor: pointer;
            border: 1.5px solid rgba(226, 232, 240, 0.9);
            box-shadow: 0 6px 14px rgba(0, 0, 0, 0.45);
            transform: translateY(0);
        }

        .playing-card.playable:hover {
            transform: translateY(-16px) scale(1.05);
            border-color: #fbbf24;
            box-shadow: 0 14px 24px rgba(0, 0, 0, 0.6), 0 0 12px rgba(251, 191, 36, 0.35);
            z-index: 30 !important;
        }

        /* Highlight cards that match the led suit (recommended legal suit, while still allowing any card) */
        .playing-card.follows-suit {
            border: 2px solid #34d399 !important;
            box-shadow: 0 8px 18px rgba(0, 0, 0, 0.55), 0 0 12px rgba(52, 211, 153, 0.45);
            transform: translateY(-6px);
        }

        .playing-card.follows-suit:hover {
            transform: translateY(-18px) scale(1.06);
            border-color: #10b981 !important;
            box-shadow: 0 14px 24px rgba(0, 0, 0, 0.65), 0 0 18px rgba(16, 185, 129, 0.65);
            z-index: 32 !important;
        }

        .playing-card.not-your-turn {
            opacity: 0.92;
            cursor: default;
        }

        /* On-Demand Hint Highlight (Only shown when Hint button is clicked) */
        @keyframes hintPulse {
            0%, 100% {
                box-shadow: 0 0 0 3px #fbbf24, 0 14px 28px rgba(251, 191, 36, 0.65);
            }
            50% {
                box-shadow: 0 0 0 6px #34d399, 0 18px 34px rgba(52, 211, 153, 0.8);
            }
        }

        .playing-card.hint-recommended {
            transform: translateY(-20px) scale(1.06) !important;
            border: 2.5px solid #fbbf24 !important;
            animation: hintPulse 1.3s infinite ease-in-out;
            z-index: 35 !important;
        }

        /* Joker Rainbow Gold Sheen */
        .playing-card.joker-card {
            background: linear-gradient(135deg, #fffbeb 0%, #fef3c7 50%, #fde68a 100%);
            border: 2px solid #d97706;
        }

        /* Face-down AI cards */
        .card-back {
            width: 38px;
            height: 56px;
            border-radius: 6px;
            background: linear-gradient(135deg, #7c2d12 0%, #9a3412 50%, #431407 100%);
            border: 1.5px solid #fbbf24;
            box-shadow: 0 3px 6px rgba(0,0,0,0.5);
            position: relative;
            overflow: hidden;
        }

        .card-back::after {
            content: "❖";
            position: absolute;
            inset: 0;
            display: flex;
            align-items: center;
            justify-content: center;
            color: rgba(251, 191, 36, 0.45);
            font-size: 16px;
        }

        /* Active Player Pulse Ring */
        @keyframes pulseGold {
            0%, 100% {
                box-shadow: 0 0 0 0 rgba(251, 191, 36, 0.7), 0 0 18px rgba(251, 191, 36, 0.5);
            }
            50% {
                box-shadow: 0 0 0 6px rgba(251, 191, 36, 0), 0 0 26px rgba(251, 191, 36, 0.85);
            }
        }

        .active-turn-glow {
            animation: pulseGold 1.6s infinite;
            border-color: #fbbf24 !important;
        }

        /* Card Pop Animation in Trick Area */
        @keyframes cardDrop {
            0% {
                opacity: 0;
                transform: scale(0.7) translateY(-15px);
            }
            100% {
                opacity: 1;
                transform: scale(1) translateY(0);
            }
        }

        .trick-card-enter {
            animation: cardDrop 0.26s cubic-bezier(0.22, 1, 0.36, 1) forwards;
        }

        /* Winner glow on winning card in center */
        @keyframes winnerFlash {
            0%, 100% {
                box-shadow: 0 0 0 3px #fbbf24, 0 0 25px #fbbf24;
                transform: scale(1.08);
            }
            50% {
                box-shadow: 0 0 0 6px #34d399, 0 0 35px #34d399;
                transform: scale(1.12);
            }
        }

        .winning-trick-card {
            animation: winnerFlash 0.8s ease-in-out infinite;
            z-index: 25;
        }

        /* Custom Scrollbar */
        ::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        ::-webkit-scrollbar-track {
            background: rgba(0, 0, 0, 0.2);
        }
        ::-webkit-scrollbar-thumb {
            background: rgba(245, 158, 11, 0.4);
            border-radius: 3px;
        }
    </style>
</head>
<body class="h-[100dvh] max-h-[100dvh] flex flex-col justify-between overflow-hidden">

    <!-- ==================== TOP SCOREBOARD & STATUS BAR ==================== -->
    <header class="w-full bg-slate-950/95 border-b border-amber-500/30 px-2 sm:px-3 py-1.5 sm:py-2 z-30 shadow-lg shrink-0">
        <div class="max-w-7xl mx-auto flex items-center justify-between gap-1.5 sm:gap-2">
            
            <!-- Brand Title -->
            <div class="flex items-center gap-2">
                <div class="w-7 h-7 sm:w-9 sm:h-9 rounded-lg bg-gradient-to-br from-amber-400 to-amber-700 flex items-center justify-center text-slate-950 font-bold shadow-md shrink-0">
                    <i class="fa-solid fa-crown text-sm sm:text-lg"></i>
                </div>
                <div>
                    <div class="flex items-center gap-1.5">
                        <h1 class="font-cinzel font-bold text-xs sm:text-base md:text-lg text-amber-300 tracking-wide leading-none">KOUT BO ARBAA</h1>
                        <span class="hidden md:inline-block text-[10px] px-2 py-0.5 rounded-full bg-amber-500/20 border border-amber-400/40 text-amber-200 font-medium">101 Pts</span>
                    </div>
                    <p class="text-[10px] text-emerald-300/80 hidden lg:block">Traditional 4-Player Partnership Game</p>
                </div>
            </div>

            <!-- Scoreboard Center Pillars -->
            <div class="flex items-center gap-1.5 sm:gap-3 bg-slate-900/90 border border-amber-500/25 rounded-xl px-2.5 sm:px-3.5 py-1 shadow-inner">
                
                <!-- Team 1: YOU & NORTH -->
                <div class="flex items-center gap-1.5 sm:gap-2.5 pr-1.5 sm:pr-3 border-r border-slate-700/80">
                    <div class="text-right">
                        <div class="text-[9px] sm:text-[10px] uppercase tracking-wider text-emerald-400 font-semibold leading-none">Us</div>
                        <div class="flex items-baseline gap-0.5 sm:gap-1 justify-end">
                            <span id="team0MatchScore" class="text-base sm:text-2xl font-extrabold text-white leading-tight">0</span>
                            <span class="text-[9px] sm:text-[11px] text-slate-400">/101</span>
                        </div>
                    </div>
                    <div class="bg-emerald-950/80 border border-emerald-500/30 rounded-lg px-2 sm:px-2.5 py-1 text-center min-w-[64px] sm:min-w-[82px]">
                        <div class="text-[9px] sm:text-[10px] uppercase tracking-wider text-emerald-300/90 font-medium">Rounds</div>
                        <div class="text-xs sm:text-sm font-extrabold text-white leading-tight"><span id="team0Tricks">0</span> <span class="text-[10px] text-slate-400 font-normal">/ 8</span></div>
                    </div>
                </div>

                <!-- Current Contract / Trump Indicator (Desktop & Tablet) -->
                <div class="hidden md:flex flex-col items-center justify-center min-w-[130px] px-1">
                    <div class="text-[9px] uppercase tracking-widest text-amber-400/90 font-semibold">Active Contract</div>
                    <div id="contractBadge" class="flex items-center gap-1 mt-0.5 px-2.5 py-0.5 rounded-full bg-amber-500/15 border border-amber-400/40 text-xs font-bold text-amber-200">
                        <span>Bidding Phase...</span>
                    </div>
                    <div id="contractBidderText" class="text-[10px] text-slate-400 mt-0.5">Dealer: South</div>
                </div>

                <!-- Team 2: EAST & WEST -->
                <div class="flex items-center gap-1.5 sm:gap-2.5 pl-1.5 sm:pl-3 md:border-l border-slate-700/80">
                    <div class="bg-rose-950/80 border border-rose-500/30 rounded-lg px-2 sm:px-2.5 py-1 text-center min-w-[64px] sm:min-w-[82px]">
                        <div class="text-[9px] sm:text-[10px] uppercase tracking-wider text-rose-300/90 font-medium">Rounds</div>
                        <div class="text-xs sm:text-sm font-extrabold text-white leading-tight"><span id="team1Tricks">0</span> <span class="text-[10px] text-slate-400 font-normal">/ 8</span></div>
                    </div>
                    <div>
                        <div class="text-[9px] sm:text-[10px] uppercase tracking-wider text-rose-400 font-semibold leading-none">Them</div>
                        <div class="flex items-baseline gap-0.5 sm:gap-1">
                            <span id="team1MatchScore" class="text-base sm:text-2xl font-extrabold text-white leading-tight">0</span>
                            <span class="text-[9px] sm:text-[11px] text-slate-400">/101</span>
                        </div>
                    </div>
                </div>

            </div>

            <!-- Action Buttons -->
            <div class="flex items-center gap-1 sm:gap-1.5">
                <button id="btnLastTrick" onclick="toggleLastTrickModal()" class="p-1.5 sm:px-2.5 sm:py-1.5 rounded-lg bg-slate-800 hover:bg-slate-700 border border-slate-600 text-xs font-medium text-slate-200 flex items-center gap-1.5 transition" title="Inspect Previous Round">
                    <i class="fa-solid fa-clock-rotate-left text-amber-400"></i>
                    <span class="hidden lg:inline">Last Round</span>
                </button>
                <button id="btnSound" onclick="toggleSound()" class="p-1.5 sm:p-2 rounded-lg bg-slate-800 hover:bg-slate-700 border border-slate-600 text-xs text-amber-300 transition" title="Toggle Sound Effects">
                    <i id="soundIcon" class="fa-solid fa-volume-high"></i>
                </button>
                <button onclick="openRulesModal()" class="p-1.5 sm:px-2.5 sm:py-1.5 rounded-lg bg-amber-500/20 hover:bg-amber-500/30 border border-amber-400/50 text-xs font-semibold text-amber-200 flex items-center gap-1.5 transition" title="Game Rules">
                    <i class="fa-solid fa-book-open"></i>
                    <span class="hidden sm:inline">Rules</span>
                </button>
                <button onclick="confirmNewMatch()" class="p-1.5 sm:px-2.5 sm:py-1.5 rounded-lg bg-emerald-600/30 hover:bg-emerald-600/50 border border-emerald-400/50 text-xs font-semibold text-emerald-200 flex items-center gap-1.5 transition" title="Start New Match">
                    <i class="fa-solid fa-rotate-right"></i>
                    <span class="hidden xl:inline">New Match</span>
                </button>
            </div>
        </div>
    </header>

    <!-- ==================== MAIN FELT CARD TABLE ==================== -->
    <main class="flex-1 felt-table gold-frame m-1.5 sm:m-2.5 rounded-2xl flex flex-col justify-between p-2 sm:p-4 relative overflow-hidden min-h-0">

        <!-- ==================== REDESIGNED EASY-TO-READ TOP-LEFT HOKUM & CONTRACT BOX ==================== -->
        <aside id="sideTrumpPanel" class="absolute top-2 left-2 sm:top-3.5 sm:left-3.5 bg-slate-950/95 backdrop-blur-md border-2 border-amber-400/80 rounded-2xl p-2.5 sm:p-3.5 w-[152px] sm:w-60 shadow-2xl z-20 transition-all">
            <!-- Header Bar with Collapse/Expand Button -->
            <div class="flex items-center justify-between pb-1.5 mb-2 border-b border-slate-800">
                <div class="flex items-center gap-1.5">
                    <span class="w-2 h-2 rounded-full bg-amber-400"></span>
                    <span class="text-[10px] sm:text-xs uppercase tracking-wider text-amber-300 font-extrabold">HOKUM (الحكم)</span>
                </div>
                <button type="button" onclick="toggleHokumBoxDetails()" class="w-6 h-6 rounded-lg bg-slate-800/90 hover:bg-slate-700 border border-slate-600 text-slate-300 hover:text-white flex items-center justify-center text-[10px] transition" title="Minimize / Expand Contract Details">
                    <i id="hokumToggleIcon" class="fa-solid fa-chevron-up"></i>
                </button>
            </div>

            <!-- Main Suit Icon + Hokum Name (Always Visible) -->
            <div class="flex items-center gap-2.5 sm:gap-3">
                <div id="sideTrumpIconBox" class="w-11 h-11 sm:w-14 sm:h-14 rounded-2xl bg-slate-900 border-2 border-amber-400/60 flex items-center justify-center text-2xl sm:text-4xl shadow-inner shrink-0">
                    <span id="sideTrumpIcon">⏳</span>
                </div>
                <div class="min-w-0 flex-1">
                    <div class="text-[9px] sm:text-[10px] uppercase tracking-wider text-slate-400 font-semibold">Trump Suit</div>
                    <div id="sideTrumpName" class="font-bold text-xs sm:text-base text-white truncate leading-tight mt-0.5">Bidding...</div>
                    <div id="sideTrumpCompactTarget" class="hidden text-[10px] font-bold text-amber-300 mt-0.5 truncate">Target: —</div>
                </div>
            </div>

            <!-- Expandable Contract Details (Target Rounds & Calling Team) -->
            <div id="hokumBoxDetails" class="mt-2.5 pt-2.5 border-t border-slate-800/90 space-y-1.5">
                <div class="flex items-center justify-between bg-slate-900/90 border border-slate-800 rounded-xl px-2.5 py-1.5">
                    <span class="text-[10px] sm:text-xs text-slate-400 font-medium">Target</span>
                    <span id="sideTrumpTarget" class="text-[11px] sm:text-xs font-extrabold text-amber-300">—</span>
                </div>
                <div class="flex items-center justify-between bg-slate-900/90 border border-slate-800 rounded-xl px-2.5 py-1.5">
                    <span class="text-[10px] sm:text-xs text-slate-400 font-medium">Caller</span>
                    <span id="sideTrumpCaller" class="text-[11px] sm:text-xs font-bold text-emerald-300 truncate max-w-[85px] sm:max-w-[125px]">—</span>
                </div>
            </div>
        </aside>

        <!-- ==================== NORTH PLAYER (AI PARTNER) ==================== -->
        <div class="w-full flex flex-col items-center z-10 pl-[154px] sm:pl-0">
            <div id="playerBox-2" class="bg-slate-950/85 border-2 border-emerald-500/40 rounded-xl px-2.5 py-1 sm:px-3.5 sm:py-2 flex items-center gap-2 sm:gap-3 shadow-lg transition-all relative">
                <div class="w-7 h-7 sm:w-9 sm:h-9 rounded-full bg-emerald-800 border border-emerald-400 flex items-center justify-center text-xs sm:text-sm font-bold text-emerald-100 shrink-0">
                    <i class="fa-solid fa-user-shield"></i>
                </div>
                <div>
                    <div class="flex items-center gap-1 sm:gap-1.5">
                        <span class="font-bold text-[11px] sm:text-sm text-white">Saad (Partner)</span>
                        <span class="text-[9px] sm:text-[10px] px-1 rounded bg-emerald-500/20 text-emerald-300 border border-emerald-500/30">NORTH</span>
                        <span id="dealerBadge-2" class="hidden text-[9px] sm:text-[10px] px-1.5 rounded bg-amber-500 text-slate-950 font-bold">D</span>
                    </div>
                    <div class="text-[10px] sm:text-[11px] text-slate-300 flex items-center gap-1.5 mt-0.5">
                        <span>Cards: <strong id="cardCount-2" class="text-amber-300">8</strong> 🂠</span>
                        <span id="bidBubble-2" class="hidden px-1.5 py-0.2 rounded-full bg-amber-500/25 border border-amber-400 text-amber-200 font-semibold text-[10px]"></span>
                    </div>
                </div>
                <!-- Mini Fanned Face-down Cards (Laptop/Tablet) -->
                <div id="aiHand-2" class="hidden md:flex items-center -space-x-5 ml-2"></div>
            </div>
        </div>

        <!-- ==================== MIDDLE ROW: WEST, TRICK ARENA, EAST ==================== -->
        <div class="flex-1 flex items-center justify-between gap-1 sm:gap-2 my-1 relative z-10 min-h-0">

            <!-- WEST PLAYER (AI OPPONENT) -->
            <div class="flex flex-col items-center shrink-0">
                <div id="playerBox-3" class="bg-slate-950/85 border-2 border-rose-500/40 rounded-xl p-1.5 sm:px-3 sm:py-2 flex flex-col sm:flex-row items-center gap-1 sm:gap-2.5 shadow-lg transition-all">
                    <div class="w-7 h-7 sm:w-9 sm:h-9 rounded-full bg-rose-900 border border-rose-400 flex items-center justify-center text-xs sm:text-sm font-bold text-rose-100">
                        <i class="fa-solid fa-user-ninja"></i>
                    </div>
                    <div class="text-center sm:text-left">
                        <div class="flex items-center justify-center sm:justify-start gap-1">
                            <span class="font-bold text-[10px] sm:text-sm text-white">Faisal</span>
                            <span class="hidden sm:inline-block text-[10px] px-1.5 rounded bg-rose-500/20 text-rose-300 border border-rose-500/30">WEST</span>
                            <span id="dealerBadge-3" class="hidden text-[9px] sm:text-[10px] px-1 rounded bg-amber-500 text-slate-950 font-bold">D</span>
                        </div>
                        <div class="text-[10px] sm:text-[11px] text-slate-300">
                            <strong id="cardCount-3" class="text-amber-300">8</strong> 🂠
                        </div>
                        <div id="bidBubble-3" class="hidden mt-0.5 px-1.5 py-0.2 rounded-full bg-amber-500/25 border border-amber-400 text-amber-200 font-semibold text-[9px] sm:text-[11px] inline-block"></div>
                    </div>
                </div>
                <div id="aiHand-3" class="hidden lg:flex items-center -space-x-5 mt-2"></div>
            </div>

            <!-- ==================== CENTER OCTAGONAL PLAY ARENA ==================== -->
            <div class="flex-1 max-w-lg mx-auto h-full max-h-[215px] sm:max-h-[285px] rounded-2xl sm:rounded-3xl bg-emerald-950/45 border-2 border-amber-500/30 shadow-inner relative flex items-center justify-center p-2 sm:p-4">
                
                <!-- Center Watermark & Led Suit Status -->
                <div class="flex flex-col items-center justify-center text-center pointer-events-none px-4">
                    <div id="ledSuitIndicator" class="text-[10px] sm:text-xs px-2.5 py-0.5 rounded-full bg-slate-950/80 border border-amber-500/30 text-amber-200 font-medium shadow">
                        Ready to Deal
                    </div>
                    <div id="statusBanner" class="mt-1 text-[11px] sm:text-sm font-semibold text-emerald-200 drop-shadow max-w-[220px] sm:max-w-xs">
                        Welcome to Kout Bo Arbaa
                    </div>
                </div>

                <!-- North Round Card Slot -->
                <div id="trickSlot-2" class="arena-card absolute top-1.5 sm:top-2.5 left-1/2 -translate-x-1/2 flex flex-col items-center"></div>

                <!-- South Round Card Slot -->
                <div id="trickSlot-0" class="arena-card absolute bottom-1.5 sm:bottom-2.5 left-1/2 -translate-x-1/2 flex flex-col items-center"></div>

                <!-- West Round Card Slot -->
                <div id="trickSlot-3" class="arena-card absolute left-2 sm:left-6 top-1/2 -translate-y-1/2 flex flex-col items-center"></div>

                <!-- East Round Card Slot -->
                <div id="trickSlot-1" class="arena-card absolute right-2 sm:right-6 top-1/2 -translate-y-1/2 flex flex-col items-center"></div>

                <!-- ==================== INTERACTIVE BIDDING OVERLAY PANEL ==================== -->
                <div id="biddingOverlay" class="hidden absolute inset-0 bg-slate-950/95 backdrop-blur-md rounded-2xl sm:rounded-3xl border-2 border-amber-400/80 p-2.5 sm:p-4 flex flex-col justify-between z-30 shadow-2xl overflow-y-auto">
                    <div class="flex items-center justify-between border-b border-slate-800 pb-1.5">
                        <div>
                            <h3 class="font-cinzel font-bold text-amber-300 text-xs sm:text-base flex items-center gap-1.5">
                                <i class="fa-solid fa-gavel"></i> Your Turn to Bid (المزايدة)
                            </h3>
                            <p id="biddingSubtext" class="text-[10px] sm:text-[11px] text-slate-300">Choose your Hokum (Trump) Suit, then Target Rounds (5–8)</p>
                        </div>
                        <div id="highestBidPill" class="text-[10px] sm:text-xs px-2 py-0.5 rounded-lg bg-amber-500/20 border border-amber-400/40 text-amber-200 font-semibold shrink-0">
                            Highest: None
                        </div>
                    </div>

                    <!-- Trump Suit Selector (Strictly Hokum) -->
                    <div class="my-1">
                        <div class="text-[9px] sm:text-[11px] uppercase tracking-wider text-slate-400 font-semibold mb-1">1. Choose Hokum Suit (Trump):</div>
                        <div class="grid grid-cols-4 gap-1.5 sm:gap-2">
                            <button type="button" onclick="selectBidSuit('S')" id="suitBtn-S" class="suit-select-btn py-1.5 sm:py-2 px-1 rounded-lg border border-amber-400 bg-amber-500/25 text-white font-bold text-xs flex flex-col items-center transition">
                                <span class="text-base sm:text-xl leading-none">♠</span>
                                <span class="text-[9px] sm:text-[10px] mt-0.5">Spades</span>
                            </button>
                            <button type="button" onclick="selectBidSuit('H')" id="suitBtn-H" class="suit-select-btn py-1.5 sm:py-2 px-1 rounded-lg border border-slate-700 bg-slate-900/90 text-rose-400 font-bold text-xs flex flex-col items-center transition">
                                <span class="text-base sm:text-xl leading-none">♥</span>
                                <span class="text-[9px] sm:text-[10px] mt-0.5 text-slate-200">Hearts</span>
                            </button>
                            <button type="button" onclick="selectBidSuit('C')" id="suitBtn-C" class="suit-select-btn py-1.5 sm:py-2 px-1 rounded-lg border border-slate-700 bg-slate-900/90 text-emerald-300 font-bold text-xs flex flex-col items-center transition">
                                <span class="text-base sm:text-xl leading-none">♣</span>
                                <span class="text-[9px] sm:text-[10px] mt-0.5 text-slate-200">Clubs</span>
                            </button>
                            <button type="button" onclick="selectBidSuit('D')" id="suitBtn-D" class="suit-select-btn py-1.5 sm:py-2 px-1 rounded-lg border border-slate-700 bg-slate-900/90 text-rose-400 font-bold text-xs flex flex-col items-center transition">
                                <span class="text-base sm:text-xl leading-none">♦</span>
                                <span class="text-[9px] sm:text-[10px] mt-0.5 text-slate-200">Diamonds</span>
                            </button>
                        </div>
                    </div>

                    <!-- Round Target Buttons (5, 6, 7, 8/Kout) -->
                    <div class="my-1">
                        <div class="text-[9px] sm:text-[11px] uppercase tracking-wider text-slate-400 font-semibold mb-1">2. Declare Target Rounds:</div>
                        <div class="grid grid-cols-4 gap-1.5 sm:gap-2">
                            <button type="button" id="bidTrickBtn-5" onclick="submitUserBid(5)" class="py-1.5 sm:py-2 px-1.5 rounded-xl bg-gradient-to-b from-emerald-600 to-emerald-800 hover:from-emerald-500 hover:to-emerald-700 border border-emerald-400 text-white font-bold text-[11px] sm:text-sm shadow transition disabled:opacity-35 disabled:cursor-not-allowed">
                                <div>5 Rnds</div>
                                <div id="bidPtsLabel-5" class="text-[9px] sm:text-[10px] text-emerald-200 font-normal">10 pts</div>
                            </button>
                            <button type="button" id="bidTrickBtn-6" onclick="submitUserBid(6)" class="py-1.5 sm:py-2 px-1.5 rounded-xl bg-gradient-to-b from-emerald-600 to-emerald-800 hover:from-emerald-500 hover:to-emerald-700 border border-emerald-400 text-white font-bold text-[11px] sm:text-sm shadow transition disabled:opacity-35 disabled:cursor-not-allowed">
                                <div>6 Rnds</div>
                                <div id="bidPtsLabel-6" class="text-[9px] sm:text-[10px] text-emerald-200 font-normal">20 pts</div>
                            </button>
                            <button type="button" id="bidTrickBtn-7" onclick="submitUserBid(7)" class="py-1.5 sm:py-2 px-1.5 rounded-xl bg-gradient-to-b from-emerald-600 to-emerald-800 hover:from-emerald-500 hover:to-emerald-700 border border-emerald-400 text-white font-bold text-[11px] sm:text-sm shadow transition disabled:opacity-35 disabled:cursor-not-allowed">
                                <div>7 Rnds</div>
                                <div id="bidPtsLabel-7" class="text-[9px] sm:text-[10px] text-emerald-200 font-normal">30 pts</div>
                            </button>
                            <button type="button" id="bidTrickBtn-8" onclick="submitUserBid(8)" class="py-1.5 sm:py-2 px-1.5 rounded-xl bg-gradient-to-b from-amber-500 to-amber-700 hover:from-amber-400 hover:to-amber-600 border border-amber-300 text-slate-950 font-extrabold text-[11px] sm:text-sm shadow transition disabled:opacity-35 disabled:cursor-not-allowed">
                                <div>8 (Kout)</div>
                                <div id="bidPtsLabel-8" class="text-[9px] sm:text-[10px] text-slate-900 font-semibold">40 pts</div>
                            </button>
                        </div>
                    </div>

                    <!-- Pass Button -->
                    <div class="flex items-center justify-between pt-1 border-t border-slate-800">
                        <span id="passHintText" class="text-[9px] sm:text-[11px] text-slate-400">Choose your strongest suit for Hokum.</span>
                        <button type="button" id="btnPassBid" onclick="submitUserPass()" class="px-4 py-1 sm:py-1.5 rounded-xl bg-slate-800 hover:bg-rose-900/70 border border-slate-600 hover:border-rose-400 text-xs sm:text-sm font-bold text-slate-200 transition disabled:opacity-40 disabled:cursor-not-allowed">
                            Pass (بس)
                        </button>
                    </div>
                </div>

            </div>

            <!-- EAST PLAYER (AI OPPONENT) -->
            <div class="flex flex-col items-center shrink-0">
                <div id="playerBox-1" class="bg-slate-950/85 border-2 border-rose-500/40 rounded-xl p-1.5 sm:px-3 sm:py-2 flex flex-col sm:flex-row items-center gap-1 sm:gap-2.5 shadow-lg transition-all">
                    <div class="w-7 h-7 sm:w-9 sm:h-9 rounded-full bg-rose-900 border border-rose-400 flex items-center justify-center text-xs sm:text-sm font-bold text-rose-100">
                        <i class="fa-solid fa-user-tie"></i>
                    </div>
                    <div class="text-center sm:text-left">
                        <div class="flex items-center justify-center sm:justify-start gap-1">
                            <span class="font-bold text-[10px] sm:text-sm text-white">Rashid</span>
                            <span class="hidden sm:inline-block text-[10px] px-1.5 rounded bg-rose-500/20 text-rose-300 border border-rose-500/30">EAST</span>
                            <span id="dealerBadge-1" class="hidden text-[9px] sm:text-[10px] px-1 rounded bg-amber-500 text-slate-950 font-bold">D</span>
                        </div>
                        <div class="text-[10px] sm:text-[11px] text-slate-300">
                            <strong id="cardCount-1" class="text-amber-300">8</strong> 🂠
                        </div>
                        <div id="bidBubble-1" class="hidden mt-0.5 px-1.5 py-0.2 rounded-full bg-amber-500/25 border border-amber-400 text-amber-200 font-semibold text-[9px] sm:text-[11px] inline-block"></div>
                    </div>
                </div>
                <div id="aiHand-1" class="hidden lg:flex items-center -space-x-5 mt-2"></div>
            </div>

        </div>

        <!-- ==================== SOUTH PLAYER (USER HAND & CONTROLS) ==================== -->
        <div class="w-full flex flex-col items-center z-20 shrink-0">
            
            <!-- South Player Header Info & On-Demand Hint Control -->
            <div class="flex items-center justify-between gap-2 w-full max-w-4xl mb-1 px-1 sm:px-2">
                <div id="playerBox-0" class="bg-slate-950/85 border-2 border-emerald-500/50 rounded-xl px-2.5 py-1 sm:px-3.5 sm:py-1.5 flex items-center gap-2 shadow-md transition-all min-w-0">
                    <div class="w-7 h-7 sm:w-8 sm:h-8 rounded-full bg-gradient-to-br from-amber-400 to-amber-600 text-slate-950 flex items-center justify-center font-bold text-xs sm:text-sm shrink-0">
                        <i class="fa-solid fa-user"></i>
                    </div>
                    <div class="min-w-0">
                        <div class="flex items-center gap-1.5">
                            <span class="font-bold text-xs sm:text-sm text-white">You (South)</span>
                            <span id="dealerBadge-0" class="hidden text-[10px] px-1.5 rounded bg-amber-500 text-slate-950 font-bold">D</span>
                            <span id="bidBubble-0" class="hidden px-2 py-0.2 rounded-full bg-amber-500/25 border border-amber-400 text-amber-200 font-semibold text-[10px]"></span>
                        </div>
                        <div id="userTurnHint" class="text-[10px] sm:text-[11px] text-emerald-300 truncate max-w-[180px] sm:max-w-md">Waiting for phase...</div>
                    </div>
                </div>

                <!-- On-Demand Hint Button, Call-Out Button & Round Counter -->
                <div class="flex items-center gap-1.5 sm:gap-2 shrink-0">
                    <button id="btnCallOut" type="button" onclick="userTriggerCallOut()" disabled
                        class="px-2.5 sm:px-3 py-1.5 rounded-xl bg-gradient-to-r from-rose-600 to-rose-700 hover:from-rose-500 hover:to-rose-600 disabled:from-slate-800 disabled:to-slate-800 disabled:text-slate-500 disabled:border-slate-700 text-white font-extrabold text-xs sm:text-sm border border-rose-400 shadow-lg flex items-center gap-1.5 transition cursor-pointer disabled:cursor-not-allowed"
                        title="Accuse an Opponent of hiding the led suit! Correct = Win Entire Hand, Wrong = Lose Entire Hand">
                        <i class="fa-solid fa-bullhorn"></i>
                        <span id="callOutBtnLabel">🚨 Call Out</span>
                    </button>

                    <button id="btnHint" type="button" onclick="requestPlayHint()" disabled
                        class="px-2.5 sm:px-3 py-1.5 rounded-xl bg-gradient-to-r from-amber-500 to-amber-600 hover:from-amber-400 hover:to-amber-500 disabled:from-slate-800 disabled:to-slate-800 disabled:text-slate-500 disabled:border-slate-700 text-slate-950 font-extrabold text-xs sm:text-sm border border-amber-300 shadow-lg flex items-center gap-1.5 transition cursor-pointer disabled:cursor-not-allowed">
                        <i class="fa-solid fa-lightbulb"></i>
                        <span>💡 Hint</span>
                    </button>

                    <div class="flex items-center bg-slate-950/80 border border-slate-700/80 rounded-xl px-2.5 py-1.5 text-[11px] sm:text-xs">
                        <span class="text-slate-300">Round <strong id="trickNumberDisplay" class="text-white">0</strong>/8</span>
                    </div>
                </div>
            </div>

            <!-- On-Demand Hint Explanation Banner (Hidden unless Hint is clicked) -->
            <div id="hintBanner" class="hidden w-full max-w-2xl mb-1 px-3 py-1.5 rounded-xl bg-amber-500/20 border border-amber-400/60 text-amber-100 text-[11px] sm:text-xs flex items-center justify-between gap-2 shadow-lg">
                <div class="flex items-center gap-1.5">
                    <span class="text-sm">💡</span>
                    <span id="hintBannerText" class="font-medium"></span>
                </div>
                <button onclick="clearActiveHint()" class="text-amber-300 hover:text-white text-xs px-1.5">
                    <i class="fa-solid fa-xmark"></i>
                </button>
            </div>

            <!-- South Player's Face-Up Hand Container -->
            <div id="southHandContainer" class="flex items-center justify-center flex-nowrap sm:gap-2 py-2 sm:py-3 px-1 sm:px-2 min-h-[98px] sm:min-h-[144px] w-full max-w-4xl overflow-x-visible">
                <!-- Dynamically populated cards -->
            </div>
        </div>

    </main>

    <!-- ==================== HAND / MATCH SUMMARY MODAL ==================== -->
    <div id="roundSummaryModal" class="hidden fixed inset-0 bg-slate-950/85 backdrop-blur-md flex items-center justify-center p-4 z-50">
        <div class="bg-slate-900 border-2 border-amber-500/60 rounded-2xl max-w-lg w-full p-5 sm:p-6 shadow-2xl text-slate-100">
            <div class="flex items-center justify-between border-b border-slate-800 pb-3">
                <div>
                    <span id="summaryTag" class="text-[11px] uppercase tracking-widest px-2.5 py-0.5 rounded-full bg-amber-500/20 text-amber-300 font-bold">Hand Summary</span>
                    <h2 id="summaryTitle" class="font-cinzel text-xl sm:text-2xl font-bold text-white mt-1">Contract Succeeded! (نجاح)</h2>
                </div>
                <div id="summaryIcon" class="w-12 h-12 rounded-2xl bg-amber-500/20 border border-amber-400/50 flex items-center justify-center text-2xl text-amber-300">
                    🏆
                </div>
            </div>

            <!-- Contract Recap Box -->
            <div class="my-4 bg-slate-950/80 border border-slate-800 rounded-xl p-3.5">
                <div class="flex items-center justify-between text-xs text-slate-400 mb-1">
                    <span>Declared Contract</span>
                    <span>Outcome</span>
                </div>
                <div class="flex items-center justify-between">
                    <div id="summaryContractDetail" class="text-sm sm:text-base font-bold text-amber-300">South & North: 6 Rounds Hokum ♠</div>
                    <div id="summaryOutcomeBadge" class="text-xs px-2.5 py-1 rounded-lg bg-emerald-500/20 border border-emerald-400 text-emerald-300 font-bold">MADE (+20 pts)</div>
                </div>
                <p id="summaryExplanation" class="text-xs text-slate-300 mt-2 leading-relaxed"></p>
            </div>

            <!-- Team Comparison Table -->
            <div class="grid grid-cols-2 gap-3 my-4">
                <div class="bg-emerald-950/40 border border-emerald-500/30 rounded-xl p-3">
                    <div class="text-xs font-bold text-emerald-300 uppercase">Us (You & Saad)</div>
                    <div class="mt-2 space-y-1 text-xs">
                        <div class="flex justify-between"><span>Rounds Won:</span><strong id="sumTeam0Tricks" class="text-white">0 / 8</strong></div>
                        <div class="flex justify-between border-t border-emerald-800/60 pt-1 mt-1"><span>Hand Score:</span><strong id="sumTeam0Delta" class="text-emerald-300">+0</strong></div>
                        <div class="flex justify-between text-sm font-bold pt-1"><span>Total Match:</span><span id="sumTeam0Total" class="text-white">0 / 101</span></div>
                    </div>
                </div>

                <div class="bg-rose-950/40 border border-rose-500/30 rounded-xl p-3">
                    <div class="text-xs font-bold text-rose-300 uppercase">Them (Rashid & Faisal)</div>
                    <div class="mt-2 space-y-1 text-xs">
                        <div class="flex justify-between"><span>Rounds Won:</span><strong id="sumTeam1Tricks" class="text-white">0 / 8</strong></div>
                        <div class="flex justify-between border-t border-rose-800/60 pt-1 mt-1"><span>Hand Score:</span><strong id="sumTeam1Delta" class="text-rose-300">+0</strong></div>
                        <div class="flex justify-between text-sm font-bold pt-1"><span>Total Match:</span><span id="sumTeam1Total" class="text-white">0 / 101</span></div>
                    </div>
                </div>
            </div>

            <div class="flex flex-col sm:flex-row justify-end gap-2.5 sm:gap-3 mt-5">
                <button id="btnReviewHand" type="button" onclick="openHandReviewModal()" class="w-full sm:w-1/2 py-3 rounded-xl bg-slate-800 hover:bg-slate-700 border border-amber-400/50 text-amber-200 font-bold text-sm shadow-lg transition flex items-center justify-center gap-2">
                    <i class="fa-solid fa-list-ol text-amber-400"></i>
                    <span>Review Rounds</span>
                </button>
                <button id="btnNextRound" type="button" onclick="handleNextRoundOrMatch()" class="w-full sm:w-1/2 py-3 rounded-xl bg-gradient-to-r from-amber-500 to-amber-600 hover:from-amber-400 hover:to-amber-500 text-slate-950 font-extrabold text-sm shadow-lg transition flex items-center justify-center gap-2">
                    <span>Deal Next Hand</span>
                    <i class="fa-solid fa-arrow-right"></i>
                </button>
            </div>
        </div>
    </div>

    <!-- ==================== ROUND-BY-ROUND HAND REVIEW MODAL ==================== -->
    <div id="handReviewModal" class="hidden fixed inset-0 bg-slate-950/90 backdrop-blur-md flex items-center justify-center p-3 sm:p-4 z-[60]">
        <div class="bg-slate-900 border-2 border-amber-500/60 rounded-2xl max-w-3xl w-full max-h-[90vh] flex flex-col shadow-2xl text-slate-100">
            <!-- Modal Header -->
            <div class="flex items-center justify-between px-4 sm:px-6 py-3.5 border-b border-slate-800 shrink-0">
                <div>
                    <span id="reviewModalSubtitle" class="text-[10px] sm:text-xs uppercase tracking-widest text-amber-400 font-bold">Play-by-Play History</span>
                    <h2 class="font-cinzel text-base sm:text-xl font-bold text-white flex items-center gap-2 mt-0.5">
                        <i class="fa-solid fa-clock-rotate-left text-amber-400"></i>
                        <span>Round-by-Round Game Review</span>
                    </h2>
                </div>
                <button type="button" onclick="closeHandReviewModal()" class="w-8 h-8 rounded-lg bg-slate-800 hover:bg-slate-700 text-slate-300 hover:text-white flex items-center justify-center transition">
                    <i class="fa-solid fa-xmark"></i>
                </button>
            </div>

            <!-- Scrollable Rounds List -->
            <div id="handReviewList" class="p-3 sm:p-5 overflow-y-auto space-y-3.5 flex-1">
                <!-- Populated dynamically with each round's 1st, 2nd, 3rd, 4th plays -->
            </div>

            <!-- Modal Footer -->
            <div class="px-4 sm:px-6 py-3 border-t border-slate-800 flex items-center justify-between gap-3 shrink-0 bg-slate-950/60 rounded-b-2xl">
                <button type="button" onclick="closeHandReviewModal()" class="px-4 py-2.5 rounded-xl bg-slate-800 hover:bg-slate-700 border border-slate-600 text-slate-200 font-bold text-xs sm:text-sm transition flex items-center gap-2">
                    <i class="fa-solid fa-arrow-left"></i>
                    <span>Back to Summary</span>
                </button>
                <button type="button" onclick="closeHandReviewModal(); handleNextRoundOrMatch();" class="px-5 py-2.5 rounded-xl bg-gradient-to-r from-amber-500 to-amber-600 hover:from-amber-400 hover:to-amber-500 text-slate-950 font-extrabold text-xs sm:text-sm shadow-lg transition flex items-center gap-2">
                    <span>Deal Next Hand</span>
                    <i class="fa-solid fa-arrow-right"></i>
                </button>
            </div>
        </div>
    </div>

    <!-- ==================== RULES & SCORING MODAL ==================== -->
    <div id="rulesModal" class="hidden fixed inset-0 bg-slate-950/85 backdrop-blur-md flex items-center justify-center p-4 z-50">
        <div class="bg-slate-900 border-2 border-amber-500/50 rounded-2xl max-w-2xl w-full max-h-[88vh] flex flex-col shadow-2xl text-slate-100">
            <div class="flex items-center justify-between px-6 py-4 border-b border-slate-800">
                <div class="flex items-center gap-2.5">
                    <i class="fa-solid fa-scroll text-amber-400 text-xl"></i>
                    <h2 class="font-cinzel text-lg sm:text-xl font-bold text-amber-300">Kout Bo Arbaa (كوت بو أربعة) — Game Rules</h2>
                </div>
                <button onclick="closeRulesModal()" class="w-8 h-8 rounded-lg bg-slate-800 hover:bg-slate-700 text-slate-300 flex items-center justify-center">
                    <i class="fa-solid fa-xmark"></i>
                </button>
            </div>

            <div class="p-6 overflow-y-auto space-y-4 text-xs sm:text-sm leading-relaxed text-slate-200">
                <div class="bg-slate-950/70 border border-amber-500/30 rounded-xl p-3.5">
                    <h3 class="font-bold text-amber-300 mb-1">1. Deck &amp; Partnerships</h3>
                    <p>Played by 4 players in two fixed partnerships (<strong>You &amp; Saad</strong> vs. <strong>Rashid &amp; Faisal</strong>). Uses a 32-card Kout deck (7 through Ace of all suits, with the <strong>7♦ removed</strong> and replaced by <strong>1 Colored Joker 🃏</strong>). Each player is dealt 8 cards per hand (8 rounds per hand).</p>
                </div>

                <div class="bg-slate-950/70 border border-slate-800 rounded-xl p-3.5">
                    <h3 class="font-bold text-amber-300 mb-1">2. Hokum Bidding Phase (المزايدة)</h3>
                    <p class="text-xs">Players bid how many rounds out of 8 (<strong>5, 6, 7, or 8 / Kout</strong>) their partnership will win and choose a <strong>Hokum (Trump)</strong> suit (♠, ♥, ♣, ♦):</p>
                    <ul class="list-disc list-inside mt-1.5 space-y-1 text-xs">
                        <li>Each new bid must call a higher round target (6, 7, or 8) than the current highest bid, or the player may <strong>Pass (بس)</strong>.</li>
                        <li>If the first 3 players Pass, the 4th player (Dealer) is forced to bid at least <strong>5 Rounds</strong> in a Hokum suit.</li>
                    </ul>
                </div>

                <div class="bg-slate-950/70 border border-slate-800 rounded-xl p-3.5">
                    <h3 class="font-bold text-amber-300 mb-1">3. Playing Rounds &amp; Call-Out Inspection (كشف الورق)</h3>
                    <ul class="list-disc list-inside space-y-1 text-xs">
                        <li>The highest bidder leads the very first round of the hand.</li>
                        <li><strong>The Colored Joker (🃏):</strong> Acts as the top Hokum card in the game and beats every card. When led, it demands the active Hokum suit.</li>
                        <li><strong>Free Card Play &amp; Call-Outs:</strong> You may play any card from your hand at any time (cards matching the led suit are highlighted in green). However, if you skip the led suit while holding it, opponents can <strong>Call You Out</strong> and inspect your hand!</li>
                        <li>If a Call-Out is <strong>Correct</strong> (caught holding the led suit), the accusing team automatically wins the entire Hand. If a Call-Out is <strong>Wrong</strong> (false accusation), the accusing team loses the entire Hand!</li>
                    </ul>
                </div>

                <div class="bg-slate-950/70 border border-emerald-500/30 rounded-xl p-3.5">
                    <h3 class="font-bold text-emerald-300 mb-1">4. Match Scoring (Target: 101 Points)</h3>
                    <div class="p-2.5 rounded bg-slate-900 text-xs mt-2">
                        <div class="font-bold text-amber-300 mb-1">Hokum Contract Values</div>
                        <div class="grid grid-cols-2 gap-1.5">
                            <div>5 Rounds = <strong>10 pts</strong></div>
                            <div>6 Rounds = <strong>20 pts</strong></div>
                            <div>7 Rounds = <strong>30 pts</strong></div>
                            <div>8 Rounds (Kout) = <strong>40 pts</strong></div>
                        </div>
                    </div>
                    <p class="mt-2 text-xs text-slate-300">
                        • <strong>Contract Made:</strong> Reaching or exceeding the bid awards the contract points to the declaring partnership.<br>
                        • <strong>Kasr (كسر - Failed Bid):</strong> If the bidding team falls short of their target rounds, they score 0 and the <strong>defending team scores DOUBLE (2x)</strong> the contract points!
                    </p>
                </div>
            </div>

            <div class="px-6 py-3 border-t border-slate-800 flex justify-end">
                <button onclick="closeRulesModal()" class="px-5 py-2 rounded-xl bg-amber-500 hover:bg-amber-400 text-slate-950 font-bold text-xs sm:text-sm transition">
                    Got It, Let's Play!
                </button>
            </div>
        </div>
    </div>

    <!-- ==================== LAST ROUND MODAL ==================== -->
    <div id="lastTrickModal" class="hidden fixed inset-0 bg-slate-950/75 backdrop-blur-sm flex items-center justify-center p-4 z-50">
        <div class="bg-slate-900 border border-amber-500/50 rounded-2xl max-w-md w-full p-5 shadow-2xl">
            <div class="flex items-center justify-between border-b border-slate-800 pb-2.5 mb-4">
                <h3 class="font-cinzel font-bold text-amber-300 text-base flex items-center gap-2">
                    <i class="fa-solid fa-clock-rotate-left"></i> Previous Round Cards
                </h3>
                <button onclick="toggleLastTrickModal()" class="text-slate-400 hover:text-white">
                    <i class="fa-solid fa-xmark"></i>
                </button>
            </div>
            <div id="lastTrickContent" class="grid grid-cols-2 gap-3 text-center py-2">
                <div class="col-span-2 text-xs text-slate-400 py-6">No completed rounds in this hand yet.</div>
            </div>
        </div>
    </div>

    <!-- ==================== CONFIRM NEW MATCH MODAL ==================== -->
    <div id="confirmMatchModal" class="hidden fixed inset-0 bg-slate-950/80 backdrop-blur-sm flex items-center justify-center p-4 z-50">
        <div class="bg-slate-900 border border-amber-500/50 rounded-2xl max-w-sm w-full p-5 shadow-2xl text-center">
            <div class="w-12 h-12 rounded-full bg-amber-500/20 border border-amber-400/40 flex items-center justify-center mx-auto text-amber-300 text-xl mb-3">
                <i class="fa-solid fa-rotate-right"></i>
            </div>
            <h3 class="font-cinzel font-bold text-lg text-white">Start a Fresh Match?</h3>
            <p class="text-xs text-slate-300 mt-1.5">This will reset both partnership scores to 0 and deal a brand-new 101-point match.</p>
            <div class="flex gap-2.5 mt-5">
                <button onclick="closeConfirmNewMatch()" class="flex-1 py-2 rounded-xl bg-slate-800 hover:bg-slate-700 text-slate-300 text-xs font-bold">Cancel</button>
                <button onclick="startNewMatch()" class="flex-1 py-2 rounded-xl bg-amber-500 hover:bg-amber-400 text-slate-950 text-xs font-extrabold">Reset & Deal</button>
            </div>
        </div>
    </div>

    <!-- ==================== CALL-OUT HAND INSPECTION MODAL ==================== -->
    <div id="callOutModal" class="hidden fixed inset-0 bg-slate-950/90 backdrop-blur-md flex items-center justify-center p-4 z-50">
        <div class="bg-slate-900 border-2 border-rose-500/80 rounded-2xl max-w-lg w-full p-5 sm:p-6 shadow-2xl text-slate-100">
            <div class="flex items-center justify-between border-b border-slate-800 pb-3">
                <div>
                    <span id="callOutBadge" class="text-[11px] uppercase tracking-widest px-2.5 py-0.5 rounded-full bg-rose-500/20 text-rose-300 font-bold">🚨 Call-Out Inspection (كشف الورق)</span>
                    <h2 id="callOutTitle" class="font-cinzel text-lg sm:text-xl font-bold text-white mt-1">Suit Challenge!</h2>
                </div>
                <div id="callOutVerdictIcon" class="w-12 h-12 rounded-2xl bg-rose-500/20 border border-rose-400/50 flex items-center justify-center text-2xl">
                    🚨
                </div>
            </div>

            <div class="my-4 bg-slate-950/90 border border-slate-800 rounded-xl p-3.5">
                <p id="callOutStory" class="text-xs sm:text-sm text-slate-200 leading-relaxed font-medium"></p>
                <div id="callOutVerdictBanner" class="mt-3 p-2.5 rounded-xl font-bold text-xs sm:text-sm text-center"></div>
            </div>

            <!-- Accused Player's Revealed Hand -->
            <div class="my-3">
                <div id="callOutHandLabel" class="text-[11px] uppercase tracking-wider text-slate-400 font-semibold mb-2 text-center">Revealed Cards in Hand:</div>
                <div id="callOutRevealedCards" class="flex items-center justify-center flex-wrap gap-1.5 py-2 px-2 bg-slate-950/70 rounded-xl border border-slate-800 min-h-[96px]"></div>
            </div>

            <div class="mt-5">
                <button type="button" onclick="acknowledgeCallOutResult()" class="w-full py-3 rounded-xl bg-gradient-to-r from-amber-500 to-amber-600 hover:from-amber-400 hover:to-amber-500 text-slate-950 font-extrabold text-sm shadow-lg transition flex items-center justify-center gap-2">
                    <span>View Hand Scoring</span>
                    <i class="fa-solid fa-gavel"></i>
                </button>
            </div>
        </div>
    </div>

    <!-- ==================== JAVASCRIPT GAME ENGINE ==================== -->
    <script>
        /********************************************************************
         * 1. CONSTANTS, DECK DEFINITIONS & GLOBAL GAME STATE
         ********************************************************************/
        const SUITS = {
            S: { name: 'Spades', symbol: '♠', colorClass: 'text-slate-900', badgeColor: 'text-slate-100' },
            H: { name: 'Hearts', symbol: '♥', colorClass: 'text-rose-600', badgeColor: 'text-rose-400' },
            C: { name: 'Clubs', symbol: '♣', colorClass: 'text-slate-900', badgeColor: 'text-emerald-300' },
            D: { name: 'Diamonds', symbol: '♦', colorClass: 'text-rose-600', badgeColor: 'text-rose-400' }
        };

        // Standard Kout rank order (High to Low): Joker > A > 10 > K > Q > J > 9 > 8 > 7
        const RANK_DEFS = [
            { rank: 'A', power: 8 },
            { rank: '10', power: 7 },
            { rank: 'K', power: 6 },
            { rank: 'Q', power: 5 },
            { rank: 'J', power: 4 },
            { rank: '9', power: 3 },
            { rank: '8', power: 2 },
            { rank: '7', power: 1 }
        ];

        const PLAYERS = [
            { index: 0, name: 'You (South)', shortName: 'You', team: 0, isAI: false },
            { index: 1, name: 'Rashid (East)', shortName: 'Rashid', team: 1, isAI: true },
            { index: 2, name: 'Saad (Partner)', shortName: 'Saad', team: 0, isAI: true },
            { index: 3, name: 'Faisal (West)', shortName: 'Faisal', team: 1, isAI: true }
        ];

        const gameState = {
            matchScores: [0, 0],
            handNumber: 1,
            dealerIndex: 3,
            phase: 'BIDDING', // 'BIDDING' | 'PLAYING' | 'ROUND_OVER' | 'MATCH_OVER'
            biddingTurnsTaken: 0,
            highestBid: null,
            contract: null,
            selectedUserBidSuit: 'S',
            currentTurn: 0,
            hands: [[], [], [], []],
            trickCount: 0,
            teamTricks: [0, 0],
            currentTrick: [],
            ledSuit: null,
            lastCompletedTrick: null,
            handHistory: [],
            activeHintCardId: null,
            hokumBoxCollapsed: false,
            claimedVoidSuits: [new Set(), new Set(), new Set(), new Set()],
            suspectOpponentPlay: null,
            pendingCallOutOutcome: null,
            soundEnabled: true,
            aiTimeoutId: null
        };

        /********************************************************************
         * 2. WEB AUDIO SYNTHESIZER (NO EXTERNAL SOUND FILES REQUIRED)
         ********************************************************************/
        let audioCtx = null;

        function playTone(freq = 440, duration = 0.08, type = 'sine', gainVal = 0.08) {
            if (!gameState.soundEnabled) return;
            try {
                if (!audioCtx) {
                    const AudioContextClass = window.AudioContext || window.webkitAudioContext;
                    if (AudioContextClass) audioCtx = new AudioContextClass();
                }
                if (!audioCtx) return;
                if (audioCtx.state === 'suspended') audioCtx.resume();

                const osc = audioCtx.createOscillator();
                const gain = audioCtx.createGain();
                osc.type = type;
                osc.frequency.setValueAtTime(freq, audioCtx.currentTime);
                gain.gain.setValueAtTime(gainVal, audioCtx.currentTime);
                gain.gain.exponentialRampToValueAtTime(0.001, audioCtx.currentTime + duration);

                osc.connect(gain);
                gain.connect(audioCtx.destination);
                osc.start();
                osc.stop(audioCtx.currentTime + duration);
            } catch (e) {
                // Ignore audio context restrictions
            }
        }

        function sfxCardPlay(isJoker = false) {
            if (isJoker) {
                playTone(587.33, 0.1, 'triangle', 0.12);
                setTimeout(() => playTone(880, 0.16, 'triangle', 0.12), 70);
            } else {
                playTone(360, 0.06, 'sine', 0.07);
            }
        }

        function sfxTrickWin(isOurTeam = true) {
            if (isOurTeam) {
                playTone(523.25, 0.09, 'triangle', 0.1);
                setTimeout(() => playTone(659.25, 0.14, 'triangle', 0.1), 80);
            } else {
                playTone(310, 0.11, 'sine', 0.08);
            }
        }

        function sfxBid() {
            playTone(493.88, 0.08, 'triangle', 0.09);
            setTimeout(() => playTone(659.25, 0.12, 'triangle', 0.09), 65);
        }

        function toggleSound() {
            gameState.soundEnabled = !gameState.soundEnabled;
            const icon = document.getElementById('soundIcon');
            if (icon) {
                icon.className = gameState.soundEnabled ? 'fa-solid fa-volume-high' : 'fa-solid fa-volume-xmark text-slate-400';
            }
            if (gameState.soundEnabled) playTone(520, 0.07);
        }

        /********************************************************************
         * 3. DECK GENERATION, FAVORABLE DEALING & HAND SORTING
         ********************************************************************/
        function buildKoutDeck() {
            const deck = [];
            for (const suitKey of ['S', 'H', 'C', 'D']) {
                for (const r of RANK_DEFS) {
                    // Remove 7 of Diamonds (7♦) to replace with the Colored Joker (32 cards total)
                    if (suitKey === 'D' && r.rank === '7') continue;
                    deck.push({
                        id: `${r.rank}_${suitKey}`,
                        rank: r.rank,
                        suit: suitKey,
                        power: r.power,
                        isJoker: false
                    });
                }
            }
            // Add 1 Colored Joker (Top Trump, power 9)
            deck.push({
                id: 'JOKER_COLOR',
                rank: 'JOKER',
                suit: null,
                power: 9,
                isJoker: true
            });
            return deck;
        }

        function shuffleDeck(deck) {
            const copy = [...deck];
            for (let i = copy.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [copy[i], copy[j]] = [copy[j], copy[i]];
            }
            return copy;
        }

        // Deal favorable, fun hands for the user (+10% boosted probability for stronger honors & suit length)
        function dealFavorableDeck() {
            let bestDeck = null;
            let bestScore = -1;

            // Extra 10% boost roll for a premium hand (4+ high honors or 4+ cards in a single suit)
            const boostHandQuality = Math.random() < 0.10;
            const totalAttempts = boostHandQuality ? 12 : 9;
            const minHonorsTarget = boostHandQuality ? 4 : 3;
            const minSuitTarget = boostHandQuality ? 4 : 3;

            for (let attempt = 0; attempt < totalAttempts; attempt++) {
                const candidate = shuffleDeck(buildKoutDeck());
                const userSlice = candidate.slice(0, 8);

                let score = 0;
                const suitCounts = { S: 0, H: 0, C: 0, D: 0 };
                let highHonors = 0;

                userSlice.forEach(c => {
                    if (c.isJoker) {
                        score += 6;
                        highHonors += 1;
                    } else {
                        suitCounts[c.suit]++;
                        if (c.power >= 6) {
                            highHonors += 1;
                            score += (c.power - 3.5);
                        }
                    }
                });

                const maxSuitLen = Math.max(...Object.values(suitCounts));
                score += maxSuitLen * 2.2;

                if (score > bestScore) {
                    bestScore = score;
                    bestDeck = candidate;
                }

                if (highHonors >= minHonorsTarget && maxSuitLen >= minSuitTarget && score >= 16) {
                    return candidate;
                }
            }
            return bestDeck;
        }

        function sortHand(hand, trumpSuit = null) {
            const suitOrder = { S: 1, H: 2, C: 3, D: 4 };
            return [...hand].sort((a, b) => {
                if (a.isJoker) return -1;
                if (b.isJoker) return 1;

                if (trumpSuit) {
                    const aTrump = a.suit === trumpSuit;
                    const bTrump = b.suit === trumpSuit;
                    if (aTrump && !bTrump) return -1;
                    if (!aTrump && bTrump) return 1;
                }

                if (a.suit !== b.suit) {
                    return suitOrder[a.suit] - suitOrder[b.suit];
                }
                return b.power - a.power;
            });
        }

        /********************************************************************
         * 4. BIDDING PHASE & AI EVALUATION LOGIC (STRICTLY HOKUM)
         ********************************************************************/
        function getBidComparisonValue(tricks) {
            return tricks * 10;
        }

        function getBidBasePoints(tricks) {
            const hokumTable = { 5: 10, 6: 20, 7: 30, 8: 40 };
            return hokumTable[tricks] || 10;
        }

        function formatBidText(bid) {
            if (!bid) return 'None';
            const roundWord = bid.tricks === 8 ? '8 Rounds (Kout)' : `${bid.tricks} Rounds`;
            const suitInfo = SUITS[bid.suit];
            return `${roundWord} Hokum ${suitInfo.symbol}`;
        }

        function startRound() {
            clearTimeout(gameState.aiTimeoutId);
            const deck = dealFavorableDeck();

            gameState.hands = [
                sortHand(deck.slice(0, 8)),
                sortHand(deck.slice(8, 16)),
                sortHand(deck.slice(16, 24)),
                sortHand(deck.slice(24, 32))
            ];

            gameState.phase = 'BIDDING';
            gameState.biddingTurnsTaken = 0;
            gameState.highestBid = null;
            gameState.contract = null;
            gameState.trickCount = 0;
            gameState.teamTricks = [0, 0];
            gameState.currentTrick = [];
            gameState.ledSuit = null;
            gameState.lastCompletedTrick = null;
            gameState.handHistory = [];
            gameState.claimedVoidSuits = [new Set(), new Set(), new Set(), new Set()];
            gameState.suspectOpponentPlay = null;
            gameState.pendingCallOutOutcome = null;
            clearActiveHint();
            updateCallOutButtonUI();

            // You (South, index 0) always call first at the start of every game/hand
            gameState.dealerIndex = 3;
            gameState.currentTurn = 0;

            hideAllBidBubbles();
            updateDealerBadges();
            updateScoreboardUI();
            renderAllHands();
            renderTrickArena();

            setStatusBanner(`Hand #${gameState.handNumber} Bidding — ${PLAYERS[gameState.currentTurn].name}'s turn`, 'Bidding Phase');
            proceedBiddingTurn();
        }

        function proceedBiddingTurn() {
            highlightActivePlayer(gameState.currentTurn);

            // Check if all 4 players have had their bidding decision
            if (gameState.biddingTurnsTaken >= 4) {
                finishBiddingPhase();
                return;
            }

            const currentPlayer = PLAYERS[gameState.currentTurn];
            if (!currentPlayer.isAI) {
                openUserBiddingPanel();
            } else {
                document.getElementById('biddingOverlay').classList.add('hidden');
                document.getElementById('userTurnHint').textContent = `Waiting for ${currentPlayer.shortName} to bid...`;
                gameState.aiTimeoutId = setTimeout(() => {
                    executeAIBid(currentPlayer.index);
                }, 900);
            }
        }

        // AI Hand Evaluation for Hokum (Trump Suit)
        function evaluateAIHandForBid(hand) {
            const hasJoker = hand.some(c => c.isJoker);
            let bestHokumSuit = 'S';
            let bestHokumTricks = 0;

            for (const s of ['S', 'H', 'C', 'D']) {
                const suitCards = hand.filter(c => c.suit === s);
                const otherAces = hand.filter(c => !c.isJoker && c.suit !== s && c.rank === 'A').length;
                const otherKings = hand.filter(c => !c.isJoker && c.suit !== s && c.rank === 'K').length;

                let est = 0;
                if (hasJoker) est += 1.25;
                suitCards.forEach(c => {
                    if (c.rank === 'A') est += 1.1;
                    else if (c.rank === 'K') est += 0.85;
                    else if (c.rank === 'Q') est += 0.65;
                    else if (c.rank === 'J') est += 0.45;
                    else est += 0.35;
                });
                // Length bonus in trump suit
                if (suitCards.length >= 4) est += (suitCards.length - 3) * 0.65;
                est += otherAces * 0.85 + otherKings * 0.4;
                // Partner support expectation (~1.15 rounds)
                est += 1.15;

                if (est > bestHokumTricks) {
                    bestHokumTricks = est;
                    bestHokumSuit = s;
                }
            }

            const targetHokum = Math.min(8, Math.floor(bestHokumTricks));
            if (targetHokum >= 5) {
                return { mode: 'HOKUM', suit: bestHokumSuit, tricks: targetHokum };
            }
            return null;
        }

        function executeAIBid(playerIndex) {
            const hand = gameState.hands[playerIndex];
            const suggestion = evaluateAIHandForBid(hand);
            const currentMinVal = gameState.highestBid ? gameState.highestBid.rankValue : 0;

            let chosenBid = null;
            if (suggestion) {
                const candidateVal = getBidComparisonValue(suggestion.tricks);
                if (candidateVal > currentMinVal) {
                    chosenBid = {
                        bidder: playerIndex,
                        tricks: suggestion.tricks,
                        mode: 'HOKUM',
                        suit: suggestion.suit,
                        rankValue: candidateVal
                    };
                }
            }

            // If all 3 previous players passed, the 4th player is forced to bid at least 5 Rounds in a Hokum suit
            if (!chosenBid && gameState.biddingTurnsTaken === 3 && !gameState.highestBid) {
                const bestSuit = ['S', 'H', 'C', 'D'].sort((a, b) =>
                    hand.filter(c => c.suit === b).length - hand.filter(c => c.suit === a).length
                )[0];
                chosenBid = {
                    bidder: playerIndex,
                    tricks: 5,
                    mode: 'HOKUM',
                    suit: bestSuit,
                    rankValue: getBidComparisonValue(5)
                };
            }

            if (chosenBid) {
                gameState.highestBid = chosenBid;
                showBidBubble(playerIndex, formatBidText(chosenBid), true);
                sfxBid();
                setStatusBanner(`${PLAYERS[playerIndex].name} bids ${formatBidText(chosenBid)}!`, 'New High Bid');
            } else {
                showBidBubble(playerIndex, 'Pass (بس)', false);
                playTone(260, 0.05);
            }

            updateScoreboardUI();
            gameState.biddingTurnsTaken++;
            gameState.currentTurn = (gameState.currentTurn + 1) % 4;

            gameState.aiTimeoutId = setTimeout(() => {
                proceedBiddingTurn();
            }, 900);
        }

        // User Bidding UI Controls
        function openUserBiddingPanel() {
            const overlay = document.getElementById('biddingOverlay');
            overlay.classList.remove('hidden');

            const isForcedFourthBidder = gameState.biddingTurnsTaken === 3 && !gameState.highestBid;
            const passBtn = document.getElementById('btnPassBid');
            const passHint = document.getElementById('passHintText');

            if (isForcedFourthBidder) {
                passBtn.disabled = true;
                passHint.textContent = 'First 3 players passed — you must bid at least 5 Rounds Hokum!';
                passHint.className = 'text-[9px] sm:text-[11px] text-amber-300 font-semibold';
                document.getElementById('userTurnHint').textContent = 'First 3 players passed! Choose a Hokum suit and bid at least 5 Rounds.';
            } else {
                passBtn.disabled = false;
                passHint.textContent = 'Choose your strongest suit for Hokum.';
                passHint.className = 'text-[9px] sm:text-[11px] text-slate-400';
                document.getElementById('userTurnHint').textContent = 'Select your Hokum bid in the center panel or Pass.';
            }

            const highPill = document.getElementById('highestBidPill');
            if (gameState.highestBid) {
                highPill.textContent = `High: ${formatBidText(gameState.highestBid)} (${PLAYERS[gameState.highestBid.bidder].shortName})`;
            } else {
                highPill.textContent = 'Highest: None';
            }

            // Default-select the user's longest suit for convenience
            const longestSuit = ['S', 'H', 'C', 'D'].sort((a, b) =>
                gameState.hands[0].filter(c => c.suit === b).length - gameState.hands[0].filter(c => c.suit === a).length
            )[0];
            selectBidSuit(longestSuit || 'S');
        }

        function selectBidSuit(suit) {
            gameState.selectedUserBidSuit = suit;
            const allSuitBtns = document.querySelectorAll('.suit-select-btn');
            allSuitBtns.forEach(btn => {
                btn.classList.remove('border-amber-400', 'bg-amber-500/25', 'ring-2', 'ring-amber-400/50');
                btn.classList.add('border-slate-700', 'bg-slate-900/90');
            });

            const activeBtn = document.getElementById(`suitBtn-${suit}`);
            if (activeBtn) {
                activeBtn.classList.remove('border-slate-700', 'bg-slate-900/90');
                activeBtn.classList.add('border-amber-400', 'bg-amber-500/25', 'ring-2', 'ring-amber-400/50');
            }

            const minVal = gameState.highestBid ? gameState.highestBid.rankValue : 0;

            for (const t of [5, 6, 7, 8]) {
                const btn = document.getElementById(`bidTrickBtn-${t}`);
                const lbl = document.getElementById(`bidPtsLabel-${t}`);
                const val = getBidComparisonValue(t);
                btn.disabled = val <= minVal;
                lbl.textContent = `${getBidBasePoints(t)} pts`;
            }
        }

        function submitUserBid(tricks) {
            if (gameState.phase !== 'BIDDING' || gameState.currentTurn !== 0) return;

            const suit = gameState.selectedUserBidSuit || 'S';
            const rankValue = getBidComparisonValue(tricks);

            if (gameState.highestBid && rankValue <= gameState.highestBid.rankValue) return;

            const newBid = {
                bidder: 0,
                tricks,
                mode: 'HOKUM',
                suit,
                rankValue
            };

            gameState.highestBid = newBid;
            document.getElementById('biddingOverlay').classList.add('hidden');
            showBidBubble(0, formatBidText(newBid), true);
            sfxBid();
            updateScoreboardUI();

            gameState.biddingTurnsTaken++;
            gameState.currentTurn = (gameState.currentTurn + 1) % 4;
            proceedBiddingTurn();
        }

        function submitUserPass() {
            if (gameState.phase !== 'BIDDING' || gameState.currentTurn !== 0) return;
            // Cannot pass if first 3 players already passed
            if (gameState.biddingTurnsTaken === 3 && !gameState.highestBid) return;

            document.getElementById('biddingOverlay').classList.add('hidden');
            showBidBubble(0, 'Pass (بس)', false);
            playTone(250, 0.06);

            gameState.biddingTurnsTaken++;
            gameState.currentTurn = (gameState.currentTurn + 1) % 4;
            proceedBiddingTurn();
        }

        function finishBiddingPhase() {
            document.getElementById('biddingOverlay').classList.add('hidden');

            // Safety fallback if highestBid is somehow unset
            if (!gameState.highestBid) {
                const fallbackSuit = ['S', 'H', 'C', 'D'].sort((a, b) =>
                    gameState.hands[gameState.dealerIndex].filter(c => c.suit === b).length -
                    gameState.hands[gameState.dealerIndex].filter(c => c.suit === a).length
                )[0];
                gameState.highestBid = {
                    bidder: gameState.dealerIndex,
                    tricks: 5,
                    mode: 'HOKUM',
                    suit: fallbackSuit,
                    rankValue: getBidComparisonValue(5)
                };
            }

            gameState.contract = { ...gameState.highestBid };
            gameState.phase = 'PLAYING';

            // Re-sort user's hand with Hokum suit prioritized
            gameState.hands[0] = sortHand(gameState.hands[0], gameState.contract.suit);

            // Only keep the winning bidder's bubble visible briefly, then start Round 1
            for (let i = 0; i < 4; i++) {
                if (i !== gameState.contract.bidder) {
                    document.getElementById(`bidBubble-${i}`).classList.add('hidden');
                }
            }

            // Highest bidder leads the very first round!
            gameState.currentTurn = gameState.contract.bidder;
            gameState.trickCount = 1;

            updateScoreboardUI();
            renderAllHands();

            const bidderName = PLAYERS[gameState.contract.bidder].name;
            setStatusBanner(
                `${bidderName} won the bid with ${formatBidText(gameState.contract)} and leads Round 1!`,
                'Round 1 of 8'
            );

            proceedTrickTurn();
        }

        /********************************************************************
         * 5. ROUND-TAKING RULES, ON-DEMAND HINT ENGINE & AI PLAY
         ********************************************************************/
        function getValidCardsForPlayer(playerIndex) {
            const hand = gameState.hands[playerIndex];
            // Leading the round: any card in hand is legal
            if (gameState.currentTrick.length === 0) {
                return hand.map(c => c.id);
            }

            const ledSuit = gameState.ledSuit;
            if (!ledSuit) {
                return hand.map(c => c.id);
            }

            // Check if player holds non-Joker cards of the led suit
            const matchingSuitCards = hand.filter(c => !c.isJoker && c.suit === ledSuit);

            if (matchingSuitCards.length > 0) {
                // Legal cards are matching suit cards, plus Colored Joker which can always act as top trump
                const validIds = matchingSuitCards.map(c => c.id);
                const jokerCard = hand.find(c => c.isJoker);
                if (jokerCard) validIds.push(jokerCard.id);
                return validIds;
            }

            // Out of led suit: player may play any card (Hokum, Joker, or discard)
            return hand.map(c => c.id);
        }

        // Determine which played card currently wins the round
        function evaluateTrickWinner(trickPlays, ledSuit, contract) {
            const trumpSuit = contract ? contract.suit : null;
            let bestPlay = trickPlays[0];

            for (let i = 1; i < trickPlays.length; i++) {
                const candidate = trickPlays[i];
                if (doesCardBeat(candidate.card, bestPlay.card, ledSuit, trumpSuit)) {
                    bestPlay = candidate;
                }
            }
            return bestPlay;
        }

        function doesCardBeat(challenger, defender, ledSuit, trumpSuit) {
            // Colored Joker beats everything
            if (challenger.isJoker) return true;
            if (defender.isJoker) return false;

            // Hokum (Trump) beats non-trump
            if (trumpSuit) {
                const chalIsTrump = challenger.suit === trumpSuit;
                const defIsTrump = defender.suit === trumpSuit;
                if (chalIsTrump && !defIsTrump) return true;
                if (!chalIsTrump && defIsTrump) return false;
                if (chalIsTrump && defIsTrump) {
                    return challenger.power > defender.power;
                }
            }

            // Otherwise, only cards matching ledSuit can beat the current winner of ledSuit
            if (challenger.suit === defender.suit) {
                return challenger.power > defender.power;
            }
            if (ledSuit && challenger.suit === ledSuit && defender.suit !== ledSuit) {
                return true;
            }
            return false;
        }

        function proceedTrickTurn() {
            clearActiveHint();
            highlightActivePlayer(gameState.currentTurn);
            updateCallOutButtonUI();
            renderAllHands();

            const activePlayer = PLAYERS[gameState.currentTurn];
            const hintBtn = document.getElementById('btnHint');

            if (!activePlayer.isAI) {
                if (hintBtn) hintBtn.disabled = false;
                const hasMatchingSuit = gameState.ledSuit &&
                    gameState.hands[0].some(c => !c.isJoker && c.suit === gameState.ledSuit);

                const ledText = gameState.currentTrick.length === 0
                    ? 'Your lead! Play any card to start this round (or click 💡 Hint).'
                    : (hasMatchingSuit
                        ? `${SUITS[gameState.ledSuit].symbol} ${SUITS[gameState.ledSuit].name} led (highlighted in green) — you can play any card, but opponents may Call You Out if you skip suit!`
                        : `No ${SUITS[gameState.ledSuit].symbol} ${SUITS[gameState.ledSuit].name} in hand — play Hokum or discard any card!`);
                document.getElementById('userTurnHint').textContent = ledText;
            } else {
                if (hintBtn) hintBtn.disabled = true;
                document.getElementById('userTurnHint').textContent = `Waiting for ${activePlayer.shortName} to play a card...`;
                // 1000ms delay between AI turns so user can follow the action
                gameState.aiTimeoutId = setTimeout(() => {
                    executeAICardPlay(activePlayer.index);
                }, 1000);
            }
        }

        // Compute best card & explanation for any player (used by AI and On-Demand Hint)
        function computeBestCardAdvice(playerIndex) {
            const hand = gameState.hands[playerIndex];
            const validIds = getValidCardsForPlayer(playerIndex);
            const validCards = hand.filter(c => validIds.includes(c.id));
            const trumpSuit = gameState.contract ? gameState.contract.suit : null;

            if (validCards.length === 1) {
                const only = validCards[0];
                return {
                    card: only,
                    reason: `Play ${formatCardLabel(only)} — it is your only legal card following suit for this round.`
                };
            }

            // CASE 1: Leading the round
            if (gameState.currentTrick.length === 0) {
                // If player has non-Joker Aces, lead an Ace to secure an immediate round
                const nonJokerAces = validCards.filter(c => !c.isJoker && c.rank === 'A');
                if (nonJokerAces.length > 0) {
                    const trumpAce = trumpSuit ? nonJokerAces.find(c => c.suit === trumpSuit) : null;
                    if (trumpAce) {
                        return {
                            card: trumpAce,
                            reason: `Lead ${formatCardLabel(trumpAce)} to win the round and draw out opponents' Hokum cards.`
                        };
                    }
                    const sideAce = nonJokerAces[0];
                    return {
                        card: sideAce,
                        reason: `Lead your high ${formatCardLabel(sideAce)} to command this round right away.`
                    };
                }

                // If holding the Colored Joker, leading Joker wins immediately and pulls Hokum
                const joker = validCards.find(c => c.isJoker);
                if (joker && trumpSuit) {
                    return {
                        card: joker,
                        reason: `Lead the 🃏 Colored Joker — it beats every card in the deck and forces opponents to follow ${SUITS[trumpSuit].name} Hokum!`
                    };
                }

                // Otherwise lead a low probe card to force out opponents' Aces and protect our Kings/Queens
                const nonTrumps = validCards.filter(c => !c.isJoker && c.suit !== trumpSuit);
                const pool = nonTrumps.length > 0 ? nonTrumps : validCards;
                const lowest = [...pool].sort((a, b) => a.power - b.power)[0];
                return {
                    card: lowest,
                    reason: `Lead low with ${formatCardLabel(lowest)} to probe the round while saving your higher cards.`
                };
            }

            // CASE 2: Following in an active round
            const currentBest = evaluateTrickWinner(gameState.currentTrick, gameState.ledSuit, gameState.contract);
            const partnerIsWinning = PLAYERS[currentBest.playerIndex].team === PLAYERS[playerIndex].team;
            const winningOptions = validCards.filter(c =>
                doesCardBeat(c, currentBest.card, gameState.ledSuit, trumpSuit)
            );

            // If partner is already winning with a strong card (Ace, King, or Joker)
            if (partnerIsWinning && (currentBest.card.power >= 7 || gameState.currentTrick.length === 3)) {
                const nonTrumps = validCards.filter(c => !c.isJoker && c.suit !== trumpSuit);
                const pool = nonTrumps.length > 0 ? nonTrumps : validCards;
                const lowSupport = [...pool].sort((a, b) => a.power - b.power)[0];
                return {
                    card: lowSupport,
                    reason: `Your partner Saad is already winning this round with ${formatCardLabel(currentBest.card)}. Play ${formatCardLabel(lowSupport)} to save your stronger cards.`
                };
            }

            // We want to win the round if possible
            if (winningOptions.length > 0) {
                const nonJokerWinners = winningOptions.filter(c => !c.isJoker);
                if (nonJokerWinners.length > 0) {
                    const chosen = gameState.currentTrick.length === 3
                        ? [...nonJokerWinners].sort((a, b) => a.power - b.power)[0]
                        : [...nonJokerWinners].sort((a, b) => b.power - a.power)[0];
                    const isTrumpCut = trumpSuit && chosen.suit === trumpSuit && gameState.ledSuit !== trumpSuit;
                    return {
                        card: chosen,
                        reason: isTrumpCut
                            ? `Cut in with Hokum ${formatCardLabel(chosen)} to overtake ${formatCardLabel(currentBest.card)} and win this round!`
                            : `Play ${formatCardLabel(chosen)} to beat ${formatCardLabel(currentBest.card)} and win this round for your team.`
                    };
                } else {
                    const opponentHasHighCard = currentBest.card.power >= 7;
                    if (opponentHasHighCard || validCards.length <= 3) {
                        return {
                            card: winningOptions[0],
                            reason: `Play the 🃏 Colored Joker now to capture this round from ${PLAYERS[currentBest.playerIndex].shortName}!`
                        };
                    }
                }
            }

            // Cannot win (or saving Joker): discard lowest rank card
            const discard = [...validCards].filter(c => !c.isJoker).sort((a, b) => a.power - b.power)[0] || validCards[0];
            return {
                card: discard,
                reason: `You cannot profitably win this round — discard your low ${formatCardLabel(discard)} and save your stronger cards.`
            };
        }

        function formatCardLabel(card) {
            if (!card) return '';
            if (card.isJoker) return '🃏 Colored Joker';
            const rankLabel = card.rank === 'J' ? 'J (Jester)' : card.rank;
            return `${rankLabel}${SUITS[card.suit].symbol}`;
        }

        function requestPlayHint() {
            if (gameState.phase !== 'PLAYING' || gameState.currentTurn !== 0) return;
            const advice = computeBestCardAdvice(0);
            if (!advice || !advice.card) return;

            gameState.activeHintCardId = advice.card.id;
            const banner = document.getElementById('hintBanner');
            const bannerText = document.getElementById('hintBannerText');
            bannerText.textContent = advice.reason;
            banner.classList.remove('hidden');

            playTone(660, 0.08, 'sine', 0.09);
            renderAllHands();
        }

        function clearActiveHint() {
            gameState.activeHintCardId = null;
            const banner = document.getElementById('hintBanner');
            if (banner) banner.classList.add('hidden');
        }

        function handleUserCardClick(cardId) {
            if (gameState.phase !== 'PLAYING' || gameState.currentTurn !== 0) return;

            // User is never forced to play a specific card: every card in hand is playable
            const existsInHand = gameState.hands[0].some(c => c.id === cardId);
            if (!existsInHand) return;

            clearActiveHint();
            playCardFromHand(0, cardId);
        }

        function executeAICardPlay(playerIndex) {
            const hand = gameState.hands[playerIndex];
            const isOpponent = PLAYERS[playerIndex].team === 1;
            const trumpSuit = gameState.contract ? gameState.contract.suit : null;

            // Occasionally (~22% chance when an AI Opponent holds only a weak card of the led suit),
            // let the AI Opponent attempt a sneaky revoke (play Hokum or off-suit discard while secretly holding ledSuit)
            // so the user can catch them with the "🚨 Call Out" button!
            if (
                isOpponent &&
                gameState.currentTrick.length > 0 &&
                gameState.ledSuit &&
                hand.length >= 2
            ) {
                const matchingSuitCards = hand.filter(c => !c.isJoker && c.suit === gameState.ledSuit);
                const offSuitCards = hand.filter(c => !c.isJoker && c.suit !== gameState.ledSuit);
                const highestMatchingPower = matchingSuitCards.reduce((max, c) => Math.max(max, c.power), 0);

                if (matchingSuitCards.length > 0 && offSuitCards.length > 0 && highestMatchingPower <= 5 && Math.random() < 0.22) {
                    const trumpOptions = trumpSuit ? offSuitCards.filter(c => c.suit === trumpSuit) : [];
                    const sneakyCard = trumpOptions.length > 0
                        ? trumpOptions.sort((a, b) => a.power - b.power)[0]
                        : offSuitCards.sort((a, b) => a.power - b.power)[0];
                    playCardFromHand(playerIndex, sneakyCard.id);
                    return;
                }
            }

            const advice = computeBestCardAdvice(playerIndex);
            playCardFromHand(playerIndex, advice.card.id);
        }

        function playCardFromHand(playerIndex, cardId) {
            const hand = gameState.hands[playerIndex];
            const cardIdx = hand.findIndex(c => c.id === cardId);
            if (cardIdx === -1) return;

            const [playedCard] = hand.splice(cardIdx, 1);
            const wasLeading = gameState.currentTrick.length === 0;

            // Set ledSuit if this is the first card of the round (Joker led in Hokum demands the Hokum suit)
            if (wasLeading) {
                if (playedCard.isJoker) {
                    gameState.ledSuit = gameState.contract.suit;
                } else {
                    gameState.ledSuit = playedCard.suit;
                }
            }

            gameState.currentTrick.push({ playerIndex, card: playedCard });
            sfxCardPlay(playedCard.isJoker);

            const suitPill = `${SUITS[gameState.ledSuit].symbol} ${SUITS[gameState.ledSuit].name} Led`;
            setStatusBanner(`${PLAYERS[playerIndex].shortName} played ${formatCardLabel(playedCard)}`, suitPill);

            renderAllHands();
            renderTrickArena();

            // Check for Call-Out / Suit-Following triggers
            if (checkAndHandleSuitCallOut(playerIndex, playedCard, wasLeading)) {
                return; // Call-Out inspection interrupted the hand!
            }

            // Check if all 4 players have played
            if (gameState.currentTrick.length === 4) {
                resolveCompletedTrick();
            } else {
                gameState.currentTurn = (gameState.currentTurn + 1) % 4;
                proceedTrickTurn();
            }
        }

        function resolveCompletedTrick() {
            const winningPlay = evaluateTrickWinner(gameState.currentTrick, gameState.ledSuit, gameState.contract);
            const winnerIndex = winningPlay.playerIndex;
            const winningTeam = PLAYERS[winnerIndex].team;

            // Highlight winning card in the center arena
            const winnerSlot = document.getElementById(`trickSlot-${winnerIndex}`);
            if (winnerSlot && winnerSlot.firstElementChild) {
                winnerSlot.firstElementChild.classList.add('winning-trick-card');
            }

            setStatusBanner(
                `${PLAYERS[winnerIndex].name} wins Round ${gameState.trickCount} with ${formatCardLabel(winningPlay.card)}!`,
                `Round ${gameState.trickCount} Winner`
            );
            sfxTrickWin(winningTeam === 0);

            gameState.aiTimeoutId = setTimeout(() => {
                // Save completed round for "Last Round" inspection and full end-of-hand review
                const completedRoundRecord = {
                    trickNumber: gameState.trickCount,
                    ledSuit: gameState.ledSuit,
                    winnerIndex: winnerIndex,
                    winnerName: PLAYERS[winnerIndex].name,
                    winnerTeam: winningTeam,
                    winningCardId: winningPlay.card.id,
                    interrupted: false,
                    plays: gameState.currentTrick.map((p, idx) => ({
                        order: idx + 1,
                        playerIndex: p.playerIndex,
                        playerName: PLAYERS[p.playerIndex].name,
                        shortName: PLAYERS[p.playerIndex].shortName,
                        team: PLAYERS[p.playerIndex].team,
                        card: { ...p.card }
                    }))
                };
                gameState.lastCompletedTrick = completedRoundRecord;
                gameState.handHistory.push(completedRoundRecord);

                gameState.teamTricks[winningTeam] += 1;
                gameState.currentTrick = [];
                gameState.ledSuit = null;
                gameState.suspectOpponentPlay = null;
                updateCallOutButtonUI();
                renderTrickArena();
                updateScoreboardUI();

                // Check if all 8 rounds of the hand have been completed
                if (gameState.trickCount >= 8) {
                    finishHandScoring();
                } else {
                    gameState.trickCount += 1;
                    gameState.currentTurn = winnerIndex; // Winner of the round leads the next round
                    setStatusBanner(
                        `${PLAYERS[winnerIndex].name} leads Round ${gameState.trickCount} of 8`,
                        `Round ${gameState.trickCount} of 8`
                    );
                    proceedTrickTurn();
                }
            }, 1350);
        }

        function finishHandScoring() {
            gameState.phase = 'ROUND_OVER';
            const contract = gameState.contract;
            const biddingTeam = PLAYERS[contract.bidder].team;
            const defendingTeam = biddingTeam === 0 ? 1 : 0;
            const tricksWonByBidders = gameState.teamTricks[biddingTeam];
            const basePts = getBidBasePoints(contract.tricks);

            const contractMade = tricksWonByBidders >= contract.tricks;
            const teamDeltas = [0, 0];
            let explanation = '';

            if (contractMade) {
                teamDeltas[biddingTeam] = basePts;
                explanation = `${PLAYERS[contract.bidder].name}'s partnership won ${tricksWonByBidders}/8 rounds (Target: ${contract.tricks}) and earned +${basePts} contract points!`;
            } else {
                const kasrPenalty = basePts * 2;
                teamDeltas[defendingTeam] = kasrPenalty;
                explanation = `Kasr (كسر)! ${PLAYERS[contract.bidder].name}'s partnership won only ${tricksWonByBidders}/8 rounds (Target: ${contract.tricks}). The defending team scores double (+${kasrPenalty} pts)!`;
            }

            gameState.matchScores[0] += teamDeltas[0];
            gameState.matchScores[1] += teamDeltas[1];
            updateScoreboardUI();

            const isMatchOver = gameState.matchScores[0] >= 101 || gameState.matchScores[1] >= 101;
            if (isMatchOver) {
                gameState.phase = 'MATCH_OVER';
            }

            showRoundSummaryModal({
                biddingTeam,
                contractMade,
                teamDeltas,
                explanation,
                isMatchOver
            });
        }

        /********************************************************************
         * 5B. CALL-OUT & HAND INSPECTION SYSTEM (كشف الورق)
         ********************************************************************/
        function updateCallOutButtonUI() {
            const btn = document.getElementById('btnCallOut');
            const label = document.getElementById('callOutBtnLabel');
            if (!btn || !label) return;

            if (gameState.phase === 'PLAYING' && gameState.suspectOpponentPlay) {
                const suspect = PLAYERS[gameState.suspectOpponentPlay.playerIndex];
                const suitSym = SUITS[gameState.suspectOpponentPlay.suitSkipped].symbol;
                btn.disabled = false;
                label.textContent = `🚨 Call Out ${suspect.shortName} (${suitSym})`;
            } else {
                btn.disabled = true;
                label.textContent = '🚨 Call Out';
            }
        }

        function checkAndHandleSuitCallOut(playerIndex, playedCard, wasLeading) {
            const remainingHand = gameState.hands[playerIndex];
            const activeLedSuit = gameState.ledSuit;
            const trumpSuit = gameState.contract ? gameState.contract.suit : null;

            // 1. Check if the User (South) just played a suit they previously skipped earlier in the hand!
            if (playerIndex === 0 && !playedCard.isJoker && gameState.claimedVoidSuits[0].has(playedCard.suit)) {
                // Low chance (15%) that opponents remember and catch you playing a previously skipped suit
                if (Math.random() < 0.15) {
                    const accuserIdx = Math.random() < 0.5 ? 1 : 3;
                    triggerCallOutResolution({
                        accuserIndex: accuserIdx,
                        accusedIndex: 0,
                        disputedSuit: playedCard.suit,
                        offendingCards: [playedCard],
                        accuserWasRight: true,
                        reasonText: `${PLAYERS[accuserIdx].name} caught you playing ${formatCardLabel(playedCard)} after you previously skipped ${SUITS[playedCard.suit].symbol} ${SUITS[playedCard.suit].name} earlier in this hand!`
                    });
                    return true;
                }
            }

            // 2. If following in a round where a suit (S, H, C, D) was led, check if player failed to follow suit
            if (!wasLeading && activeLedSuit) {
                const followedSuit = (!playedCard.isJoker && playedCard.suit === activeLedSuit) ||
                    (playedCard.isJoker && trumpSuit === activeLedSuit);

                if (!followedSuit && !playedCard.isJoker) {
                    const hiddenSuitCards = remainingHand.filter(c => !c.isJoker && c.suit === activeLedSuit);
                    const actuallyHadSuit = hiddenSuitCards.length > 0;

                    if (playerIndex === 0) {
                        // Record that User claimed void in activeLedSuit
                        gameState.claimedVoidSuits[0].add(activeLedSuit);

                        if (actuallyHadSuit) {
                            // User is hiding the led suit! Keep AI detection rate very low (12% if trumping, 6% if discarding)
                            const playedTrump = trumpSuit && playedCard.suit === trumpSuit;
                            const callChance = playedTrump ? 0.12 : 0.06;
                            if (Math.random() < callChance) {
                                const accuserIdx = Math.random() < 0.5 ? 1 : 3;
                                triggerCallOutResolution({
                                    accuserIndex: accuserIdx,
                                    accusedIndex: 0,
                                    disputedSuit: activeLedSuit,
                                    offendingCards: hiddenSuitCards,
                                    accuserWasRight: true,
                                    reasonText: `${PLAYERS[accuserIdx].name} challenged your ${formatCardLabel(playedCard)} and demanded a hand inspection for ${SUITS[activeLedSuit].symbol} ${SUITS[activeLedSuit].name}!`
                                });
                                return true;
                            }
                        } else {
                            // User genuinely does NOT have the led suit!
                            // If User trumped an opponent's card, give AI an 18% chance to get suspicious and make a FALSE accusation!
                            const playedTrump = trumpSuit && playedCard.suit === trumpSuit;
                            if (playedTrump && remainingHand.length >= 2 && Math.random() < 0.18) {
                                const accuserIdx = Math.random() < 0.5 ? 1 : 3;
                                triggerCallOutResolution({
                                    accuserIndex: accuserIdx,
                                    accusedIndex: 0,
                                    disputedSuit: activeLedSuit,
                                    offendingCards: [],
                                    accuserWasRight: false,
                                    reasonText: `${PLAYERS[accuserIdx].name} got suspicious when you played Hokum ${formatCardLabel(playedCard)} and accused you of hiding ${SUITS[activeLedSuit].symbol} ${SUITS[activeLedSuit].name}!`
                                });
                                return true;
                            }
                        }
                    } else if (PLAYERS[playerIndex].team === 1) {
                        // An AI Opponent (East or West) played off-suit! Enable the User's "🚨 Call Out" button
                        gameState.suspectOpponentPlay = {
                            playerIndex,
                            suitSkipped: activeLedSuit,
                            actuallyHadSuit,
                            offendingCards: [...hiddenSuitCards],
                            cardPlayed: playedCard,
                            roundNum: gameState.trickCount
                        };
                        updateCallOutButtonUI();
                    }
                }
            }
            return false;
        }

        function userTriggerCallOut() {
            if (gameState.phase !== 'PLAYING' || !gameState.suspectOpponentPlay) return;
            clearTimeout(gameState.aiTimeoutId);

            const suspect = gameState.suspectOpponentPlay;
            const currentHidden = gameState.hands[suspect.playerIndex].filter(
                c => !c.isJoker && c.suit === suspect.suitSkipped
            );
            const wasRight = suspect.actuallyHadSuit || currentHidden.length > 0;
            const proofCards = currentHidden.length > 0 ? currentHidden : suspect.offendingCards;

            triggerCallOutResolution({
                accuserIndex: 0,
                accusedIndex: suspect.playerIndex,
                disputedSuit: suspect.suitSkipped,
                offendingCards: proofCards,
                accuserWasRight: wasRight,
                reasonText: `You called out ${PLAYERS[suspect.playerIndex].name} for playing ${formatCardLabel(suspect.cardPlayed)} instead of following ${SUITS[suspect.suitSkipped].symbol} ${SUITS[suspect.suitSkipped].name}!`
            });
        }

        function triggerCallOutResolution({ accuserIndex, accusedIndex, disputedSuit, offendingCards, accuserWasRight, reasonText }) {
            clearTimeout(gameState.aiTimeoutId);
            gameState.phase = 'ROUND_OVER';
            gameState.suspectOpponentPlay = null;
            updateCallOutButtonUI();

            // Record the interrupted round in handHistory so the user can review it as well
            if (gameState.currentTrick.length > 0) {
                gameState.handHistory.push({
                    trickNumber: gameState.trickCount,
                    ledSuit: gameState.ledSuit,
                    winnerIndex: null,
                    winnerName: `Interrupted by Call-Out (${PLAYERS[accuserIndex].shortName} → ${PLAYERS[accusedIndex].shortName})`,
                    winnerTeam: accuserWasRight ? PLAYERS[accuserIndex].team : PLAYERS[accusedIndex].team,
                    winningCardId: null,
                    interrupted: true,
                    plays: gameState.currentTrick.map((p, idx) => ({
                        order: idx + 1,
                        playerIndex: p.playerIndex,
                        playerName: PLAYERS[p.playerIndex].name,
                        shortName: PLAYERS[p.playerIndex].shortName,
                        team: PLAYERS[p.playerIndex].team,
                        card: { ...p.card }
                    }))
                });
            }

            const accuserTeam = PLAYERS[accuserIndex].team;
            const accusedTeam = PLAYERS[accusedIndex].team;
            const winningTeam = accuserWasRight ? accuserTeam : accusedTeam;
// ... existing code ... -->
        function handleNextRoundOrMatch() {
            document.getElementById('roundSummaryModal').classList.add('hidden');
            document.getElementById('handReviewModal').classList.add('hidden');
            if (gameState.phase === 'MATCH_OVER') {
                startNewMatch();
            } else {
                gameState.handNumber += 1;
                gameState.dealerIndex = 3;
                startRound();
            }
        }

        function openHandReviewModal() {
            const modal = document.getElementById('handReviewModal');
            const listEl = document.getElementById('handReviewList');
            const subtitleEl = document.getElementById('reviewModalSubtitle');

            if (gameState.contract) {
                const callerName = PLAYERS[gameState.contract.bidder].name;
                subtitleEl.textContent = `Hand #${gameState.handNumber} • Contract: ${formatBidText(gameState.contract)} by ${callerName}`;
            } else {
                subtitleEl.textContent = `Hand #${gameState.handNumber} Play-by-Play History`;
            }

            if (!gameState.handHistory || gameState.handHistory.length === 0) {
                listEl.innerHTML = `<div class="text-center text-xs sm:text-sm text-slate-400 py-8">No rounds were recorded for this hand.</div>`;
            } else {
                const orderLabels = ['1st (Lead)', '2nd', '3rd', '4th'];

                listEl.innerHTML = gameState.handHistory.map(round => {
                    const ledSuitInfo = round.ledSuit ? SUITS[round.ledSuit] : null;
                    const ledBadge = ledSuitInfo
                        ? `<span class="px-2 py-0.5 rounded-full bg-slate-800 border border-slate-700 text-[10px] sm:text-xs font-semibold ${ledSuitInfo.badgeColor}">Led: ${ledSuitInfo.symbol} ${ledSuitInfo.name}</span>`
                        : '';

                    const winnerBadgeClass = round.interrupted
                        ? 'bg-rose-500/20 border-rose-400/50 text-rose-300'
                        : (round.winnerTeam === 0
                            ? 'bg-emerald-500/20 border-emerald-400/50 text-emerald-300'
                            : 'bg-rose-500/20 border-rose-400/50 text-rose-300');

                    const winnerIcon = round.interrupted ? '🚨' : '🏆';

                    const playsHTML = round.plays.map((p, idx) => {
                        const isWinningCard = !round.interrupted && round.winningCardId === p.card.id;
                        const teamTagColor = p.team === 0 ? 'text-emerald-300' : 'text-rose-300';
                        const cardBorderClass = isWinningCard
                            ? 'border-2 border-amber-400 bg-amber-500/10 shadow-lg ring-2 ring-amber-400/40'
                            : 'border border-slate-800 bg-slate-900/90';

                        return `
                            <div class="rounded-xl p-2 sm:p-2.5 flex flex-col items-center justify-between ${cardBorderClass} relative">
                                <div class="w-full flex items-center justify-between text-[9px] sm:text-[10px] mb-1 pb-1 border-b border-slate-800/80">
                                    <span class="px-1.5 py-0.2 rounded bg-slate-800 text-amber-300 font-bold">${orderLabels[idx] || `${p.order}th`}</span>
                                    ${isWinningCard ? `<span class="text-[9px] sm:text-[10px] font-extrabold text-amber-300">★ Won</span>` : ''}
                                </div>
                                <div class="text-[11px] sm:text-xs font-bold ${teamTagColor} mb-1.5 text-center truncate w-full">
                                    ${p.playerName}
                                </div>
                                <div class="my-0.5">
                                    ${createCardHTML(p.card, false, false, false, true)}
                                </div>
                            </div>
                        `;
                    }).join('');

                    return `
                        <div class="bg-slate-950/85 border border-slate-800 rounded-2xl p-3 sm:p-4 shadow-md">
                            <div class="flex flex-wrap items-center justify-between gap-2 pb-2.5 mb-3 border-b border-slate-800/90">
                                <div class="flex items-center gap-2">
                                    <span class="px-2.5 py-0.5 rounded-lg bg-amber-500/20 border border-amber-400/40 text-amber-300 font-cinzel font-bold text-xs sm:text-sm">
                                        Round ${round.trickNumber}
                                    </span>
                                    ${ledBadge}
                                </div>
                                <div class="px-2.5 py-0.5 rounded-lg border text-[11px] sm:text-xs font-bold ${winnerBadgeClass}">
                                    ${winnerIcon} ${round.interrupted ? round.winnerName : `Won by ${round.winnerName}`}
                                </div>
                            </div>
                            <div class="grid grid-cols-2 sm:grid-cols-4 gap-2 sm:gap-3">
                                ${playsHTML}
                            </div>
                        </div>
                    `;
                }).join('');
            }

            modal.classList.remove('hidden');
        }

        function closeHandReviewModal() {
            document.getElementById('handReviewModal').classList.add('hidden');
        }

        function startNewMatch() {
// ... existing code ... -->
