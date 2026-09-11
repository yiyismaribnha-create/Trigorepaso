# Trigorepaso
Pagina interactiva que te ayudara a estudiar trigonometria,repasar y afianzar conceptos,y aprender de manera divertida.
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>TrigoRepaso — Con TrigoGlotón</title>
  
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Nunito:wght@400;600;700;800;900&display=swap" rel="stylesheet">
  
  <script src="https://unpkg.com/lucide@latest"></script>

  <style>
    :root {
      --bg: #f8fafc;
      --surface: #ffffff;
      --text: #0f172a;
      --text-muted: #64748b;
      --border: #e2e8f0;
      
      --cat-basicas: #2563eb;
      --cat-basicas-bg: #eff6ff;
      --cat-pitagorica: #16a34a;
      --cat-pitagorica-bg: #f0fdf4;
      --cat-reglas: #ea580c;
      --cat-reglas-bg: #fff7ed;
      --cat-transf: #9333ea;
      --cat-transf-bg: #faf5ff;

      --active-color: var(--cat-basicas);
      --active-bg: var(--cat-basicas-bg);
      
      --radius: 16px;
      --shadow: 0 10px 25px -5px rgba(0,0,0,0.05), 0 8px 10px -6px rgba(0,0,0,0.01);
    }

    * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Nunito', sans-serif; -webkit-tap-highlight-color: transparent; }
    body { background-color: var(--bg); color: var(--text); display: flex; justify-content: center; min-height: 100vh; padding: 12px; }

    #app { width: 100%; max-width: 480px; display: flex; flex-direction: column; position: relative; padding-bottom: 60px; }

    /* Header & Navigation */
    .app-header { display: flex; align-items: center; justify-content: space-between; padding: 8px 0 16px; }
    .btn-back { background: var(--surface); border: 1px solid var(--border); padding: 8px 14px; border-radius: 20px; font-weight: 700; color: var(--text-muted); cursor: pointer; display: flex; align-items: center; gap: 6px; font-size: 0.9rem; transition: all 0.2s; }
    .btn-back:hover { background: #f1f5f9; color: var(--text); }
    
    /* Category Chips Filter Bar */
    .chips-container { display: flex; gap: 8px; overflow-x: auto; padding: 4px 0 12px; scrollbar-width: none; }
    .chips-container::-webkit-scrollbar { display: none; }
    .chip { padding: 6px 14px; border-radius: 20px; font-size: 0.85rem; font-weight: 700; border: 1px solid var(--border); background: var(--surface); color: var(--text-muted); cursor: pointer; flex-shrink: 0; transition: all 0.2s; display: flex; align-items: center; gap: 4px; }
    .chip.active { background: var(--active-bg); border-color: var(--active-color); color: var(--active-color); }

    /* Concept Banner */
    .concept-banner { background: var(--active-bg); border: 1px solid var(--active-color); border-radius: 12px; padding: 10px 14px; margin-bottom: 12px; display: flex; align-items: center; justify-content: space-between; cursor: pointer; color: var(--active-color); font-weight: 700; font-size: 0.85rem; transition: transform 0.1s; }
    .concept-banner:active { transform: scale(0.98); }

    /* Screen Management */
    .screen { display: none; flex-direction: column; flex: 1; }
    .screen.active { display: flex; }

    /* Screen 1: Home */
    .brand { text-align: center; margin: 12px 0 16px; }
    .brand-icon { width: 64px; height: 64px; background: #dbeafe; color: var(--cat-basicas); border-radius: 50%; display: inline-flex; align-items: center; justify-content: center; margin-bottom: 8px; }
    .brand h1 { font-size: 1.8rem; font-weight: 800; }
    .brand p { color: var(--text-muted); font-size: 0.95rem; }

    .progress-overview { background: var(--surface); border-radius: var(--radius); padding: 16px; border: 1px solid var(--border); margin-bottom: 20px; box-shadow: var(--shadow); }
    .progress-title { font-size: 0.85rem; font-weight: 800; color: var(--text-muted); text-transform: uppercase; letter-spacing: 0.5px; margin-bottom: 12px; }
    .progress-row { display: flex; align-items: center; gap: 10px; margin-bottom: 10px; }
    .progress-row:last-child { margin-bottom: 0; }
    .progress-label { font-size: 0.85rem; font-weight: 700; width: 130px; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
    .progress-bar-bg { flex: 1; height: 8px; background: #e2e8f0; border-radius: 4px; overflow: hidden; }
    .progress-bar-fill { height: 100%; border-radius: 4px; transition: width 0.3s; }
    .progress-val { font-size: 0.8rem; font-weight: 800; width: 35px; text-align: right; }

    .mode-buttons { display: flex; flex-direction: column; gap: 12px; }
    .btn-mode { background: var(--surface); border: 2px solid var(--border); border-radius: var(--radius); padding: 18px; display: flex; align-items: center; gap: 16px; cursor: pointer; text-align: left; transition: all 0.2s; box-shadow: var(--shadow); }
    .btn-mode:hover { transform: translateY(-2px); border-color: var(--active-color); }
    .btn-mode-icon { width: 48px; height: 48px; border-radius: 12px; display: flex; align-items: center; justify-content: center; color: white; flex-shrink: 0; }
    .btn-mode h3 { font-size: 1.1rem; font-weight: 800; }
    .btn-mode p { font-size: 0.85rem; color: var(--text-muted); margin-top: 2px; }

    /* Screen 2: Flashcards */
    .card-meta { display: flex; justify-content: space-between; align-items: center; margin-bottom: 12px; font-weight: 700; font-size: 0.85rem; color: var(--text-muted); }
    .flashcard-container { perspective: 1000px; height: 320px; margin-bottom: 16px; width: 100%; }
    .flashcard { width: 100%; height: 100%; position: relative; transform-style: preserve-3d; transition: transform 0.5s cubic-bezier(0.175, 0.885, 0.32, 1.275); cursor: pointer; }
    .flashcard.flipped { transform: rotateY(180deg); }
    .card-face { position: absolute; width: 100%; height: 100%; backface-visibility: hidden; border-radius: var(--radius); border: 2px solid var(--border); background: var(--surface); box-shadow: var(--shadow); display: flex; flex-direction: column; justify-content: center; align-items: center; padding: 24px; text-align: center; }
    .card-back { transform: rotateY(180deg); background: var(--active-bg); border-color: var(--active-color); }
    .card-label { position: absolute; top: 12px; left: 16px; font-size: 0.75rem; font-weight: 800; text-transform: uppercase; letter-spacing: 0.5px; opacity: 0.6; }
    .card-content { font-size: 1.1rem; font-weight: 700; color: var(--text); display: flex; flex-direction: column; align-items: center; justify-content: center; gap: 8px; width: 100%; }
    .card-hint { position: absolute; bottom: 12px; font-size: 0.75rem; color: var(--text-muted); font-weight: 600; display: flex; align-items: center; gap: 4px; }

    .card-controls { display: flex; flex-direction: column; gap: 12px; }
    .grading-btns { display: flex; gap: 12px; }
    .btn-grade { flex: 1; padding: 14px; border: none; border-radius: 12px; font-weight: 800; font-size: 0.95rem; cursor: pointer; display: flex; align-items: center; justify-content: center; gap: 6px; color: white; transition: transform 0.1s; }
    .btn-grade:active { transform: scale(0.98); }
    .btn-again { background: #e11d48; }
    .btn-know { background: #16a34a; }
    .nav-btns { display: flex; justify-content: space-between; gap: 8px; }
    .btn-nav { flex: 1; padding: 10px; background: var(--surface); border: 1px solid var(--border); border-radius: 10px; font-weight: 700; cursor: pointer; color: var(--text-muted); display: flex; align-items: center; justify-content: center; gap: 4px; }

    /* Screen 3: Memory Game */
    .game-header { display: flex; justify-content: space-between; align-items: center; background: var(--surface); border: 1px solid var(--border); padding: 10px 16px; border-radius: 12px; margin-bottom: 12px; font-weight: 700; font-size: 0.85rem; }
    .difficulty-selector { display: flex; gap: 6px; margin-bottom: 12px; justify-content: center; }
    .btn-diff { padding: 4px 12px; border-radius: 12px; border: 1px solid var(--border); background: var(--surface); font-size: 0.8rem; font-weight: 700; cursor: pointer; }
    .btn-diff.active { background: var(--active-color); color: white; border-color: var(--active-color); }
    
    .grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 8px; position: relative; }
    .grid.grid-4 { grid-template-columns: repeat(4, 1fr); }
    
    .memory-card { aspect-ratio: 1; background: var(--surface); border: 2px solid var(--border); border-radius: 10px; display: flex; align-items: center; justify-content: center; text-align: center; padding: 6px; cursor: pointer; font-weight: 700; font-size: 0.78rem; user-select: none; transition: transform 0.2s, background 0.3s, opacity 0.5s; position: relative; width: 100%; height: 100%; overflow: hidden; }
    .memory-card.flipped { background: var(--active-bg); border-color: var(--active-color); }
    .memory-card.matched { visibility: hidden; opacity: 0; transition: opacity 0.4s, visibility 0.4s; }
    .memory-card.eaten { opacity: 0; pointer-events: none; transform: scale(0); transition: all 0.5s ease-out; }
    
    .card-inner-content { width: 100%; height: 100%; display: flex; flex-direction: column; align-items: center; justify-content: center; word-break: break-word; }
    .card-inner-content svg { max-width: 100%; max-height: 100%; }

    /* Pop-up de animación de TrigoGlotón cuando devora cartas por error */
    .monster-error-overlay { position: absolute; inset: 0; background: rgba(239, 68, 68, 0.2); border-radius: var(--radius); pointer-events: none; display: flex; flex-direction: column; align-items: center; justify-content: center; opacity: 0; transform: scale(0.5); transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275); z-index: 50; }
    .monster-error-overlay.active { opacity: 1; transform: scale(1); }
    .monster-error-avatar { width: 120px; height: 120px; filter: drop-shadow(0 10px 15px rgba(0,0,0,0.3)); animation: monsterBite 0.5s infinite alternate; }
    .monster-error-text { background: #dc2626; color: white; font-weight: 900; font-size: 0.85rem; padding: 6px 14px; border-radius: 20px; margin-top: 6px; box-shadow: 0 4px 6px rgba(0,0,0,0.2); text-align: center; }

    @keyframes monsterBite {
      0% { transform: scale(1) rotate(-6deg); }
      100% { transform: scale(1.18) rotate(6deg); }
    }

    /* Screen 4: Explicación Detallada */
    .explanation-card { background: var(--surface); border-radius: var(--radius); border: 1px solid var(--border); padding: 20px; box-shadow: var(--shadow); margin-bottom: 16px; }
    .explanation-header { display: flex; align-items: center; gap: 12px; margin-bottom: 16px; }
    .explanation-badge { width: 40px; height: 40px; border-radius: 10px; background: var(--active-bg); color: var(--active-color); display: flex; align-items: center; justify-content: center; }
    .explanation-title { font-weight: 900; font-size: 1.25rem; color: var(--text); }
    
    .graphic-box { background: var(--active-bg); border: 1px dashed var(--active-color); border-radius: 12px; padding: 16px; display: flex; flex-direction: column; align-items: center; margin: 16px 0; text-align: center; }
    .graphic-caption { font-size: 0.85rem; font-weight: 700; color: var(--text-muted); margin-top: 8px; }

    .detail-section { margin-bottom: 16px; }
    .detail-section h4 { font-size: 0.98rem; font-weight: 800; color: var(--text); margin-bottom: 6px; display: flex; align-items: center; gap: 6px; }
    .detail-section p { font-size: 0.9rem; color: var(--text-muted); line-height: 1.6; }
    .formula-badge { background: #f1f5f9; border-left: 4px solid var(--active-color); padding: 8px 12px; font-weight: 800; border-radius: 4px; margin: 8px 0; color: var(--text); font-size: 0.9rem; }

    /* Sección TrigoGlotón Extendida */
    .monster-tips-card { background: #f0fdf4; border: 2px solid #86efac; border-radius: var(--radius); padding: 16px; display: flex; flex-direction: column; gap: 12px; box-shadow: var(--shadow); position: relative; margin-bottom: 16px; }
    .monster-header { display: flex; align-items: center; gap: 12px; }
    .monster-avatar { width: 64px; height: 64px; flex-shrink: 0; }
    .monster-info h4 { font-size: 1rem; font-weight: 900; color: #166534; }
    .monster-info p { font-size: 0.78rem; font-weight: 700; color: #15803d; }
    
    .monster-speech { background: #ffffff; border: 1px solid #bbf7d0; border-radius: 12px; padding: 12px; font-size: 0.85rem; color: #14532d; position: relative; line-height: 1.4; }
    .monster-speech::before { content: ''; position: absolute; top: -8px; left: 24px; width: 0; height: 0; border-left: 8px solid transparent; border-right: 8px solid transparent; border-bottom: 8px solid #ffffff; }

    .monster-section-title { font-weight: 900; font-size: 0.85rem; color: #15803d; margin-top: 10px; text-transform: uppercase; letter-spacing: 0.5px; border-bottom: 1px stroke #bbf7d0; padding-bottom: 2px; }
    
    .tips-list { list-style: none; display: flex; flex-direction: column; gap: 8px; margin-top: 6px; }
    .tips-list li { font-size: 0.85rem; color: #166534; font-weight: 700; display: flex; gap: 6px; align-items: flex-start; }

    .btn-return-home { background: var(--surface); border: 2px solid var(--border); color: var(--text); font-weight: 800; padding: 14px; border-radius: 12px; display: flex; align-items: center; justify-content: center; gap: 8px; width: 100%; cursor: pointer; transition: all 0.2s; }
    .btn-return-home:hover { border-color: var(--active-color); color: var(--active-color); }

    /* Overlays / Modals */
    .overlay { position: fixed; top: 0; left: 0; right: 0; bottom: 0; background: rgba(15, 23, 42, 0.6); backdrop-filter: blur(4px); display: none; align-items: center; justify-content: center; padding: 16px; z-index: 100; }
    .overlay.active { display: flex; }
    .modal { background: var(--surface); border-radius: var(--radius); padding: 24px; width: 100%; max-width: 400px; text-align: center; box-shadow: var(--shadow); max-height: 85vh; overflow-y: auto; }
    .modal h2 { font-weight: 800; font-size: 1.3rem; margin-bottom: 12px; color: var(--text); }
    .modal-body { color: var(--text-muted); font-size: 0.92rem; text-align: left; line-height: 1.5; }
    .modal-btns { display: flex; flex-direction: column; gap: 8px; margin-top: 18px; }
    .btn-primary { padding: 12px; background: var(--active-color); color: white; border: none; border-radius: 10px; font-weight: 800; cursor: pointer; width: 100%; }

    .btn-faq { position: fixed; bottom: 16px; right: 16px; width: 44px; height: 44px; border-radius: 50%; background: var(--text); color: white; border: none; font-weight: 800; font-size: 1.2rem; cursor: pointer; box-shadow: var(--shadow); display: flex; align-items: center; justify-content: center; z-index: 90; }
    
    .svg-container { width: 100%; max-height: 130px; display: flex; justify-content: center; align-items: center; }
    .svg-container svg { max-height: 120px; width: auto; }

    /* Estilos específicos de retroalimentación final */
    .feedback-box { background: #fef2f2; border: 1px solid #fca5a5; border-radius: 12px; padding: 12px; margin: 10px 0; color: #991b1b; font-size: 0.85rem; font-weight: 700; line-height: 1.4; }
    .feedback-item { background: #ffffff; border-left: 3px solid #dc2626; padding: 8px 10px; border-radius: 4px; margin-top: 8px; color: #450a0a; }
  </style>
</head>
<body>

  <div id="app">
    <!-- Header General con Botón Volver -->
    <div id="top-nav" class="app-header" style="display: none;">
      <button class="btn-back" onclick="navigateTo(1)"><i data-lucide="arrow-left"></i> Volver al Inicio</button>
      <span id="current-screen-title" style="font-weight: 800; font-size: 0.9rem; color: var(--text-muted);"></span>
    </div>

    <!-- Botones Superiores de Temas -->
    <div class="chips-container" id="chips-bar"></div>

    <!-- Banner para ir a Explicación Detallada -->
    <div id="concept-banner" class="concept-banner" onclick="openExplanationTab()">
      <span id="concept-banner-title"><i data-lucide="book-open" size="16"></i> Ver explicación detallada del tema</span>
      <i data-lucide="chevron-right" size="16"></i>
    </div>

    <!-- Pantalla 1: Inicio -->
    <section id="screen-1" class="screen active">
      <div class="brand">
        <div class="brand-icon"><i data-lucide="activity" size="32"></i></div>
        <h1>TrigoRepaso</h1>
        <p>Aprende trigonometría junto a TrigoGlotón</p>
      </div>

      <div class="progress-overview">
        <div class="progress-title">Progreso por temas</div>
        <div id="progress-rows"></div>
      </div>

      <div class="mode-buttons">
        <button class="btn-mode" onclick="navigateTo(2)">
          <div class="btn-mode-icon" style="background: var(--cat-basicas);"><i data-lucide="layers"></i></div>
          <div>
            <h3>Tarjetas de estudio</h3>
            <p>Repasos rápidos con repetición espaciada</p>
          </div>
        </button>

        <button class="btn-mode" onclick="navigateTo(3)">
          <div class="btn-mode-icon" style="background: var(--cat-pitagorica);"><i data-lucide="grid"></i></div>
          <div>
            <h3>Juego de memoria</h3>
            <p>Empareja conceptos, fórmulas y gráficas</p>
          </div>
        </button>
      </div>
    </section>

    <!-- Pantalla 2: Tarjetas de Estudio -->
    <section id="screen-2" class="screen">
      <div class="card-meta">
        <span id="card-category-label">Categoría</span>
        <span id="card-counter">1 / 10</span>
        <span id="category-mastery-label">0% dominado</span>
      </div>

      <div class="flashcard-container">
        <div class="flashcard" id="flashcard" onclick="flipCard()">
          <div class="card-face card-front">
            <span class="card-label">Pregunta / Concepto</span>
            <div class="card-content" id="card-front-text">---</div>
            <span class="card-hint">Toca para voltear <i data-lucide="rotate-cw" size="12"></i></span>
          </div>
          <div class="card-face card-back">
            <span class="card-label">Respuesta / Representación</span>
            <div class="card-content" id="card-back-text">---</div>
          </div>
        </div>
      </div>

      <div class="card-controls">
        <div class="grading-btns" id="grading-btns" style="visibility: hidden;">
          <button class="btn-grade btn-again" onclick="gradeCard('repasar')"><i data-lucide="refresh-cw"></i> Repasar</button>
          <button class="btn-grade btn-know" onclick="gradeCard('dominada')"><i data-lucide="check-circle"></i> Lo sé</button>
        </div>
        <div class="nav-btns">
          <button class="btn-nav" onclick="prevCard()"><i data-lucide="chevron-left"></i> Anterior</button>
          <button class="btn-nav" onclick="flipCard()"><i data-lucide="rotate-cw"></i> Voltear</button>
          <button class="btn-nav" onclick="nextCard()">Siguiente <i data-lucide="chevron-right"></i></button>
        </div>
      </div>
    </section>

    <!-- Pantalla 3: Juego de Memoria -->
    <section id="screen-3" class="screen">
      <div class="difficulty-selector">
        <button class="btn-diff active" id="diff-facil" onclick="setDifficulty('facil')">Fácil (6)</button>
        <button class="btn-diff" id="diff-medio" onclick="setDifficulty('medio')">Medio (8)</button>
        <button class="btn-diff" id="diff-dificil" onclick="setDifficulty('dificil')">Difícil (12)</button>
      </div>

      <div class="game-header">
        <div><i data-lucide="clock" size="14"></i> <span id="game-timer">00:00</span></div>
        <div id="game-status-msg" style="color:var(--active-color);">¡Memoriza las cartas!</div>
        <div>Puntos: <span id="game-score" style="color:#e11d48; font-weight:900;">1000</span></div>
      </div>

      <div style="position: relative;">
        <div class="grid" id="memory-grid"></div>
        <!-- Overlay TrigoGlotón cuando devora cartas por error -->
        <div class="monster-error-overlay" id="monster-error-pop">
          <div class="monster-error-avatar" id="monster-error-svg"></div>
          <div class="monster-error-text">¡OM NOM NOM!<br>¡ME COMÍ TUS CARTAS Y TUS PUNTOS!</div>
        </div>
      </div>
    </section>

    <!-- Pantalla 4: Pestaña de Explicación Detallada -->
    <section id="screen-4" class="screen">
      <div class="explanation-card">
        <div class="explanation-header">
          <div class="explanation-badge" id="exp-badge"><i data-lucide="book-open"></i></div>
          <div class="explanation-title" id="exp-title">Título del Tema</div>
        </div>

        <div id="exp-body-content"></div>

        <div class="graphic-box">
          <div id="exp-graphic"></div>
          <div class="graphic-caption" id="exp-graphic-caption">Gráfica del concepto</div>
        </div>
      </div>

      <!-- TrigoGlotón Card -->
      <div class="monster-tips-card">
        <div class="monster-header">
          <div class="monster-avatar" id="monster-svg-container"></div>
          <div class="monster-info">
            <h4>¡TrigoGlotón al rescate! 🦖🍴</h4>
            <p>El monstruo devorador de errores matemáticos</p>
          </div>
        </div>
        <div class="monster-speech">
          <span id="monster-quote">"¡Si te equivocas en un signo, me lo como!"</span>
          
          <div class="monster-section-title">💡 Tips para no reprobar</div>
          <ul class="tips-list" id="exp-tips-list"></ul>

          <div class="monster-section-title">❌ Mitos que TrigoGlotón se comió</div>
          <ul class="tips-list" id="exp-myths-list"></ul>

          <div class="monster-section-title">🔑 Secreto de Examen</div>
          <p id="exp-secret-text" style="font-weight: 700; color: #166534; font-size:0.85rem; margin-top:4px;"></p>
        </div>
      </div>

      <button class="btn-return-home" onclick="navigateTo(1)">
        <i data-lucide="home" size="18"></i> Volver a la página principal
      </button>
    </section>
  </div>

  <button class="btn-faq" onclick="showFaqModal()" title="Ayuda">?</button>

  <div class="overlay" id="app-modal">
    <div class="modal">
      <h2 id="modal-title">Título</h2>
      <div class="modal-body" id="modal-desc">Contenido...</div>
      <div class="modal-btns">
        <button class="btn-primary" id="modal-primary-btn" onclick="closeModal()">Entendido</button>
      </div>
    </div>
  </div>

  <script>
    // SVG de TrigoGlotón
    const MONSTER_SVG = `
      <svg viewBox="0 0 120 120" width="100%" height="100%">
        <ellipse cx="60" cy="70" rx="42" ry="38" fill="#22c55e" stroke="#15803d" stroke-width="3"/>
        <path d="M 30 75 Q 60 95 90 75 Z" fill="#86efac" opacity="0.6"/>
        <circle cx="60" cy="40" r="28" fill="#22c55e" stroke="#15803d" stroke-width="3"/>
        <polygon points="50,12 55,2 60,12" fill="#eab308"/>
        <polygon points="62,12 67,2 72,12" fill="#eab308"/>
        <polygon points="74,15 79,6 84,17" fill="#eab308"/>
        <circle cx="48" cy="32" r="8" fill="#ffffff" stroke="#15803d" stroke-width="2"/>
        <circle cx="48" cy="32" r="3" fill="#000000"/>
        <circle cx="72" cy="32" r="9" fill="#ffffff" stroke="#15803d" stroke-width="2"/>
        <circle cx="71" cy="32" r="4" fill="#000000"/>
        <path d="M 38 48 Q 60 62 82 48 Q 60 54 38 48 Z" fill="#7f1d1d" stroke="#15803d" stroke-width="2"/>
        <polygon points="42,49 46,54 50,49" fill="#ffffff"/>
        <polygon points="54,50 58,56 62,50" fill="#ffffff"/>
        <polygon points="66,50 70,55 74,49" fill="#ffffff"/>
        <path d="M 52 53 Q 60 70 78 60 Q 65 53 58 52" fill="#f43f5e"/>
        <path d="M 20 65 Q 10 50 25 55" stroke="#15803d" stroke-width="4" stroke-linecap="round" fill="none"/>
        <path d="M 100 65 Q 110 50 95 55" stroke="#15803d" stroke-width="4" stroke-linecap="round" fill="none"/>
      </svg>
    `;

    // --- GRÁFICAS SVG DE LAS FUNCIONES ---
    const SVGS = {
      seno: `<div class="svg-container"><svg viewBox="0 0 200 100"><path d="M 10 50 Q 55 0 100 50 T 190 50" fill="none" stroke="#2563eb" stroke-width="4"/><line x1="10" y1="50" x2="190" y2="50" stroke="#cbd5e1" stroke-width="2"/><line x1="100" y1="10" x2="100" y2="90" stroke="#cbd5e1" stroke-width="2"/><text x="140" y="30" fill="#2563eb" font-weight="bold" font-size="14">f(x) = sen(x)</text></svg></div>`,
      coseno: `<div class="svg-container"><svg viewBox="0 0 200 100"><path d="M 10 10 Q 55 90 100 50 T 190 90" fill="none" stroke="#16a34a" stroke-width="4"/><line x1="10" y1="50" x2="190" y2="50" stroke="#cbd5e1" stroke-width="2"/><line x1="100" y1="10" x2="100" y2="90" stroke="#cbd5e1" stroke-width="2"/><text x="130" y="25" fill="#16a34a" font-weight="bold" font-size="14">f(x) = cos(x)</text></svg></div>`,
      tangente: `<div class="svg-container"><svg viewBox="0 0 200 100"><path d="M 20 90 Q 50 80 50 50 Q 50 20 80 10" fill="none" stroke="#ea580c" stroke-width="4"/><line x1="10" y1="50" x2="190" y2="50" stroke="#cbd5e1" stroke-width="2"/><line x1="100" y1="10" x2="100" y2="90" stroke="#cbd5e1" stroke-width="2"/><text x="110" y="30" fill="#ea580c" font-weight="bold" font-size="14">f(x) = tan(x)</text></svg></div>`,
      circuloUnitario: `<div class="svg-container"><svg viewBox="0 0 100 100"><circle cx="50" cy="50" r="35" fill="none" stroke="#2563eb" stroke-width="3"/><line x1="10" y1="50" x2="90" y2="50" stroke="#94a3b8"/><line x1="50" y1="10" x2="50" y2="90" stroke="#94a3b8"/><line x1="50" y1="50" x2="75" y2="25" stroke="#e11d48" stroke-width="2"/><circle cx="75" cy="25" r="3" fill="#e11d48"/><text x="58" y="22" fill="#0f172a" font-size="8" font-weight="bold">(cos θ, sen θ)</text></svg></div>`,
      pitagoras: `<div class="svg-container"><svg viewBox="0 0 120 90"><polygon points="20,70 100,70 100,20" fill="#f0fdf4" stroke="#16a34a" stroke-width="3"/><text x="55" y="85" fill="#16a34a" font-size="9" font-weight="bold">cos²(θ)</text><text x="102" y="50" fill="#16a34a" font-size="9" font-weight="bold">sen²(θ)</text><text x="50" y="40" fill="#16a34a" font-size="9" font-weight="bold">1</text></svg></div>`,
      transformada: `<div class="svg-container"><svg viewBox="0 0 200 100"><path d="M 10 50 Q 32 0 55 50 T 100 50 T 145 50 T 190 50" fill="none" stroke="#9333ea" stroke-width="4"/><line x1="10" y1="50" x2="190" y2="50" stroke="#cbd5e1" stroke-width="2"/><line x1="100" y1="10" x2="100" y2="90" stroke="#cbd5e1" stroke-width="2"/><text x="110" y="25" fill="#9333ea" font-weight="bold" font-size="12">y = A·sen(Bx)</text></svg></div>`
    };

    // --- BASE DE DATOS EXTENDIDA DE EXPLICACIONES Y DATO DE EXPLICACIÓN DE FALLO ---
    const CATEGORIES = [
      { 
        id: 'todas', 
        name: 'Todos los temas', 
        color: '#2563eb', 
        bg: '#eff6ff', 
        details: [
          { subtitle: '1. El Corazón de la Trigonometría', text: 'La trigonometría no es solo memorizar triángulos, es el estudio de las relaciones periódicas. Desde las ondas de sonido hasta la órbita de los planetas, todo se modela mediante senos y cosenos.' },
          { subtitle: '2. Mapa de Estudio para el Examen', text: 'Comienza dominando el Círculo Unitario. Una vez entiendes cómo varían X (coseno) y Y (seno), las identidades pitagóricas y las transformaciones de funciones surgen de manera natural.' }
        ],
        graphic: SVGS.circuloUnitario,
        caption: 'Círculo Unitario: La cuna de las funciones trigonométricas',
        quote: '"¡Mmm, me encanta tragarme los errores en las conversiones de grados a radianes! Pon atención aquí:"',
        tips: [
          'Recordar que π equivale a 180°. Si ves 2π, ¡es una vuelta completa de 360°!',
          'En el examen, dibuja siempre un plano cartesiano rápido para ubicar en qué cuadrante el seno y coseno son positivos o negativos.'
        ],
        myths: [
          'Mito: "Seno y Coseno son lo mismo en cualquier ángulo". ¡Falso! Se desfasan por 90° (π/2).',
          'Mito: "Las funciones trigonométricas solo sirven para triángulos". ¡Falso! Describen olas, sonido y electricidad.'
        ],
        secret: 'Aprende de memoria los valores notables de 0°, 30°, 45°, 60° y 90° usando la regla de los dedos.'
      },
      { 
        id: 'basicas', 
        name: 'Conceptos básicos', 
        color: '#2563eb', 
        bg: '#eff6ff', 
        details: [
          { subtitle: 'El Círculo Trigonométrico (Unitario)', text: 'Es una circunferencia centrada en el origen (0,0) con radio exactamente igual a r = 1. Para cualquier ángulo θ trazado desde el eje horizontal positivo, las coordenadas del punto de corte son:' },
          { subtitle: 'Definición Fundamental de Coordenadas', text: '<div class="formula-badge">Punto P = (x, y) = (cos θ, sen θ)</div>Esto significa que la proyección horizontal siempre es el Coseno y la proyección vertical siempre es el Seno.' },
          { subtitle: 'Gráfica de las Funciones Seno y Coseno', text: 'Al desenrollar el círculo unitario en un plano, sen(x) empieza en 0 y cos(x) empieza en 1.' },
          { subtitle: 'Medida de Ángulos: Grados vs Radianes', text: 'Un radián es el ángulo central que subtiende un arco de longitud igual al radio. Para convertir entre sistemas usas la proporción fundamental: <div class="formula-badge">180° = π rad &nbsp;|&nbsp; Grados a Radianes: × (π / 180)</div>' }
        ],
        graphic: SVGS.seno,
        caption: 'Gráfica de la Función Seno f(x) = sen(x) con Periodo 2π',
        quote: '"¡GLUP! Me acabo de tragar un triángulo entero. No confundas la X con el Seno o me dará indigestión:"',
        tips: [
          '¡X es Coseno, Y es Seno! Repítelo: "Coseno camina acostado (eje X), Seno se levanta (eje Y)".',
          'Si te piden sen(90°), mira el punto superior del círculo (0,1) -> ¡La Y vale 1, así que sen(90°) = 1!'
        ],
        myths: [
          'Mito: "sen(A + B) es igual a sen(A) + sen(B)". ¡Falso! Seno no se distribuye multiplicando.',
          'Mito: "El radio del círculo unitario puede cambiar". ¡Falso! Siempre mide exactamente 1.'
        ],
        secret: 'En el cuadrante I todos son positivos; en el II solo Seno; en el III solo Tangente; en el IV solo Coseno ("Todos Sentados Tomando Café").'
      },
      { 
        id: 'pitagorica', 
        name: 'Identidades Pitagóricas', 
        color: '#16a34a', 
        bg: '#f0fdf4', 
        details: [
          { subtitle: 'Origen Geométrico', text: 'Dentro del círculo unitario, el radio (hipotenusa = 1), la altura (cateto opuesto = sen θ) y la base (cateto adyacente = cos θ) forman un triángulo rectángulo. Aplicando el Teorema de Pitágoras:' },
          { subtitle: 'La Identidad Madre', text: '<div class="formula-badge">sen²(θ) + cos²(θ) = 1</div>' },
          { subtitle: 'Obtención de las Secundarias', text: 'No las memorices a la fuerza; divídelas. Si divides la Identidad Madre entre cos²(θ): <div class="formula-badge">1 + tan²(θ) = sec²(θ)</div>Si la divides entre sen²(θ): <div class="formula-badge">1 + cot²(θ) = csc²(θ)</div>' }
        ],
        graphic: SVGS.pitagoras,
        caption: 'Triángulo rectángulo interno que genera la identidad sen²(θ) + cos²(θ) = 1',
        quote: '"¡ÑAM ÑAM! Me he comido el signo "+". ¡Que no se te olvide en la identidad fundamental!"',
        tips: [
          'Ojo de Glotón: Si en una simplificación ves "1 - sen²(θ)", ¡cómetelo y reemplázalo de inmediato por cos²(θ)!',
          'Recuerda que los cuadrados van en la función: sen²(θ), NO sen(θ²).'
        ],
        myths: [
          'Mito: "sen(θ) + cos(θ) = 1". ¡Falso! Solo funciona con los cuadrados sen²(θ) + cos²(θ) = 1.',
          'Mito: "sec²(θ) - tan²(θ) = -1". ¡Falso! sec²(θ) - tan²(θ) = +1.'
        ],
        secret: 'Cuando tengas dudas despejando identidades, pasa todo a términos de Seno y Coseno; ¡el 90% de los problemas se resuelven así!'
      },
      { 
        id: 'reglas', 
        name: 'Identidades y Reglas', 
        color: '#ea580c', 
        bg: '#fff7ed', 
        details: [
          { subtitle: 'Funciones Recíprocas', text: 'Las funciones recíprocas se obtienen invirtiendo la fracción. ¡Cuidado con no confundirlas con las funciones inversas!' },
          { subtitle: 'Regla del Cruce de Letras', text: '<div class="formula-badge">sec(θ) = 1 / cos(θ) &nbsp;|&nbsp; csc(θ) = 1 / sen(θ)</div>' },
          { subtitle: 'Gráfica de Tangente', text: 'A diferencia de seno y coseno, la tangente tiene asíntotas donde el coseno vale cero.' },
          { subtitle: 'Identidad del Ángulo Doble', text: 'Se usan para simplificar expresiones complejas: <div class="formula-badge">sen(2θ) = 2 · sen(θ) · cos(θ)</div><div class="formula-badge">cos(2θ) = cos²(θ) - sen²(θ)</div>' }
        ],
        graphic: SVGS.tangente,
        caption: 'Gráfica de la función Tangente f(x) = tan(x) con asíntotas verticales',
        quote: '"¡CROANCH! Me acabo de masticar una Tangente dividida por cero. ¡Cuidado con las asíntotas!"',
        tips: [
          'Truco de cruce: La Secante (empieza con S) va con Coseno (empieza con C). La Cosecante (empieza con C) va con Seno (empieza con S).',
          'La tangente es un cociente directo: tan(θ) = sen(θ) / cos(θ). ¡Si cos(θ) es cero, la tan(θ) explota!'
        ],
        myths: [
          'Mito: "cos(2θ) = 2cos(θ)". ¡Error común! cos(2θ) es cos²(θ) - sen²(θ).',
          'Mito: "tan(x) está definida para todo valor de x". ¡Falso! No existe en π/2, 3π/2, etc.'
        ],
        secret: 'Para recordar tan(x), piensa en sen(x)/cos(x). Cuando sen(x)=0, tan(x)=0. Cuando cos(x)=0, hay una asíntota vertical.'
      },
      { 
        id: 'transf', 
        name: 'Transformaciones', 
        color: '#9333ea', 
        bg: '#faf5ff', 
        details: [
          { subtitle: 'Modelo General Senoidal', text: 'Toda onda senoidal o cosenoidal modificada responde a la ecuación canónica: <div class="formula-badge">y = A · sen(B · x - C) + D</div>' },
          { subtitle: 'Parámetros y Efectos Visuales', text: '• <b>Amplitud (|A|):</b> Estira o comprime verticalmente la onda.<br>• <b>Periodo (T = 2π / |B|):</b> Ancho de un ciclo completo. Si B crece, la onda se aprieta horizontalmente.<br>• <b>Desplazamiento Vertical (D):</b> Sube o baja la línea media de la gráfica.' }
        ],
        graphic: SVGS.transformada,
        caption: 'Onda transformada con alta frecuencia (Periodo reducido por el valor de B)',
        quote: '"¡BURP! Me comí la amplitud A y la onda quedó plana. ¡Mira bien cuánto vale A!"',
        tips: [
          'Para calcular el Periodo rápido en tu examen: Divide 2π entre el número B que multiplica a la x.',
          'Si la A es negativa (ej. y = -3sen(x)), la gráfica se voltea verticalmente (se refleja).'
        ],
        myths: [
          'Mito: "El número B en y = sen(Bx) es el periodo". ¡Falso! B determina cuántos ciclos caben en 2π. El periodo es 2π/B.',
          'Mito: "Cambiar D altera la amplitud". ¡Falso! D solo mueve la gráfica hacia arriba o hacia abajo.'
        ],
        secret: 'La amplitud siempre es un número positivo (valor absoluto |A|). ¡Si ves -5sen(x), la amplitud es 5, no -5!'
      }
    ];

    // Banco de ítems para Flashcards y Memoria
    const DATA_BANK = [
      { id: 'b1', categoria: 'basicas', frente: 'Función Seno en Círculo Unitario', reverso: SVGS.circuloUnitario + 'Coordenada Y del punto', pair_a: 'Seno en Círculo', pair_b: SVGS.circuloUnitario, explanation: 'Recuerda que en el Círculo Unitario la coordenada Y representa el Seno (sen θ) y la X el Coseno (cos θ).' },
      { id: 'b2', categoria: 'basicas', frente: 'Gráfica de la función Seno', reverso: SVGS.seno, pair_a: 'Función sen(x)', pair_b: SVGS.seno, explanation: 'La función sen(x) parte desde el origen (0,0) y alcanza su valor máximo de 1 en π/2 (90°).' },
      { id: 'b3', categoria: 'basicas', frente: 'Gráfica de la función Coseno', reverso: SVGS.coseno, pair_a: 'Función cos(x)', pair_b: SVGS.coseno, explanation: 'La función cos(x) NO parte de cero, inicia en su valor máximo (0,1) y cruza por cero en π/2 (90°).' },
      { id: 'b4', categoria: 'basicas', frente: 'Equivalencia de π Radianes', reverso: '<b>180° (Grados)</b>', pair_a: 'π Radianes', pair_b: '180 Grados', explanation: 'π radianes representan media vuelta a la circunferencia, equivale exactamente a 180° sexagesimales.' },
      
      { id: 'p1', categoria: 'pitagorica', frente: 'Identidad Pitagórica Fundamental', reverso: SVGS.pitagoras + '<b>sen²(θ) + cos²(θ) = 1</b>', pair_a: 'sen²(θ) + cos²(θ)', pair_b: '1', explanation: 'Nace del Teorema de Pitágoras en el círculo de radio 1: la suma de los catetos al cuadrado siempre es igual a 1.' },
      { id: 'p2', categoria: 'pitagorica', frente: 'Identidad para 1 + tan²(θ)', reverso: '<b>sec²(θ)</b>', pair_a: '1 + tan²(θ)', pair_b: 'sec²(θ)', explanation: 'Se obtiene al dividir toda la identidad madre [sen²(θ) + cos²(θ) = 1] entre cos²(θ).' },
      { id: 'p3', categoria: 'pitagorica', frente: 'Identidad para 1 + cot²(θ)', reverso: '<b>csc²(θ)</b>', pair_a: '1 + cot²(θ)', pair_b: 'csc²(θ)', explanation: 'Se obtiene al dividir la identidad madre entre sen²(θ).' },

      { id: 'r1', categoria: 'reglas', frente: 'Gráfica de la función Tangente', reverso: SVGS.tangente, pair_a: 'Función tan(x)', pair_b: SVGS.tangente, explanation: 'La tangente se dispara al infinito formando asíntotas verticales en π/2 y 3π/2 porque el coseno se vuelve cero.' },
      { id: 'r2', categoria: 'reglas', frente: 'Ángulo Doble: sen(2θ)', reverso: '<b>2 · sen(θ) · cos(θ)</b>', pair_a: 'sen(2θ)', pair_b: '2·sen(θ)·cos(θ)', explanation: '¡Ojo! sen(2θ) no es simplemente multiplicar por 2, requiere el producto del seno por el coseno duplicado.' },
      { id: 'r3', categoria: 'reglas', frente: 'Recíproca de sec(θ)', reverso: '<b>1 / cos(θ)</b>', pair_a: 'sec(θ)', pair_b: '1 / cos(θ)', explanation: 'Aplica la regla de cruce: la Secante (S) es la recíproca del Coseno (C).' },
      { id: 'r4', categoria: 'reglas', frente: 'Cociente de tan(θ)', reverso: '<b>sen(θ) / cos(θ)</b>', pair_a: 'tan(θ)', pair_b: 'sen(θ)/cos(θ)', explanation: 'La tangente representa la pendiente, equivalente a dividir la altura (Seno) entre la base (Coseno).' },

      { id: 't1', categoria: 'transf', frente: 'Amplitud de onda y = A·sen(x)', reverso: '<b>|A|</b> (Altura máxima)', pair_a: 'Amplitud (A)', pair_b: '|A|', explanation: 'La amplitud representa la distancia desde la línea media hasta el pico. Siempre es un valor positivo |A|.' },
      { id: 't2', categoria: 'transf', frente: 'Cálculo del Nuevo Periodo', reverso: '<b>2π / |B|</b>', pair_a: 'Nuevo Periodo', pair_b: '2π / |B|', explanation: 'El periodo natural 2π se comprime o estira al dividirlo entre la frecuencia angular B.' }
    ];

    let currentScreen = 1;
    let selectedCategory = 'todas';
    let currentDifficulty = 'facil';
    let userProgress = JSON.parse(localStorage.getItem('trigo_progress_v4')) || {}; 

    let activeDeck = [];
    let currentCardIndex = 0;
    let isFlipped = false;

    // Variables del Juego de Memoria
    let gameCards = [];
    let flippedMemoryCards = [];
    let matchedPairs = 0;
    let gameMoves = 0;
    let userScore = 1000;
    let failedPairs = new Set(); // Guarda los IDs en los que falló
    let gameTimer = null;
    let previewTimer = null;
    let secondsElapsed = 0;
    let isPreviewing = false;

    window.addEventListener('DOMContentLoaded', () => {
      renderChips();
      renderHomeProgress();
      lucide.createIcons();
    });

    // --- MANEJO DE TEMAS ---
    function selectCategory(catId) {
      selectedCategory = catId;
      const categoryObj = CATEGORIES.find(c => c.id === catId) || CATEGORIES[0];
      
      document.documentElement.style.setProperty('--active-color', categoryObj.color);
      document.documentElement.style.setProperty('--active-bg', categoryObj.bg);

      renderChips();

      if (currentScreen === 2) setupFlashcards();
      if (currentScreen === 3) setupMemoryGame();
      if (currentScreen === 4) renderExplanationTab();
    }

    function renderChips() {
      const container = document.getElementById('chips-bar');
      container.innerHTML = CATEGORIES.map(c => `
        <button class="chip ${c.id === selectedCategory ? 'active' : ''}" onclick="selectCategory('${c.id}')">
          ${c.name}
        </button>
      `).join('');
    }

    function openExplanationTab() {
      navigateTo(4);
    }

    function renderExplanationTab() {
      const catObj = CATEGORIES.find(c => c.id === selectedCategory) || CATEGORIES[0];
      
      document.getElementById('exp-badge').style.background = catObj.bg;
      document.getElementById('exp-badge').style.color = catObj.color;
      document.getElementById('exp-title').innerText = catObj.name;

      const detailsHtml = catObj.details.map(d => `
        <div class="detail-section">
          <h4><i data-lucide="check-circle-2" size="16" style="color:var(--active-color)"></i> ${d.subtitle}</h4>
          <p>${d.text}</p>
        </div>
      `).join('');
      document.getElementById('exp-body-content').innerHTML = detailsHtml;

      document.getElementById('exp-graphic').innerHTML = catObj.graphic;
      document.getElementById('exp-graphic-caption').innerText = catObj.caption;

      // Inyectar a TrigoGlotón
      document.getElementById('monster-svg-container').innerHTML = MONSTER_SVG;
      document.getElementById('monster-quote').innerText = catObj.quote || '"¡Me encantas las matemáticas!"';

      // Tips
      const tipsHtml = catObj.tips.map(t => `
        <li><i data-lucide="utensils" size="14" style="flex-shrink:0; margin-top:3px; color:#166534;"></i> <span>${t}</span></li>
      `).join('');
      document.getElementById('exp-tips-list').innerHTML = tipsHtml;

      // Mitos
      const mythsHtml = catObj.myths ? catObj.myths.map(m => `
        <li><i data-lucide="skull" size="14" style="flex-shrink:0; margin-top:3px; color:#dc2626;"></i> <span>${m}</span></li>
      `).join('') : '';
      document.getElementById('exp-myths-list').innerHTML = mythsHtml;

      // Secreto
      document.getElementById('exp-secret-text').innerText = catObj.secret || '';

      lucide.createIcons();
    }

    function navigateTo(screenNum) {
      currentScreen = screenNum;
      document.querySelectorAll('.screen').forEach((s, idx) => {
        s.classList.toggle('active', idx + 1 === screenNum);
      });

      const topNav = document.getElementById('top-nav');
      const screenTitle = document.getElementById('current-screen-title');

      if (screenNum === 1) {
        topNav.style.display = 'none';
        renderHomeProgress();
      } else {
        topNav.style.display = 'flex';
        if (screenNum === 2) screenTitle.innerText = 'Tarjetas de Estudio';
        if (screenNum === 3) screenTitle.innerText = 'Juego de Memoria';
        if (screenNum === 4) screenTitle.innerText = 'Explicación Detallada';

        if (screenNum === 2) setupFlashcards();
        if (screenNum === 3) setupMemoryGame();
        if (screenNum === 4) renderExplanationTab();
      }
    }

    function saveProgress(cardId, status) {
      userProgress[cardId] = status;
      localStorage.setItem('trigo_progress_v4', JSON.stringify(userProgress));
    }

    function getCategoryMastery(catId) {
      const cards = catId === 'todas' ? DATA_BANK : DATA_BANK.filter(d => d.categoria === catId);
      if (cards.length === 0) return 0;
      const mastered = cards.filter(c => userProgress[c.id] === 'dominada').length;
      return Math.round((mastered / cards.length) * 100);
    }

    function renderHomeProgress() {
      const container = document.getElementById('progress-rows');
      const validCats = CATEGORIES.filter(c => c.id !== 'todas');
      
      container.innerHTML = validCats.map(c => {
        const pct = getCategoryMastery(c.id);
        return `
          <div class="progress-row">
            <span class="progress-label">${c.name}</span>
            <div class="progress-bar-bg">
              <div class="progress-bar-fill" style="width: ${pct}%; background: ${c.color};"></div>
            </div>
            <span class="progress-val">${pct}%</span>
          </div>
        `;
      }).join('');
    }

    // --- TARJETAS DE ESTUDIO ---
    function setupFlashcards() {
      activeDeck = selectedCategory === 'todas' 
        ? [...DATA_BANK] 
        : DATA_BANK.filter(d => d.categoria === selectedCategory);
      
      if (activeDeck.length === 0) activeDeck = [...DATA_BANK];

      currentCardIndex = 0;
      renderCard();
    }

    function renderCard() {
      const cardElem = document.getElementById('flashcard');
      cardElem.classList.remove('flipped');
      isFlipped = false;
      document.getElementById('grading-btns').style.visibility = 'hidden';

      const item = activeDeck[currentCardIndex];
      const catObj = CATEGORIES.find(c => c.id === item.categoria);

      document.getElementById('card-category-label').innerText = catObj ? catObj.name : '';
      document.getElementById('card-counter').innerText = `${currentCardIndex + 1} / ${activeDeck.length}`;
      document.getElementById('category-mastery-label').innerText = `${getCategoryMastery(item.categoria)}% dominado`;

      document.getElementById('card-front-text').innerHTML = item.frente;
      document.getElementById('card-back-text').innerHTML = item.reverso;

      lucide.createIcons();
    }

    function flipCard() {
      if (activeDeck.length === 0) return;
      isFlipped = !isFlipped;
      document.getElementById('flashcard').classList.toggle('flipped', isFlipped);
      if (isFlipped) {
        document.getElementById('grading-btns').style.visibility = 'visible';
      }
    }

    function gradeCard(status) {
      const item = activeDeck[currentCardIndex];
      saveProgress(item.id, status);
      nextCard();
    }

    function nextCard() {
      if (activeDeck.length === 0) return;
      currentCardIndex = (currentCardIndex + 1) % activeDeck.length;
      renderCard();
    }

    function prevCard() {
      if (activeDeck.length === 0) return;
      currentCardIndex = (currentCardIndex - 1 + activeDeck.length) % activeDeck.length;
      renderCard();
    }

    // --- JUEGO DE MEMORIA CON SISTEMA DE REGAÑO Y RETROALIMENTACIÓN ---
    function setDifficulty(diff) {
      currentDifficulty = diff;
      document.querySelectorAll('.btn-diff').forEach(b => b.classList.remove('active'));
      document.getElementById(`diff-${diff}`).classList.add('active');
      setupMemoryGame();
    }

    function setupMemoryGame() {
      clearInterval(gameTimer);
      clearInterval(previewTimer);
      gameTimer = null;
      previewTimer = null;
      secondsElapsed = 0;
      gameMoves = 0;
      userScore = 1000;
      matchedPairs = 0;
      flippedMemoryCards = [];
      failedPairs.clear();
      isPreviewing = true;
      
      document.getElementById('game-timer').innerText = '00:00';
      document.getElementById('game-score').innerText = userScore;

      let pool = selectedCategory === 'todas'
        ? DATA_BANK
        : DATA_BANK.filter(d => d.categoria === selectedCategory);

      let pairCount = 3; 
      let previewTimeLeft = 8; 

      if (currentDifficulty === 'medio') {
        pairCount = 4;
        previewTimeLeft = 12; 
      } else if (currentDifficulty === 'dificil') {
        pairCount = 6;
        previewTimeLeft = 15; 
      }

      if (pool.length < pairCount) {
        pool = [...pool, ...DATA_BANK.filter(d => !pool.includes(d))];
      }

      const selectedPool = [...pool].sort(() => Math.random() - 0.5).slice(0, pairCount);

      gameCards = [];
      selectedPool.forEach(item => {
        const contentA = item.pair_a || item.frente;
        const contentB = item.pair_b || item.reverso;

        gameCards.push({ pairId: item.id, content: contentA, original: item });
        gameCards.push({ pairId: item.id, content: contentB, original: item });
      });

      gameCards.sort(() => Math.random() - 0.5);

      const grid = document.getElementById('memory-grid');
      grid.className = `grid ${pairCount >= 4 ? 'grid-4' : ''}`;

      grid.innerHTML = gameCards.map((card, idx) => `
        <div class="memory-card flipped" id="mem-card-${idx}" onclick="flipMemoryCard(${idx})">
          <div class="card-inner-content">${card.content}</div>
          <i data-lucide="help-circle" class="card-icon-back" style="display:none;"></i>
        </div>
      `).join('');

      lucide.createIcons();

      const statusMsg = document.getElementById('game-status-msg');
      statusMsg.innerText = `¡Memoriza! (${previewTimeLeft}s)`;

      previewTimer = setInterval(() => {
        previewTimeLeft--;
        if (previewTimeLeft > 0) {
          statusMsg.innerText = `¡Memoriza! (${previewTimeLeft}s)`;
        } else {
          clearInterval(previewTimer);
          document.querySelectorAll('.memory-card').forEach(card => {
            card.classList.remove('flipped');
            const innerContent = card.querySelector('.card-inner-content');
            const iconBack = card.querySelector('.card-icon-back');
            if (innerContent) innerContent.style.display = 'none';
            if (iconBack) iconBack.style.display = 'block';
          });
          isPreviewing = false;
          statusMsg.innerText = '¡Encuentra los pares!';
        }
      }, 1000);
    }

    function startTimer() {
      if (gameTimer || isPreviewing) return;
      gameTimer = setInterval(() => {
        secondsElapsed++;
        const mins = String(Math.floor(secondsElapsed / 60)).padStart(2, '0');
        const secs = String(secondsElapsed % 60).padStart(2, '0');
        document.getElementById('game-timer').innerText = `${mins}:${secs}`;
        
        // El tiempo también disminuye levemente la puntuación
        if (userScore > 0) {
          userScore = Math.max(0, userScore - 1);
          document.getElementById('game-score').innerText = userScore;
        }
      }, 1000);
    }

    function flipMemoryCard(idx) {
      if (isPreviewing) return;
      startTimer();

      const cardElem = document.getElementById(`mem-card-${idx}`);

      if (flippedMemoryCards.length >= 2 || cardElem.classList.contains('flipped') || cardElem.classList.contains('matched') || cardElem.classList.contains('eaten')) {
        return;
      }

      cardElem.classList.add('flipped');
      cardElem.querySelector('.card-inner-content').style.display = 'flex';
      cardElem.querySelector('.card-icon-back').style.display = 'none';
      flippedMemoryCards.push({ idx, ...gameCards[idx] });

      if (flippedMemoryCards.length === 2) {
        gameMoves++;
        checkMemoryMatch();
      }
    }

    function triggerMonsterBiteAnimation() {
      const overlay = document.getElementById('monster-error-pop');
      document.getElementById('monster-error-svg').innerHTML = MONSTER_SVG;
      overlay.classList.add('active');

      setTimeout(() => {
        overlay.classList.remove('active');
      }, 1100);
    }

    function checkMemoryMatch() {
      const [c1, c2] = flippedMemoryCards;

      if (c1.pairId === c2.pairId) {
        // MATCH CORRECTO
        setTimeout(() => {
          document.getElementById(`mem-card-${c1.idx}`).classList.add('matched');
          document.getElementById(`mem-card-${c2.idx}`).classList.add('matched');
          matchedPairs++;
          flippedMemoryCards = [];

          if (matchedPairs === gameCards.length / 2) {
            clearInterval(gameTimer);
            finishGameWithFeedback();
          }
        }, 400);
      } else {
        // ERROR: ¡TrigoGlotón se come las cartas y resta puntos!
        failedPairs.add(c1.pairId);
        failedPairs.add(c2.pairId);

        // Bajar puntuación por error (-100 pts)
        userScore = Math.max(0, userScore - 100);
        document.getElementById('game-score').innerText = userScore;

        setTimeout(() => {
          triggerMonsterBiteAnimation();
        }, 200);

        setTimeout(() => {
          // Las cartas desaparecen del tablero porque se las comió
          const card1 = document.getElementById(`mem-card-${c1.idx}`);
          const card2 = document.getElementById(`mem-card-${c2.idx}`);
          if (card1) card1.classList.add('eaten');
          if (card2) card2.classList.add('eaten');

          // Cuentan como pares resueltos/eliminados para avanzar el juego
          matchedPairs++;
          flippedMemoryCards = [];

          if (matchedPairs === gameCards.length / 2) {
            clearInterval(gameTimer);
            finishGameWithFeedback();
          }
        }, 1200);
      }
    }

    // FIN DEL JUEGO: RETROALIMENTACIÓN, REGAÑO Y AGRADECIMIENTO DE TRIGOGLOTÓN
    function finishGameWithFeedback() {
      const timeStr = document.getElementById('game-timer').innerText;
      let title = "¡Juego Terminado! 🦖";
      let bodyHtml = "";

      if (failedPairs.size === 0) {
        // Partida Perfecta
        bodyHtml = `
          <div style="text-align:center; margin-bottom:12px;">
            <div style="width:80px; height:80px; margin:0 auto;">${MONSTER_SVG}</div>
            <h3 style="color:#16a34a; font-weight:900; margin-top:6px;">¡PERFECCIÓN ABSOLUTA!</h3>
            <p style="font-size:0.88rem; color:#475569; margin-top:4px;">Puntuación Final: <b style="color:#e11d48; font-size:1.1rem;">${userScore} pts</b> | Tiempo: <b>${timeStr}</b></p>
          </div>
          <div class="feedback-box" style="background:#f0fdf4; border-color:#86efac; color:#166534;">
            <b>¡GRRRR! ¡Hoy me dejaste con hambre! 💚</b><br>
            No cometiste ni un solo error. Eres un verdadero maestro de la trigonometría. ¡Gracias por intentar alimentarme, pero tus conocimientos fueron demasiado sólidos!
          </div>
        `;
      } else {
        // Hubo errores
        let errorDetails = "";
        failedPairs.forEach(pairId => {
          const item = DATA_BANK.find(d => d.id === pairId);
          if (item) {
            errorDetails += `
              <div class="feedback-item">
                <b>• Tema fallado:</b> ${item.frente}<br>
                <span style="font-weight:600; font-size:0.82rem; color:#64748b;"><b>Explicación:</b> ${item.explanation}</span>
              </div>
            `;
          }
        });

        bodyHtml = `
          <div style="text-align:center; margin-bottom:10px;">
            <div style="width:75px; height:75px; margin:0 auto;">${MONSTER_SVG}</div>
            <p style="font-size:0.88rem; color:#475569; margin-top:4px;">Puntuación Final: <b style="color:#e11d48; font-size:1.1rem;">${userScore} pts</b> | Tiempo: <b>${timeStr}</b></p>
          </div>

          <div class="feedback-box">
            <b style="font-size:0.95rem;">📢 ¡El Regaño de TrigoGlotón!</b><br>
            "¡¡CROANCH!! ¿En serio intentaste juntar esas cartas? ¡Estaban totalmente equivocadas! Me las tuve que comer para que no reprobaras el examen. Pon más atención a la teoría."
          </div>

          <div style="margin: 12px 0;">
            <b style="font-size:0.85rem; text-transform:uppercase; color:#0f172a; font-weight:800;">📖 Lo que fallaste y debes repasar:</b>
            ${errorDetails}
          </div>

          <div style="background:#f0fdf4; border:1px stroke #86efac; border-radius:10px; padding:10px; font-size:0.85rem; color:#166534; font-weight:700; text-align:center;">
            💚 <b>"Pero no te desanimes... ¡Muchas gracias por alimentarme con tus errores! Estaban deliciosos 😋. ¡Repasa la teoría y vuelve a intentarlo!"</b>
          </div>
        `;
      }

      showModal(title, bodyHtml, () => {
        setupMemoryGame();
      });
    }

    // --- MODAL GENERAL ---
    function showModal(title, htmlContent, onPrimaryAction) {
      document.getElementById('modal-title').innerText = title;
      document.getElementById('modal-desc').innerHTML = htmlContent;
      const primaryBtn = document.getElementById('modal-primary-btn');

      primaryBtn.onclick = () => { closeModal(); if (onPrimaryAction) onPrimaryAction(); };
      document.getElementById('app-modal').classList.add('active');
    }

    function closeModal() {
      document.getElementById('app-modal').classList.remove('active');
    }

    function showFaqModal() {
      const faqHTML = `
        <p><b>Reglas del Juego con TrigoGlotón:</b><br><br>1. Inicias con <b>1000 puntos</b>.<br>2. Empareja las cartas antes de que se agote el tiempo.<br>3. <b>¡Cuidado con equivocarte!</b> Si seleccionas dos cartas incorrectas, TrigoGlotón saldrá, **devorará las cartas para siempre** y te restará **100 puntos**.<br>4. Al finalizar, recibirás una explicación detallada de lo que fallaste para ayudarte a mejorar.</p>
      `;
      showModal('¿Cómo jugar?', faqHTML);
    }
  </script>
</body>
</html>
