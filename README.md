<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>EduAI — Учись с ИИ</title>
<link href="https://fonts.googleapis.com/css2?family=Unbounded:wght@400;600;800&family=Inter:wght@300;400;500;600&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #0a0a0f;
    --card: #12121a;
    --card2: #1a1a26;
    --accent: #6c63ff;
    --accent2: #ff6584;
    --accent3: #43e97b;
    --text: #f0f0ff;
    --muted: #7070a0;
    --border: #2a2a40;
    --radius: 18px;
  }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body { background: var(--bg); color: var(--text); font-family: 'Inter', sans-serif; min-height: 100vh; overflow-x: hidden; }

  /* SCREENS */
  .screen { display: none; min-height: 100vh; }
  .screen.active { display: flex; flex-direction: column; }

  /* HOME */
  #home {
    background: radial-gradient(ellipse at 20% 20%, #1a1060 0%, transparent 60%),
                radial-gradient(ellipse at 80% 80%, #1a0030 0%, transparent 60%), #0a0a0f;
    align-items: center; justify-content: center; text-align: center; padding: 30px 20px;
  }
  .home-logo { font-family: 'Unbounded', sans-serif; font-size: 48px; font-weight: 800;
    background: linear-gradient(135deg, #6c63ff, #ff6584, #43e97b);
    -webkit-background-clip: text; -webkit-text-fill-color: transparent; margin-bottom: 8px; }
  .home-sub { color: var(--muted); font-size: 15px; margin-bottom: 50px; }
  .home-cards { display: grid; grid-template-columns: 1fr 1fr; gap: 16px; max-width: 400px; width: 100%; margin-bottom: 30px; }
  .home-card {
    background: var(--card); border: 1.5px solid var(--border); border-radius: var(--radius);
    padding: 24px 16px; cursor: pointer; transition: all .25s; text-align: center;
  }
  .home-card:hover { transform: translateY(-4px); border-color: var(--accent); box-shadow: 0 8px 32px #6c63ff30; }
  .home-card .icon { font-size: 36px; margin-bottom: 10px; }
  .home-card .label { font-family: 'Unbounded', sans-serif; font-size: 13px; font-weight: 600; }
  .home-card .desc { font-size: 11px; color: var(--muted); margin-top: 4px; }
  .home-card.ai-card { grid-column: 1/-1; }

  /* TOPBAR */
  .topbar {
    display: flex; align-items: center; padding: 16px 20px; gap: 12px;
    background: var(--card); border-bottom: 1px solid var(--border); position: sticky; top: 0; z-index: 100;
  }
  .back-btn {
    background: var(--card2); border: none; color: var(--text); width: 38px; height: 38px;
    border-radius: 10px; cursor: pointer; font-size: 18px; display: flex; align-items: center; justify-content: center;
    transition: background .2s;
  }
  .back-btn:hover { background: var(--accent); }
  .topbar-title { font-family: 'Unbounded', sans-serif; font-size: 15px; font-weight: 600; }
  .topbar-badge { margin-left: auto; background: var(--accent); color: #fff; font-size: 11px; padding: 4px 10px; border-radius: 20px; font-weight: 600; }

  /* ===== ENGLISH ===== */
  #english { background: #0a0a0f; }
  .eng-content { flex: 1; overflow-y: auto; padding: 20px; }
  .eng-tabs { display: flex; gap: 8px; margin-bottom: 20px; overflow-x: auto; padding-bottom: 4px; }
  .eng-tab {
    background: var(--card); border: 1.5px solid var(--border); color: var(--muted);
    border-radius: 30px; padding: 8px 18px; cursor: pointer; font-size: 13px; white-space: nowrap;
    transition: all .2s;
  }
  .eng-tab.active { background: var(--accent); border-color: var(--accent); color: #fff; font-weight: 600; }

  .eng-panel { display: none; }
  .eng-panel.active { display: block; }

  /* Alphabet */
  .alpha-grid { display: grid; grid-template-columns: repeat(6, 1fr); gap: 8px; }
  .alpha-card {
    background: var(--card2); border: 1px solid var(--border); border-radius: 12px;
    padding: 10px 6px; text-align: center; cursor: pointer; transition: all .2s;
  }
  .alpha-card:hover { border-color: var(--accent); background: #1e1e30; }
  .alpha-card .big { font-size: 22px; font-weight: 700; color: var(--accent); }
  .alpha-card .small { font-size: 11px; color: var(--muted); }

  /* Words */
  .word-list { display: flex; flex-direction: column; gap: 10px; }
  .word-card {
    background: var(--card2); border: 1px solid var(--border); border-radius: 14px;
    padding: 14px 16px; display: flex; align-items: center; gap: 14px; cursor: pointer;
    transition: all .2s;
  }
  .word-card:hover { border-color: var(--accent2); }
  .word-card .flag { font-size: 22px; }
  .word-card .en { font-size: 17px; font-weight: 600; }
  .word-card .ru { font-size: 13px; color: var(--muted); }
  .word-card .tr { font-size: 12px; color: var(--accent); }

  /* Quiz */
  .quiz-box { background: var(--card2); border: 1px solid var(--border); border-radius: var(--radius); padding: 24px; }
  .quiz-q { font-size: 20px; font-weight: 700; margin-bottom: 20px; text-align: center; }
  .quiz-opts { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
  .quiz-opt {
    background: var(--card); border: 1.5px solid var(--border); border-radius: 12px;
    padding: 14px; text-align: center; cursor: pointer; font-size: 14px; transition: all .2s;
  }
  .quiz-opt:hover { border-color: var(--accent); background: #1e1e30; }
  .quiz-opt.correct { border-color: var(--accent3); background: #0d2a1a; color: var(--accent3); }
  .quiz-opt.wrong { border-color: var(--accent2); background: #2a0d1a; color: var(--accent2); }
  .quiz-score { text-align: center; color: var(--accent); font-size: 14px; margin-bottom: 16px; }
  .quiz-next { width: 100%; background: var(--accent); border: none; color: #fff; border-radius: 12px;
    padding: 14px; font-size: 15px; font-weight: 600; cursor: pointer; margin-top: 16px; }

  /* Phrases */
  .phrase-card {
    background: var(--card2); border: 1px solid var(--border); border-radius: 14px; padding: 16px;
    margin-bottom: 10px; cursor: pointer; transition: all .2s;
  }
  .phrase-card:hover { border-color: var(--accent); }
  .phrase-en { font-size: 16px; font-weight: 600; margin-bottom: 4px; }
  .phrase-ru { font-size: 13px; color: var(--muted); margin-bottom: 8px; }
  .phrase-ctx { font-size: 12px; color: var(--accent3); background: #0d2a1a; padding: 6px 10px; border-radius: 8px; }

  /* Grammar modal */
  .gram-card {
    background: var(--card2); border: 1.5px solid var(--border); border-radius: 14px;
    padding: 18px; margin-bottom: 12px; cursor: pointer; transition: all .2s;
  }
  .gram-card:hover { border-color: var(--accent); transform: translateY(-2px); }
  .gram-header { display: flex; align-items: center; gap: 12px; margin-bottom: 8px; }
  .gram-icon { font-size: 28px; }
  .gram-title { font-size: 16px; font-weight: 700; }
  .gram-desc { font-size: 13px; color: var(--muted); margin-bottom: 10px; line-height: 1.5; }
  .gram-rule { background: #1a1a30; border-left: 3px solid var(--accent); padding: 8px 12px; border-radius: 0 8px 8px 0; font-size: 13px; margin-bottom: 8px; font-family: monospace; color: #c0c0ff; }
  .gram-example { background: #0d2a1a; border-left: 3px solid var(--accent3); padding: 8px 12px; border-radius: 0 8px 8px 0; font-size: 13px; color: var(--accent3); }
  .gram-table { width: 100%; border-collapse: collapse; margin: 10px 0; font-size: 13px; }
  .gram-table th { background: var(--accent); color: #fff; padding: 6px 10px; text-align: left; }
  .gram-table td { padding: 6px 10px; border-bottom: 1px solid var(--border); }
  .gram-table tr:nth-child(even) td { background: #15152a; }

  /* Modal overlay */
  .modal-overlay {
    display: none; position: fixed; inset: 0; background: rgba(0,0,0,.85);
    z-index: 500; align-items: flex-end; justify-content: center;
  }
  .modal-overlay.open { display: flex; }
  .modal-sheet {
    background: var(--card); border-radius: 20px 20px 0 0; padding: 20px;
    max-height: 85vh; overflow-y: auto; width: 100%; max-width: 560px;
    animation: slideUp .3s ease;
  }
  @keyframes slideUp { from{transform:translateY(100%)} to{transform:translateY(0)} }
  .modal-close { float: right; background: var(--card2); border: none; color: var(--muted);
    width: 32px; height: 32px; border-radius: 50%; cursor: pointer; font-size: 16px; }
  .modal-title { font-family: 'Unbounded', sans-serif; font-size: 17px; font-weight: 700; margin-bottom: 16px; }

  /* ===== MATH ===== */
  #math { background: #0a0a0f; }
  .math-content { flex: 1; overflow-y: auto; padding: 20px; }
  .math-tabs { display: flex; gap: 8px; margin-bottom: 20px; overflow-x: auto; padding-bottom: 4px; }
  .math-tab {
    background: var(--card); border: 1.5px solid var(--border); color: var(--muted);
    border-radius: 30px; padding: 8px 18px; cursor: pointer; font-size: 13px; white-space: nowrap;
    transition: all .2s;
  }
  .math-tab.active { background: #ff6584; border-color: #ff6584; color: #fff; font-weight: 600; }
  .math-panel { display: none; }
  .math-panel.active { display: block; }

  .math-topic { background: var(--card2); border: 1px solid var(--border); border-radius: 14px;
    padding: 16px; margin-bottom: 10px; cursor: pointer; transition: all .2s; }
  .math-topic:hover { border-color: #ff6584; }
  .math-topic .t-title { font-size: 15px; font-weight: 600; margin-bottom: 4px; }
  .math-topic .t-desc { font-size: 12px; color: var(--muted); }
  .math-topic .t-icon { font-size: 24px; margin-bottom: 6px; }

  /* Math solver */
  .solver-area { background: var(--card2); border: 1px solid var(--border); border-radius: var(--radius); padding: 20px; margin-bottom: 16px; }
  .solver-input {
    width: 100%; background: var(--card); border: 1.5px solid var(--border); color: var(--text);
    border-radius: 12px; padding: 14px; font-size: 16px; font-family: 'Inter', sans-serif;
    resize: none; margin-bottom: 12px;
  }
  .solver-input:focus { outline: none; border-color: #ff6584; }
  .solve-btn {
    background: linear-gradient(135deg, #ff6584, #ff9966); border: none; color: #fff;
    border-radius: 12px; padding: 14px 28px; font-size: 15px; font-weight: 600; cursor: pointer;
    width: 100%; transition: opacity .2s;
  }
  .solve-btn:hover { opacity: 0.85; }
  .solution-box { background: var(--card2); border: 1.5px solid var(--accent3); border-radius: 14px; padding: 18px; margin-top: 14px; }
  .solution-step { padding: 8px 0; border-bottom: 1px solid var(--border); font-size: 14px; }
  .solution-step:last-child { border-bottom: none; }
  .step-num { color: var(--accent); font-weight: 700; margin-right: 6px; }

  /* Graph */
  canvas { border-radius: 12px; max-width: 100%; }
  .graph-controls { display: flex; gap: 8px; margin-bottom: 12px; flex-wrap: wrap; }
  .graph-fn-input {
    flex: 1; background: var(--card); border: 1.5px solid var(--border); color: var(--text);
    border-radius: 10px; padding: 10px 14px; font-size: 14px;
  }
  .graph-fn-input:focus { outline: none; border-color: var(--accent); }
  .graph-btn {
    background: var(--accent); border: none; color: #fff; border-radius: 10px;
    padding: 10px 18px; cursor: pointer; font-weight: 600;
  }

  /* ===== AI SOLVER ===== */
  #ai { background: #0a0a0f; }
  .ai-content { flex: 1; overflow-y: auto; padding: 20px; display: flex; flex-direction: column; gap: 16px; }

  .ai-chat { flex: 1; display: flex; flex-direction: column; gap: 12px; }
  .msg { max-width: 88%; padding: 14px 16px; border-radius: 16px; font-size: 14px; line-height: 1.6; }
  .msg.bot { background: var(--card2); border: 1px solid var(--border); align-self: flex-start; }
  .msg.user { background: var(--accent); align-self: flex-end; }
  .msg pre { background: #0a0a0f; padding: 10px; border-radius: 8px; overflow-x: auto; font-size: 12px; margin-top: 8px; }

  .ai-input-bar {
    display: flex; gap: 8px; background: var(--card); border: 1.5px solid var(--border);
    border-radius: 16px; padding: 10px 14px; align-items: flex-end;
  }
  .ai-input-bar textarea {
    flex: 1; background: transparent; border: none; color: var(--text); font-size: 14px;
    font-family: 'Inter', sans-serif; resize: none; max-height: 120px; overflow-y: auto; line-height: 1.5;
  }
  .ai-input-bar textarea:focus { outline: none; }
  .ai-btns { display: flex; gap: 6px; }
  .ai-icon-btn {
    background: var(--card2); border: none; color: var(--muted); width: 38px; height: 38px;
    border-radius: 10px; cursor: pointer; font-size: 18px; display: flex; align-items: center; justify-content: center;
    transition: all .2s;
  }
  .ai-icon-btn:hover { background: var(--accent); color: #fff; }
  .ai-send { background: var(--accent); border: none; color: #fff; width: 38px; height: 38px;
    border-radius: 10px; cursor: pointer; font-size: 18px; display: flex; align-items: center; justify-content: center; }

  /* Photo preview */
  .photo-preview { position: relative; background: var(--card2); border: 1px solid var(--border);
    border-radius: 14px; overflow: hidden; }
  .photo-preview img { width: 100%; max-height: 220px; object-fit: contain; display: block; }
  .photo-preview .remove-photo {
    position: absolute; top: 8px; right: 8px; background: rgba(0,0,0,.7); border: none;
    color: #fff; width: 28px; height: 28px; border-radius: 50%; cursor: pointer; font-size: 14px;
  }
  .cam-overlay {
    display: none; position: fixed; inset: 0; background: #000; z-index: 999;
    flex-direction: column; align-items: center; justify-content: center;
  }
  .cam-overlay.open { display: flex; }
  #camVideo { width: 100%; max-width: 500px; border-radius: 16px; }
  .cam-controls { display: flex; gap: 20px; margin-top: 20px; }
  .cam-btn {
    background: var(--card2); border: 2px solid var(--border); color: var(--text);
    padding: 12px 28px; border-radius: 30px; cursor: pointer; font-size: 15px; font-weight: 600;
  }
  .cam-shoot { background: var(--accent); border-color: var(--accent); }
  canvas#camCanvas { display: none; }

  .typing-dots span { display: inline-block; width: 6px; height: 6px; background: var(--muted); border-radius: 50%; margin: 0 2px; animation: dot 1.2s infinite; }
  .typing-dots span:nth-child(2) { animation-delay: .2s; }
  .typing-dots span:nth-child(3) { animation-delay: .4s; }
  @keyframes dot { 0%,80%,100%{transform:scale(0)} 40%{transform:scale(1)} }

  /* General utility */
  .section-title { font-family: 'Unbounded', sans-serif; font-size: 14px; font-weight: 600;
    color: var(--muted); text-transform: uppercase; letter-spacing: 1px; margin-bottom: 14px; }
  .pill { display: inline-block; padding: 4px 12px; border-radius: 30px; font-size: 11px; font-weight: 600; }
  .pill.easy { background: #0d2a1a; color: var(--accent3); }
  .pill.med { background: #1a1a0a; color: #ffcc44; }
  .pill.hard { background: #2a0d1a; color: var(--accent2); }

  .progress-bar { background: var(--card); border-radius: 30px; height: 8px; overflow: hidden; margin: 8px 0; }
  .progress-fill { height: 100%; border-radius: 30px; background: linear-gradient(90deg, var(--accent), var(--accent2)); transition: width .4s; }

  .empty-state { text-align: center; padding: 40px 20px; color: var(--muted); }
  .empty-state .e-icon { font-size: 48px; margin-bottom: 12px; }

  /* Animations */
  @keyframes fadeIn { from{opacity:0;transform:translateY(16px)} to{opacity:1;transform:translateY(0)} }
  .screen.active { animation: fadeIn .3s ease; }

  /* Scrollbar */
  ::-webkit-scrollbar { width: 4px; height: 4px; }
  ::-webkit-scrollbar-track { background: transparent; }
  ::-webkit-scrollbar-thumb { background: var(--border); border-radius: 4px; }
</style>
</head>
<body>

<!-- HOME -->
<div id="home" class="screen active">
  <div class="home-logo">EduAI</div>
  <div class="home-sub">Учись с искусственным интеллектом</div>
  <div class="home-cards">
    <div class="home-card" onclick="goTo('english')">
      <div class="icon">🇬🇧</div>
      <div class="label">Английский</div>
      <div class="desc">С нуля до разговорного</div>
    </div>
    <div class="home-card" onclick="goTo('math')">
      <div class="icon">📐</div>
      <div class="label">Математика</div>
      <div class="desc">С нуля до продвинутого</div>
    </div>
    <div class="home-card ai-card" onclick="goTo('ai')">
      <div class="icon">🤖</div>
      <div class="label">ИИ-Помощник</div>
      <div class="desc">Объяснит всё, решит фото задачи, строит графики — без лимитов</div>
    </div>
  </div>
</div>

<!-- ENGLISH -->
<div id="english" class="screen">
  <div class="topbar">
    <button class="back-btn" onclick="goHome()">←</button>
    <div class="topbar-title">🇬🇧 Английский язык</div>
    <div class="topbar-badge" id="eng-score">0 XP</div>
  </div>
  <div class="eng-content">
    <div class="eng-tabs">
      <div class="eng-tab active" onclick="engTab('alphabet',this)">Алфавит</div>
      <div class="eng-tab" onclick="engTab('words',this)">Слова</div>
      <div class="eng-tab" onclick="engTab('phrases',this)">Фразы</div>
      <div class="eng-tab" onclick="engTab('grammar',this)">Грамматика</div>
      <div class="eng-tab" onclick="engTab('quiz',this)">Тест</div>
    </div>

    <!-- Alphabet -->
    <div id="eng-alphabet" class="eng-panel active">
      <div class="section-title">Алфавит — 26 букв</div>
      <div class="alpha-grid" id="alphaGrid"></div>
    </div>

    <!-- Words -->
    <div id="eng-words" class="eng-panel">
      <div class="section-title">Базовые слова</div>
      <div class="word-list" id="wordList"></div>
    </div>

    <!-- Phrases -->
    <div id="eng-phrases" class="eng-panel">
      <div class="section-title">Разговорные фразы</div>
      <div class="eng-tabs" id="phraseFilter" style="margin-bottom:14px;">
        <div class="eng-tab active" onclick="filterPhrases('all',this)">Все</div>
        <div class="eng-tab" onclick="filterPhrases('🤝 Знакомство',this)">🤝 Знакомство</div>
        <div class="eng-tab" onclick="filterPhrases('🙏 Вежливость',this)">🙏 Вежливость</div>
        <div class="eng-tab" onclick="filterPhrases('💬 Понимание',this)">💬 Понимание</div>
        <div class="eng-tab" onclick="filterPhrases('🛍 Магазин',this)">🛍 Магазин</div>
        <div class="eng-tab" onclick="filterPhrases('📍 Ориентация',this)">📍 Ориентация</div>
        <div class="eng-tab" onclick="filterPhrases('🍽 Ресторан',this)">🍽 Ресторан</div>
        <div class="eng-tab" onclick="filterPhrases('😊 Эмоции',this)">😊 Эмоции</div>
        <div class="eng-tab" onclick="filterPhrases('👋 Прощание',this)">👋 Прощание</div>
      </div>
      <div id="phraseList"></div>
    </div>

    <!-- Grammar -->
    <div id="eng-grammar" class="eng-panel">
      <div class="section-title">Грамматика</div>
      <div id="grammarList"></div>
    </div>

    <!-- Quiz -->
    <div id="eng-quiz" class="eng-panel">
      <div class="quiz-score" id="quizScore">Счёт: 0 / 0</div>
      <div class="quiz-box">
        <div class="quiz-q" id="quizQ">Нажмите «Начать тест»</div>
        <div class="quiz-opts" id="quizOpts"></div>
        <button class="quiz-next" onclick="nextQuiz()">▶ Начать тест</button>
      </div>
    </div>
  </div>
</div>

<!-- MATH -->
<div id="math" class="screen">
  <div class="topbar">
    <button class="back-btn" onclick="goHome()">←</button>
    <div class="topbar-title">📐 Математика</div>
  </div>
  <div class="math-content">
    <div class="math-tabs">
      <div class="math-tab active" onclick="mathTab('topics',this)">Темы</div>
      <div class="math-tab" onclick="mathTab('solver',this)">Решатель</div>
      <div class="math-tab" onclick="mathTab('graph',this)">График</div>
    </div>

    <!-- Topics -->
    <div id="math-topics" class="math-panel active">
      <div class="section-title">Все темы</div>
      <div id="mathTopicList"></div>
    </div>

    <!-- Solver -->
    <div id="math-solver" class="math-panel">
      <div class="section-title">Решатель задач</div>
      <div class="solver-area">
        <textarea class="solver-input" id="mathProblem" rows="3" placeholder="Введите задачу... например: 2x + 5 = 15  или  найди производную x²+3x"></textarea>
        <button class="solve-btn" onclick="solveMath()">🔢 Решить</button>
        <div id="solutionBox"></div>
      </div>
    </div>

    <!-- Graph -->
    <div id="math-graph" class="math-panel">
      <div class="section-title">Построитель графиков</div>
      <div class="graph-controls">
        <input class="graph-fn-input" id="graphFn" placeholder="Функция: x^2, sin(x), 2*x+1..." value="x^2"/>
        <button class="graph-btn" onclick="drawGraph()">📈</button>
      </div>
      <canvas id="graphCanvas" width="340" height="300" style="background:#12121a;width:100%;"></canvas>
    </div>
  </div>
</div>

<!-- AI -->
<div id="ai" class="screen">
  <div class="topbar">
    <button class="back-btn" onclick="goHome()">←</button>
    <div class="topbar-title">🤖 ИИ-Помощник</div>
    <div class="topbar-badge">∞ Безлимит</div>
  </div>
  <div class="ai-content">
    <div class="ai-chat" id="aiChat">
      <div class="msg bot">👋 Привет! Я ИИ-помощник. Могу объяснить любую тему по английскому или математике, решить задачу с текста или <strong>фотографии</strong>. Просто напиши или сними фото задания!</div>
    </div>
    <div id="photoPreviewWrap"></div>
    <div class="ai-input-bar">
      <textarea id="aiInput" rows="1" placeholder="Спроси что угодно..." oninput="autoResize(this)" onkeydown="aiEnter(event)"></textarea>
      <div class="ai-btns">
        <input type="file" id="fileInput" accept="image/*" style="display:none" onchange="handleFile(event)"/>
        <button class="ai-icon-btn" title="Галерея" onclick="document.getElementById('fileInput').click()">🖼</button>
        <button class="ai-icon-btn" title="Камера" onclick="openCamera()">📷</button>
        <button class="ai-send" onclick="sendAI()">➤</button>
      </div>
    </div>
  </div>
</div>

<!-- Grammar modal -->
<div class="modal-overlay" id="gramModal" onclick="closeGramModal(event)">
  <div class="modal-sheet" id="gramModalContent">
    <button class="modal-close" onclick="document.getElementById('gramModal').classList.remove('open')">✕</button>
    <div class="modal-title" id="gramModalTitle"></div>
    <div id="gramModalBody"></div>
  </div>
</div>

<!-- Camera overlay -->
<div class="cam-overlay" id="camOverlay">
  <video id="camVideo" autoplay playsinline></video>
  <canvas id="camCanvas"></canvas>
  <div class="cam-controls">
    <button class="cam-btn" onclick="closeCamera()">✕ Отмена</button>
    <button class="cam-btn cam-shoot" onclick="shootPhoto()">📸 Снять</button>
  </div>
</div>

<script>
// =================== NAVIGATION ===================
function goTo(screen) {
  document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
  document.getElementById(screen).classList.add('active');
}
function goHome() {
  goTo('home');
}

// =================== ENGLISH DATA ===================
const alphabet = [
  {l:'A',p:'эй'},{l:'B',p:'би'},{l:'C',p:'си'},{l:'D',p:'ди'},
  {l:'E',p:'и'},{l:'F',p:'эф'},{l:'G',p:'джи'},{l:'H',p:'эйч'},
  {l:'I',p:'ай'},{l:'J',p:'джэй'},{l:'K',p:'кэй'},{l:'L',p:'эл'},
  {l:'M',p:'эм'},{l:'N',p:'эн'},{l:'O',p:'оу'},{l:'P',p:'пи'},
  {l:'Q',p:'кью'},{l:'R',p:'ар'},{l:'S',p:'эс'},{l:'T',p:'ти'},
  {l:'U',p:'ю'},{l:'V',p:'ви'},{l:'W',p:'дабл-ю'},{l:'X',p:'экс'},
  {l:'Y',p:'вай'},{l:'Z',p:'зед'}
];

const words = [
  {en:'Hello',ru:'Привет',tr:'хэлоу',cat:'👋'},
  {en:'Goodbye',ru:'Пока',tr:'гудбай',cat:'👋'},
  {en:'Yes',ru:'Да',tr:'йес',cat:'✅'},
  {en:'No',ru:'Нет',tr:'ноу',cat:'❌'},
  {en:'Please',ru:'Пожалуйста',tr:'плиз',cat:'🙏'},
  {en:'Thank you',ru:'Спасибо',tr:'сэнк ю',cat:'💙'},
  {en:'Water',ru:'Вода',tr:'вотер',cat:'💧'},
  {en:'Food',ru:'Еда',tr:'фуд',cat:'🍕'},
  {en:'House',ru:'Дом',tr:'хаус',cat:'🏠'},
  {en:'Car',ru:'Машина',tr:'кар',cat:'🚗'},
  {en:'Book',ru:'Книга',tr:'бук',cat:'📚'},
  {en:'Time',ru:'Время',tr:'тайм',cat:'⏰'},
  {en:'Love',ru:'Любовь',tr:'лав',cat:'❤️'},
  {en:'Friend',ru:'Друг',tr:'фрэнд',cat:'🤝'},
  {en:'Work',ru:'Работа',tr:'вёрк',cat:'💼'},
  {en:'Money',ru:'Деньги',tr:'мани',cat:'💰'},
  {en:'School',ru:'Школа',tr:'скул',cat:'🎓'},
  {en:'Doctor',ru:'Врач',tr:'доктор',cat:'👨‍⚕️'},
  {en:'Beautiful',ru:'Красивый',tr:'бьютифул',cat:'✨'},
  {en:'Happy',ru:'Счастливый',tr:'хэпи',cat:'😊'}
];

const phrases = [
  // Знакомство
  {en:"How are you?", ru:"Как дела?", ctx:"🤝 Знакомство"},
  {en:"I'm fine, thanks!", ru:"Всё хорошо, спасибо!", ctx:"🤝 Знакомство"},
  {en:"What's your name?", ru:"Как тебя зовут?", ctx:"🤝 Знакомство"},
  {en:"My name is...", ru:"Меня зовут...", ctx:"🤝 Знакомство"},
  {en:"Nice to meet you!", ru:"Приятно познакомиться!", ctx:"🤝 Знакомство"},
  {en:"Where are you from?", ru:"Откуда ты?", ctx:"🤝 Знакомство"},
  {en:"How old are you?", ru:"Сколько тебе лет?", ctx:"🤝 Знакомство"},
  {en:"I am 25 years old.", ru:"Мне 25 лет.", ctx:"🤝 Знакомство"},
  {en:"What do you do?", ru:"Кем ты работаешь?", ctx:"🤝 Знакомство"},
  {en:"I'm a student.", ru:"Я студент.", ctx:"🤝 Знакомство"},
  // Вежливость
  {en:"Please.", ru:"Пожалуйста.", ctx:"🙏 Вежливость"},
  {en:"Thank you very much!", ru:"Большое спасибо!", ctx:"🙏 Вежливость"},
  {en:"You're welcome!", ru:"Не за что!", ctx:"🙏 Вежливость"},
  {en:"Excuse me.", ru:"Извините.", ctx:"🙏 Вежливость"},
  {en:"I'm sorry.", ru:"Мне жаль / Простите.", ctx:"🙏 Вежливость"},
  {en:"No problem!", ru:"Без проблем!", ctx:"🙏 Вежливость"},
  {en:"Of course!", ru:"Конечно!", ctx:"🙏 Вежливость"},
  {en:"Sure!", ru:"Конечно! / Ладно!", ctx:"🙏 Вежливость"},
  {en:"Don't worry.", ru:"Не беспокойся.", ctx:"🙏 Вежливость"},
  {en:"Take care!", ru:"Береги себя!", ctx:"🙏 Вежливость"},
  // Понимание
  {en:"I don't understand.", ru:"Я не понимаю.", ctx:"💬 Понимание"},
  {en:"Can you repeat that?", ru:"Можете повторить?", ctx:"💬 Понимание"},
  {en:"Can you speak slower?", ru:"Можете говорить медленнее?", ctx:"💬 Понимание"},
  {en:"What does that mean?", ru:"Что это значит?", ctx:"💬 Понимание"},
  {en:"How do you say... in English?", ru:"Как сказать... по-английски?", ctx:"💬 Понимание"},
  {en:"I understand!", ru:"Я понимаю!", ctx:"💬 Понимание"},
  {en:"Can you write it down?", ru:"Можете записать?", ctx:"💬 Понимание"},
  {en:"I know a little English.", ru:"Я немного знаю английский.", ctx:"💬 Понимание"},
  // Магазин
  {en:"How much does it cost?", ru:"Сколько это стоит?", ctx:"🛍 Магазин"},
  {en:"That's too expensive.", ru:"Это слишком дорого.", ctx:"🛍 Магазин"},
  {en:"Do you have a discount?", ru:"Есть скидка?", ctx:"🛍 Магазин"},
  {en:"I'd like to buy this.", ru:"Я хочу купить это.", ctx:"🛍 Магазин"},
  {en:"Do you accept credit cards?", ru:"Вы принимаете карты?", ctx:"🛍 Магазин"},
  {en:"Can I try it on?", ru:"Можно примерить?", ctx:"🛍 Магазин"},
  {en:"What size is this?", ru:"Какой это размер?", ctx:"🛍 Магазин"},
  {en:"I'll take it!", ru:"Я это возьму!", ctx:"🛍 Магазин"},
  // Ориентация
  {en:"Where is the toilet?", ru:"Где туалет?", ctx:"📍 Ориентация"},
  {en:"How do I get to...?", ru:"Как добраться до...?", ctx:"📍 Ориентация"},
  {en:"Is it far?", ru:"Это далеко?", ctx:"📍 Ориентация"},
  {en:"Turn left / right.", ru:"Поверните налево / направо.", ctx:"📍 Ориентация"},
  {en:"Go straight ahead.", ru:"Идите прямо.", ctx:"📍 Ориентация"},
  {en:"I'm lost.", ru:"Я заблудился.", ctx:"📍 Ориентация"},
  // Ресторан
  {en:"A table for two, please.", ru:"Столик на двоих, пожалуйста.", ctx:"🍽 Ресторан"},
  {en:"What do you recommend?", ru:"Что вы рекомендуете?", ctx:"🍽 Ресторан"},
  {en:"I'd like to order...", ru:"Я хотел бы заказать...", ctx:"🍽 Ресторан"},
  {en:"The bill, please!", ru:"Счёт, пожалуйста!", ctx:"🍽 Ресторан"},
  {en:"It was delicious!", ru:"Это было вкусно!", ctx:"🍽 Ресторан"},
  {en:"I'm allergic to...", ru:"У меня аллергия на...", ctx:"🍽 Ресторан"},
  // Эмоции
  {en:"I'm happy!", ru:"Я счастлив!", ctx:"😊 Эмоции"},
  {en:"I'm tired.", ru:"Я устал.", ctx:"😊 Эмоции"},
  {en:"I'm hungry.", ru:"Я голоден.", ctx:"😊 Эмоции"},
  {en:"I'm bored.", ru:"Мне скучно.", ctx:"😊 Эмоции"},
  {en:"That's amazing!", ru:"Это удивительно!", ctx:"😊 Эмоции"},
  {en:"I love it!", ru:"Мне это очень нравится!", ctx:"😊 Эмоции"},
  {en:"That's funny!", ru:"Это смешно!", ctx:"😊 Эмоции"},
  // Прощание
  {en:"Goodbye!", ru:"До свидания!", ctx:"👋 Прощание"},
  {en:"See you later!", ru:"До встречи!", ctx:"👋 Прощание"},
  {en:"See you tomorrow!", ru:"Увидимся завтра!", ctx:"👋 Прощание"},
  {en:"Have a good day!", ru:"Хорошего дня!", ctx:"👋 Прощание"},
  {en:"Good night!", ru:"Спокойной ночи!", ctx:"👋 Прощание"},
  {en:"Take care!", ru:"Береги себя!", ctx:"👋 Прощание"},
];

const grammar = [
  {
    title:"Глагол TO BE", icon:"🔤",
    desc:"Самый важный глагол. Означает «быть/являться». Меняется в зависимости от лица.",
    rule:"I am | You are | He/She/It is | We/They are",
    examples:["I am a student. — Я студент.", "She is happy. — Она счастлива.", "They are friends. — Они друзья."],
    table:[["Местоимение","Глагол","Пример"],["I","am","I am tired"],["You","are","You are kind"],["He/She/It","is","He is tall"],["We/They","are","We are ready"]],
    tip:"❗ В вопросе: Am I? / Is she? / Are they?"
  },
  {
    title:"Present Simple", icon:"🕐",
    desc:"Привычные действия, факты, расписания. Для he/she/it добавляем -s или -es.",
    rule:"I/You/We/They + V | He/She/It + V+s",
    examples:["I work every day. — Я работаю каждый день.","She works at school. — Она работает в школе.","The sun rises in the east. — Солнце встаёт на востоке."],
    table:[["Лицо","Форма","Пример"],["I/You/We/They","работать → work","I work"],["He/She/It","work → works","She works"],["Вопрос","Do/Does + V?","Does she work?"],["Отрицание","don't / doesn't","I don't work"]],
    tip:"⏰ Сигналы: always, every day, usually, often, never"
  },
  {
    title:"Present Continuous", icon:"▶️",
    desc:"Действие происходит СЕЙЧАС, в данный момент.",
    rule:"am/is/are + V-ing",
    examples:["I am reading now. — Я читаю сейчас.","She is cooking dinner. — Она готовит ужин.","They are playing football. — Они играют в футбол."],
    table:[["Лицо","Форма","Пример"],["I","am + V-ing","I am eating"],["He/She/It","is + V-ing","She is sleeping"],["You/We/They","are + V-ing","They are running"]],
    tip:"⚡ Сигналы: now, right now, at the moment, look! listen!"
  },
  {
    title:"Past Simple", icon:"⏪",
    desc:"Завершённые действия в прошлом. Правильные глаголы: +ed. Неправильные — 2-я форма.",
    rule:"V+ed (правильные) | V2 (неправильные)",
    examples:["I worked yesterday. — Я работал вчера.","She went to school. — Она пошла в школу.","We played tennis. — Мы играли в теннис."],
    table:[["Тип","Пример","Прошедшее"],["Правильный","work","worked"],["Неправильный","go","went"],["Неправильный","see","saw"],["Неправильный","have","had"]],
    tip:"📅 Сигналы: yesterday, ago, last week, in 2020"
  },
  {
    title:"Future Simple", icon:"⏩",
    desc:"Будущие действия, обещания, предсказания.",
    rule:"will + V (для всех лиц)",
    examples:["I will call you. — Я позвоню тебе.","She will help us. — Она нам поможет.","It will rain tomorrow. — Завтра будет дождь."],
    table:[["Форма","Пример"],["Утверждение","I will go"],["Отрицание","I will not (won't) go"],["Вопрос","Will you go?"],["Краткий ответ","Yes, I will / No, I won't"]],
    tip:"🔮 Сигналы: tomorrow, next week, soon, in the future"
  },
  {
    title:"Артикли A / An / The", icon:"📖",
    desc:"Артикли ставятся перед существительными. В русском языке их нет!",
    rule:"A/An = неопределённый | The = определённый",
    examples:["I saw a cat. — Я видел кошку (какую-то).","The cat is black. — Кошка (та самая) чёрная.","An apple a day. — Яблоко в день."],
    table:[["Артикль","Когда использовать","Пример"],["A","перед согласным, впервые","a book, a car"],["An","перед гласным (a,e,i,o,u)","an apple, an egg"],["The","известный предмет","the sun, the moon"],["—","без артикля: имена, страны","Russia, Moscow, John"]],
    tip:"☀️ The sun, the moon, the sky — всегда с THE!"
  },
  {
    title:"Вопросы", icon:"❓",
    desc:"Как строить вопросы в английском. Порядок слов меняется!",
    rule:"Do/Does + S + V? | Did + S + V? | Is/Are + S?",
    examples:["Do you like music? — Ты любишь музыку?","Does she work here? — Она работает здесь?","Did you see him? — Ты его видел?"],
    table:[["Время","Вспом. глагол","Пример"],["Present Simple","do/does","Do you speak English?"],["Past Simple","did","Did she call?"],["Present Cont.","is/are","Are you coming?"],["Future","will","Will you help me?"]],
    tip:"❓ Вопросительное слово: What? Where? When? Who? Why? How?"
  },
  {
    title:"Отрицания", icon:"🚫",
    desc:"Как сказать «нет» по-английски. Нужен вспомогательный глагол + not.",
    rule:"don't / doesn't / didn't / won't + V",
    examples:["I don't know. — Я не знаю.","She doesn't like coffee. — Она не любит кофе.","I didn't go. — Я не пошёл."],
    table:[["Время","Форма","Пример"],["Present Simple (I/You)","don't + V","I don't want"],["Present Simple (He/She)","doesn't + V","She doesn't know"],["Past Simple","didn't + V","He didn't come"],["Future","won't + V","I won't go"]],
    tip:"✅ После don't/doesn't/didn't — всегда инфинитив (без -s и -ed)!"
  }
];

function openGrammar(idx) {
  const g = grammar[idx];
  document.getElementById('gramModalTitle').textContent = g.icon + ' ' + g.title;
  let html = `<div class="gram-desc">${g.desc}</div>`;
  html += `<div class="gram-rule">📌 Правило: ${g.rule}</div>`;
  if(g.table) {
    html += '<table class="gram-table">';
    g.table.forEach((row,i) => {
      html += i===0 ? '<tr>' + row.map(c=>`<th>${c}</th>`).join('') + '</tr>'
                    : '<tr>' + row.map(c=>`<td>${c}</td>`).join('') + '</tr>';
    });
    html += '</table>';
  }
  html += '<div style="margin-top:10px;"><div class="section-title" style="font-size:12px;">Примеры</div>';
  g.examples.forEach(ex => {
    html += `<div class="gram-example" style="margin-bottom:8px;">💬 ${ex}</div>`;
  });
  html += `</div><div class="gram-rule" style="border-color:var(--accent2);color:var(--accent2);margin-top:12px;">${g.tip}</div>`;
  document.getElementById('gramModalBody').innerHTML = html;
  document.getElementById('gramModal').classList.add('open');
}

function closeGramModal(e) {
  if(e.target === document.getElementById('gramModal'))
    document.getElementById('gramModal').classList.remove('open');
}

// =================== ENGLISH INIT ===================
function initEnglish() {
  // Alphabet
  const ag = document.getElementById('alphaGrid');
  ag.innerHTML = alphabet.map(a =>
    `<div class="alpha-card">
      <div class="big">${a.l}</div>
      <div class="small">${a.p}</div>
    </div>`
  ).join('');

  // Words
  const wl = document.getElementById('wordList');
  wl.innerHTML = words.map(w =>
    `<div class="word-card">
      <span class="flag">${w.cat}</span>
      <div><div class="en">${w.en}</div><div class="ru">${w.ru}</div><div class="tr">[${w.tr}]</div></div>
    </div>`
  ).join('');

  // Phrases
  const pl = document.getElementById('phraseList');
  pl.innerHTML = phrases.map(p =>
    `<div class="phrase-card">
      <div class="phrase-en">${p.en}</div>
      <div class="phrase-ru">${p.ru}</div>
      <div class="phrase-ctx">${p.ctx}</div>
    </div>`
  ).join('');

  // Grammar
  const gl = document.getElementById('grammarList');
  gl.innerHTML = grammar.map((g,i) =>
    `<div class="gram-card" onclick="openGrammar(${i})">
      <div class="gram-header">
        <div class="gram-icon">${g.icon}</div>
        <div class="gram-title">${g.title}</div>
      </div>
      <div class="gram-desc" style="margin-bottom:8px;">${g.desc}</div>
      <div class="gram-rule">${g.rule}</div>
      <div style="font-size:12px;color:var(--accent);margin-top:8px;">👆 Нажми для подробного урока</div>
    </div>`
  ).join('');
}

// Quiz logic
let quizWords = [...words], quizIdx = 0, quizCorrect = 0, quizTotal = 0, quizLocked = false;
function shuffleArr(a) { return [...a].sort(()=>Math.random()-.5); }

function nextQuiz() {
  if(quizLocked) return;
  quizIdx = Math.floor(Math.random() * words.length);
  const w = words[quizIdx];
  const wrongs = shuffleArr(words.filter(x=>x.en!==w.en)).slice(0,3).map(x=>x.ru);
  const opts = shuffleArr([w.ru, ...wrongs]);
  document.getElementById('quizQ').textContent = `"${w.en}" — что значит?`;
  const optsEl = document.getElementById('quizOpts');
  optsEl.innerHTML = opts.map(o =>
    `<div class="quiz-opt" onclick="checkQuiz(this,'${o}','${w.ru}')">${o}</div>`
  ).join('');
  quizLocked = false;
}
function checkQuiz(el, chosen, correct) {
  if(quizLocked) return;
  quizLocked = true; quizTotal++;
  if(chosen === correct) { el.classList.add('correct'); quizCorrect++; }
  else {
    el.classList.add('wrong');
    document.querySelectorAll('.quiz-opt').forEach(o => { if(o.textContent===correct) o.classList.add('correct'); });
  }
  document.getElementById('quizScore').textContent = `Счёт: ${quizCorrect} / ${quizTotal}`;
  document.getElementById('eng-score').textContent = (quizCorrect*10) + ' XP';
  setTimeout(()=>{ quizLocked=false; nextQuiz(); }, 1200);
}

function engTab(name, el) {
  document.querySelectorAll('.eng-panel').forEach(p=>p.classList.remove('active'));
  document.querySelectorAll('.eng-tab').forEach(t=>t.classList.remove('active'));
  document.getElementById('eng-'+name).classList.add('active');
  el.classList.add('active');
}

function filterPhrases(cat, el) {
  document.querySelectorAll('#phraseFilter .eng-tab').forEach(t=>t.classList.remove('active'));
  el.classList.add('active');
  const filtered = cat === 'all' ? phrases : phrases.filter(p => p.ctx === cat);
  document.getElementById('phraseList').innerHTML = filtered.map(p =>
    `<div class="phrase-card">
      <div class="phrase-en">${p.en}</div>
      <div class="phrase-ru">${p.ru}</div>
      <div class="phrase-ctx">${p.ctx}</div>
    </div>`
  ).join('');
}

// =================== MATH DATA ===================
const mathTopics = [
  {icon:'🔢',title:'Числа и счёт',lvl:'easy',desc:'Натуральные, целые, дроби, проценты'},
  {icon:'➕',title:'Арифметика',lvl:'easy',desc:'Сложение, вычитание, умножение, деление'},
  {icon:'🔡',title:'Алгебра. Начало',lvl:'easy',desc:'Переменные, выражения, уравнения'},
  {icon:'📐',title:'Геометрия',lvl:'med',desc:'Фигуры, периметр, площадь, объём'},
  {icon:'📊',title:'Функции и графики',lvl:'med',desc:'Линейная, квадратная, показательная'},
  {icon:'🔄',title:'Уравнения',lvl:'med',desc:'Линейные, квадратные, системы'},
  {icon:'∫',title:'Производная',lvl:'hard',desc:'Понятие, правила дифференцирования'},
  {icon:'∑',title:'Интеграл',lvl:'hard',desc:'Неопределённый и определённый интеграл'},
  {icon:'📉',title:'Вероятность',lvl:'med',desc:'Комбинаторика, статистика'},
  {icon:'🧮',title:'Тригонометрия',lvl:'hard',desc:'sin, cos, tan и формулы'}
];

function initMath() {
  const ml = document.getElementById('mathTopicList');
  ml.innerHTML = mathTopics.map(t =>
    `<div class="math-topic" onclick="askAIAboutTopic('${t.title}')">
      <div class="t-icon">${t.icon}</div>
      <span class="pill ${t.lvl}">${t.lvl==='easy'?'Начало':t.lvl==='med'?'Средний':'Сложный'}</span>
      <div class="t-title" style="margin-top:6px;">${t.title}</div>
      <div class="t-desc">${t.desc}</div>
    </div>`
  ).join('');
}

function askAIAboutTopic(topic) {
  goTo('ai');
  document.getElementById('aiInput').value = `Объясни тему "${topic}" с нуля, понятно, с примерами`;
  sendAI();
}

function mathTab(name, el) {
  document.querySelectorAll('.math-panel').forEach(p=>p.classList.remove('active'));
  document.querySelectorAll('.math-tab').forEach(t=>t.classList.remove('active'));
  document.getElementById('math-'+name).classList.add('active');
  el.classList.add('active');
  if(name==='graph') setTimeout(drawGraph, 100);
}

// =================== MATH SOLVER ===================
function solveMath() {
  const prob = document.getElementById('mathProblem').value.trim();
  if(!prob) return;
  document.getElementById('solutionBox').innerHTML = '<div class="solution-box"><div class="typing-dots"><span></span><span></span><span></span></div></div>';
  callAIAPI('Ты математический решатель. Реши задачу пошагово, каждый шаг с новой строки, начиная с "Шаг 1:", "Шаг 2:" и т.д. В конце напиши "Ответ:". Задача: '+prob)
    .then(result => {
      const steps = result.split('\n').filter(s=>s.trim());
      document.getElementById('solutionBox').innerHTML = `
        <div class="solution-box">
          ${steps.map((s,i)=>`<div class="solution-step">${s}</div>`).join('')}
        </div>`;
    }).catch(()=> {
      document.getElementById('solutionBox').innerHTML = '<div class="solution-box"><div class="solution-step">Ошибка. Попробуйте снова.</div></div>';
    });
}

// =================== GRAPH ===================
function drawGraph() {
  const canvas = document.getElementById('graphCanvas');
  const ctx = canvas.getContext('2d');
  const fn = document.getElementById('graphFn').value || 'x*x';
  const W = canvas.width, H = canvas.height;
  const cx = W/2, cy = H/2, scale = 30;

  ctx.clearRect(0,0,W,H);
  ctx.fillStyle = '#12121a'; ctx.fillRect(0,0,W,H);

  // Grid
  ctx.strokeStyle = '#2a2a40'; ctx.lineWidth = 1;
  for(let x=-cx; x<=cx; x+=scale){ ctx.beginPath(); ctx.moveTo(cx+x,0); ctx.lineTo(cx+x,H); ctx.stroke(); }
  for(let y=-cy; y<=cy; y+=scale){ ctx.beginPath(); ctx.moveTo(0,cy+y); ctx.lineTo(W,cy+y); ctx.stroke(); }

  // Axes
  ctx.strokeStyle = '#5a5a80'; ctx.lineWidth = 1.5;
  ctx.beginPath(); ctx.moveTo(0,cy); ctx.lineTo(W,cy); ctx.stroke();
  ctx.beginPath(); ctx.moveTo(cx,0); ctx.lineTo(cx,H); ctx.stroke();

  // Labels
  ctx.fillStyle = '#7070a0'; ctx.font = '10px Inter'; ctx.textAlign = 'center';
  for(let i=-10;i<=10;i++){
    if(i===0) continue;
    const sx = cx + i*scale, sy = cy + i*scale;
    if(sx>0&&sx<W) ctx.fillText(i, sx, cy+12);
    if(sy>0&&sy<H){ ctx.textAlign='right'; ctx.fillText(-i, cx-4, sy+3); ctx.textAlign='center'; }
  }

  // Function
  const safe = fn.replace(/\^/g,'**').replace(/sin/g,'Math.sin').replace(/cos/g,'Math.cos')
    .replace(/tan/g,'Math.tan').replace(/sqrt/g,'Math.sqrt').replace(/abs/g,'Math.abs')
    .replace(/log/g,'Math.log').replace(/exp/g,'Math.exp').replace(/pi/g,'Math.PI');
  const evalFn = x => { try { return eval(safe.replace(/x/g,'('+x+')')); } catch(e){ return NaN; } };

  ctx.strokeStyle = '#6c63ff'; ctx.lineWidth = 2.5; ctx.beginPath();
  let started = false;
  for(let px=0; px<=W; px+=0.5){
    const x = (px-cx)/scale;
    const y = evalFn(x);
    if(!isFinite(y)||Math.abs(y)>1000){ started=false; continue; }
    const py = cy - y*scale;
    if(!started){ ctx.moveTo(px,py); started=true; } else ctx.lineTo(px,py);
  }
  ctx.stroke();

  ctx.fillStyle='#6c63ff'; ctx.font='12px Inter'; ctx.textAlign='left';
  ctx.fillText('y = '+fn, 10, 20);
}

// =================== AI CHAT ===================
let pendingImage = null;

function autoResize(el) {
  el.style.height = 'auto';
  el.style.height = Math.min(el.scrollHeight, 120) + 'px';
}

function aiEnter(e) {
  if(e.key==='Enter'&&!e.shiftKey){ e.preventDefault(); sendAI(); }
}

async function sendAI() {
  const input = document.getElementById('aiInput');
  const text = input.value.trim();
  if(!text && !pendingImage) return;

  addMsg(text || '📷 Задача с фото', 'user');
  input.value = ''; input.style.height='auto';

  let content;
  if(pendingImage) {
    const base64 = pendingImage.split(',')[1];
    const mime = pendingImage.split(';')[0].split(':')[1];
    content = [
      { type:'image', source:{ type:'base64', media_type: mime, data: base64 } },
      { type:'text', text: text || 'Реши задачу на этом фото. Объясни каждый шаг подробно. Если математика — пошагово. Если английский — переведи и объясни.' }
    ];
    clearPhoto();
  } else {
    content = text;
  }

  // Create streaming bot message
  const chat = document.getElementById('aiChat');
  const botDiv = document.createElement('div');
  botDiv.className = 'msg bot';
  botDiv.innerHTML = '<div class="typing-dots"><span></span><span></span><span></span></div>';
  chat.appendChild(botDiv);
  chat.scrollTop = chat.scrollHeight;

  try {
    await streamAI(content, botDiv);
  } catch(e) {
    botDiv.innerHTML = '⚠️ Ошибка. Проверьте интернет.';
  }
}

async function streamAI(userContent, targetDiv) {
  const sys = `Ты умный образовательный ИИ-помощник. Отвечай на русском языке. Будь краток и точен.
Если вопрос по математике — решай пошагово, показывай все действия.
Если вопрос по английскому — объясняй с примерами и переводами.
Если на фото задача — реши точно и подробно. Никогда не ошибайся.`;

  const response = await fetch('https://api.anthropic.com/v1/messages', {
    method:'POST',
    headers:{ 'Content-Type':'application/json' },
    body: JSON.stringify({
      model:'claude-haiku-4-5-20251001',
      max_tokens:800,
      stream: true,
      system: sys,
      messages:[{ role:'user', content: userContent }]
    })
  });

  if(!response.ok) {
    const err = await response.json();
    throw new Error(err.error?.message || 'API error');
  }

  const reader = response.body.getReader();
  const decoder = new TextDecoder();
  let fullText = '';
  targetDiv.innerHTML = '';

  while(true) {
    const {done, value} = await reader.read();
    if(done) break;
    const chunk = decoder.decode(value);
    const lines = chunk.split('\n');
    for(const line of lines) {
      if(!line.startsWith('data: ')) continue;
      const data = line.slice(6).trim();
      if(data === '[DONE]') break;
      try {
        const parsed = JSON.parse(data);
        if(parsed.type === 'content_block_delta' && parsed.delta?.text) {
          fullText += parsed.delta.text;
          targetDiv.innerHTML = fullText
            .replace(/\n/g,'<br>')
            .replace(/\*\*(.*?)\*\*/g,'<strong>$1</strong>')
            .replace(/`(.*?)`/g,'<code style="background:#0a0a0f;padding:2px 6px;border-radius:4px;">$1</code>');
          document.getElementById('aiChat').scrollTop = 9999;
        }
      } catch(_) {}
    }
  }
}

async function callAIAPI(systemText, userContent) {
  // Used for math solver (non-streaming)
  const sys = systemText || `Ты математический решатель. Решай задачи пошагово. Только русский язык. Кратко и точно.`;
  const response = await fetch('https://api.anthropic.com/v1/messages', {
    method:'POST',
    headers:{ 'Content-Type':'application/json' },
    body: JSON.stringify({
      model:'claude-haiku-4-5-20251001',
      max_tokens:600,
      system: sys,
      messages:[{ role:'user', content: userContent || systemText }]
    })
  });
  const data = await response.json();
  if(!response.ok) throw new Error(data.error?.message || 'API error');
  return data.content.map(b=>b.text||'').join('\n');
}

function addMsg(text, role) {
  const chat = document.getElementById('aiChat');
  const div = document.createElement('div');
  div.className = 'msg ' + role;
  div.innerHTML = text.replace(/\n/g,'<br>').replace(/\*\*(.*?)\*\*/g,'<strong>$1</strong>');
  chat.appendChild(div);
  chat.scrollTop = chat.scrollHeight;
  return div;
}

function addTyping() {
  const chat = document.getElementById('aiChat');
  const div = document.createElement('div');
  div.className = 'msg bot';
  div.id = 'typing-' + Date.now();
  div.innerHTML = '<div class="typing-dots"><span></span><span></span><span></span></div>';
  chat.appendChild(div);
  chat.scrollTop = chat.scrollHeight;
  return div.id;
}
function removeTyping(id) {
  const el = document.getElementById(id);
  if(el) el.remove();
}

// =================== PHOTO ===================
function handleFile(e) {
  const file = e.target.files[0];
  if(!file) return;
  const reader = new FileReader();
  reader.onload = ev => showPhoto(ev.target.result);
  reader.readAsDataURL(file);
  e.target.value = '';
}

function showPhoto(dataUrl) {
  pendingImage = dataUrl;
  const wrap = document.getElementById('photoPreviewWrap');
  wrap.innerHTML = `
    <div class="photo-preview">
      <img src="${dataUrl}" alt="Фото задачи"/>
      <button class="remove-photo" onclick="clearPhoto()">✕</button>
    </div>`;
}

function clearPhoto() {
  pendingImage = null;
  document.getElementById('photoPreviewWrap').innerHTML = '';
}

// =================== CAMERA ===================
let camStream = null;
async function openCamera() {
  const overlay = document.getElementById('camOverlay');
  overlay.classList.add('open');
  try {
    camStream = await navigator.mediaDevices.getUserMedia({ video:{ facingMode:'environment' }, audio:false });
    document.getElementById('camVideo').srcObject = camStream;
  } catch(e) {
    // fallback to front camera
    try {
      camStream = await navigator.mediaDevices.getUserMedia({ video:true, audio:false });
      document.getElementById('camVideo').srcObject = camStream;
    } catch(e2) {
      closeCamera();
      alert('Нет доступа к камере. Разрешите доступ в браузере.');
    }
  }
}

function closeCamera() {
  if(camStream) { camStream.getTracks().forEach(t=>t.stop()); camStream=null; }
  document.getElementById('camOverlay').classList.remove('open');
}

function shootPhoto() {
  const video = document.getElementById('camVideo');
  const canvas = document.getElementById('camCanvas');
  canvas.width = video.videoWidth || 640;
  canvas.height = video.videoHeight || 480;
  canvas.getContext('2d').drawImage(video,0,0);
  const dataUrl = canvas.toDataURL('image/jpeg', 0.85);
  closeCamera();
  showPhoto(dataUrl);
}

// =================== INIT ===================
initEnglish();
initMath();
drawGraph();
</script>
</body>
</html>

