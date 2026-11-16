[Orbit_Frontier_English.html](https://github.com/user-attachments/files/34891825/Orbit_Frontier_English.html)
!DOCTYPE html>
<html class="dark" lang="en">
<head>
  <meta charset="utf-8">
  <meta content="width=device-width, initial-scale=1.0" name="viewport">
  <title>Space Advance: Orbital Survival • Orbit Frontier</title>
  <link href="https://fonts.googleapis.com" rel="preconnect">
  <link crossorigin="" href="https://fonts.gstatic.com" rel="preconnect">
  <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@400;500;600;700;800&family=Press+Start+2P&family=Space+Grotesk:wght@500;600;700&display=swap" rel="stylesheet">
  <link href="https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined:wght,FILL@100..700,0..1&display=swap" rel="stylesheet">
  <script src="https://cdn.tailwindcss.com?plugins=forms,container-queries"></script>
  <script>
    tailwind.config = {
      darkMode: "class",
      theme: {
        extend: {
          colors: {
            surface: "#181114",
            "surface-low": "#211a1d",
            "surface-card": "#261e23",
            "surface-high": "#32292e",
            "surface-border": "#43363d",
            primary: "#ff6b4a",
            "primary-light": "#ffb4a3",
            cyan: "#7bd0ff",
            amber: "#fbbf24",
            emerald: "#34d399",
            rose: "#f87171",
            purple: "#c084fc",
            food: "#bf8c15",
            "food-light": "#e0a82e",
            "food-dark": "#a17410",
            energy: "#4f889e",
            "energy-light": "#69a7bf",
            "energy-dark": "#3e6d80",
            rad: "#6bfa05",
            "rad-light": "#88ff33",
            "rad-dark": "#55c904"
          },
          fontFamily: {
            sans: ["Outfit", "sans-serif"],
            mono: ["Space Grotesk", "monospace"],
            pixel: ["'Press Start 2P'", "monospace"]
          }
        }
      }
    };
  </script>
  <style>
    body {
      background-color: #070918;
      color: #eedfe3;
      font-family: 'Outfit', sans-serif;
      user-select: none;
    }
    /* Camada fixa de plano de fundo espacial pixel art visível durante todo o jogo */
    #space-bg-layer {
      position: fixed;
      inset: 0;
      z-index: 0;
      pointer-events: none;
      background-color: #070918;
      background-image:
        linear-gradient(180deg, rgba(7, 9, 24, 0.28) 0%, rgba(7, 9, 24, 0.42) 100%),
        url('./Gemini_Generated_Image_u9wdshu9wdshu9wd.jfif');
      background-size: cover;
      background-position: right center;
      background-repeat: no-repeat;
      image-rendering: pixelated;
    }
    [hidden] { display: none !important; }
    .font-mono-code { font-family: 'Space Grotesk', monospace; }
    .font-pixel { font-family: 'Press Start 2P', monospace; }
    ::-webkit-scrollbar { width: 5px; height: 5px; }
    ::-webkit-scrollbar-track { background: #0a0d1f; }
    ::-webkit-scrollbar-thumb { background: #3b3236; border-radius: 3px; }
    
    /* Dynamic Screen Flashes for Space Weather and Clicks */
    @keyframes alert-pulse {
      0%, 100% { box-shadow: inset 0 0 0 0 rgba(248, 113, 113, 0); }
      50% { box-shadow: inset 0 0 45px 8px rgba(248, 113, 113, 0.45); }
    }
    .screen-alert {
      animation: alert-pulse 1.2s infinite;
    }
    
    /* Floating action numbers */
    .float-text {
      position: fixed;
      pointer-events: none;
      font-weight: 700;
      font-family: 'Space Grotesk', monospace;
      animation: floatUp 0.85s cubic-bezier(0.1, 0.8, 0.3, 1) forwards;
      z-index: 60;
      text-shadow: 0 2px 6px rgba(0, 0, 0, 0.85);
    }
    @keyframes floatUp {
      0% { opacity: 1; transform: translate(-50%, 0) scale(1); }
      50% { transform: translate(-50%, -24px) scale(1.15); }
      100% { opacity: 0; transform: translate(-50%, -46px) scale(0.9); }
    }

    .click-card:active:not(:disabled),
    .key-pressed {
      transform: scale(0.96);
    }

    /* Destaque visual para guiar crianças quando falta algum recurso */
    @keyframes guide-glow {
      0%, 100% { box-shadow: 0 0 0 0 rgba(251, 191, 36, 0.2); border-color: #fbbf24; }
      50% { box-shadow: 0 0 18px 4px rgba(251, 191, 36, 0.65); border-color: #fde047; }
    }
    .kid-guide-pulse {
      animation: guide-glow 1.1s infinite;
    }

    /* Retro Pixel Menu Effects */
    .pixel-title-glow {
      color: #ffd000;
      text-shadow:
        0 4px 0 #d95b00,
        0 0 18px rgba(255, 190, 0, 0.75);
    }
    .pixel-option-active {
      color: #ffd000;
      text-shadow: 0 0 12px rgba(255, 208, 0, 0.85);
    }
    .pixel-box {
      background-color: #050712;
      border: 2px solid #f59e0b;
      box-shadow: 0 4px 0 #b45309, 0 0 15px rgba(245, 158, 11, 0.25);
    }
    .scanlines {
      background: linear-gradient(
        rgba(18, 16, 16, 0) 50%, 
        rgba(0, 0, 0, 0.25) 50%
      );
      background-size: 100% 4px;
    }
  </style>
</head>
<!--
  ============================================================================
  ARQUITETURA DO PROJETO - AVANÇO ESPACIAL: SOBREVIVÊNCIA ORBITAL (ORBIT FRONTIER)
  ============================================================================
  1. MENU PIXEL ART RETRO (ORBIT EXPLORE • DEEP SPACE ODYSSEY • 1994):
     - Tela inicial interativa com canvas de estrelas + estrela cadente diagonal,
       navegação por setas/enter e mouse, música procedural chiptune e opções.
  2. SEMÂNTICA DE CORES & ALERTAS NA BARRA SUPERIOR:
     - Green = Success / Health saudável; Yellow = Caution / Guia; Vermelho = Erro / Danger.
     - Alertas de status e Clima Espacial (DONKI) integrados no cabeçalho fixo.
  3. OTIMIZAÇÃO PARA CRIANÇAS (7 A 12 ANOS):
     - Linguagem simples, guia visual pulsante nos botões necessários e dicas claras.
  ============================================================================
-->
<body class="min-h-screen flex flex-col antialiased selection:bg-primary selection:text-white transition-all duration-300 relative" id="game-body">

  <!-- PLANO DE FUNDO ESPACIAL PIXEL ART (IMAGEM + FALLBACK PROCEDURAL) -->
  <div id="space-bg-layer">
    <canvas class="w-full h-full block opacity-0 transition-opacity duration-300" id="space-bg-canvas"></canvas>
  </div>

  <!-- ==================== TELA INICIAL: MENU RETRO PIXEL ART (ORBIT EXPLORE) ==================== -->
  <div class="fixed inset-0 z-50 bg-[#03050c] flex flex-col justify-between overflow-hidden" id="retro-menu-screen">
    <!-- Canvas Estrelado & Estrela Cadente -->
    <canvas class="absolute inset-0 w-full h-full pointer-events-none" id="menu-starfield"></canvas>
    <div class="absolute inset-0 scanlines pointer-events-none opacity-40"></div>

    <!-- Faixa Superior Estilo Retro & Botão de Música -->
    <div class="relative z-10 flex flex-wrap items-center justify-between gap-2 px-4 sm:px-6 pt-3">
      <div class="font-pixel text-[9px] sm:text-[11px] text-neutral-300 tracking-wider leading-relaxed">
        SPACE MISSION • EARTH, MOON, SUN & MARS • AGES 7-12
      </div>
      <button class="pixel-box px-3 py-2 flex items-center gap-2 font-pixel text-[10px] sm:text-xs text-amber hover:brightness-110 active:translate-y-0.5 transition cursor-pointer" id="btn-menu-music" onclick="toggleMenuMusic()" type="button">
        <span class="inline-flex items-end gap-0.5 h-3" id="music-bars">
          <span class="w-1 h-2 bg-emerald-400 animate-pulse"></span>
          <span class="w-1 h-3 bg-emerald-400 animate-bounce"></span>
          <span class="w-1 h-1.5 bg-emerald-400 animate-pulse"></span>
        </span>
        <span class="material-symbols-outlined text-[15px] text-amber" id="menu-music-icon">volume_up</span>
        <span id="menu-music-label">MUSIC: ENGAGED</span>
      </button>
    </div>

    <!-- Centro Esquerdo: Título Principal & Opções de Menu -->
    <div class="relative z-10 max-w-6xl w-full mx-auto px-6 sm:px-12 my-auto">
      <div class="max-w-xl">
        <h1 class="font-pixel text-3xl sm:text-5xl md:text-6xl leading-tight pixel-title-glow tracking-wide">
          ORBIT<br>EXPLORE
        </h1>
        <div class="flex items-center gap-2.5 mt-4 font-pixel text-[10px] sm:text-xs text-amber-200 tracking-widest">
          <span class="w-2.5 h-2.5 bg-amber inline-block"></span>
          <span>DEEP SPACE ODYSSEY • 1994</span>
        </div>

        <!-- Opções Principais (START, SETTINGS, EXIT) -->
        <nav class="mt-10 sm:mt-12 flex flex-col items-start gap-5 font-pixel text-sm sm:text-lg" id="menu-options-list">
          <button class="menu-opt flex items-center gap-3 pixel-option-active transition cursor-pointer text-left" data-index="0" onclick="startFromMenu()" onmouseenter="selectMenuIndex(0)" type="button">
            <span class="menu-arrow text-amber">►</span>
            <span>START</span>
          </button>
          <button class="menu-opt flex items-center gap-3 text-slate-300 hover:text-amber transition cursor-pointer text-left" data-index="1" onclick="openMenuSettings()" onmouseenter="selectMenuIndex(1)" type="button">
            <span class="menu-arrow opacity-0 text-amber">►</span>
            <span>SETTINGS</span>
          </button>
          <button class="menu-opt flex items-center gap-3 text-slate-400 hover:text-rose-400 transition cursor-pointer text-left" data-index="2" onclick="openMenuExit()" onmouseenter="selectMenuIndex(2)" type="button">
            <span class="menu-arrow opacity-0 text-amber">►</span>
            <span>EXIT</span>
          </button>
        </nav>

        <!-- Resumo da Missão Selecionada -->
        <div class="mt-8 inline-flex flex-wrap items-center gap-3 px-3 py-2 rounded bg-white/5 border border-white/10 font-mono-code text-xs text-neutral-300">
          <span>🪐 Planet: <strong class="text-amber" id="menu-preview-dest">MARS (ARES)</strong></span>
          <span>•</span>
          <span>⭐ Mode: <strong class="text-emerald-400" id="menu-preview-diff">EXPLORER (AGES 7-12)</strong></span>
        </div>
      </div>
    </div>

    <!-- Painel Pixel Art de SETTINGS (Configurações para Crianças) -->
    <div class="hidden fixed inset-0 z-20 bg-black/80 backdrop-blur-sm flex items-center justify-center p-4" id="menu-settings-panel">
      <div class="pixel-box max-w-md w-full p-5 sm:p-6 flex flex-col gap-5">
        <div class="flex items-center justify-between border-b border-amber/40 pb-3">
          <h2 class="font-pixel text-xs sm:text-sm text-amber">SETTINGS • MISSION</h2>
          <button class="font-pixel text-xs text-rose-400 hover:text-white cursor-pointer" onclick="closeMenuSettings()" type="button">[X]</button>
        </div>

        <!-- Escolha de Planeta -->
        <div class="space-y-2">
          <label class="font-pixel text-[10px] text-cyan block">1. MISSION DESTINATION:</label>
          <div class="grid grid-cols-1 gap-2.5 font-mono-code text-xs">
            <button class="p-2.5 rounded border-2 border-amber bg-amber/20 text-white font-bold transition cursor-pointer text-left flex items-center justify-between" id="set-dest-mars" onclick="setMenuDestination(0)" type="button">
              <span>🔴 MARS (ARES PLANITIA)</span>
              <span class="text-[10px] text-emerald-400 font-pixel">ATIVO</span>
            </button>
          </div>
        </div>

        <!-- Escolha de Dificuldade -->
        <div class="space-y-2">
          <label class="font-pixel text-[10px] text-emerald-400 block">2. DIFFICULTY LEVEL:</label>
          <div class="grid grid-cols-2 gap-2.5 font-mono-code text-xs">
            <button class="p-2.5 rounded border-2 border-emerald-400 bg-emerald-500/20 text-white font-bold transition cursor-pointer" id="set-diff-easy" onclick="setMenuDifficulty('easy')" type="button">
              🌟 EXPLORADOR (Easy)
            </button>
            <button class="p-2.5 rounded border border-neutral-600 bg-neutral-900 text-neutral-300 font-bold transition cursor-pointer" id="set-diff-normal" onclick="setMenuDifficulty('normal')" type="button">
              🚀 ASTRONAUT (Normal)
            </button>
          </div>
          <p class="text-[11px] text-neutral-300 font-sans">
            In Explorer mode <strong>Explorer</strong>, resources last longer, and glowing visual hints help you manage your base!
          </p>
        </div>

        <button class="w-full py-3 bg-emerald-500 hover:bg-emerald-400 text-black font-pixel text-xs rounded transition cursor-pointer" onclick="closeMenuSettings()" type="button">
          SAVE & RETURN
        </button>
      </div>
    </div>

    <!-- Painel Pixel Art de EXIT -->
    <div class="hidden fixed inset-0 z-20 bg-black/85 backdrop-blur-sm flex items-center justify-center p-4" id="menu-exit-panel">
      <div class="pixel-box max-w-md w-full p-6 text-center flex flex-col gap-4">
        <span class="material-symbols-outlined text-amber text-[42px] mx-auto">satellite_alt</span>
        <h2 class="font-pixel text-sm text-amber leading-relaxed">BASE ON STANDBY</h2>
        <p class="font-sans text-sm text-neutral-300">
          The astronauts are resting at the orbital station. When you are ready to explore space again, click the button below!
        </p>
        <button class="w-full py-3 bg-amber hover:bg-yellow-300 text-black font-pixel text-xs rounded transition cursor-pointer" onclick="closeMenuExit()" type="button">
          ► RETURN TO MENU
        </button>
      </div>
    </div>

    <!-- Barra Inferior Clicável Estilo Retro -->
    <div class="relative z-10 px-4 pb-5 flex justify-center">
      <button class="pixel-box max-w-2xl w-full px-4 py-3 flex flex-col sm:flex-row items-center justify-between gap-2 text-center hover:brightness-110 active:translate-y-0.5 transition cursor-pointer" onclick="startFromMenu()" type="button">
        <div class="flex items-center gap-2 font-pixel text-[9px] sm:text-[11px] text-amber leading-relaxed">
          <span class="w-1.5 h-1.5 bg-amber rounded-full"></span>
          <span>CLICK ANYWHERE TO ENGAGE AUDIO &amp; START EXPEDITION</span>
        </div>
        <span class="font-pixel text-[9px] sm:text-[10px] text-cyan shrink-0">[PRESS ANY KEY]</span>
      </button>
    </div>
  </div>

  <!-- ==================== TOPO: HUD GLOBAL, BARRA DE ALERTAS & 5 RECURSOS ESSENCIAIS ==================== -->
  <header class="sticky top-0 z-40 bg-[#0d1024]/80 backdrop-blur-md border-b border-surface-border/70 px-3 lg:px-6 py-2">
    <div class="max-w-7xl mx-auto flex flex-col gap-2">
      <!-- Barra Superior: Destino, Tempo e Controles de Simulação -->
      <div class="flex flex-wrap items-center justify-between gap-2">
        <div class="flex items-center gap-2.5">
          <button class="w-9 h-9 rounded-xl bg-gradient-to-br from-primary to-orange-700 flex items-center justify-center text-white shadow-md shadow-primary/25 hover:brightness-110 transition" onclick="returnToMenu()" title="Return to Main Menu" type="button">
            <span class="material-symbols-outlined text-[21px]">rocket_launch</span>
          </button>
          <div>
            <div class="flex items-center gap-2">
              <h2 class="font-bold text-sm sm:text-base text-white tracking-wide">Space Advance</h2>
              <span class="text-[10px] font-mono-code font-bold uppercase px-2 py-0.5 rounded bg-primary/20 text-primary border border-primary/40" id="badge-dest">MARS (ARES)</span>
              <span class="hidden sm:inline-block text-[10px] font-mono-code px-2 py-0.5 rounded bg-emerald-500/15 text-emerald-400 border border-emerald-500/30" id="badge-mode">Explorer</span>
            </div>
            <div class="text-xs text-neutral-400 font-mono-code flex items-center gap-2">
              <span class="w-2 h-2 rounded-full bg-emerald-400 animate-pulse"></span>
              <span id="label-hud-sector">Ares Planitia Base</span>
              <span>• Time: <strong class="text-white" id="hud-timer">00:00</strong></span>
            </div>
          </div>
        </div>

        <!-- População, Health em Verde e Botões de Menu / Som / NASA -->
        <div class="flex items-center gap-2 font-mono-code text-xs">
          <!-- População -->
          <div class="bg-surface-low border border-surface-border px-2.5 py-1.5 rounded-xl flex items-center gap-1.5">
            <span class="material-symbols-outlined text-purple text-[18px]">group</span>
            <div>
              <span class="text-neutral-400 text-[10px] block leading-none">Crew</span>
              <span class="font-bold text-white text-xs"><span id="hud-pop">2</span>/<span id="hud-pop-cap">5</span></span>
            </div>
          </div>

          <!-- VIDA / SAÚDE GERAL EM #05fa5b -->
          <div class="bg-[#05fa5b]/10 border border-[#05fa5b]/60 px-3 py-1.5 rounded-xl flex items-center gap-2 min-w-[130px]" id="hud-health-box">
            <span class="material-symbols-outlined text-[#05fa5b] text-[18px]" id="hud-health-icon">favorite</span>
            <div class="w-full">
              <div class="flex justify-between text-[10px]">
                <span class="text-[#05fa5b] font-bold" id="hud-health-label">Health</span>
                <span class="font-bold text-[#05fa5b]" id="hud-health-pct">100%</span>
              </div>
              <div class="w-full h-2 rounded-full bg-surface-high overflow-hidden mt-0.5">
                <div class="h-full bg-[#05fa5b] transition-all duration-300" id="hud-health-bar" style="width: 100%;"></div>
              </div>
            </div>
          </div>

          <!-- Áudio procedural Toggle -->
          <button class="w-8 h-8 rounded-xl bg-surface-low hover:bg-surface-high border border-surface-border text-neutral-300 flex items-center justify-center transition active:scale-95" id="btn-sound-toggle" title="Ligar/Desligar Som" type="button">
            <span class="material-symbols-outlined text-[17px]" id="icon-sound">volume_up</span>
          </button>

          <!-- Drawer Data Científicos -->
          <button class="px-2.5 py-1.5 rounded-xl bg-surface-low hover:bg-surface-high border border-cyan/40 text-cyan text-xs flex items-center gap-1 transition active:scale-95" onclick="toggleDataDrawer()" type="button">
            <span class="material-symbols-outlined text-[16px]">science</span>
            <span class="hidden md:inline">NASA</span>
          </button>

          <!-- Botão Menu -->
          <button class="px-2.5 py-1.5 rounded-xl bg-surface-low hover:bg-surface-high border border-amber/40 text-amber text-xs flex items-center gap-1 transition active:scale-95" onclick="returnToMenu()" title="Abrir Main Menu" type="button">
            <span class="material-symbols-outlined text-[16px]">home</span>
            <span class="hidden sm:inline">Menu</span>
          </button>
        </div>
      </div>

      <!-- FAIXA DE ALERTAS NA BARRA SUPERIOR (STATUS + CLIMA ESPACIAL DONKI) -->
      <div class="rounded-xl px-3 py-1.5 border border-emerald-500/40 bg-emerald-950/35 flex flex-col md:flex-row items-center justify-between gap-2 text-xs font-mono-code transition-all" id="top-alert-bar">
        <div class="flex items-center gap-2 w-full md:w-auto">
          <span class="material-symbols-outlined text-emerald-400 text-[18px] shrink-0" id="top-alert-icon">check_circle</span>
          <span class="px-1.5 py-0.5 rounded text-[10px] font-bold uppercase bg-emerald-500/20 text-emerald-300 shrink-0" id="top-alert-badge">ALL CLEAR</span>
          <span class="text-emerald-100 font-sans text-xs sm:text-sm font-medium min-w-0" id="top-alert-text">
            All systems go! Click the colored buttons below to collect resources.
          </span>
          <span class="hidden text-xs font-bold text-amber shrink-0" id="weather-timer">10s</span>
        </div>
        <!-- Botões de Ação Rápida de Clima Espacial (quando houver evento) -->
        <div class="flex flex-wrap items-center gap-1.5 w-full md:w-auto justify-end shrink-0" id="weather-actions"></div>
      </div>

      <!-- OS 5 RECURSOS ESSENCIAIS (SEMPRE VISÍVEIS) + PESQUISA CIENTÍFICA -->
      <div class="grid grid-cols-2 sm:grid-cols-3 lg:grid-cols-6 gap-2 font-mono-code">
        <!-- 1. ÁGUA -->
        <div class="bg-surface-card/75 backdrop-blur-sm border border-surface-border/80 rounded-xl p-2 transition duration-150" id="card-hud-water">
          <div class="flex items-center justify-between text-[11px] mb-0.5">
            <span class="flex items-center gap-1 text-cyan font-bold">
              <span class="material-symbols-outlined text-[15px]">water_drop</span> Water
            </span>
            <span class="text-[10px] font-bold text-rose-400" id="rate-water">-1.0/s</span>
          </div>
          <div class="flex items-baseline justify-between">
            <span class="text-base sm:text-lg font-bold text-white tracking-tight" id="val-water">100</span>
            <span class="text-[10px] text-neutral-400">/ <span id="cap-water">200</span></span>
          </div>
          <div class="w-full h-2 bg-surface-high border border-surface-border/60 rounded-full overflow-hidden mt-1.5">
            <div class="h-full bg-cyan transition-all duration-200" id="bar-water" style="width: 50%;"></div>
          </div>
        </div>

        <!-- 2. COMIDA -->
        <div class="bg-surface-card border border-surface-border/80 rounded-xl p-2 transition duration-150" id="card-hud-food">
          <div class="flex items-center justify-between text-[11px] mb-0.5">
            <span class="flex items-center gap-1 text-food font-bold">
              <span class="material-symbols-outlined text-[15px]">restaurant</span> Food
            </span>
            <span class="text-[10px] font-bold text-rose-400" id="rate-food">-1.0/s</span>
          </div>
          <div class="flex items-baseline justify-between">
            <span class="text-base sm:text-lg font-bold text-white tracking-tight" id="val-food">100</span>
            <span class="text-[10px] text-neutral-400">/ <span id="cap-food">200</span></span>
          </div>
          <div class="w-full h-2 bg-surface-high border border-surface-border/60 rounded-full overflow-hidden mt-1.5">
            <div class="h-full bg-food transition-all duration-200" id="bar-food" style="width: 50%;"></div>
          </div>
        </div>

        <!-- 3. OXIGÊNIO (O2) -->
        <div class="bg-surface-card border border-emerald-400/50 rounded-xl p-2 transition duration-150" id="card-hud-o2">
          <div class="flex items-center justify-between text-[11px] mb-0.5">
            <span class="flex items-center gap-1 text-emerald-400 font-bold whitespace-nowrap" id="label-hud-o2">
              <span class="material-symbols-outlined text-[15px]">air</span> Ar
            </span>
            <span class="text-[10px] font-bold text-rose-400" id="rate-o2">-1.2/s</span>
          </div>
          <div class="flex items-baseline justify-between">
            <span class="text-base sm:text-lg font-bold text-white tracking-tight" id="val-o2">120</span>
            <span class="text-[10px] text-neutral-400">/ <span id="cap-o2">200</span></span>
          </div>
          <div class="w-full h-2 bg-surface-high rounded-full overflow-hidden mt-1 border border-white/5">
            <div class="h-full rounded-full transition-all duration-200" id="bar-o2" style="width: 60%; background-color: #34d399;"></div>
          </div>
        </div>

        <!-- 4. ENERGIA -->
        <div class="bg-surface-card border border-surface-border/80 rounded-xl p-2 transition duration-150" id="card-hud-energy">
          <div class="flex items-center justify-between text-[11px] mb-0.5">
            <span class="flex items-center gap-1 text-energy font-bold">
              <span class="material-symbols-outlined text-[15px]">bolt</span> Energy
            </span>
            <span class="text-[10px] font-bold text-emerald-400" id="rate-energy">+3.0/s</span>
          </div>
          <div class="flex items-baseline justify-between">
            <span class="text-base sm:text-lg font-bold text-white tracking-tight" id="val-energy">120</span>
            <span class="text-[10px] text-neutral-400">/ <span id="cap-energy">250</span></span>
          </div>
          <div class="w-full h-2 bg-surface-high border border-surface-border/60 rounded-full overflow-hidden mt-1.5">
            <div class="h-full bg-energy transition-all duration-200" id="bar-energy" style="width: 48%;"></div>
          </div>
        </div>

        <!-- 5. RADIAÇÃO AMBIENTAL ACUMULADA -->
        <div class="bg-surface-card border border-surface-border/80 rounded-xl p-2 transition duration-150" id="card-hud-rad">
          <div class="flex items-center justify-between text-[11px] mb-0.5">
            <span class="flex items-center gap-1 text-rad font-bold">
              <span class="material-symbols-outlined text-[15px]">radiology</span> Radiation
            </span>
            <span class="text-[10px] font-bold text-rose-400" id="rate-rad">+0.2/s</span>
          </div>
          <div class="flex items-baseline justify-between">
            <span class="text-base sm:text-lg font-bold text-white tracking-tight" id="val-rad">10</span>
            <span class="text-[10px] text-neutral-400">/ 100 max</span>
          </div>
          <div class="w-full h-2 bg-surface-high border border-surface-border/60 rounded-full overflow-hidden mt-1.5">
            <div class="h-full bg-rad transition-all duration-200" id="bar-rad" style="width: 10%;"></div>
          </div>
        </div>

        <!-- 6. CIÊNCIA / PESQUISA (PROGRESSO) -->
        <div class="bg-surface-card border border-[#a405fa]/60 rounded-xl p-2 transition duration-150 bg-[#a405fa]/10" id="card-hud-sci">
          <div class="flex items-center justify-between text-[11px] mb-0.5">
            <span class="flex items-center gap-1 text-[#a405fa] font-bold">
              <span class="material-symbols-outlined text-[15px]">psychology</span> Science
            </span>
            <span class="text-[10px] font-bold text-emerald-400" id="rate-sci">+2.0/s</span>
          </div>
          <div class="flex items-baseline justify-between">
            <span class="text-base sm:text-lg font-bold text-white tracking-tight" id="val-sci">15</span>
            <span class="text-[10px] text-[#c354ff] font-semibold">para melhorias</span>
          </div>
          <div class="w-full h-2 bg-surface-high border border-surface-border/60 rounded-full overflow-hidden mt-1.5">
            <div class="h-full bg-[#a405fa] transition-all duration-200" id="bar-sci" style="width: 15%;"></div>
          </div>
        </div>
      </div>
    </div>
  </header>

  <!-- ==================== CONTEÚDO PRINCIPAL: CLICKER & GERENCIAMENTO ==================== -->
  <main class="relative z-10 flex-1 max-w-7xl w-full mx-auto p-3 sm:p-4 lg:p-5 flex flex-col gap-4">

    <!-- ABAS: CADA OPÇÃO EM UMA TELA -->
    <nav class="flex flex-wrap gap-2 font-mono-code text-xs font-bold" role="tablist">
      <button class="screen-tab px-3 py-2 rounded-xl border transition" data-tab="acoes" onclick="showScreen('acoes')" role="tab" type="button">🖱️ Actions</button>
      <button class="screen-tab px-3 py-2 rounded-xl border transition" data-tab="melhorias" onclick="showScreen('melhorias')" role="tab" type="button">🔬 Research</button>
      <button class="screen-tab px-3 py-2 rounded-xl border transition" data-tab="upgrades" onclick="showScreen('upgrades')" role="tab" type="button">🤖 Upgrades</button>
      <button class="screen-tab px-3 py-2 rounded-xl border transition" data-tab="dados" onclick="showScreen('dados')" role="tab" type="button">📊 Data</button>
    </nav>

    <!-- TELA: AÇÕES -->
    <div class="flex flex-col gap-4" data-screen="acoes">

    <!-- DICA RÁPIDA PARA CRIANÇAS (7 A 12 ANOS) -->
    <div class="bg-surface-low border border-cyan/30 rounded-2xl px-4 py-2.5 flex flex-wrap items-center justify-between gap-2 text-xs">
      <div class="flex items-center gap-2 text-neutral-200">
        <span class="material-symbols-outlined text-amber text-[20px]">lightbulb</span>
        <span><strong>How to play:</strong> click the buttons to collect <span class="text-cyan font-bold">Water</span>, <span class="text-[#bf8c15] font-bold">Food</span>, <span class="text-emerald-400 font-bold">Ar</span> e <span class="text-[#4f889e] font-bold">Energy</span>! Use <span class="text-[#a405fa] font-bold">Science</span> to upgrade your base.</span>
      </div>
      <span class="font-mono-code text-[11px] text-emerald-400 font-bold">Green = gain • Red = cost • Yellow = caution • Blue = upgrade</span>
    </div>
    
    <div class="flex items-center gap-3"><span class="text-[11px] font-mono-code font-bold uppercase tracking-widest text-neutral-400">Actions</span><span class="flex-1 h-px bg-surface-border/70"></span></div>
    <!-- SEÇÃO SUPERIOR: NÚCLEO CLICKER (5 AÇÕES INSTANTÂNEAS TÁTEIS COM TRADE-OFF) -->
    <section>
      <div class="flex items-center justify-between mb-2">
        <div class="flex items-center gap-2">
          <span class="material-symbols-outlined text-primary text-[20px]">touch_app</span>
          <h2 class="font-bold text-white text-sm sm:text-base uppercase tracking-wider font-mono-code">Click to Collect!</h2>
        </div>
        <span class="text-[11px] font-mono-code text-neutral-400">Press 1–5 or tap</span>
      </div>

      <div class="grid grid-cols-2 md:grid-cols-5 gap-2.5 sm:gap-3.5">
        <!-- AÇÃO 1: MINERAR GELO -->
        <button class="click-card relative overflow-hidden bg-surface-card hover:bg-[#2e242a] active:bg-[#382b33] disabled:opacity-45 disabled:cursor-not-allowed border border-cyan/40 hover:border-cyan rounded-2xl p-3 sm:p-4 text-left transition shadow-md flex flex-col justify-between group" id="btn-click-ice" onclick="actionMineIce(event)" type="button">
          <div class="flex items-start justify-between w-full">
            <div class="w-10 h-10 rounded-xl bg-cyan/15 text-cyan flex items-center justify-center group-hover:scale-110 transition">
              <span class="material-symbols-outlined text-[22px]">ac_unit</span>
            </div>
            <div class="flex items-center gap-1">
              <span class="hidden sm:inline font-mono-code text-[10px] text-neutral-400 bg-surface-high px-1.5 py-0.5 rounded">[1]</span>
              <span class="font-mono-code text-[10px] text-emerald-400 bg-emerald-500/15 px-1.5 py-0.5 rounded font-bold">+Water</span>
            </div>
          </div>
          <div class="mt-3">
            <h3 class="font-bold text-white text-sm">Melt Ice</h3>
            <p class="text-xs text-neutral-300 font-mono-code mt-0.5 leading-tight">
              <span class="text-emerald-400 font-bold">+<span id="click-val-ice">8</span> Water</span> <br><span class="text-[#ff0000] font-bold">Costs 5 Energy</span>
            </p>
          </div>
        </button>

        <!-- AÇÃO 2: COLHER ESTUFA -->
        <button class="click-card relative overflow-hidden bg-surface-card hover:bg-[#2e242a] active:bg-[#382b33] disabled:opacity-45 disabled:cursor-not-allowed border border-[#bf8c15]/50 hover:border-[#bf8c15] rounded-2xl p-3 sm:p-4 text-left transition shadow-md flex flex-col justify-between group" id="btn-click-greenhouse" onclick="actionHarvestFood(event)" type="button">
          <div class="flex items-start justify-between w-full">
            <div class="w-10 h-10 rounded-xl bg-[#bf8c15]/20 text-[#bf8c15] flex items-center justify-center group-hover:scale-110 transition">
              <span class="material-symbols-outlined text-[22px]">psychiatry</span>
            </div>
            <div class="flex items-center gap-1">
              <span class="hidden sm:inline font-mono-code text-[10px] text-neutral-400 bg-surface-high px-1.5 py-0.5 rounded">[2]</span>
              <span class="font-mono-code text-[10px] text-emerald-400 bg-emerald-500/15 px-1.5 py-0.5 rounded font-bold">+Food</span>
            </div>
          </div>
          <div class="mt-3">
            <h3 class="font-bold text-white text-sm">Harvest Plants</h3>
            <p class="text-xs text-neutral-300 font-mono-code mt-0.5 leading-tight">
              <span class="text-emerald-400 font-bold">+<span id="click-val-food">8</span> Food</span> <br><span class="text-[#ff0000] font-bold">Costs 4 Energy</span>
            </p>
          </div>
        </button>

        <!-- AÇÃO 3: ELETRÓLISE (ÁGUA -> O2) -->
        <button class="click-card relative overflow-hidden bg-surface-card hover:bg-[#2e242a] active:bg-[#382b33] disabled:opacity-45 disabled:cursor-not-allowed border border-emerald-400/40 hover:border-emerald-400 rounded-2xl p-3 sm:p-4 text-left transition shadow-md flex flex-col justify-between group" id="btn-click-o2" onclick="actionElectrolysis(event)" type="button">
          <div class="flex items-start justify-between w-full">
            <div class="w-10 h-10 rounded-xl bg-emerald-500/15 text-emerald-400 flex items-center justify-center group-hover:scale-110 transition">
              <span class="material-symbols-outlined text-[22px]">air</span>
            </div>
            <div class="flex items-center gap-1">
              <span class="hidden sm:inline font-mono-code text-[10px] text-neutral-400 bg-surface-high px-1.5 py-0.5 rounded">[3]</span>
              <span class="font-mono-code text-[10px] text-emerald-400 bg-emerald-500/15 px-1.5 py-0.5 rounded font-bold">+Ar</span>
            </div>
          </div>
          <div class="mt-3">
            <h3 class="font-bold text-white text-sm">Generate Oxygen</h3>
            <p class="text-xs text-neutral-300 font-mono-code mt-0.5 leading-tight">
              <span class="text-emerald-400 font-bold">+<span id="click-val-o2">10</span> Ar</span> <br><span class="text-[#ff0000]">Costs 4 Water and 5 Energy</span>
            </p>
          </div>
        </button>

        <!-- AÇÃO 4: COLETAR SOLAR -->
        <button class="click-card relative overflow-hidden bg-surface-card hover:bg-[#2e242a] active:bg-[#382b33] border border-[#4f889e]/50 hover:border-[#4f889e] rounded-2xl p-3 sm:p-4 text-left transition shadow-md flex flex-col justify-between group" id="btn-click-solar" onclick="actionCollectSolar(event)" type="button">
          <div class="flex items-start justify-between w-full">
            <div class="w-10 h-10 rounded-xl bg-[#4f889e]/20 text-[#4f889e] flex items-center justify-center group-hover:scale-110 transition">
              <span class="material-symbols-outlined text-[22px]">solar_power</span>
            </div>
            <div class="flex items-center gap-1">
              <span class="hidden sm:inline font-mono-code text-[10px] text-neutral-400 bg-surface-high px-1.5 py-0.5 rounded">[4]</span>
              <span class="font-mono-code text-[10px] text-emerald-400 bg-emerald-500/15 px-1.5 py-0.5 rounded font-bold">+Energy</span>
            </div>
          </div>
          <div class="mt-3">
            <h3 class="font-bold text-white text-sm">Solar Panel</h3>
            <p class="text-xs text-neutral-300 font-mono-code mt-0.5 leading-tight">
              <span class="text-emerald-400 font-bold">+<span id="click-val-solar">12</span> Energy</span> <br><span class="text-emerald-400">Free (No cost!)</span>
            </p>
          </div>
        </button>

        <!-- AÇÃO 5: CONDUZIR EXPERIMENTO CIENTÍFICO -->
        <button class="click-card col-span-2 md:col-span-1 relative overflow-hidden bg-surface-card hover:bg-[#2e242a] active:bg-[#382b33] disabled:opacity-45 disabled:cursor-not-allowed border border-[#a405fa]/60 hover:border-[#a405fa] rounded-2xl p-3 sm:p-4 text-left transition shadow-md flex flex-col justify-between group" id="btn-click-experiment" onclick="actionResearchExp(event)" type="button">
          <div class="flex items-start justify-between w-full">
            <div class="w-10 h-10 rounded-xl bg-[#a405fa]/20 text-[#a405fa] flex items-center justify-center group-hover:scale-110 transition">
              <span class="material-symbols-outlined text-[22px]">biotech</span>
            </div>
            <div class="flex items-center gap-1">
              <span class="hidden sm:inline font-mono-code text-[10px] text-neutral-400 bg-surface-high px-1.5 py-0.5 rounded">[5]</span>
              <span class="font-mono-code text-[10px] text-emerald-400 bg-emerald-500/15 px-1.5 py-0.5 rounded font-bold">+Science</span>
            </div>
          </div>
          <div class="mt-3">
            <h3 class="font-bold text-white text-sm">Research</h3>
            <p class="text-xs text-neutral-300 font-mono-code mt-0.5 leading-tight">
              <span class="text-emerald-400 font-bold">+<span id="click-val-sci">6</span> Science</span> <br><span class="text-[#ff0000] font-bold">Costs 6 Energy</span>
            </p>
          </div>
        </button>
      </div>
    </section>

    <!-- SEÇÃO CENTRAL EM 2 COLUNAS: GESTÃO DE TRIPULAÇÃO & UPGRADES COM PESQUISA -->
    <div class="flex flex-col gap-4">
      
      <!-- COLUNA ESQUERDA (5 COLS): TRIPULAÇÃO & BLINDAGEM ATIVA -->
      <div class="lg:col-span-5 flex flex-col gap-4">
        
        <div class="flex items-center gap-3"><span class="text-[11px] font-mono-code font-bold uppercase tracking-widest text-neutral-400">Crew & Shield</span><span class="flex-1 h-px bg-surface-border/70"></span></div>
        <!-- CARD 1: RECRUTAMENTO DE ASTRONAUTS COM TRADE-OFF VISÍVEL -->
        <div class="bg-surface-card border border-surface-border/80 rounded-2xl p-4 shadow-lg flex flex-col justify-between">
          <div>
            <div class="flex items-center justify-between border-b border-surface-border/60 pb-2.5 mb-3">
              <div class="flex items-center gap-2">
                <span class="material-symbols-outlined text-purple text-[20px]">person_add</span>
                <h3 class="font-bold text-white text-sm font-mono-code uppercase">Astronaut Crew</h3>
              </div>
              <span class="font-mono-code text-xs text-neutral-300 font-bold" id="label-pop-ratio">2 / 5 Astronauts</span>
            </div>

            <div class="p-3 bg-surface-low rounded-xl border border-surface-border text-xs leading-relaxed space-y-1.5">
              <p class="text-neutral-200 font-medium">
                <strong class="text-amber">Warning:</strong> each astronaut generates <strong class="text-emerald-400">+1 Science</strong> every second, but consumes <strong class="text-[#ff0000]">Water, Food e Ar</strong> every second.
              </p>
              <p class="text-[11px] text-neutral-400 font-mono-code">
                Tip: Stock up on supplies before recruiting more astronauts.
              </p>
            </div>

            <!-- Taxa de Consumo Atual da Crew -->
            <div class="grid grid-cols-3 gap-2 text-center font-mono-code text-[11px] mt-3">
              <div class="bg-surface-high/60 p-2 rounded-lg border border-surface-border">
                <span class="text-neutral-400 block text-[10px]">Water Usage</span>
                <span class="font-bold text-[#ff0000]" id="crew-drain-water">-1.0/s</span>
              </div>
              <div class="bg-surface-high/60 p-2 rounded-lg border border-surface-border">
                <span class="text-neutral-400 block text-[10px]">Food Usage</span>
                <span class="font-bold text-[#ff0000]" id="crew-drain-food">-1.0/s</span>
              </div>
              <div class="bg-surface-high/60 p-2 rounded-lg border border-surface-border">
                <span class="text-neutral-400 block text-[10px]">Oxygen Usage</span>
                <span class="font-bold text-[#ff0000]" id="crew-drain-o2">-1.2/s</span>
              </div>
            </div>
          </div>

          <p class="mt-4 pt-3 border-t border-surface-border/60 text-[11px] text-neutral-400 font-mono-code">New astronauts arrive from Earth during special events!</p>
        </div>

        <!-- CARD 2: BLINDAGEM ELETROMAGNÉTICA DE RADIAÇÃO -->
        <div class="bg-surface-card border border-surface-border/80 rounded-2xl p-4 shadow-lg flex flex-col justify-between">
          <div>
            <div class="flex items-center justify-between border-b border-surface-border/60 pb-2 mb-3">
              <div class="flex items-center gap-2">
                <span class="material-symbols-outlined text-cyan text-[20px]">shield</span>
                <h3 class="font-bold text-white text-sm font-mono-code uppercase">Radiation Shield</h3>
              </div>
              <span class="font-mono-code text-[10px] px-2 py-0.5 rounded uppercase font-bold bg-neutral-800 text-neutral-400" id="badge-shield">OFF</span>
            </div>
            <p class="text-xs text-neutral-300 leading-snug">
              Turn on the shield to remove dangerous radiation from the environment (<span class="text-emerald-400 font-bold">-1.3 per second</span>). It consumes some energy.
            </p>
            <div class="mt-2 text-xs font-mono-code flex items-center justify-between bg-surface-low p-2 rounded-xl">
              <span class="text-neutral-400">Energy consumption:</span>
              <span class="text-[#ff0000] font-bold" id="shield-drain-info">-4.0 Energy/s</span>
            </div>
          </div>

          <button class="mt-3.5 w-full py-2.5 rounded-xl bg-surface-low hover:bg-surface-high border border-surface-border text-neutral-200 hover:text-white font-bold text-xs font-mono-code transition active:scale-95 flex items-center justify-center gap-1.5" id="btn-toggle-shield" onclick="toggleShield()" type="button">
            <span class="material-symbols-outlined text-[16px]">power_settings_new</span>
            <span class="text-[#ff0000]" id="lbl-toggle-shield">Activate Shield (-4.0 Energy/s)</span>
          </button>
        </div>

      </div>

    </div>
    </div>

    <!-- TELA: MELHORIAS -->
    <div class="flex flex-col gap-4" data-screen="melhorias">
      <!-- COLUNA DIREITA (7 COLS): ÁRVORE DE UPGRADES ACELERADOS COM PESQUISA -->
      <div class="lg:col-span-7 bg-surface-card border border-surface-border/80 rounded-2xl p-4 shadow-lg flex flex-col">
        <div class="flex flex-wrap items-center justify-between border-b border-surface-border/60 pb-2.5 mb-3 gap-2">
          <div class="flex items-center gap-2">
            <span class="material-symbols-outlined text-cyan text-[20px]">precision_manufacturing</span>
            <h3 class="font-bold text-white text-sm font-mono-code uppercase">Upgrades (spend Science)</h3>
          </div>
          <div class="font-mono-code text-xs bg-blue-500/15 px-2.5 py-1 rounded-lg border border-blue-500/50">
            <span class="text-neutral-300">Available:</span> <strong class="text-[#c354ff]" id="upgrades-sci-disp">15 Science</strong>
          </div>
        </div>

        <!-- Grid de Upgrades com Pesquisa -->
        <div class="grid grid-cols-1 sm:grid-cols-2 gap-3 flex-1">
          
          <!-- 1. RECICLADOR DE ÁGUA -->
          <div class="bg-surface-low border border-surface-border p-3 rounded-xl flex flex-col justify-between">
            <div>
              <div class="flex items-center justify-between">
                <span class="text-xs font-bold text-blue-400 flex items-center gap-1">
                  <span class="material-symbols-outlined text-[16px]">water_ec</span> Water Recycler
                </span>
              </div>
              <p class="text-[11px] text-neutral-300 mt-1 leading-tight">
                Consumes less Water and grants +2 Water per click!
              </p>
            </div>
            <div class="mt-2.5 pt-2 border-t border-surface-border/50 flex items-center justify-between">
              <span class="font-mono-code text-[10px] text-blue-400 font-bold" id="cost-up-recycler">15 Science</span>
              <button class="px-3 py-1 rounded-lg bg-blue-500 hover:bg-blue-400 disabled:opacity-40 disabled:cursor-not-allowed text-white font-bold text-[11px] font-mono-code transition" id="btn-up-recycler" onclick="buyUpgrade('recycler', event)" type="button">
                Upgrade
              </button>
            </div>
          </div>

          <!-- 2. ESTUFA HIDROPÔNICA AVANÇADA -->
          <div class="bg-surface-low border border-surface-border p-3 rounded-xl flex flex-col justify-between">
            <div>
              <div class="flex items-center justify-between">
                <span class="text-xs font-bold text-blue-400 flex items-center gap-1">
                  <span class="material-symbols-outlined text-[16px]">potted_plant</span> Advanced Greenhouse
                </span>
              </div>
              <p class="text-[11px] text-neutral-300 mt-1 leading-tight">
                Grants +4 Food per click and generates +1 Food every second!
              </p>
            </div>
            <div class="mt-2.5 pt-2 border-t border-surface-border/50 flex items-center justify-between">
              <span class="font-mono-code text-[10px] text-blue-400 font-bold" id="cost-up-greenhouse">20 Science</span>
              <button class="px-3 py-1 rounded-lg bg-blue-500 hover:bg-blue-400 disabled:opacity-40 disabled:cursor-not-allowed text-white font-bold text-[11px] font-mono-code transition" id="btn-up-greenhouse" onclick="buyUpgrade('greenhouse', event)" type="button">
                Upgrade
              </button>
            </div>
          </div>

          <!-- 3. PAINEL SOLAR DOBRÁVEL -->
          <div class="bg-surface-low border border-surface-border p-3 rounded-xl flex flex-col justify-between">
            <div>
              <div class="flex items-center justify-between">
                <span class="text-xs font-bold text-blue-400 flex items-center gap-1">
                  <span class="material-symbols-outlined text-[16px]">solar_power</span> Foldable Solar Panels
                </span>
              </div>
              <p class="text-[11px] text-neutral-300 mt-1 leading-tight">
                Generates +2 Energy every second and grants +5 Energy per click!
              </p>
            </div>
            <div class="mt-2.5 pt-2 border-t border-surface-border/50 flex items-center justify-between">
              <span class="font-mono-code text-[10px] text-blue-400 font-bold" id="cost-up-solar">18 Science</span>
              <button class="px-3 py-1 rounded-lg bg-blue-500 hover:bg-blue-400 disabled:opacity-40 disabled:cursor-not-allowed text-white font-bold text-[11px] font-mono-code transition" id="btn-up-solar" onclick="buyUpgrade('solar', event)" type="button">
                Upgrade
              </button>
            </div>
          </div>

          <!-- 4. BLINDAGEM REFORÇADA -->
          <div class="bg-surface-low border border-surface-border p-3 rounded-xl flex flex-col justify-between">
            <div>
              <div class="flex items-center justify-between">
                <span class="text-xs font-bold text-blue-400 flex items-center gap-1">
                  <span class="material-symbols-outlined text-[16px]">security</span> Optimized Shield
                </span>
              </div>
              <p class="text-[11px] text-neutral-300 mt-1 leading-tight">
                The shield consumes less energy while protecting the base.
              </p>
            </div>
            <div class="mt-2.5 pt-2 border-t border-surface-border/50 flex items-center justify-between">
              <span class="font-mono-code text-[10px] text-blue-400 font-bold" id="cost-up-shield">25 Science</span>
              <button class="px-3 py-1 rounded-lg bg-blue-500 hover:bg-blue-400 disabled:opacity-40 disabled:cursor-not-allowed text-white font-bold text-[11px] font-mono-code transition" id="btn-up-shield" onclick="buyUpgrade('shield', event)" type="button">
                Upgrade
              </button>
            </div>
          </div>

          <!-- 5. MÓDULO HABITACIONAL -->
          <div class="bg-surface-low border border-surface-border p-3 rounded-xl flex flex-col justify-between">
            <div>
              <div class="flex items-center justify-between">
                <span class="text-xs font-bold text-blue-400 flex items-center gap-1">
                  <span class="material-symbols-outlined text-[16px]">domain</span> Habitat Module
                </span>
              </div>
              <p class="text-[11px] text-neutral-300 mt-1 leading-tight">
                Each purchase: +10 astronaut capacity and +50 of each resource. Unlimited purchases!
              </p>
            </div>
            <div class="mt-2.5 pt-2 border-t border-surface-border/50 flex items-center justify-between">
              <span class="font-mono-code text-[10px] text-blue-400 font-bold" id="cost-up-habitat">30 Science</span>
              <button class="px-3 py-1 rounded-lg bg-blue-500 hover:bg-blue-400 disabled:opacity-40 disabled:cursor-not-allowed text-white font-bold text-[11px] font-mono-code transition" id="btn-up-habitat" onclick="buyUpgrade('habitat', event)" type="button">
                Build
              </button>
            </div>
          </div>

          <!-- 6. ANTENA DEEP SPACE NASA -->
          <div class="bg-surface-low border border-surface-border p-3 rounded-xl flex flex-col justify-between">
            <div>
              <div class="flex items-center justify-between">
                <span class="text-xs font-bold text-blue-400 flex items-center gap-1">
                  <span class="material-symbols-outlined text-[16px]">settings_input_antenna</span> Space Antenna
                </span>
              </div>
              <p class="text-[11px] text-neutral-300 mt-1 leading-tight">
                Communicates with Earth: <span class="text-emerald-400 font-bold">+1.5 Science</span> every second and <span class="text-emerald-400 font-bold">+3</span> per research!
              </p>
            </div>
            <div class="mt-2.5 pt-2 border-t border-surface-border/50 flex items-center justify-between">
              <span class="font-mono-code text-[10px] text-blue-400 font-bold" id="cost-up-antenna">35 Science</span>
              <button class="px-3 py-1 rounded-lg bg-blue-500 hover:bg-blue-400 disabled:opacity-40 disabled:cursor-not-allowed text-white font-bold text-[11px] font-mono-code transition" id="btn-up-antenna" onclick="buyUpgrade('antenna', event)" type="button">
                Install
              </button>
            </div>
          </div>

        </div>


      </div>

    </div>

    <!-- TELA: UPGRADES -->
    <div data-screen="upgrades">
    <!-- PAINEL: UPGRADES (ROBÔS QUE TRABALHAM SOZINHOS) -->
    <section class="bg-surface-card border border-surface-border/80 rounded-2xl p-4 shadow-lg flex flex-col gap-3">
      <div class="flex items-center gap-3"><span class="text-[11px] font-mono-code font-bold uppercase tracking-widest text-neutral-400">Upgrades</span><span class="flex-1 h-px bg-surface-border/70"></span></div>
        <!-- SUBSEÇÃO: UPGRADES DE AUTOMATIZAÇÃO (ROBÔS E DRONES AUTÔNOMOS) -->
        <div class="mt-4 pt-3 border-t border-blue-500/40">
          <div class="flex flex-wrap items-center justify-between gap-2 mb-2.5">
            <div class="flex items-center gap-1.5">
              <span class="material-symbols-outlined text-blue-400 text-[18px]">smart_toy</span>
              <h4 class="font-bold text-blue-400 text-xs font-mono-code uppercase">Robôs que trabalham sozinhos</h4>
            </div>
            <span class="font-mono-code text-[10px] text-blue-300 bg-blue-500/15 px-2 py-0.5 rounded border border-blue-500/30">100% Automatic</span>
          </div>

          <div class="grid grid-cols-1 sm:grid-cols-2 gap-2.5">
            <!-- AUTO 1: ROBÔ MINERADOR DE GELO -->
            <div class="bg-surface-low border border-blue-500/40 p-3 rounded-xl flex flex-col justify-between">
              <div>
                <div class="flex items-center justify-between">
                  <span class="text-xs font-bold text-blue-400 flex items-center gap-1">
                    <span class="material-symbols-outlined text-[15px]">precision_manufacturing</span> Ice Mining Robot
                  </span>
                </div>
                <p class="text-[11px] text-neutral-300 mt-1 leading-tight">
                  Automatically collects Water: <strong class="text-emerald-400">+2 Water</strong> every second, at no energy cost!
                </p>
              </div>
              <div class="mt-2.5 pt-2 border-t border-surface-border/50 flex items-center justify-between">
                <span class="font-mono-code text-[10px] text-blue-400 font-bold" id="cost-up-autoIce">22 Science</span>
                <button class="px-2.5 py-1 rounded-lg bg-blue-500 hover:bg-blue-400 disabled:opacity-40 disabled:cursor-not-allowed text-white font-bold text-[11px] font-mono-code transition" id="btn-up-autoIce" onclick="buyUpgrade('autoIce', event)" type="button">
                  Activate
                </button>
              </div>
            </div>

            <!-- AUTO 2: ELETROLISADOR AUTOMÁTICO O2 -->
            <div class="bg-surface-low border border-emerald-400/35 p-3 rounded-xl flex flex-col justify-between">
              <div>
                <div class="flex items-center justify-between">
                  <span class="text-xs font-bold text-blue-400 flex items-center gap-1">
                    <span class="material-symbols-outlined text-[15px]">air</span> Oxygen Generator
                  </span>
                </div>
                <p class="text-[11px] text-neutral-300 mt-1 leading-tight">
                  Automatically generates Oxygen: <strong class="text-emerald-400">+2.5 Ar</strong> every second for the crew!
                </p>
              </div>
              <div class="mt-2.5 pt-2 border-t border-surface-border/50 flex items-center justify-between">
                <span class="font-mono-code text-[10px] text-blue-400 font-bold" id="cost-up-autoO2">24 Science</span>
                <button class="px-2.5 py-1 rounded-lg bg-blue-500 hover:bg-blue-400 disabled:opacity-40 disabled:cursor-not-allowed text-white font-bold text-[11px] font-mono-code transition" id="btn-up-autoO2" onclick="buyUpgrade('autoO2', event)" type="button">
                  Activate
                </button>
              </div>
            </div>

            <!-- AUTO 3: DRONE CIENTISTA AUTÔNOMO -->
            <div class="bg-surface-low border border-blue-500/40 p-3 rounded-xl flex flex-col justify-between">
              <div>
                <div class="flex items-center justify-between">
                  <span class="text-xs font-bold text-blue-400 flex items-center gap-1">
                    <span class="material-symbols-outlined text-[15px]">smart_toy</span> Science Drone
                  </span>
                </div>
                <p class="text-[11px] text-neutral-300 mt-1 leading-tight">
                  Explores Mars autonomously: <strong class="text-emerald-400">+2 Science</strong> every second, plus bonuses!
                </p>
              </div>
              <div class="mt-2.5 pt-2 border-t border-surface-border/50 flex items-center justify-between">
                <span class="font-mono-code text-[10px] text-blue-400 font-bold" id="cost-up-autoSci">26 Science</span>
                <button class="px-2.5 py-1 rounded-lg bg-blue-500 hover:bg-blue-400 disabled:opacity-40 disabled:cursor-not-allowed text-white font-bold text-[11px] font-mono-code transition" id="btn-up-autoSci" onclick="buyUpgrade('autoSci', event)" type="button">
                  Activate
                </button>
              </div>
            </div>

            <!-- AUTO 4: ALGAS PRODUTORAS DE AR (OXIGÊNIO LENTO, CRESCE COM O NÍVEL) -->
            <div class="bg-surface-low border border-blue-500/40 p-3 rounded-xl flex flex-col justify-between">
              <div>
                <div class="flex items-center justify-between">
                  <span class="text-xs font-bold text-blue-400 flex items-center gap-1">
                    <span class="material-symbols-outlined text-[15px]">eco</span> Oxygen-Producing Algae
                  </span>
                </div>
                <p class="text-[11px] text-neutral-300 mt-1 leading-tight">
                  Algae generate <strong class="text-emerald-400">+1 Ar</strong> every second, continuously!
                </p>
              </div>
              <div class="mt-2.5 pt-2 border-t border-surface-border/50 flex items-center justify-between">
                <span class="font-mono-code text-[10px] text-blue-400 font-bold" id="cost-up-algae">20 Science</span>
                <button class="px-2.5 py-1 rounded-lg bg-blue-500 hover:bg-blue-400 disabled:opacity-40 disabled:cursor-not-allowed text-white font-bold text-[11px] font-mono-code transition" id="btn-up-algae" onclick="buyUpgrade('algae', event)" type="button">
                  Activate
                </button>
              </div>
            </div>
          </div>
        </div>
    </section>
    </div>

    <!-- TELA: DADOS -->
    <div class="flex flex-col gap-4" data-screen="dados">
    <!-- SEÇÃO INFERIOR: DADOS DA MISSÃO & FONTE NASA -->
    <div class="flex items-center gap-3"><span class="text-[11px] font-mono-code font-bold uppercase tracking-widest text-neutral-400">Data</span><span class="flex-1 h-px bg-surface-border/70"></span></div>
    <!-- SEÇÃO INFERIOR: STATUS DE ESCALONAMENTO & FONTE NASA -->
    <div class="bg-surface-card/75 backdrop-blur-sm border border-surface-border/80 rounded-2xl p-3 shadow-lg flex flex-col sm:flex-row items-center justify-between gap-2 text-xs font-mono-code">
      <div class="flex items-center gap-2 text-neutral-300">
        <span class="material-symbols-outlined text-blue-400 text-[18px]">explore</span>
        <span>Mission Stage: <strong class="text-emerald-400" id="label-diff-scale">Phase 1 (Beginning)</strong></span>
      </div>
      <div class="flex items-center gap-3">
        <button class="text-cyan underline hover:text-white" onclick="openDataSourceModal()" type="button">Real NASA Facts (DONKI &amp; OSDR)</button>
      </div>
    </div>

    </div>
  </main>

  <!-- ==================== GAVETA RECOLHÍVEL: DADOS CIENTÍFICOS NASA ==================== -->
  <aside class="fixed inset-y-0 right-0 w-full sm:w-96 bg-surface-card/95 backdrop-blur-md border-l border-surface-border z-50 transform translate-x-full transition-transform duration-300 flex flex-col p-5 shadow-2xl" id="data-drawer">
    <div class="flex items-center justify-between border-b border-surface-border pb-3 mb-4">
      <div class="flex items-center gap-2 text-cyan">
        <span class="material-symbols-outlined text-[22px]">science</span>
        <h3 class="font-bold text-white text-base font-mono-code">NASA Scientific Data</h3>
      </div>
      <button class="w-7 h-7 rounded-lg bg-surface-low hover:bg-surface-high text-neutral-300 flex items-center justify-center" onclick="toggleDataDrawer()" type="button">
        <span class="material-symbols-outlined text-[18px]">close</span>
      </button>
    </div>

    <div class="flex-1 overflow-y-auto space-y-4 text-xs font-mono-code pr-1">
      <!-- Status da API / Dataset Fallback -->
      <div class="p-3 rounded-xl bg-surface-low border border-surface-border">
        <div class="flex items-center justify-between mb-1">
          <span class="text-neutral-400 text-[10px]">Origem dos Data:</span>
          <span class="px-1.5 py-0.5 rounded text-[10px] font-bold bg-emerald-500/20 text-emerald-400" id="api-status-tag">DONKI Offline Data</span>
        </div>
        <p class="text-neutral-300 text-[11px] leading-tight">
          Real NASA data about solar storms and what astronauts need to survive in space.
        </p>
      </div>

      <!-- Métricas DONKI Atuais -->
      <div class="space-y-2">
        <h4 class="font-bold text-white text-xs uppercase tracking-wider text-neutral-400">Space Conditions</h4>
        <div class="p-2.5 rounded-lg bg-surface-low border border-surface-border/60 flex justify-between">
          <span class="text-neutral-400">Solar Wind:</span>
          <span class="text-amber font-bold" id="live-solar-wind">480 km/s (Normal)</span>
        </div>
        <div class="p-2.5 rounded-lg bg-surface-low border border-surface-border/60 flex justify-between">
          <span class="text-neutral-400">Magnetic Activity:</span>
          <span class="text-cyan font-bold" id="live-kp-index">Kp 2 (Calmo)</span>
        </div>
        <div class="p-2.5 rounded-lg bg-surface-low border border-surface-border/60 flex justify-between">
          <span class="text-neutral-400">Space Radiation:</span>
          <span class="text-rad font-bold" id="live-rad-bg">0.20 mSv/s</span>
        </div>
      </div>

      <!-- Explicação Educativa 7-12 Anos -->
      <div class="p-3 rounded-xl bg-purple/10 border border-purple/30 text-purple-200 font-sans text-xs leading-relaxed">
        <strong class="font-bold block mb-1 text-white">Did You Know? (NASA)</strong>
        In space, each astronaut needs almost 1 kilogram of pure oxygen and 2.5 liters of water per day! Since the Moon and Mars do not have Earth’s magnetic shield, we need to activate our shields when the Sun releases storms!
      </div>

      <button class="w-full py-2 rounded-xl bg-surface-low hover:bg-surface-high border border-cyan/40 text-cyan text-xs font-bold transition" onclick="openDataSourceModal()" type="button">
        Ver Fontes Oficiais da NASA →
      </button>
    </div>
  </aside>

  <!-- ==================== MODAL DE FIM DE JOGO: WARNING JUNTO AO ÍCONE ==================== -->
  <div class="fixed inset-0 z-50 bg-black/85 backdrop-blur-md hidden flex items-center justify-center p-4" id="modal-gameover">
    <div class="bg-surface-card border-2 border-[#ff0000] rounded-3xl max-w-lg w-full p-6 shadow-2xl flex flex-col gap-4 text-center">
      
      <!-- ÍCONE DE ALERTA COM "WARNING" BEM PRÓXIMO EM #ff0000 -->
      <div class="flex flex-col items-center">
        <div class="inline-flex items-center gap-1.5 px-4 py-2 rounded-2xl bg-[#ff0000]/20 border border-[#ff0000] text-[#ff0000] shadow-lg shadow-[#ff0000]/20">
          <span class="material-symbols-outlined text-[30px] leading-none text-[#ff0000]">warning</span>
          <span class="font-pixel text-base sm:text-lg tracking-wider text-[#ff0000] leading-none">WARNING</span>
        </div>
        <h2 class="text-2xl font-bold text-white mt-3">Orbital Mission Over!</h2>
        <p class="text-xs font-mono-code text-neutral-300 mt-0.5">You survived for: <strong class="text-[#05fa5b]" id="go-time">02:45</strong></p>
      </div>

      <!-- Diagnóstico Pedagógico Claro da Falha -->
      <div class="bg-[#ff0000]/15 border border-[#ff0000]/60 rounded-2xl p-4 text-left text-xs">
        <span class="text-[#ff0000] text-[10px] font-mono-code block uppercase font-bold">What happened at the base:</span>
        <p class="text-white mt-1 leading-relaxed text-sm font-semibold" id="go-reason">
          The crew ran out of oxygen to breathe!
        </p>
        <div class="bg-amber-950/40 border border-amber/40 rounded-xl p-2.5 mt-3 text-amber-200 text-xs" id="go-tip">
          Tip for next time: Click "Generate Oxygen" whenever the green bar gets low!
        </div>
      </div>
<!-- Quando a carcaça não aguenta, a moral sustenta -->
      <!-- Estatísticas da Partida -->
      <div class="grid grid-cols-3 gap-2 font-mono-code text-xs">
        <div class="bg-surface-low p-2 rounded-xl border border-surface-border">
          <span class="text-neutral-400 block text-[10px]">Largest Crew</span>
          <span class="font-bold text-white" id="go-stat-pop">4 astronauts</span>
        </div>
        <div class="bg-surface-low p-2 rounded-xl border border-[#a405fa]/40">
          <span class="text-neutral-400 block text-[10px]">Total Science</span>
          <span class="font-bold text-[#a405fa]" id="go-stat-sci">128 Science</span>
        </div>
        <div class="bg-surface-low p-2 rounded-xl border border-surface-border">
          <span class="text-neutral-400 block text-[10px]">Your Clicks</span>
          <span class="font-bold text-cyan" id="go-stat-clicks">94 actions</span>
        </div>
      </div>

      <!-- Botões: Jogar Novamente ou Main Menu -->
      <div class="flex flex-col sm:flex-row gap-2.5">
        <button class="flex-1 py-3 rounded-2xl bg-[#05faf2] hover:bg-[#3bfcf5] border border-[#05faf2] text-[#07191a] font-bold text-sm font-mono-code transition shadow-lg shadow-[#05faf2]/35 flex items-center justify-center gap-2 active:scale-95" onclick="restartGame()" type="button">
          <span class="material-symbols-outlined text-[20px]">refresh</span>
          <span>Play Again</span>
        </button>
        <button class="py-3 px-4 rounded-2xl bg-surface-low hover:bg-surface-high border border-amber/50 text-amber font-bold text-sm font-mono-code transition flex items-center justify-center gap-1.5 active:scale-95" onclick="returnToMenu()" type="button">
          <span class="material-symbols-outlined text-[18px]">home</span>
          <span>Menu</span>
        </button>
      </div>
    </div>
  </div>

  <!-- ==================== MODAL EXPLICATIVO: DE ONDE VÊM ESSES DADOS (LINKS NASA) ==================== -->
  <div class="fixed inset-0 z-50 bg-black/80 backdrop-blur-sm hidden flex items-center justify-center p-4" id="modal-data-source">
    <div class="bg-surface-card border border-surface-border rounded-2xl max-w-lg w-full p-5 sm:p-6 shadow-2xl flex flex-col gap-4 font-mono-code text-xs">
      <div class="flex items-center justify-between border-b border-surface-border pb-3">
        <div class="flex items-center gap-2.5 text-emerald-400">
          <span class="material-symbols-outlined text-[24px]">verified</span>
          <h3 class="font-bold text-white text-base">Real NASA Facts</h3>
        </div>
        <button class="w-7 h-7 rounded-lg bg-surface-low hover:bg-surface-high text-neutral-400 hover:text-white flex items-center justify-center" onclick="closeDataSourceModal()" type="button">
          <span class="material-symbols-outlined text-[18px]">close</span>
        </button>
      </div>
      <p class="text-neutral-200 leading-relaxed font-sans text-xs sm:text-sm">
        This game was created using real information used by NASA scientists and astronauts: <strong>NASA</strong>:
      </p>
      <div class="space-y-3">
        <div class="bg-surface-low p-3 rounded-xl border border-surface-border">
          <h4 class="font-bold text-cyan flex items-center gap-1.5 text-xs">
            <span class="material-symbols-outlined text-[16px]">sunny</span> Space Weather (NASA DONKI)
          </h4>
          <p class="text-neutral-300 text-[11px] mt-1 leading-normal font-sans">
            The Sun sometimes releases bursts of solar wind and radiation called solar storms. Scientists monitor the Sun around the clock to warn astronauts!
          </p>
          <a class="text-cyan underline text-[10px] mt-1.5 inline-block" href="https://kauai.ccmc.gsfc.nasa.gov/DONKI/" rel="noopener noreferrer" target="_blank">Visitar NASA DONKI →</a>
        </div>
        <div class="bg-surface-low p-3 rounded-xl border border-surface-border">
          <h4 class="font-bold text-emerald-400 flex items-center gap-1.5 text-xs">
            <span class="material-symbols-outlined text-[16px]">medical_services</span> Life in Space (NASA OSDR)
          </h4>
          <p class="text-neutral-300 text-[11px] mt-1 leading-normal font-sans">
            Studies how to protect astronauts’ health, recycle water, and produce oxygen using electricity (electrolysis) on the Moon and Mars.
          </p>
          <a class="text-emerald-400 underline text-[10px] mt-1.5 inline-block" href="https://osdr.nasa.gov/" rel="noopener noreferrer" target="_blank">Visitar NASA OSDR →</a>
        </div>
      </div>
      <div class="flex justify-end pt-2 border-t border-surface-border">
        <button class="px-4 py-2 rounded-xl bg-emerald-500 hover:bg-emerald-400 text-black font-bold transition" onclick="closeDataSourceModal()" type="button">
          Return to Base
        </button>
      </div>
    </div>
  </div>

  <!-- ==================== RODAPÉ EDUCATIVO COMPACTO ==================== -->
  <footer class="relative z-10 bg-[#0d1024]/75 backdrop-blur-sm border-t border-surface-border/50 py-2.5 px-4 text-center text-xs font-mono-code text-neutral-300">
    <div class="max-w-7xl mx-auto flex flex-col sm:flex-row items-center justify-between gap-2">
      <div>Orbit Explore • Space Advance (7 a 12 anos)</div>
      <div class="flex items-center gap-3">
        <span class="text-emerald-400">Green = Success</span>
        <span>•</span>
        <span class="text-amber">Yellow = Caution</span>
        <span>•</span>
        <span class="text-rose-400">Red = Danger</span>
      </div>
    </div>
  </footer>

  <!-- ==================== MOTOR DO JOGO JAVASCRIPT ==================== -->
  <script>
    (function() {
      // -------------------------------------------------------------
      // 1. ÁUDIO PROCEDURAL & TRILHA CHIPTUNE ESPACIAL (WEB AUDIO API)
      // -------------------------------------------------------------
      let audioCtx = null;
      let soundEnabled = true;
      let musicTimer = null;
      let musicStep = 0;

      function initAudio() {
        if (!audioCtx) {
          const AudioContext = window.AudioContext || window.webkitAudioContext;
          if (AudioContext) {
            audioCtx = new AudioContext();
          }
        }
        if (audioCtx && audioCtx.state === 'suspended') {
          audioCtx.resume();
        }
      }

      // Trilha Chiptune Retro Suave para o Menu e Exploração
      const CHIPTUNE_NOTES = [261.63, 329.63, 392.00, 523.25, 392.00, 329.63, 293.66, 392.00];

      function startBackgroundMusic() {
        if (musicTimer) return;
        musicTimer = setInterval(() => {
          if (!soundEnabled) return;
          try {
            initAudio();
            if (!audioCtx || audioCtx.state !== 'running') return;
            const now = audioCtx.currentTime;
            const osc = audioCtx.createOscillator();
            const gain = audioCtx.createGain();
            osc.type = 'triangle';
            const freq = CHIPTUNE_NOTES[musicStep % CHIPTUNE_NOTES.length];
            musicStep++;
            osc.frequency.setValueAtTime(freq, now);
            gain.gain.setValueAtTime(0.035, now);
            gain.gain.exponentialRampToValueAtTime(0.001, now + 0.35);
            osc.connect(gain);
            gain.connect(audioCtx.destination);
            osc.start(now);
            osc.stop(now + 0.36);
          } catch (e) {}
        }, 480);
      }

      function playSound(type) {
        if (!soundEnabled) return;
        try {
          initAudio();
          if (!audioCtx) return;

          const now = audioCtx.currentTime;
          const osc = audioCtx.createOscillator();
          const gain = audioCtx.createGain();
          osc.connect(gain);
          gain.connect(audioCtx.destination);

          if (type === 'click') {
            osc.type = 'triangle';
            osc.frequency.setValueAtTime(440, now);
            osc.frequency.exponentialRampToValueAtTime(880, now + 0.08);
            gain.gain.setValueAtTime(0.12, now);
            gain.gain.exponentialRampToValueAtTime(0.01, now + 0.08);
            osc.start(now);
            osc.stop(now + 0.08);
          } else if (type === 'upgrade') {
            osc.type = 'sine';
            osc.frequency.setValueAtTime(320, now);
            osc.frequency.exponentialRampToValueAtTime(640, now + 0.15);
            gain.gain.setValueAtTime(0.15, now);
            gain.gain.exponentialRampToValueAtTime(0.01, now + 0.15);
            osc.start(now);
            osc.stop(now + 0.15);
          } else if (type === 'alert') {
            osc.type = 'sawtooth';
            osc.frequency.setValueAtTime(220, now);
            osc.frequency.setValueAtTime(180, now + 0.1);
            gain.gain.setValueAtTime(0.16, now);
            gain.gain.exponentialRampToValueAtTime(0.01, now + 0.25);
            osc.start(now);
            osc.stop(now + 0.25);
          } else if (type === 'gameover') {
            osc.type = 'sawtooth';
            osc.frequency.setValueAtTime(160, now);
            osc.frequency.linearRampToValueAtTime(80, now + 0.6);
            gain.gain.setValueAtTime(0.22, now);
            gain.gain.linearRampToValueAtTime(0.01, now + 0.6);
            osc.start(now);
            osc.stop(now + 0.6);
          }
        } catch (e) {}
      }

      function syncSoundUI() {
        const icon = document.getElementById('icon-sound');
        const menuIcon = document.getElementById('menu-music-icon');
        const menuLabel = document.getElementById('menu-music-label');
        const musicBars = document.getElementById('music-bars');

        if (icon) icon.textContent = soundEnabled ? 'volume_up' : 'volume_off';
        if (menuIcon) menuIcon.textContent = soundEnabled ? 'volume_up' : 'volume_off';
        if (menuLabel) menuLabel.textContent = soundEnabled ? 'MUSIC: ENGAGED' : 'MUSIC: MUTED';
        if (musicBars) musicBars.style.opacity = soundEnabled ? '1' : '0.25';
      }

      window.toggleMenuMusic = function() {
        soundEnabled = !soundEnabled;
        syncSoundUI();
        if (soundEnabled) {
          initAudio();
          startBackgroundMusic();
          playSound('click');
        }
      };

      document.getElementById('btn-sound-toggle').addEventListener('click', () => {
        window.toggleMenuMusic();
      });

      // -------------------------------------------------------------
      // 2. MENU PRINCIPAL RETRO PIXEL ART & CANVAS DE ESTRELAS
      // -------------------------------------------------------------
      let inMenu = true;
      let menuSelectedIndex = 0;
      let selectedDestIndex = 0;
      let selectedDifficulty = 'easy'; // 'easy' (Explorer 7-12 anos) ou 'normal' (Astronaut)

      const DESTINATIONS = [
        { name: 'MARS (ARES)', sector: 'Ares Planitia Base', diffMult: 1.0 }
      ];

      // Animação do Céu Estrelado com Estrela Cadente (inspirado na imagem)
      function initMenuStarfield() {
        const canvas = document.getElementById('menu-starfield');
        if (!canvas) return;
        const ctx = canvas.getContext('2d');
        let stars = [];
        let shootingStar = { x: 0, y: 0, len: 0, speed: 0, active: false, wait: 30 };

        function resize() {
          canvas.width = window.innerWidth;
          canvas.height = window.innerHeight;
          stars = Array.from({ length: 120 }, () => ({
            x: Math.random() * canvas.width,
            y: Math.random() * canvas.height,
            size: Math.random() > 0.8 ? 2.5 : 1.5,
            alpha: Math.random(),
            speed: 0.015 + Math.random() * 0.025,
            color: Math.random() > 0.85 ? '#ffd000' : (Math.random() > 0.7 ? '#7bd0ff' : '#ffffff')
          }));
        }

        resize();
        window.addEventListener('resize', resize);

        function loop() {
          if (inMenu) {
            ctx.fillStyle = '#03050c';
            ctx.fillRect(0, 0, canvas.width, canvas.height);

            // Desenhar estrelas piscando
            stars.forEach(st => {
              st.alpha += st.speed;
              if (st.alpha > 1 || st.alpha < 0.15) st.speed *= -1;
              ctx.globalAlpha = Math.max(0.15, Math.min(1, st.alpha));
              ctx.fillStyle = st.color;
              ctx.fillRect(Math.floor(st.x), Math.floor(st.y), st.size, st.size);
            });

            // Estrela cadente diagonal no topo
            ctx.globalAlpha = 1;
            if (!shootingStar.active) {
              shootingStar.wait--;
              if (shootingStar.wait <= 0) {
                shootingStar = {
                  x: canvas.width * (0.45 + Math.random() * 0.35),
                  y: canvas.height * 0.04,
                  len: 75,
                  speed: 7,
                  active: true,
                  wait: 160 + Math.floor(Math.random() * 140)
                };
              }
            } else {
              const grad = ctx.createLinearGradient(
                shootingStar.x, shootingStar.y,
                shootingStar.x + shootingStar.len, shootingStar.y - shootingStar.len * 0.65
              );
              grad.addColorStop(0, '#ffffff');
              grad.addColorStop(0.3, '#ff9d3b');
              grad.addColorStop(1, 'rgba(255, 157, 59, 0)');
              ctx.strokeStyle = grad;
              ctx.lineWidth = 2.5;
              ctx.beginPath();
              ctx.moveTo(shootingStar.x, shootingStar.y);
              ctx.lineTo(shootingStar.x + shootingStar.len, shootingStar.y - shootingStar.len * 0.65);
              ctx.stroke();

              ctx.fillStyle = '#ffffff';
              ctx.fillRect(shootingStar.x - 1.5, shootingStar.y - 1.5, 3, 3);

              shootingStar.x -= shootingStar.speed;
              shootingStar.y += shootingStar.speed * 0.65;

              if (shootingStar.y > canvas.height * 0.55 || shootingStar.x < 0) {
                shootingStar.active = false;
              }
            }
          }
          requestAnimationFrame(loop);
        }
        loop();
      }

      window.selectMenuIndex = function(idx) {
        menuSelectedIndex = idx;
        const opts = document.querySelectorAll('.menu-opt');
        opts.forEach((btn, i) => {
          const arrow = btn.querySelector('.menu-arrow');
          if (i === idx) {
            btn.classList.add('pixel-option-active');
            btn.classList.remove('text-slate-300', 'text-slate-400');
            if (arrow) arrow.classList.remove('opacity-0');
          } else {
            btn.classList.remove('pixel-option-active');
            btn.classList.add(i === 1 ? 'text-slate-300' : 'text-slate-400');
            if (arrow) arrow.classList.add('opacity-0');
          }
        });
      };

      window.openMenuSettings = function() {
        playSound('click');
        document.getElementById('menu-settings-panel').classList.remove('hidden');
      };

      window.closeMenuSettings = function() {
        playSound('click');
        document.getElementById('menu-settings-panel').classList.add('hidden');
      };

      window.openMenuExit = function() {
        playSound('click');
        document.getElementById('menu-exit-panel').classList.remove('hidden');
      };

      window.closeMenuExit = function() {
        playSound('click');
        document.getElementById('menu-exit-panel').classList.add('hidden');
      };

      window.setMenuDestination = function(idx) {
        selectedDestIndex = 0;
        playSound('click');
        const btnMars = document.getElementById('set-dest-mars');
        if (btnMars) {
          btnMars.className = 'p-2.5 rounded border-2 border-amber bg-amber/20 text-white font-bold transition cursor-pointer text-left flex items-center justify-between';
        }
        document.getElementById('menu-preview-dest').textContent = DESTINATIONS[0].name;
      };

      window.setMenuDifficulty = function(diffKey) {
        selectedDifficulty = diffKey;
        playSound('click');
        const btnEasy = document.getElementById('set-diff-easy');
        const btnNormal = document.getElementById('set-diff-normal');
        const previewDiff = document.getElementById('menu-preview-diff');
        if (diffKey === 'easy') {
          btnEasy.className = 'p-2.5 rounded border-2 border-emerald-400 bg-emerald-500/20 text-white font-bold transition cursor-pointer';
          btnNormal.className = 'p-2.5 rounded border border-neutral-600 bg-neutral-900 text-neutral-300 font-bold transition cursor-pointer';
          previewDiff.textContent = 'EXPLORER (AGES 7-12)';
          previewDiff.className = 'text-emerald-400';
        } else {
          btnNormal.className = 'p-2.5 rounded border-2 border-amber bg-amber/20 text-white font-bold transition cursor-pointer';
          btnEasy.className = 'p-2.5 rounded border border-neutral-600 bg-neutral-900 text-neutral-300 font-bold transition cursor-pointer';
          previewDiff.textContent = 'ASTRONAUT (NORMAL)';
          previewDiff.className = 'text-amber';
        }
      };

      window.startFromMenu = function() {
        initAudio();
        startBackgroundMusic();
        playSound('upgrade');
        inMenu = false;
        document.getElementById('retro-menu-screen').classList.add('hidden');
        document.getElementById('menu-settings-panel').classList.add('hidden');
        document.getElementById('menu-exit-panel').classList.add('hidden');
        restartGame(selectedDestIndex);
      };

      window.returnToMenu = function() {
        inMenu = true;
        playSound('click');
        document.getElementById('modal-gameover').classList.add('hidden');
        document.getElementById('retro-menu-screen').classList.remove('hidden');
        selectMenuIndex(0);
      };

      // -------------------------------------------------------------
      // 3. ESTADO DO JOGO & DADOS DA PARTIDA
      // -------------------------------------------------------------
      let S = {
        destination: DESTINATIONS[0],
        difficulty: 'easy',
        secondsAlive: 0,
        isGameOver: false,
        totalClicks: 0,
        totalSciEarned: 0,
        peakPopulation: 2,

        // 5 Recursos Essenciais + Science
        water: 120,
        food: 120,
        o2: 120,
        energy: 140,
        radiation: 5,
        science: 20,

        // Limites máximos
        capWater: 200,
        capFood: 200,
        capO2: 200,
        capEnergy: 250,
        capPop: 5,

        // População & Health
        population: 2,
        // Níveis de Upgrades (6 Clássicos + 3 Automatizações)
        upgrades: {
          recycler: 0,
          greenhouse: 0,
          solar: 0,
          shield: 0,
          habitat: 0,
          antenna: 0,
          autoIce: 0,
          autoO2: 0,
          autoSci: 0,
          algae: 0
        },

        // Evento de clima ativo
        activeEvent: null,
        eventTimer: 0,
        nextEventCountdown: 22
      };

      function getModeDrainMultiplier() {
        return S.difficulty === 'easy' ? 0.72 : 1.0;
      }

      // Algas: +1 Ar/s quando compradas
      function algaeO2Rate() {
        return S.upgrades.algae * 1.0;
      }

      // Recursos ilimitados: mostra ∞ e a barra usa a escala antiga (200/250) só para visual
      function fmtCap(v) { return isFinite(v) ? v : '∞'; }
      function barPct(v, cap, ref) { return Math.min(100, (v / (isFinite(cap) ? cap : ref)) * 100); }

      function getShieldDrain() {
        return Number(Math.max(0.5, 4.0 - (S.upgrades.shield * 0.35)).toFixed(1));
      }

      function getDifficultyPhase() {
        if (S.secondsAlive < 60) {
          return { phase: 1, label: 'Phase 1 (Beginning)', colorClass: 'text-emerald-400', radMult: 0.9, eventMin: 20, eventRange: 8 };
        } else if (S.secondsAlive < 150) {
          return { phase: 2, label: 'Phase 2 (Moderate Activity)', colorClass: 'text-amber', radMult: 1.25, eventMin: 16, eventRange: 8 };
        } else {
          return { phase: 3, label: 'Phase 3 (Intense Storms)', colorClass: 'text-rose-400', radMult: 1.6, eventMin: 13, eventRange: 6 };
        }
      }

      // -------------------------------------------------------------
      // 4. BARRA SUPERIOR DE ALERTAS (VERDE / AMARELO / VERMELHO)
      // -------------------------------------------------------------
      let currentStatusAlert = {
        type: 'success', // 'success' (verde), 'warning' (amarelo), 'error' (vermelho)
        badge: 'ALL CLEAR',
        text: 'All systems go! Click the colored buttons below to collect resources.'
      };

      function setTopAlert(type, badge, text) {
        currentStatusAlert = { type, badge, text };
        renderTopAlertBar();
      }

      function renderTopAlertBar() {
        const bar = document.getElementById('top-alert-bar');
        const icon = document.getElementById('top-alert-icon');
        const badgeEl = document.getElementById('top-alert-badge');
        const textEl = document.getElementById('top-alert-text');
        const timerEl = document.getElementById('weather-timer');
        const actionsDiv = document.getElementById('weather-actions');

        if (!bar) return;

        // Se houver Evento de Clima Espacial ativo, ele assume destaque na barra superior
        if (S.activeEvent) {
          const isGoodEvent = S.activeEvent.id === 'earth_comm';
          if (isGoodEvent) {
            bar.className = 'rounded-xl px-3 py-1.5 border border-emerald-400/70 bg-emerald-950/65 flex flex-col md:flex-row items-center justify-between gap-2 text-xs font-mono-code transition-all';
            icon.className = 'material-symbols-outlined text-emerald-400 text-[18px] shrink-0';
            icon.textContent = 'cell_tower';
            badgeEl.className = 'px-1.5 py-0.5 rounded text-[10px] font-bold uppercase bg-emerald-500/25 text-emerald-300 shrink-0';
            badgeEl.textContent = 'NASA BONUS';
          } else {
            bar.className = 'rounded-xl px-3 py-1.5 border border-amber/80 bg-amber-950/75 flex flex-col md:flex-row items-center justify-between gap-2 text-xs font-mono-code transition-all';
            icon.className = 'material-symbols-outlined text-amber text-[18px] shrink-0 animate-bounce';
            icon.textContent = 'warning';
            badgeEl.className = 'px-1.5 py-0.5 rounded text-[10px] font-bold uppercase bg-amber/25 text-amber shrink-0';
            badgeEl.textContent = 'SOLAR ALERT';
          }
          textEl.className = 'text-white font-sans text-xs sm:text-sm font-semibold';
          textEl.textContent = `${S.activeEvent.title}: ${S.activeEvent.shortDesc}`;
          timerEl.classList.remove('hidden');
          timerEl.textContent = `${String(S.eventTimer).padStart(2, '0')}s`;

          actionsDiv.innerHTML = '';
          S.activeEvent.actions.forEach(act => {
            const btn = document.createElement('button');
            btn.type = 'button';
            btn.className = isGoodEvent
              ? 'px-2.5 py-1 rounded-lg bg-emerald-500 hover:bg-emerald-400 text-black font-bold text-[11px] transition active:scale-95'
              : 'px-2.5 py-1 rounded-lg bg-amber hover:bg-yellow-300 text-black font-bold text-[11px] transition active:scale-95';
            btn.textContent = act.text;
            btn.setAttribute('onclick', act.onclick);
            actionsDiv.appendChild(btn);
          });
          return;
        }

        // Caso contrário, exibe o status normal com semântica Verde / Amarelo / Vermelho
        timerEl.classList.add('hidden');
        actionsDiv.innerHTML = '';
        textEl.textContent = currentStatusAlert.text;
        badgeEl.textContent = currentStatusAlert.badge;

        if (currentStatusAlert.type === 'error') {
          bar.className = 'rounded-xl px-3 py-1.5 border border-rose-500/75 bg-rose-950/65 flex flex-col md:flex-row items-center justify-between gap-2 text-xs font-mono-code transition-all';
          icon.className = 'material-symbols-outlined text-rose-400 text-[18px] shrink-0 animate-pulse';
          icon.textContent = 'error';
          badgeEl.className = 'px-1.5 py-0.5 rounded text-[10px] font-bold uppercase bg-rose-500/25 text-rose-300 shrink-0';
          textEl.className = 'text-rose-100 font-sans text-xs sm:text-sm font-semibold min-w-0';
        } else if (currentStatusAlert.type === 'warning') {
          bar.className = 'rounded-xl px-3 py-1.5 border border-amber/70 bg-amber-950/55 flex flex-col md:flex-row items-center justify-between gap-2 text-xs font-mono-code transition-all';
          icon.className = 'material-symbols-outlined text-amber text-[18px] shrink-0';
          icon.textContent = 'warning';
          badgeEl.className = 'px-1.5 py-0.5 rounded text-[10px] font-bold uppercase bg-amber/25 text-amber shrink-0';
          textEl.className = 'text-amber-100 font-sans text-xs sm:text-sm font-medium min-w-0';
        } else {
          bar.className = 'rounded-xl px-3 py-1.5 border border-emerald-500/45 bg-emerald-950/40 flex flex-col md:flex-row items-center justify-between gap-2 text-xs font-mono-code transition-all';
          icon.className = 'material-symbols-outlined text-emerald-400 text-[18px] shrink-0';
          icon.textContent = 'check_circle';
          badgeEl.className = 'px-1.5 py-0.5 rounded text-[10px] font-bold uppercase bg-emerald-500/20 text-emerald-300 shrink-0';
          textEl.className = 'text-emerald-100 font-sans text-xs sm:text-sm font-medium min-w-0';
        }
      }

      // -------------------------------------------------------------
      // 5. NÚCLEO CLICKER (<100ms FEEDBACK, NÚMEROS FLUTUANTES)
      // -------------------------------------------------------------
      function spawnFloatingText(targetEl, text, colorClass = 'text-emerald-400') {
        if (!targetEl) return;
        const rect = targetEl.getBoundingClientRect();
        const span = document.createElement('span');
        span.className = `float-text ${colorClass} text-xs`;
        span.textContent = text;
        span.style.left = `${rect.left + rect.width / 2}px`;
        span.style.top = `${rect.top}px`;
        document.body.appendChild(span);
        setTimeout(() => span.remove(), 900);
      }

      window.actionMineIce = function(e) {
        if (S.isGameOver || inMenu) return;
        const btn = (e && e.currentTarget) || document.getElementById('btn-click-ice');
        if (S.energy < 5) {
          setTopAlert('error', 'ERROR: ENERGY', 'Not enough energy! Click the yellow "Solar Panel" button first.');
          playSound('alert');
          return;
        }
        S.energy -= 5;
        const gain = 8 + (S.upgrades.recycler * 2);
        S.water = Math.min(S.capWater, S.water + gain);
        S.totalClicks++;
        playSound('click');
        spawnFloatingText(btn, `+${gain} Water!`, 'text-emerald-400');
        setTopAlert('success', 'SUCCESS', `You melted ice and gained +${gain} liters of Water!`);
        renderHUD();
      };

      window.actionHarvestFood = function(e) {
        if (S.isGameOver || inMenu) return;
        const btn = (e && e.currentTarget) || document.getElementById('btn-click-greenhouse');
        if (S.energy < 4) {
          setTopAlert('error', 'ERROR: ENERGY', 'Not enough energy for the greenhouse! Click "Solar Panel".');
          playSound('alert');
          return;
        }
        S.energy -= 4;
        const gain = 8 + (S.upgrades.greenhouse * 4);
        S.food = Math.min(S.capFood, S.food + gain);
        S.totalClicks++;
        playSound('click');
        spawnFloatingText(btn, `+${gain} Food!`, 'text-emerald-400');
        setTopAlert('success', 'SUCCESS', `Harvest complete! +${gain} Food stored for the crew.`);
        renderHUD();
      };

      window.actionElectrolysis = function(e) {
        if (S.isGameOver || inMenu) return;
        const btn = (e && e.currentTarget) || document.getElementById('btn-click-o2');
        if (S.water < 4) {
          setTopAlert('error', 'ERROR: WATER', 'You need 4 Water to generate oxygen! Click "Melt Ice".');
          playSound('alert');
          return;
        }
        if (S.energy < 5) {
          setTopAlert('error', 'ERROR: ENERGY', 'You need 5 Energy! Click "Solar Panel".');
          playSound('alert');
          return;
        }
        S.water -= 4;
        S.energy -= 5;
        const gain = 10 + (S.upgrades.habitat * 2);
        S.o2 = Math.min(S.capO2, S.o2 + gain);
        S.totalClicks++;
        playSound('click');
        spawnFloatingText(btn, `+${gain} Ar!`, 'text-emerald-400');
        setTopAlert('success', 'SUCCESS', `Fresh oxygen generated! +${gain} Oxygen to breathe.`);
        renderHUD();
      };

      window.actionCollectSolar = function(e) {
        if (S.isGameOver || inMenu) return;
        const btn = (e && e.currentTarget) || document.getElementById('btn-click-solar');
        let gain = 12 + (S.upgrades.solar * 5);
        if (S.activeEvent && S.activeEvent.id === 'dust_storm') {
          gain = Math.max(4, Math.floor(gain * 0.35));
        }
        S.energy = Math.min(S.capEnergy, S.energy + gain);
        S.totalClicks++;
        playSound('click');
        spawnFloatingText(btn, `+${gain} Energy!`, 'text-emerald-400');
        if (!S.activeEvent) {
          setTopAlert('success', 'SUCCESS', `Batteries charged! +${gain} solar Energy.`);
        }
        renderHUD();
      };

      window.actionResearchExp = function(e) {
        if (S.isGameOver || inMenu) return;
        const btn = (e && e.currentTarget) || document.getElementById('btn-click-experiment');
        if (S.energy < 6) {
          setTopAlert('error', 'ERROR: ENERGY', 'The computer is out of power! Click "Solar Panel".');
          playSound('alert');
          return;
        }
        S.energy -= 6;
        const gain = 6 + (S.upgrades.antenna * 3);
        S.science += gain;
        S.totalSciEarned += gain;
        S.totalClicks++;
        playSound('click');
        spawnFloatingText(btn, `+${gain} Science`, 'text-emerald-400');
        setTopAlert('success', 'SUCCESS', `Experiment complete! +${gain} Science points.`);
        renderHUD();
      };

      // Atalhos de Teclado (Menu e Jogo)
      document.addEventListener('keydown', (e) => {
        if (inMenu) {
          const settingsOpen = !document.getElementById('menu-settings-panel').classList.contains('hidden');
          const exitOpen = !document.getElementById('menu-exit-panel').classList.contains('hidden');
          if (settingsOpen || exitOpen) {
            if (e.key === 'Escape' || e.key === 'Enter') {
              closeMenuSettings();
              closeMenuExit();
            }
            return;
          }
          if (e.key === 'ArrowDown' || e.key === 's' || e.key === 'S') {
            e.preventDefault();
            playSound('click');
            selectMenuIndex((menuSelectedIndex + 1) % 3);
          } else if (e.key === 'ArrowUp' || e.key === 'w' || e.key === 'W') {
            e.preventDefault();
            playSound('click');
            selectMenuIndex((menuSelectedIndex + 2) % 3);
          } else if (e.key === 'Enter' || e.key === ' ') {
            e.preventDefault();
            if (menuSelectedIndex === 0) startFromMenu();
            else if (menuSelectedIndex === 1) openMenuSettings();
            else if (menuSelectedIndex === 2) openMenuExit();
          } else if (e.key.length === 1) {
            // [PRESS ANY KEY] inicia diretamente como na imagem
            startFromMenu();
          }
          return;
        }

        if (S.isGameOver) return;
        if (e.target.tagName === 'INPUT' || e.target.tagName === 'TEXTAREA') return;

        const keyMap = {
          '1': { fn: actionMineIce, id: 'btn-click-ice' },
          '2': { fn: actionHarvestFood, id: 'btn-click-greenhouse' },
          '3': { fn: actionElectrolysis, id: 'btn-click-o2' },
          '4': { fn: actionCollectSolar, id: 'btn-click-solar' },
          '5': { fn: actionResearchExp, id: 'btn-click-experiment' }
        };

        const actionObj = keyMap[e.key];
        if (actionObj) {
          const btn = document.getElementById(actionObj.id);
          if (btn) {
            btn.classList.add('key-pressed');
            setTimeout(() => btn.classList.remove('key-pressed'), 100);
          }
          actionObj.fn();
        }
      });

      // -------------------------------------------------------------
      // 6. POPULAÇÃO, RECRUTAMENTO & BLINDAGEM
      // -------------------------------------------------------------

      window.toggleShield = function() {
        if (S.isGameOver || inMenu) return;
        const drain = getShieldDrain();
        if (!S.shieldActive && S.energy < drain) {
          setTopAlert('error', 'ERROR: ENERGY', `You need ${drain.toFixed(1)} Energy to activate the shield!`);
          playSound('alert');
          return;
        }
        S.shieldActive = !S.shieldActive;
        playSound('click');
        if (S.shieldActive) {
          setTopAlert('success', 'SHIELD ACTIVATED', `Shield activated! Reducing radiation (-${drain.toFixed(1)} Energy/s).`);
        } else {
          setTopAlert('warning', 'CAUTION', 'Shield deactivated to conserve energy.');
        }
        renderHUD();
      };

      // -------------------------------------------------------------
      // 7. SISTEMA DE UPGRADES ACELERADOS COM PESQUISA (SCI)
      // -------------------------------------------------------------
      const UPGRADES_DEF = {
        recycler:   { baseCost: 15, maxLvl: Infinity, costMult: 1.3, defaultBtnLabel: 'Upgrade', title: 'Water Recycler' },
        greenhouse: { baseCost: 20, maxLvl: Infinity, costMult: 1.3, defaultBtnLabel: 'Upgrade', title: 'Advanced Greenhouse' },
        solar:      { baseCost: 18, maxLvl: Infinity, costMult: 1.3, defaultBtnLabel: 'Upgrade', title: 'Foldable Solar Panels' },
        shield:     { baseCost: 25, maxLvl: Infinity, costMult: 1.3, defaultBtnLabel: 'Upgrade', title: 'Optimized Shield' },
        habitat:    { baseCost: 30, maxLvl: Infinity, costMult: 1.3, defaultBtnLabel: 'Build', title: 'Habitat Module' },
        antenna:    { baseCost: 35, maxLvl: Infinity, costMult: 1.3, defaultBtnLabel: 'Install', title: 'Space Antenna' },
        autoIce:    { baseCost: 22, maxLvl: Infinity, costMult: 1.3, defaultBtnLabel: 'Activate',   title: 'Ice Mining Robot' },
        autoO2:     { baseCost: 24, maxLvl: Infinity, costMult: 1.3, defaultBtnLabel: 'Activate',   title: 'Oxygen Generator' },
        autoSci:    { baseCost: 26, maxLvl: Infinity, costMult: 1.3, defaultBtnLabel: 'Activate',  title: 'Science Drone' },
        algae:      { baseCost: 20, maxLvl: Infinity, costMult: 1.3, defaultBtnLabel: 'Activate',   title: 'Oxygen-Producing Algae' }
      };

      function getUpgradeCost(key) {
        const def = UPGRADES_DEF[key];
        const lvl = S.upgrades[key];
        return Math.round(def.baseCost * Math.pow(def.costMult, lvl));
      }

      window.buyUpgrade = function(key, e) {
        if (S.isGameOver || inMenu) return;
        const def = UPGRADES_DEF[key];
        const currentLvl = S.upgrades[key];
        if (currentLvl >= def.maxLvl) return;

        const btn = (e && e.currentTarget) || document.getElementById(`btn-up-${key}`);
        const cost = getUpgradeCost(key);
        if (S.science >= cost) {
          S.science -= cost;
          S.upgrades[key] += 1;

          if (key === 'habitat') {
            S.capPop += 10;
            S.capWater += 50;
            S.capFood += 50;
            S.capO2 += 50;
            S.capEnergy += 50;
          }

          setTopAlert('success', 'UPGRADE COMPLETE', `Congratulations! ${def.title} has been upgraded!`);
          playSound('upgrade');
          spawnFloatingText(btn, 'Melhorado!', 'text-blue-400');
          renderHUD();
        } else {
          setTopAlert('error', 'ERROR: SCIENCE', `You need ${cost} Science points! Click "Research".`);
          playSound('alert');
        }
      };

      // -------------------------------------------------------------
      // 8. EVENTOS DE CLIMA ESPACIAL (NASA DONKI INSPIRATION)
      // -------------------------------------------------------------
      const WEATHER_EVENTS = [
        {
          id: 'solar_storm',
          title: 'Solar Storm!',
          shortDesc: 'The Sun has released a burst of radiation! Use the shield to protect the crew.',
          duration: 10,
          actions: [
            { text: 'Super Shield (-25 Energy)', onclick: 'resolveEvent("shield_boost", event)' },
            { text: 'Abrigar Crew', onclick: 'resolveEvent("shelter", event)' }
          ]
        },
        {
          id: 'solar_flare',
          title: 'Solar Flare!',
          shortDesc: 'The solar panels are generating less energy for a few seconds.',
          duration: 9,
          actions: [
            { text: 'Adjust Panels (-15 Energy)', onclick: 'resolveEvent("reorient_panels", event)' }
          ]
        },
        {
          id: 'earth_comm',
          title: 'Strong Signal from Earth!',
          shortDesc: 'NASA has sent help! Choose extra Science or Water:',
          duration: 12,
          actions: [
            { text: 'Gain +25 Science', onclick: 'resolveEvent("send_telemetry", event)' },
            { text: 'Receive +30 Water', onclick: 'resolveEvent("call_supplies", event)' }
          ]
        },
        {
          id: 'dust_storm',
          title: 'Dust Cloud!',
          shortDesc: 'Dust has covered the solar panels! Clean the sensors to restore normal operation.',
          duration: 10,
          actions: [
            { text: 'Clean Panels (-10 Energy)', onclick: 'resolveEvent("clean_sensors", event)' }
          ]
        },
        {
          id: 'new_astronauts',
          title: 'Astronaut Arrival!',
          shortDesc: '',
          duration: 6,
          actions: []
        }
      ];

      function triggerRandomWeatherEvent() {
        const randEvt = WEATHER_EVENTS[Math.floor(Math.random() * WEATHER_EVENTS.length)];
        const diff = getDifficultyPhase();
        S.activeEvent = { ...randEvt };
        if (randEvt.id === 'new_astronauts') {
          // Chegada: entra 1 astronauta se a base tiver espaço
          if (S.population < S.capPop) {
            S.population += 1;
            if (S.population > S.peakPopulation) S.peakPopulation = S.population;
            S.activeEvent.shortDesc = `A spacecraft from Earth has arrived! 1 more astronaut has joined the base (there are now ${S.population}).`;
          } else {
            S.activeEvent.shortDesc = 'A spacecraft from Earth has arrived, but the base is full! Buy the Habitat Module to make room for more astronauts.';
          }
        }
        S.eventTimer = randEvt.id === 'earth_comm' ? randEvt.duration : randEvt.duration + (diff.phase - 1) * 2;
        playSound(randEvt.id === 'earth_comm' ? 'upgrade' : 'alert');
        renderTopAlertBar();
        renderHUD();
      }

      window.resolveEvent = function(actionType, e) {
        if (!S.activeEvent || S.isGameOver || inMenu) return;
        const btn = e && e.currentTarget;

        if (actionType === 'shield_boost') {
          if (S.energy >= 25) {
            S.energy -= 25;
            S.radiation = Math.max(0, S.radiation - 15);
            endActiveEvent();
            setTopAlert('success', 'SUCCESS', 'Super Shield activated! Radiation reduced by 15!');
            playSound('upgrade');
            spawnFloatingText(btn, '-15 Radiation!', 'text-emerald-400');
          } else {
            setTopAlert('error', 'ERROR: ENERGY', 'You need 25 Energy! Click "Solar Panel".');
            playSound('alert');
          }
        } else if (actionType === 'shelter') {
          const sciCost = Math.max(2, Math.floor(S.science * 0.25));
          S.science = Math.max(0, S.science - sciCost);
          S.radiation = Math.max(0, S.radiation - 8);
          endActiveEvent();
          setTopAlert('warning', 'CAUTION', `Crew protected in the safe shelter! (-${sciCost} Science).`);
          playSound('upgrade');
          spawnFloatingText(btn, 'Abrigo Seguro!', 'text-emerald-400');
        } else if (actionType === 'reorient_panels') {
          if (S.energy >= 15) {
            S.energy -= 15;
            endActiveEvent();
            setTopAlert('success', 'SUCCESS', 'Solar panels successfully adjusted!');
            playSound('upgrade');
            spawnFloatingText(btn, 'Panels OK!', 'text-emerald-400');
          } else {
            setTopAlert('error', 'ERROR: ENERGY', 'You need 15 Energy to rotate the panels!');
            playSound('alert');
          }
        } else if (actionType === 'send_telemetry') {
          S.science += 25;
          S.totalSciEarned += 25;
          endActiveEvent();
          setTopAlert('success', 'SUCCESS', 'Data sent to Earth! You gained +25 Science!');
          playSound('upgrade');
          spawnFloatingText(btn, '+25 Science!', 'text-emerald-400');
        } else if (actionType === 'call_supplies') {
          S.water = Math.min(S.capWater, S.water + 30);
          endActiveEvent();
          setTopAlert('success', 'SUCCESS', 'Supply rocket has arrived! +30 Water at the base!');
          playSound('upgrade');
          spawnFloatingText(btn, '+30 Water!', 'text-emerald-400');
        } else if (actionType === 'clean_sensors') {
          if (S.energy >= 10) {
            S.energy -= 10;
            endActiveEvent();
            setTopAlert('success', 'SUCCESS', 'Dust cleared! The solar panels are shining again!');
            playSound('upgrade');
            spawnFloatingText(btn, 'Panels Cleaned!', 'text-emerald-400');
          } else {
            setTopAlert('error', 'ERROR: ENERGY', 'You need 10 Energy to blow away the dust!');
            playSound('alert');
          }
        }
        renderHUD();
      };

      function endActiveEvent() {
        S.activeEvent = null;
        S.eventTimer = 0;
        document.getElementById('game-body').classList.remove('screen-alert');
        renderTopAlertBar();
      }

      // -------------------------------------------------------------
      // 9. CONDIÇÃO DE DERROTA & DIAGNÓSTICO PEDAGÓGICO (7-12 ANOS)
      // -------------------------------------------------------------
      function triggerGameOver(reasonText, tipText) {
        if (S.isGameOver) return;
        S.isGameOver = true;
        playSound('gameover');

        const min = String(Math.floor(S.secondsAlive / 60)).padStart(2, '0');
        const sec = String(S.secondsAlive % 60).padStart(2, '0');

        document.getElementById('go-time').textContent = `${min}:${sec}`;
        document.getElementById('go-reason').textContent = reasonText;
        document.getElementById('go-tip').textContent = `💡 Tip for success: ${tipText}`;
        document.getElementById('go-stat-pop').textContent = `${S.peakPopulation} astronauts`;
        document.getElementById('go-stat-sci').textContent = `${Math.round(S.totalSciEarned)} Science`;
        document.getElementById('go-stat-clicks').textContent = `${S.totalClicks} clicks`;

        document.getElementById('modal-gameover').classList.remove('hidden');
      }

      window.restartGame = function() {
        const nextDest = DESTINATIONS[0];

        S = {
          destination: nextDest,
          difficulty: selectedDifficulty,
          secondsAlive: 0,
          isGameOver: false,
          totalClicks: 0,
          totalSciEarned: 0,
          peakPopulation: 2,

          water: 120,
          food: 120,
          o2: 120,
          energy: 140,
          radiation: 5,
          science: 20,

          capWater: 200,
          capFood: 200,
          capO2: 200,
          capEnergy: 250,
          capPop: 5,

          population: 2,
          health: 100,
          shieldActive: false,

          upgrades: {
            recycler: 0,
            greenhouse: 0,
            solar: 0,
            shield: 0,
            habitat: 0,
            antenna: 0,
            autoIce: 0,
            autoO2: 0,
            autoSci: 0,
            algae: 0
          },

          activeEvent: null,
          eventTimer: 0,
          nextEventCountdown: 22
        };

        document.getElementById('modal-gameover').classList.add('hidden');
        document.getElementById('badge-dest').textContent = nextDest.name;
        document.getElementById('label-hud-sector').textContent = nextDest.sector;
        const badgeMode = document.getElementById('badge-mode');
        if (badgeMode) {
          badgeMode.textContent = S.difficulty === 'easy' ? 'Explorer' : 'Astronaut';
        }
        endActiveEvent();
        setTopAlert('success', 'MISSION STARTED', `Welcome to ${nextDest.name}! Keep Health in the green by taking care of your base.`);
        renderHUD();
      };

      // -------------------------------------------------------------
      // 10. CICLO DE SIMULAÇÃO (1 SEGUNDO / 1000MS)
      // -------------------------------------------------------------
      setInterval(() => {
        if (S.isGameOver || inMenu) return;

        S.secondsAlive++;
        const diff = getDifficultyPhase();
        const drainMult = getModeDrainMultiplier();

        // 1. Dreno por astronauta ajustado pelo modo
        const waterPerAstro = Math.max(0.05, (0.5 - (S.upgrades.recycler * 0.045)) * drainMult);
        const foodPerAstro = 0.5 * drainMult;
        const o2PerAstro = 0.6 * drainMult;

        const totalWaterDrain = S.population * waterPerAstro;
        const totalFoodDrain = S.population * foodPerAstro;
        const totalO2Drain = S.population * o2PerAstro;

        // 2. Produção passiva e automatizada (Ice Mining Robot, Estufa, Oxygen Generator, Painéis, Science Drone)
        const autoWaterGain = S.upgrades.autoIce * 2.0;
        const passiveFood = S.upgrades.greenhouse * 1.0;
        const autoO2Gain = S.upgrades.autoO2 * 2.5;

        S.water = Math.min(S.capWater, Math.max(0, S.water - totalWaterDrain + autoWaterGain));
        S.food = Math.min(S.capFood, Math.max(0, S.food - totalFoodDrain + passiveFood));
        S.o2 = Math.min(S.capO2, Math.max(0, S.o2 - totalO2Drain + autoO2Gain + algaeO2Rate()));

        let solarGain = 3.2 + (S.upgrades.solar * 2.0);
        if (S.activeEvent && S.activeEvent.id === 'dust_storm') {
          solarGain *= 0.35;
        } else if (S.activeEvent && S.activeEvent.id === 'solar_flare') {
          solarGain *= 0.45;
        }
        S.energy = Math.min(S.capEnergy, S.energy + solarGain);

        // Blindagem eletromagnética drenando energia
        if (S.shieldActive) {
          const shieldDrain = getShieldDrain();
          if (S.energy >= shieldDrain) {
            S.energy -= shieldDrain;
            S.radiation = Math.max(0, S.radiation - 1.3);
          } else {
            S.shieldActive = false;
            playSound('alert');
            setTopAlert('error', 'SHIELD DEACTIVATED', 'The shield has run out of energy! Click "Solar Panel".');
          }
        }

        // Radiation ambiental natural acumulando
        let radInc = 0.22 * S.destination.diffMult * diff.radMult * drainMult;
        if (S.activeEvent && S.activeEvent.id === 'solar_storm') {
          radInc = 2.2 * diff.radMult * drainMult;
        }
        if (!S.shieldActive) {
          S.radiation = Math.min(100, S.radiation + radInc);
        }

        // Science passiva gerada pelos astronautas + Antena + Science Drone Autônomo
        const droneBonus = S.upgrades.autoSci > 0 && S.secondsAlive % 10 === 0 ? S.upgrades.autoSci * 2 : 0;
        const sciGain = (S.population * 1.0) + (S.upgrades.antenna * 1.5) + (S.upgrades.autoSci * 2.0) + droneBonus;
        S.science += sciGain;
        S.totalSciEarned += sciGain;

        // 3. Verificação de Health / Saúde e Avisos Preventivos para Crianças
        let healthDamage = 0;
        let failReasons = [];

        if (S.water <= 0) {
          healthDamage += 7 * drainMult;
          failReasons.push('ran out of Water');
        }
        if (S.food <= 0) {
          healthDamage += 5 * drainMult;
          failReasons.push('ran out of Food');
        }
        if (S.o2 <= 0) {
          healthDamage += 10 * drainMult;
          failReasons.push('ran out of Oxygen');
        }
        if (S.radiation >= 85) {
          healthDamage += 6 * drainMult;
          failReasons.push('Radiation became too high');
        }

        if (healthDamage > 0) {
          S.health = Math.max(0, S.health - healthDamage);
          setTopAlert('error', 'BASE IN DANGER', `Warning: ${failReasons.join(' and ')}! Act quickly to replenish supplies!`);
        } else {
          // Avisos amarelos preventivos antes de acabar
          if (!S.activeEvent) {
            if (S.o2 < 35) {
              setTopAlert('warning', 'OXYGEN ALERT', 'Oxygen is running low! Click "Generate Oxygen".');
            } else if (S.water < 35) {
              setTopAlert('warning', 'WATER ALERT', 'Water is running low! Click "Melt Ice".');
            } else if (S.food < 35) {
              setTopAlert('warning', 'FOOD ALERT', 'Food is running low in the greenhouse! Click "Harvest Plants".');
            } else if (S.radiation >= 65 && !S.shieldActive) {
              setTopAlert('warning', 'RADIATION ALERT', 'Radiation is rising! Activate the "Radiation Shield".');
            }
          }

          if (S.health < 100 && S.water > 25 && S.food > 25 && S.o2 > 25 && S.radiation < 50) {
            S.health = Math.min(100, S.health + 1.5);
          }
        }

        // CONDIÇÃO DE DERROTA
        if (S.health <= 0) {
          const reasonMsg = failReasons.length > 0
            ? `The mission ended because ${failReasons.join(' and ')} at the base!`
            : 'The space base systems ran out of resources!';
          let tipMsg = 'Click the Water, Food, and Energy buttons whenever their bars get low.';
          if (failReasons.includes('ran out of Oxygen')) {
            tipMsg = 'Click "Melt Ice" to get Water, then "Generate Oxygen" so everyone can breathe!';
          } else if (failReasons.includes('Radiation became too high')) {
            tipMsg = 'When the red Radiation bar rises, click "Activate Shield" to clear it!';
          }
          triggerGameOver(reasonMsg, tipMsg);
          return;
        }

        // 4. Temporizador de Eventos de Clima Espacial
        if (S.activeEvent) {
          S.eventTimer--;
          if (S.eventTimer <= 0) {
            const endedName = S.activeEvent.title;
            endActiveEvent();
            setTopAlert('success', 'ALL CLEAR', `${endedName} has ended! The base is back to normal.`);
          } else {
            renderTopAlertBar();
          }
        } else {
          S.nextEventCountdown--;
          if (S.nextEventCountdown <= 0) {
            triggerRandomWeatherEvent();
            S.nextEventCountdown = diff.eventMin + Math.floor(Math.random() * diff.eventRange);
          }
        }

        renderHUD();
      }, 1000);

      // -------------------------------------------------------------
      // 11. RENDERIZAÇÃO & INTERFACE REATIVA (<100MS)
      // -------------------------------------------------------------
      function applyRateStyle(el, val, invert = false) {
        if (!el) return;
        const effective = invert ? -val : val;
        if (effective > 0.05) {
          el.className = 'text-[10px] font-bold text-emerald-400';
        } else if (effective < -0.05) {
          el.className = 'text-[10px] font-bold text-rose-400';
        } else {
          el.className = 'text-[10px] font-bold text-amber';
        }
      }

      function renderHUD() {
        const diff = getDifficultyPhase();
        const shieldDrain = getShieldDrain();
        const drainMult = getModeDrainMultiplier();

        // Timer de Sobrevivência
        const min = String(Math.floor(S.secondsAlive / 60)).padStart(2, '0');
        const sec = String(S.secondsAlive % 60).padStart(2, '0');
        document.getElementById('hud-timer').textContent = `${min}:${sec}`;

        // População e VIDA EM VERDE (Amarelo em atenção, Vermelho em perigo)
        document.getElementById('hud-pop').textContent = S.population;
        document.getElementById('hud-pop-cap').textContent = fmtCap(S.capPop);
        document.getElementById('label-pop-ratio').textContent = `${S.population} / ${fmtCap(S.capPop)} Astronauts`;
        document.getElementById('hud-health-pct').textContent = `${Math.round(S.health)}%`;

        const healthBox = document.getElementById('hud-health-box');
        const healthBar = document.getElementById('hud-health-bar');
        const healthLabelEl = document.getElementById('hud-health-label');
        const healthPctEl = document.getElementById('hud-health-pct');
        const healthIcon = document.getElementById('hud-health-icon');
        healthBar.style.width = `${Math.max(0, Math.min(100, S.health))}%`;

        if (S.health > 50) {
          healthBox.className = 'bg-[#05fa5b]/10 border border-[#05fa5b]/60 px-3 py-1.5 rounded-xl flex items-center gap-2 min-w-[130px]';
          healthBar.className = 'h-full bg-[#05fa5b] transition-all duration-300';
          if (healthLabelEl) healthLabelEl.className = 'text-[#05fa5b] font-bold';
          healthPctEl.className = 'font-bold text-[#05fa5b]';
          healthIcon.className = 'material-symbols-outlined text-[#05fa5b] text-[18px]';
        } else if (S.health > 25) {
          healthBox.className = 'bg-amber-950/55 border border-amber/60 px-3 py-1.5 rounded-xl flex items-center gap-2 min-w-[130px]';
          healthBar.className = 'h-full bg-amber transition-all duration-300';
          if (healthLabelEl) healthLabelEl.className = 'text-amber font-bold';
          healthPctEl.className = 'font-bold text-amber';
          healthIcon.className = 'material-symbols-outlined text-amber text-[18px]';
        } else {
          healthBox.className = 'bg-[#ff0000]/20 border border-[#ff0000] px-3 py-1.5 rounded-xl flex items-center gap-2 min-w-[130px]';
          healthBar.className = 'h-full bg-[#ff0000] transition-all duration-300';
          if (healthLabelEl) healthLabelEl.className = 'text-[#ff0000] font-bold';
          healthPctEl.className = 'font-bold text-[#ff0000]';
          healthIcon.className = 'material-symbols-outlined text-[#ff0000] text-[18px] animate-pulse';
        }

        // Recursos Básicos
        document.getElementById('val-water').textContent = Math.round(S.water);
        document.getElementById('cap-water').textContent = fmtCap(S.capWater);
        document.getElementById('bar-water').style.width = `${barPct(S.water, S.capWater, 200)}%`;

        document.getElementById('val-food').textContent = Math.round(S.food);
        document.getElementById('cap-food').textContent = fmtCap(S.capFood);
        document.getElementById('bar-food').style.width = `${barPct(S.food, S.capFood, 200)}%`;

        const o2Pct = barPct(S.o2, S.capO2, 200);
        const barO2El = document.getElementById('bar-o2');
        const cardO2El = document.getElementById('card-hud-o2');
        const labelO2El = document.getElementById('label-hud-o2');
        document.getElementById('val-o2').textContent = Math.round(S.o2);
        document.getElementById('cap-o2').textContent = fmtCap(S.capO2);
        if (barO2El) {
          barO2El.style.width = `${o2Pct}%`;
          if (S.o2 <= 15) {
            barO2El.style.backgroundColor = '#ff0000';
            if (cardO2El) cardO2El.style.borderColor = '#ff0000';
            if (labelO2El) labelO2El.style.color = '#ff0000';
          } else if (S.o2 < 35) {
            barO2El.style.backgroundColor = '#fbbf24';
            if (cardO2El) cardO2El.style.borderColor = '#fbbf24';
            if (labelO2El) labelO2El.style.color = '#fbbf24';
          } else {
            barO2El.style.backgroundColor = '#34d399';
            if (cardO2El) cardO2El.style.borderColor = '';
            if (labelO2El) labelO2El.style.color = '#34d399';
          }
        }

        document.getElementById('val-energy').textContent = Math.round(S.energy);
        document.getElementById('cap-energy').textContent = fmtCap(S.capEnergy);
        document.getElementById('bar-energy').style.width = `${barPct(S.energy, S.capEnergy, 250)}%`;

        document.getElementById('val-rad').textContent = Math.round(S.radiation);
        document.getElementById('bar-rad').style.width = `${Math.min(100, S.radiation)}%`;

        document.getElementById('val-sci').textContent = Math.round(S.science);
        document.getElementById('bar-sci').style.width = `${Math.min(100, (S.science / 100) * 100)}%`;
        // Taxas exibidas no HUD (incluindo produção automática dos robôs)
        const waterNet = (S.upgrades.autoIce * 2.0) - (S.population * Math.max(0.05, (0.5 - (S.upgrades.recycler * 0.045)) * drainMult));
        const foodNet = (S.upgrades.greenhouse * 1.0) - (S.population * 0.5 * drainMult);
        const o2Net = (S.upgrades.autoO2 * 2.5) + algaeO2Rate() - (S.population * 0.6 * drainMult);

        let solarGain = 3.2 + (S.upgrades.solar * 2.0);
        if (S.activeEvent && S.activeEvent.id === 'dust_storm') solarGain *= 0.35;
        else if (S.activeEvent && S.activeEvent.id === 'solar_flare') solarGain *= 0.45;
        let energyNet = solarGain - (S.shieldActive ? shieldDrain : 0);

        const baseRadRate = 0.22 * S.destination.diffMult * diff.radMult * drainMult;
        const radRate = S.shieldActive
          ? -1.3
          : (S.activeEvent && S.activeEvent.id === 'solar_storm' ? 2.2 * diff.radMult * drainMult : baseRadRate);
        const sciNet = (S.population * 1.0) + (S.upgrades.antenna * 1.5) + (S.upgrades.autoSci * 2.0);

        const elRateWater = document.getElementById('rate-water');
        const elRateFood = document.getElementById('rate-food');
        const elRateO2 = document.getElementById('rate-o2');
        const elRateEnergy = document.getElementById('rate-energy');
        const elRateRad = document.getElementById('rate-rad');
        const elRateSci = document.getElementById('rate-sci');

        elRateWater.textContent = `${waterNet >= 0 ? '+' : ''}${waterNet.toFixed(1)}/s`;
        applyRateStyle(elRateWater, waterNet);

        elRateFood.textContent = `${foodNet >= 0 ? '+' : ''}${foodNet.toFixed(1)}/s`;
        applyRateStyle(elRateFood, foodNet);

        elRateO2.textContent = `${o2Net >= 0 ? '+' : ''}${o2Net.toFixed(1)}/s`;
        applyRateStyle(elRateO2, o2Net);

        elRateEnergy.textContent = `${energyNet >= 0 ? '+' : ''}${energyNet.toFixed(1)}/s`;
        applyRateStyle(elRateEnergy, energyNet);

        elRateRad.textContent = `${radRate >= 0 ? '+' : ''}${radRate.toFixed(2)}/s`;
        applyRateStyle(elRateRad, radRate, true);

        elRateSci.textContent = `+${sciNet.toFixed(1)}/s`;
        elRateSci.className = 'text-[10px] font-bold text-emerald-400';

        const liveRadBg = document.getElementById('live-rad-bg');
        if (liveRadBg) liveRadBg.textContent = `${baseRadRate.toFixed(2)} mSv/s`;

        // Dreno detalhado da equipe
        const crewWaterOnly = -(S.population * Math.max(0.05, (0.5 - (S.upgrades.recycler * 0.045)) * drainMult));
        const crewO2Only = -(S.population * 0.6 * drainMult);
        document.getElementById('crew-drain-water').textContent = `${crewWaterOnly.toFixed(1)}/s`;
        document.getElementById('crew-drain-food').textContent = `-${(S.population * 0.5 * drainMult).toFixed(1)}/s`;
        document.getElementById('crew-drain-o2').textContent = `${crewO2Only.toFixed(1)}/s`;

        // Valores de clique atualizados & Guia Visual para Crianças (pulso amarelo no botão necessário)
        document.getElementById('click-val-ice').textContent = 8 + (S.upgrades.recycler * 2);
        document.getElementById('click-val-food').textContent = 8 + (S.upgrades.greenhouse * 4);
        document.getElementById('click-val-o2').textContent = 10 + (S.upgrades.habitat * 2);
        let solarClickVal = 12 + (S.upgrades.solar * 5);
        if (S.activeEvent && S.activeEvent.id === 'dust_storm') solarClickVal = Math.max(4, Math.floor(solarClickVal * 0.35));
        document.getElementById('click-val-solar').textContent = solarClickVal;
        document.getElementById('click-val-sci').textContent = 6 + (S.upgrades.antenna * 3);

        const btnIce = document.getElementById('btn-click-ice');
        const btnFood = document.getElementById('btn-click-greenhouse');
        const btnO2 = document.getElementById('btn-click-o2');
        const btnSolar = document.getElementById('btn-click-solar');
        const btnExp = document.getElementById('btn-click-experiment');

        btnIce.disabled = S.energy < 5;
        btnFood.disabled = S.energy < 4;
        btnO2.disabled = S.water < 4 || S.energy < 5;
        btnExp.disabled = S.energy < 6;

        // Pulso visual de atenção para ajudar crianças a saberem onde clicar
        btnSolar.classList.toggle('kid-guide-pulse', S.energy < 15);
        btnIce.classList.toggle('kid-guide-pulse', S.water < 35 && S.energy >= 5);
        btnFood.classList.toggle('kid-guide-pulse', S.food < 35 && S.energy >= 4);
        btnO2.classList.toggle('kid-guide-pulse', S.o2 < 35 && S.water >= 4 && S.energy >= 5);

        // Botão de Blindagem & Dreno Dinâmico
        document.getElementById('shield-drain-info').textContent = `-${shieldDrain.toFixed(1)} Energy/s`;
        const badgeShield = document.getElementById('badge-shield');
        const lblShield = document.getElementById('lbl-toggle-shield');
        const btnShield = document.getElementById('btn-toggle-shield');
        btnShield.classList.toggle('kid-guide-pulse', S.radiation >= 65 && !S.shieldActive && S.energy >= shieldDrain);

        if (S.shieldActive) {
          badgeShield.textContent = 'ON (PROTECTING)';
          badgeShield.className = 'font-mono-code text-[10px] px-2 py-0.5 rounded uppercase font-bold bg-emerald-500/20 text-emerald-400 border border-emerald-500/40';
          lblShield.className = 'text-[#ff0000]';
          lblShield.textContent = `Deactivate Shield (-${shieldDrain.toFixed(1)} Energy/s)`;
          btnShield.className = 'mt-3.5 w-full py-2.5 rounded-xl bg-surface-low hover:bg-surface-high border border-surface-border text-neutral-200 hover:text-white font-bold text-xs font-mono-code transition active:scale-95 flex items-center justify-center gap-1.5';
        } else {
          badgeShield.textContent = 'OFF';
          badgeShield.className = 'font-mono-code text-[10px] px-2 py-0.5 rounded uppercase font-bold bg-neutral-800 text-neutral-400';
          lblShield.className = 'text-[#ff0000]';
          lblShield.textContent = `Activate Shield (-${shieldDrain.toFixed(1)} Energy/s)`;
          btnShield.className = 'mt-3.5 w-full py-2.5 rounded-xl bg-surface-low hover:bg-surface-high border border-surface-border text-neutral-200 hover:text-white font-bold text-xs font-mono-code transition active:scale-95 flex items-center justify-center gap-1.5';
        }

        // Renderização de cada Upgrade
        Object.keys(UPGRADES_DEF).forEach(key => {
          const def = UPGRADES_DEF[key];
          const lvl = S.upgrades[key];
          const cost = getUpgradeCost(key);
          const lvlEl = document.getElementById(`lvl-up-${key}`);
          const costEl = document.getElementById(`cost-up-${key}`);
          const btnEl = document.getElementById(`btn-up-${key}`);

          if (costEl) costEl.textContent = lvl >= def.maxLvl ? 'MAXIMUM' : `${cost} Science`;
          if (btnEl) {
            btnEl.disabled = lvl >= def.maxLvl || S.science < cost;
            btnEl.textContent = lvl >= def.maxLvl ? 'Complete' : def.defaultBtnLabel;
          }
        });

        // Escala de Dificuldade com o Tempo
        const diffScaleEl = document.getElementById('label-diff-scale');
        diffScaleEl.textContent = diff.label;
        diffScaleEl.className = diff.colorClass;
      }

      // -------------------------------------------------------------
      // 12. INTEGRAÇÃO HÍBRIDA NASA DONKI API (SILENT FETCH + FALLBACK)
      // -------------------------------------------------------------
      function fetchNasaDonkiTelemetry() {
        fetch('https://api.nasa.gov/DONKI/CME?startDate=2024-01-01&api_key=DEMO_KEY')
          .then(res => res.json())
          .then(data => {
            if (Array.isArray(data) && data.length > 0) {
              const tag = document.getElementById('api-status-tag');
              if (tag) {
                tag.textContent = 'NASA DONKI Conectado';
                tag.className = 'px-1.5 py-0.5 rounded text-[10px] font-bold bg-emerald-500/20 text-emerald-400';
              }
              const wind = document.getElementById('live-solar-wind');
              if (wind) wind.textContent = '620 km/s (Solar Wind)';
            }
          })
          .catch(() => {
            const tag = document.getElementById('api-status-tag');
            if (tag) {
              tag.textContent = 'NASA DONKI & OSDR (Active)';
              tag.className = 'px-1.5 py-0.5 rounded text-[10px] font-bold bg-emerald-500/20 text-emerald-400';
            }
          });
      }

      // Drawer e Modais
      window.toggleDataDrawer = function() {
        const d = document.getElementById('data-drawer');
        d.classList.toggle('translate-x-full');
      };

      window.openDataSourceModal = function() {
        document.getElementById('modal-data-source').classList.remove('hidden');
      };

      window.closeDataSourceModal = function() {
        document.getElementById('modal-data-source').classList.add('hidden');
      };

      // -------------------------------------------------------------
      // 13. RENDERIZADOR PIXEL ART DO BACKGROUND (IMAGEM + FALLBACK)
      // -------------------------------------------------------------
      function initGameSpaceBackground() {
        const layer = document.getElementById('space-bg-layer');
        const canvas = document.getElementById('space-bg-canvas');
        if (!layer || !canvas) return;

        // Desenha cena espacial pixel art fiel (nebulosa roxa, estrelas 4 pontas, galáxias, Lua, satélite e Terra à direita)
        function drawPixelSpaceScene() {
          const w = 480;
          const h = 270;
          canvas.width = w;
          canvas.height = h;
          const ctx = canvas.getContext('2d');
          ctx.imageSmoothingEnabled = false;

          // Fundo azul-marinho espacial profundo
          ctx.fillStyle = '#07091e';
          ctx.fillRect(0, 0, w, h);

          // Nebulosa diagonal roxa pixelada
          for (let y = 0; y < h; y += 2) {
            for (let x = 0; x < w; x += 2) {
              const diag = (x * 0.55 + y) - 230;
              const wave = Math.sin(x * 0.03) * 28 + Math.cos(y * 0.05) * 18;
              const dist = Math.abs(diag + wave);
              if (dist < 46 && ((x + y) % 4 === 0 || dist < 24)) {
                ctx.fillStyle = dist < 20 ? '#4c1669' : '#2c0d47';
                ctx.fillRect(x, y, 2, 2);
              }
            }
          }

          // Estrelas pequenas de fundo
          let seed = 42;
          function rand() {
            seed = (seed * 1664525 + 1013904223) % 4294967296;
            return seed / 4294967296;
          }
          const starColors = ['#ffffff', '#7bd0ff', '#ffd000', '#c084fc'];
          for (let i = 0; i < 210; i++) {
            const sx = Math.floor(rand() * w);
            const sy = Math.floor(rand() * h);
            ctx.fillStyle = starColors[i % starColors.length];
            ctx.fillRect(sx, sy, 1, 1);
          }

          // Estrelas brilhantes de 4 pontas (azuis e douradas)
          function drawFourPointStar(cx, cy, r, color) {
            ctx.fillStyle = color;
            ctx.fillRect(cx - r, cy, r * 2 + 1, 1);
            ctx.fillRect(cx, cy - r, 1, r * 2 + 1);
            ctx.fillStyle = '#ffffff';
            ctx.fillRect(cx - 1, cy - 1, 3, 3);
          }
          const bigStars = [
            [32, 24, 5, '#7bd0ff'], [134, 34, 7, '#ffd000'], [188, 28, 6, '#ffd000'],
            [274, 52, 8, '#7bd0ff'], [356, 36, 6, '#7bd0ff'], [30, 92, 6, '#ffd000'],
            [98, 68, 7, '#7bd0ff'], [214, 92, 6, '#ffd000'], [144, 152, 7, '#7bd0ff'],
            [52, 210, 7, '#ffd000'], [246, 208, 7, '#ffd000'], [32, 248, 6, '#7bd0ff'],
            [92, 244, 5, '#7bd0ff'], [208, 250, 5, '#7bd0ff'], [418, 256, 5, '#7bd0ff']
          ];
          bigStars.forEach(([x, y, r, c]) => drawFourPointStar(x, y, r, c));

          // Galáxias espirais pixeladas
          function drawGalaxy(gx, gy) {
            ctx.fillStyle = '#3b2478';
            for (let a = 0; a < 28; a++) {
              const ang = a * 0.35;
              const rad = a * 0.48;
              ctx.fillRect(Math.round(gx + Math.cos(ang) * rad * 1.3), Math.round(gy + Math.sin(ang) * rad * 0.7), 2, 1);
              ctx.fillRect(Math.round(gx - Math.cos(ang) * rad * 1.3), Math.round(gy - Math.sin(ang) * rad * 0.7), 2, 1);
            }
            ctx.fillStyle = '#e0f2fe';
            ctx.fillRect(gx - 2, gy - 1, 4, 3);
          }
          drawGalaxy(72, 48);
          drawGalaxy(278, 182);
          drawGalaxy(362, 238);

          // Lua pequena perto da Terra
          ctx.fillStyle = '#94a3b8';
          ctx.beginPath();
          ctx.arc(336, 94, 6, 0, Math.PI * 2);
          ctx.fill();
          ctx.fillStyle = '#64748b';
          ctx.fillRect(334, 92, 2, 2);
          ctx.fillRect(338, 95, 2, 2);

          // Satélite pixel art em órbita
          ctx.fillStyle = '#38bdf8';
          ctx.fillRect(336, 198, 5, 4);
          ctx.fillRect(346, 192, 5, 4);
          ctx.fillStyle = '#e2e8f0';
          ctx.fillRect(341, 195, 5, 5);

          // Planeta Terra em pixel art no lado direito
          const ex = 456;
          const ey = 136;
          const er = 96;

          // Brilho atmosférico azul
          ctx.fillStyle = 'rgba(56, 189, 248, 0.28)';
          ctx.beginPath();
          ctx.arc(ex, ey, er + 5, 0, Math.PI * 2);
          ctx.fill();

          // Oceano
          ctx.fillStyle = '#1d6ce0';
          ctx.beginPath();
          ctx.arc(ex, ey, er, 0, Math.PI * 2);
          ctx.fill();

          // Continentes e nuvens pixeladas dentro do disco da Terra
          ctx.save();
          ctx.beginPath();
          ctx.arc(ex, ey, er - 1, 0, Math.PI * 2);
          ctx.clip();

          // América do Sul / Norte / África estilizadas em pixel art
          ctx.fillStyle = '#4d9e39';
          ctx.fillRect(376, 146, 34, 24);
          ctx.fillRect(388, 170, 28, 38);
          ctx.fillRect(394, 208, 16, 18);
          ctx.fillRect(382, 74, 38, 42);
          ctx.fillStyle = '#c89d66';
          ctx.fillRect(436, 124, 44, 46);
          ctx.fillStyle = '#4d9e39';
          ctx.fillRect(444, 88, 36, 34);
          ctx.fillRect(448, 170, 32, 34);

          // Nuvens brancas pixeladas
          ctx.fillStyle = '#f8fafc';
          ctx.fillRect(392, 58, 46, 12);
          ctx.fillRect(380, 96, 52, 8);
          ctx.fillRect(416, 114, 44, 7);
          ctx.fillRect(372, 134, 36, 6);
          ctx.fillRect(410, 184, 42, 8);

          // Sombra de curvatura do planeta
          ctx.fillStyle = 'rgba(5, 10, 32, 0.35)';
          ctx.fillRect(ex + 35, ey - er, er, er * 2);
          ctx.restore();

          // Overlay suave para garantir legibilidade dos textos do jogo
          ctx.fillStyle = 'rgba(7, 9, 24, 0.25)';
          ctx.fillRect(0, 0, w, h);
        }

        drawPixelSpaceScene();
        canvas.classList.remove('opacity-0');

        // Verifica se a imagem local ./Gemini_Generated_Image_u9wdshu9wdshu9wd.jfif carregou;
        // se carregar, oculta o canvas fallback e exibe a imagem original nítida
        const testImg = new Image();
        testImg.onload = function() {
          canvas.classList.add('opacity-0');
        };
        testImg.onerror = function() {
          layer.style.backgroundImage = 'none';
          canvas.classList.remove('opacity-0');
        };
        testImg.src = './Gemini_Generated_Image_u9wdshu9wdshu9wd.jfif';
      }

      // Telas do jogo (abas)
      window.showScreen = function(name) {
        document.querySelectorAll('[data-screen]').forEach(el => { el.hidden = el.dataset.screen !== name; });
        document.querySelectorAll('.screen-tab').forEach(b => {
          const on = b.dataset.tab === name;
          b.classList.toggle('bg-blue-500/20', on);
          b.classList.toggle('border-blue-500', on);
          b.classList.toggle('text-blue-300', on);
          b.classList.toggle('bg-surface-low', !on);
          b.classList.toggle('border-surface-border', !on);
          b.classList.toggle('text-neutral-300', !on);
          b.setAttribute('aria-selected', String(on));
        });
      };
      showScreen('acoes');

      // Inicialização
      initGameSpaceBackground();
      initMenuStarfield();
      renderTopAlertBar();
      renderHUD();
      fetchNasaDonkiTelemetry();
    })();
  </script>
</body>
</html>
