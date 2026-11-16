# mousebute.github.io

<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=yes">
  <title>Квест для Дариши ❤️</title>

  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;500;600&family=Playfair+Display:ital,wght@0,500;0,600;0,700;1,500&display=swap" rel="stylesheet">

  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }

    :root {
      --pink-50: #fff5f9;
      --pink-100: #ffe6f0;
      --pink-200: #ffd0e0;
      --pink-300: #fbb6c4;
      --pink-400: #f58ea6;
      --pink-500: #e9678a;
      --pink-600: #c94f6e;
      --plum: #4a2c3a;
      --gold: #e8c48a;
      --green-border: #7fc97f;
      --green-text: #2e6b2e;
      --red-border: #e67e7e;
      --red-text: #a33a3a;
    }

    body {
      font-family: 'Montserrat', sans-serif;
      background: linear-gradient(160deg, #ffe6f0 0%, #fff5f9 45%, #ffffff 100%);
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 24px;
      position: relative;
      overflow-x: hidden;
      color: var(--plum);
      -webkit-font-smoothing: antialiased;
    }

    /* ===== ПАДАЮЩИЕ СЕРДЕЧКИ (Оптимизированные) ===== */
    .hearts-background {
      position: fixed; inset: 0;
      pointer-events: none; z-index: 0; overflow: hidden;
    }
    .heart {
      position: absolute; top: -10vh;
      animation-name: fall; animation-timing-function: linear;
      animation-iteration-count: infinite;
      user-select: none; opacity: 0.6;
      /* Убрали тяжелый filter: drop-shadow */
    }
    .heart:nth-child(1)  { left: 3%;  font-size: 12px; animation-duration: 7s;   animation-delay: 0s;    color: #ffc2d1; }
    .heart:nth-child(2)  { left: 15%; font-size: 14px; animation-duration: 9s;   animation-delay: 2.5s;  color: #ffd9e2; }
    .heart:nth-child(3)  { left: 28%; font-size: 10px; animation-duration: 6s;   animation-delay: 5s;    color: #ffb3c6; }
    .heart:nth-child(4)  { left: 40%; font-size: 13px; animation-duration: 8s;   animation-delay: 1s;    color: #ffc2d1; }
    .heart:nth-child(5)  { left: 55%; font-size: 11px; animation-duration: 7.5s; animation-delay: 3.5s;  color: #ffd9e2; }
    .heart:nth-child(6)  { left: 68%; font-size: 14px; animation-duration: 9.5s; animation-delay: 0.5s;  color: #ffb3c6; }
    .heart:nth-child(7)  { left: 80%; font-size: 12px; animation-duration: 6.5s; animation-delay: 4s;    color: #ffc2d1; }
    .heart:nth-child(8)  { left: 92%; font-size: 10px; animation-duration: 8.5s; animation-delay: 2s;    color: #ffd9e2; }
    .heart:nth-child(9)  { left: 8%;  font-size: 22px; animation-duration: 11s;   animation-delay: 0s;    color: #ffb3c6; }
    .heart:nth-child(10) { left: 22%; font-size: 18px; animation-duration: 13s;   animation-delay: 1.5s;  color: #ffc2d1; }
    .heart:nth-child(11) { left: 42%; font-size: 24px; animation-duration: 10s;   animation-delay: 3s;    color: #ffa5b9; }
    .heart:nth-child(12) { left: 62%; font-size: 18px; animation-duration: 12s;   animation-delay: 0.8s;  color: #ffd9e2; }
    .heart:nth-child(13) { left: 78%; font-size: 26px; animation-duration: 14s;   animation-delay: 2.2s;  color: #ffb3c6; }
    .heart:nth-child(14) { left: 88%; font-size: 20px; animation-duration: 11.5s; animation-delay: 4.2s;  color: #ffc2d1; }

    @keyframes fall {
      0%   { transform: translateY(-10vh) rotate(0deg); opacity: 0.7; }
      100% { transform: translateY(110vh) rotate(360deg); opacity: 0; }
    }

    /* =========================================
       КОНВЕРТ И АНИМАЦИЯ
       ========================================= */
    
    .envelope-wrapper {
      position: relative;
      z-index: 1;
      width: 100%; 
      max-width: 800px;
      perspective: 1500px;
      margin-top: 30px;
    }

    .envelope-back {
      position: absolute;
      inset: 0;
      background: linear-gradient(160deg, #f2c1d1 0%, #e69ab1 100%);
      border-radius: 16px;
      border: 1px solid rgba(255, 255, 255, 0.6);
      z-index: 1;
    }

    .envelope-flap {
      position: absolute;
      top: -1px; left: -1px; right: -1px;
      height: 140px; 
      background: linear-gradient(180deg, #fde6ee 0%, #f2c1d1 100%);
      clip-path: polygon(0 0, 100% 0, 50% 100%);
      transform-origin: top center;
      transition: transform 0.5s ease, z-index 0s 0.25s;
      z-index: 10;
      border-radius: 16px 16px 0 0;
    }

    /* Состояние "Открыто" */
    .envelope-wrapper.opening .envelope-flap {
      transform: rotateX(-180deg);
      z-index: 1; 
    }

    .envelope-front {
      position: absolute;
      bottom: -1px; left: -1px; right: -1px;
      height: 80px;
      background: linear-gradient(0deg, #e69ab1 0%, #f2c1d1 100%);
      clip-path: polygon(0 100%, 100% 100%, 100% 40%, 50% 0, 0 40%);
      z-index: 5;
      border-radius: 0 0 16px 16px;
      pointer-events: none;
    }

    .app-container {
      position: relative;
      z-index: 2;
      background: #ffffff;
      margin: 20px 24px 80px 24px;
      border-radius: 12px;
      padding: 32px 36px;
      box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
      /* Убрали transition transform для оптимизации */
    }

    /* ===== ЭКРАНЫ ===== */
    .screen { display: none; opacity: 0; transition: opacity 0.4s ease; }
    .screen.active { display: block; opacity: 1; }

    /* ===== ТИПОГРАФИКА ===== */
    h1, h2 {
      font-family: 'Playfair Display', serif;
      font-weight: 700;
      color: var(--pink-600);
      letter-spacing: 0.3px;
    }
    h1 {
      font-size: 2.4rem; line-height: 1.25; margin-bottom: 20px;
      background: linear-gradient(135deg, #c94f6e 0%, #e9678a 100%);
      -webkit-background-clip: text; -webkit-text-fill-color: transparent;
      background-clip: text;
    }
    h2 { font-size: 1.9rem; margin-bottom: 16px; }
    p { font-size: 1.08rem; line-height: 1.7; color: #5a3b47; margin-bottom: 16px; }
    .romantic-text {
      font-family: 'Playfair Display', serif; font-style: italic;
      color: var(--pink-600); font-size: 1.25rem;
    }

    /* ===== КНОПКИ ===== */
    .btn {
      display: inline-block;
      background: linear-gradient(135deg, #f9a7b8 0%, #f37b95 100%);
      color: #fff; font-family: 'Montserrat', sans-serif; font-weight: 600;
      font-size: 1.15rem; padding: 18px 44px;
      border: none; border-radius: 50px; cursor: pointer;
      box-shadow: 0 8px 20px rgba(245, 142, 166, 0.35);
      transition: all 0.3s ease;
      text-align: center; letter-spacing: 0.5px;
      margin-top: 24px; width: 100%; max-width: 320px;
    }
    .btn:hover {
      box-shadow: 0 12px 25px rgba(245, 142, 166, 0.5);
      transform: translateY(-2px);
    }
    
    .next-btn { display: none; }
    .next-btn.visible { display: inline-block; }
    .next-btn.highlight {
      box-shadow:
        0 0 0 4px rgba(255, 255, 255, 0.9),
        0 0 0 8px rgba(243, 123, 149, 0.15),
        0 12px 32px rgba(243, 123, 149, 0.6);
      animation: pulseGlow 1.6s ease-in-out infinite alternate;
    }
    @keyframes pulseGlow {
      0%   { box-shadow: 0 0 0 4px rgba(255,255,255,0.9), 0 0 0 8px rgba(243,123,149,0.15), 0 12px 28px rgba(243,123,149,0.5); }
      100% { box-shadow: 0 0 0 4px rgba(255,255,255,0.95), 0 0 0 12px rgba(243,123,149,0.1), 0 16px 40px rgba(243,123,149,0.85); }
    }

    /* ===== ВАРИАНТЫ ===== */
    .options { display: flex; flex-direction: column; gap: 14px; margin: 24px 0 16px; }
    .option {
      position: relative;
      background: #fff;
      border: 2px solid #fbd0db;
      border-radius: 26px;
      padding: 18px 24px;
      font-size: 1.1rem; font-weight: 500; color: var(--plum);
      cursor: pointer; transition: all 0.25s ease;
      text-align: left; font-family: 'Montserrat', sans-serif;
      box-shadow: 0 3px 10px rgba(255, 182, 193, 0.1);
      display: flex; align-items: center; gap: 14px;
      overflow: hidden;
    }
    .option:hover {
      background: #fff7fa; border-color: #f7a1b5;
      transform: translateY(-2px);
      box-shadow: 0 8px 18px rgba(245, 142, 166, 0.2);
    }

    /* Индикатор-кружок */
    .option::before {
      content: ''; display: inline-block;
      width: 24px; height: 24px; border-radius: 50%;
      border: 2px solid #fbd0db; background: #fff;
      flex-shrink: 0; transition: all 0.25s ease;
      position: relative; z-index: 1;
    }

    /* Выбранный */
    .option.selected {
      background: #ffe9f0; border-color: var(--pink-400);
      box-shadow: 0 0 0 3px rgba(243, 123, 149, 0.18), 0 8px 20px rgba(243, 123, 149, 0.25);
    }
    .option.selected::before { background: var(--pink-400); border-color: var(--pink-400); box-shadow: inset 0 0 0 4px #fff; }

    /* ✅ Правильный ответ */
    .option.correct {
      background: linear-gradient(135deg, #eafbe9 0%, #d7f2d6 100%);
      border-color: var(--green-border);
      color: var(--green-text);
      box-shadow: 0 0 0 3px rgba(127, 201, 127, 0.2), 0 10px 28px rgba(127, 201, 127, 0.35);
      animation: correctPop 0.4s ease;
    }
    .option.correct::before {
      background: var(--green-border); border-color: var(--green-border);
      box-shadow: inset 0 0 0 4px #fff;
    }
    .option.correct::after {
      content: '✓';
      position: absolute; right: 22px; top: 50%;
      transform: translateY(-50%);
      font-size: 1.2rem; font-weight: 700; color: var(--green-border);
    }
    @keyframes correctPop {
      0% { transform: scale(1); }
      50% { transform: scale(1.02); }
      100% { transform: scale(1); }
    }

    /* ❌ Неправильный ответ */
    .option.wrong {
      background: linear-gradient(135deg, #fff0f0 0%, #ffe0e0 100%);
      border-color: var(--red-border);
      color: var(--red-text);
      box-shadow: 0 0 0 3px rgba(230, 126, 126, 0.2), 0 10px 28px rgba(230, 126, 126, 0.3);
      animation: shake 0.4s ease;
    }
    .option.wrong::before {
      background: var(--red-border); border-color: var(--red-border);
      box-shadow: inset 0 0 0 4px #fff;
    }
    .option.wrong::after {
      content: '✕';
      position: absolute; right: 22px; top: 50%;
      transform: translateY(-50%);
      font-size: 1.1rem; font-weight: 700; color: var(--red-border);
    }
    @keyframes shake {
      0%, 100% { transform: translateX(0); }
      25%      { transform: translateX(-4px); }
      75%      { transform: translateX(4px); }
    }

    /* ===== СООБЩЕНИЕ ===== */
    .feedback {
      font-size: 1rem; color: var(--red-text);
      min-height: 28px; margin-top: 8px;
      font-weight: 500; font-style: italic;
      transition: color 0.3s ease;
    }

    /* ===== ПРОГРЕСС ===== */
    .progress { display: flex; justify-content: center; gap: 10px; margin-bottom: 24px; }
    .progress-dot {
      width: 10px; height: 10px; border-radius: 50%;
      background: #fbd0db; transition: all 0.3s ease;
    }
    .progress-dot.active {
      background: var(--pink-400);
      box-shadow: 0 0 0 4px rgba(243, 123, 149, 0.2);
      transform: scale(1.3);
    }
    .progress-dot.completed { background: var(--green-border); }

    /* ===== ФИНАЛ ===== */
    .final-screen { text-align: center; position: relative; }
    .final-gift {
      font-size: 4.5rem; margin: 24px 0; display: inline-block;
      animation: bounceGift 2s ease-in-out infinite;
    }
    @keyframes bounceGift { 0%, 100% { transform: translateY(0); } 50% { transform: translateY(-10px); } }
    .final-message {
      font-family: 'Playfair Display', serif; font-size: 1.35rem; line-height: 1.75;
      color: #a63a55; background: linear-gradient(135deg, rgba(255,240,245,0.9) 0%, rgba(255,225,235,0.9) 100%);
      padding: 32px 28px; border-radius: 32px;
      border: 1.5px dashed #f7a1b5; margin: 24px 0;
    }

    /* ===== ВЗРЫВ СЕРДЕЧЕК ===== */
    .explosion-heart {
      position: fixed; font-size: 28px; pointer-events: none; z-index: 9999;
      animation: explode 1.2s ease-out forwards;
      opacity: 1;
    }
    @keyframes explode {
      0%   { transform: translate(0, 0) scale(1) rotate(0deg); opacity: 1; }
      100% { transform: translate(var(--tx), var(--ty)) scale(0.2) rotate(180deg); opacity: 0; }
    }

    /* ===== МОБИЛЬНАЯ ОПТИМИЗАЦИЯ ===== */
    @media (max-width: 600px) {
      body { padding: 16px; }
      
      /* Скрываем половину сердечек */
      .heart:nth-child(n+11) { display: none; }
      
      /* Упрощаем тени */
      .app-container {
        box-shadow: 0 6px 18px rgba(0, 0, 0, 0.08);
        padding: 24px 20px; 
        margin: 16px 16px 70px 16px;
      }
      
      .envelope-flap { height: 100px; }
      .envelope-front { height: 60px; }
      
      .btn { padding: 16px 28px; font-size: 1rem; }
      .option { padding: 16px 18px; font-size: 1rem; border-radius: 22px; }
      .option.correct::after, .option.wrong::after { right: 16px; }
      
      h1 { font-size: 1.85rem; }
      h2 { font-size: 1.55rem; }
      
      .final-message { font-size: 1.1rem; padding: 24px 18px; }
      .final-gift { font-size: 3.2rem; }
      
      /* Отключаем hover-эффекты, чтобы не дергалось */
      .btn:hover, .option:hover {
        transform: none;
        box-shadow: none;
      }
    }
    @media (min-width: 768px) {
      .app-container { padding: 44px 56px; }
    }
    
    /* Уважаем системные настройки */
    @media (prefers-reduced-motion: reduce) {
      .heart { display: none; }
      .envelope-flap, .app-container, .option, .btn { transition: none !important; }
      .explosion-heart { display: none; }
      .btn, .option, .progress-dot { transition: none; }
      .option.correct, .option.wrong { animation: none; }
      .final-gift { animation: none; }
    }
  </style>
</head>
<body>

  <!-- ФОН (14 сердечек для телефона, остальные скрыты через CSS) -->
  <div class="hearts-background" aria-hidden="true">
    <div class="heart">❤️</div><div class="heart">💗</div><div class="heart">💖</div><div class="heart">💕</div>
    <div class="heart">❤️</div><div class="heart">💗</div><div class="heart">💖</div><div class="heart">💕</div>
    <div class="heart">❤️</div><div class="heart">💗</div><div class="heart">💖</div><div class="heart">💕</div>
    <div class="heart">❤️</div><div class="heart">💗</div>
  </div>

  <!-- ОБЕРТКА КОНВЕРТА -->
  <div class="envelope-wrapper" id="envelopeWrapper">
    
    <!-- Задник конверта -->
    <div class="envelope-back"></div>

    <!-- Клапан конверта (верхний) -->
    <div class="envelope-flap"></div>

    <!-- Само письмо -->
    <div class="app-container" id="appContainer">

      <!-- СТАРТ -->
      <div class="screen active" id="screen-start">
        <h1>Привет, Дариша! ❤️</h1>
        <p>Я приготовил для тебя маленький романтичный квест. <br>Ты готова? 😊</p>
        <p class="romantic-text">Четыре вопроса, четыре шага — и тебя ждёт сюрприз...</p>
        <button class="btn" id="startBtn">Начать квест</button>
      </div>

      <!-- ВОПРОС 1 -->
      <div class="screen" id="screen-q1">
        <div class="progress">
          <div class="progress-dot active"></div><div class="progress-dot"></div>
          <div class="progress-dot"></div><div class="progress-dot"></div>
        </div>
        <h2>Вопрос 1 из 4</h2>
        <p>Когда мы начали встречаться?</p>
        <div class="options" id="options-q1">
          <div class="option" data-correct="true">05.05.2025</div>
          <div class="option" data-correct="false">06.07.2025</div>
          <div class="option" data-correct="false">21.04.2025</div>
        </div>
        <div class="feedback" id="feedback-q1"></div>
        <button class="btn next-btn" id="next-q1">Далее</button>
      </div>

      <!-- ВОПРОС 2 -->
      <div class="screen" id="screen-q2">
        <div class="progress">
          <div class="progress-dot completed"></div><div class="progress-dot active"></div>
          <div class="progress-dot"></div><div class="progress-dot"></div>
        </div>
        <h2>Вопрос 2 из 4</h2>
        <p>Где была наша первая прогулка?</p>
        <div class="options" id="options-q2">
          <div class="option" data-correct="false">В парке</div>
          <div class="option" data-correct="true">В лесу</div>
          <div class="option" data-correct="false">Во дворе</div>
        </div>
        <div class="feedback" id="feedback-q2"></div>
        <button class="btn next-btn" id="next-q2">Далее</button>
      </div>

      <!-- ВОПРОС 3 -->
      <div class="screen" id="screen-q3">
        <div class="progress">
          <div class="progress-dot completed"></div><div class="progress-dot completed"></div>
          <div class="progress-dot active"></div><div class="progress-dot"></div>
        </div>
        <h2>Вопрос 3 из 4</h2>
        <p>Что мы любили раньше пить?</p>
        <div class="options" id="options-q3">
          <div class="option" data-correct="false">Колу</div>
          <div class="option" data-correct="false">Сок</div>
          <div class="option" data-correct="true">Череп</div>
        </div>
        <div class="feedback" id="feedback-q3"></div>
        <button class="btn next-btn" id="next-q3">Далее</button>
      </div>

      <!-- ВОПРОС 4 -->
      <div class="screen" id="screen-q4">
        <div class="progress">
          <div class="progress-dot completed"></div><div class="progress-dot completed"></div>
          <div class="progress-dot completed"></div><div class="progress-dot active"></div>
        </div>
        <h2>Вопрос 4 из 4</h2>
        <p>Поцелуешь Масю 10 раз?</p>
        <div class="options" id="options-q4">
          <div class="option" data-correct="true">Да :)</div>
          <div class="option" data-correct="false">Нет :(</div>
        </div>
        <div class="feedback" id="feedback-q4"></div>
        <button class="btn next-btn" id="next-q4">Завершить</button>
      </div>

      <!-- ФИНАЛ -->
      <div class="screen final-screen" id="screen-final">
        <h1>Ты справилась! 🎉</h1>
        <div class="final-gift">🎁</div>
        <div class="final-message">
          Твой подарок ждет тебя там, где ты обычно стоишь в ступоре, думая, что надеть.<br>
          Найди коробку с бантиком! 🎀
        </div>
        <p class="romantic-text">Я тебя очень люблю ❤️</p>
      </div>

    </div>

    <!-- Нижний карман конверта -->
    <div class="envelope-front"></div>
  </div>

  <script>
    (function() {
      'use strict';

      const CONFIG = {
        wrongMessages: {
          q1: 'Подумай еще, солнышко! 😉',
          q2: 'Не угадала, милая! Попробуй снова 💭',
          q3: 'Почти! Но подумай ещё разок 😘',
          q4: 'Ну-у-у, попробуй ещё раз! 😏'
        },
        correctMessage: 'Верно! Ты знаешь меня лучше всех 💖',
        nextButtonDelay: 450,
        transitionDelay: 350
      };

      const screens = {
        start: document.getElementById('screen-start'),
        q1: document.getElementById('screen-q1'),
        q2: document.getElementById('screen-q2'),
        q3: document.getElementById('screen-q3'),
        q4: document.getElementById('screen-q4'),
        final: document.getElementById('screen-final')
      };

      const wrapper = document.getElementById('envelopeWrapper');

      // Инициализация
      window.addEventListener('DOMContentLoaded', () => {
        setTimeout(() => wrapper.classList.add('opening'), 100);
      });

      function goToScreen(from, to) {
        const fromEl = screens[from];
        const toEl = screens[to];
        if (!fromEl || !toEl) return;

        // Закрываем конверт
        wrapper.classList.remove('opening');
        
        fromEl.style.opacity = '0';

        setTimeout(() => {
          fromEl.classList.remove('active');
          fromEl.style.opacity = '1';

          toEl.classList.add('active');
          toEl.style.opacity = '0';

          requestAnimationFrame(() => {
            requestAnimationFrame(() => {
              toEl.style.opacity = '1';
              // Открываем конверт
              wrapper.classList.add('opening');
            });
          });
        }, CONFIG.transitionDelay);
      }

      document.getElementById('startBtn').addEventListener('click', () => {
        goToScreen('start', 'q1');
      });

      ['q1', 'q2', 'q3', 'q4'].forEach(id => {
        setupQuestion(id, 'next-' + id, 'feedback-' + id);
      });

      function setupQuestion(questionId, nextBtnId, feedbackId) {
        const screen = screens[questionId];
        if (!screen) return;

        const optionsContainer = screen.querySelector('.options');
        const nextBtn = document.getElementById(nextBtnId);
        const feedbackEl = document.getElementById(feedbackId);
        if (!optionsContainer || !nextBtn || !feedbackEl) return;

        const options = optionsContainer.querySelectorAll('.option');

        options.forEach(option => {
          option.addEventListener('click', function() {
            if (nextBtn.classList.contains('highlight')) return;

            const isCorrect = this.dataset.correct === 'true';

            options.forEach(opt => opt.classList.remove('selected', 'correct', 'wrong'));
            this.classList.add('selected');

            // Принудительный reflow для перезапуска анимации
            void this.offsetWidth;

            if (isCorrect) {
              this.classList.add('correct');
              feedbackEl.textContent = CONFIG.correctMessage;
              feedbackEl.style.color = '#2e8b57';

              setTimeout(() => {
                nextBtn.classList.add('visible', 'highlight');
              }, CONFIG.nextButtonDelay);
            } else {
              this.classList.add('wrong');
              feedbackEl.textContent = CONFIG.wrongMessages[questionId] || 'Подумай ещё, солнышко! 😉';
              feedbackEl.style.color = '#a33a3a';
              nextBtn.classList.remove('visible', 'highlight');
            }
          });
        });

        nextBtn.addEventListener('click', function() {
          if (!this.classList.contains('highlight')) return;

          const nextScreenMap = { q1: 'q2', q2: 'q3', q3: 'q4', q4: 'final' };
          const nextScreen = nextScreenMap[questionId];
          if (!nextScreen) return;

          if (questionId === 'q4') explodeHearts();

          goToScreen(questionId, nextScreen);
        });
      }

      function explodeHearts() {
        const emojis = ['❤️', '💖', '💗', '💕', '💘', '💝', '🌸', '✨'];
        const container = document.getElementById('appContainer');
        const rect = container.getBoundingClientRect();
        const cx = rect.left + rect.width / 2;
        const cy = rect.top + rect.height / 2;

        for (let i = 0; i < 30; i++) {
          const heart = document.createElement('div');
          heart.className = 'explosion-heart';
          heart.textContent = emojis[Math.floor(Math.random() * emojis.length)];

          const angle = Math.random() * Math.PI * 2;
          const dist = 100 + Math.random() * 250;

          heart.style.left = cx + 'px';
          heart.style.top = cy + 'px';
          heart.style.setProperty('--tx', Math.cos(angle) * dist + 'px');
          heart.style.setProperty('--ty', Math.sin(angle) * dist - 50 + 'px');
          heart.style.fontSize = (20 + Math.random() * 20) + 'px';
          heart.style.animationDelay = (Math.random() * 0.2) + 's';

          document.body.appendChild(heart);
          setTimeout(() => heart.remove(), 1500);
        }
      }

      // Доступность
      document.querySelectorAll('.option').forEach(opt => {
        opt.setAttribute('tabindex', '0');
        opt.setAttribute('role', 'button');
        opt.addEventListener('keydown', function(e) {
          if (e.key === 'Enter' || e.key === ' ') {
            e.preventDefault();
            this.click();
          }
        });
      });

    })();
  </script>
</body>
</html>
