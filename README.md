# Birthday-wishes
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Happy Birthday Surprise!</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Dancing+Script:wght@700&family=Great+Vibes&family=Poppins:wght@300;400;600;700&display=swap" rel="stylesheet">
  <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: 'Poppins', sans-serif;
      -webkit-tap-highlight-color: transparent;
    }
    :root {
      --primary: #ff4b72;
      --secondary: #6c5ce7;
      --gold: #f59e0b;
      --accent: #ff7675;
      --bg: #0f1026;
      --card-bg: rgba(255, 255, 255, 0.08);
      --border: rgba(255, 255, 255, 0.15);
      --text: #ffffff;
      --text-muted: #dcdde1;
    }
    body {
      background: radial-gradient(circle at 50% 20%, #1e1b4b 0%, #090a16 100%);
      color: var(--text);
      min-height: 100vh;
      overflow-x: hidden;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      position: relative;
    }
    
    /* Background Particle Canvas */
    #sparkle-canvas {
      position: fixed;
      top: 0;
      left: 0;
      width: 100vw;
      height: 100vh;
      pointer-events: none;
      z-index: 0;
    }

    /* Top Progress Bar / Navigation Dots */
    .nav-container {
      position: fixed;
      top: 18px;
      z-index: 100;
      display: flex;
      gap: 12px;
      background: rgba(15, 16, 38, 0.7);
      backdrop-filter: blur(10px);
      padding: 8px 18px;
      border-radius: 30px;
      border: 1px solid var(--border);
    }
    .dot {
      width: 13px;
      height: 13px;
      border-radius: 50%;
      background: rgba(255,255,255,0.25);
      cursor: pointer;
      transition: all 0.3s ease;
    }
    .dot.active {
      background: var(--primary);
      box-shadow: 0 0 10px var(--primary);
      transform: scale(1.25);
    }

    /* Container for Slides */
    .slides-wrapper {
      position: relative;
      width: 100%;
      max-width: 850px;
      min-height: 85vh;
      padding: 70px 20px 80px 20px;
      display: flex;
      align-items: center;
      justify-content: center;
      z-index: 10;
    }

    .slide {
      display: none;
      width: 100%;
      flex-direction: column;
      align-items: center;
      text-align: center;
      animation: fadeIn 0.7s cubic-bezier(0.16, 1, 0.3, 1) forwards;
    }
    .slide.active {
      display: flex;
    }

    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(18px) scale(0.97); }
      to { opacity: 1; transform: translateY(0) scale(1); }
    }

    /* General Typography */
    h1, h2 {
      font-family: 'Great Vibes', cursive;
      color: #ffd166;
      text-shadow: 0 0 20px rgba(255, 209, 102, 0.5);
      line-height: 1.2;
    }
    h1 { font-size: 3.4rem; margin-bottom: 8px; }
    h2 { font-size: 2.8rem; margin-bottom: 12px; }
    p.subtitle {
      font-size: 1.1rem;
      color: var(--text-muted);
      max-width: 580px;
      line-height: 1.6;
      margin-bottom: 25px;
    }

    /* Buttons */
    .action-btn {
      background: linear-gradient(135deg, #ff4b72, #e84393);
      color: white;
      border: none;
      padding: 14px 34px;
      font-size: 1.05rem;
      font-weight: 600;
      border-radius: 50px;
      cursor: pointer;
      box-shadow: 0 8px 25px rgba(255, 75, 114, 0.45);
      transition: all 0.3s ease;
      display: inline-flex;
      align-items: center;
      gap: 10px;
      margin-top: 20px;
    }
    .action-btn:hover {
      transform: translateY(-3px) scale(1.03);
      box-shadow: 0 12px 30px rgba(255, 75, 114, 0.65);
    }
    .action-btn:active {
      transform: translateY(1px);
    }

    /* Floating / Customizer bar */
    .bottom-bar {
      position: fixed;
      bottom: 15px;
      display: flex;
      gap: 15px;
      z-index: 90;
    }
    .sub-btn {
      background: rgba(255, 255, 255, 0.12);
      backdrop-filter: blur(8px);
      border: 1px solid var(--border);
      color: #fff;
      padding: 8px 18px;
      border-radius: 20px;
      font-size: 0.85rem;
      cursor: pointer;
      transition: all 0.2s ease;
    }
    .sub-btn:hover {
      background: rgba(255, 255, 255, 0.25);
    }

    /* ========================================================
       SLIDE 1: ARROW POPPING BALLOON ANIMATION
       ======================================================== */
    .balloon-stage {
      position: relative;
      width: 320px;
      height: 320px;
      margin: 15px auto;
      display: flex;
      align-items: center;
      justify-content: center;
    }
    .main-balloon {
      width: 170px;
      height: 220px;
      background: radial-gradient(circle at 35% 30%, #ff7675, #d63031);
      border-radius: 50% 50% 50% 50% / 40% 40% 60% 60%;
      position: relative;
      box-shadow: 0 15px 35px rgba(214, 48, 49, 0.5), inset 8px 8px 15px rgba(255, 255, 255, 0.4);
      animation: floatBalloon 3s ease-in-out infinite alternate;
      cursor: pointer;
      transition: transform 0.2s;
    }
    .main-balloon::after {
      content: '';
      position: absolute;
      bottom: -12px;
      left: 50%;
      transform: translateX(-50%);
      border-left: 10px solid transparent;
      border-right: 10px solid transparent;
      border-bottom: 14px solid #d63031;
    }
    .main-balloon-string {
      position: absolute;
      bottom: -70px;
      left: 50%;
      width: 2px;
      height: 60px;
      background: rgba(255, 255, 255, 0.6);
      transform: translateX(-50%);
    }
    @keyframes floatBalloon {
      0% { transform: translateY(0px) rotate(-2deg); }
      100% { transform: translateY(-18px) rotate(2deg); }
    }
    /* Arrow Animation */
    .arrow-container {
      position: absolute;
      left: -200px;
      top: 45%;
      font-size: 3.5rem;
      filter: drop-shadow(0 0 10px #ffd166);
      transition: all 0.55s cubic-bezier(0.25, 1, 0.5, 1);
      opacity: 0;
      pointer-events: none;
      z-index: 15;
    }
    .arrow-container.shoot {
      left: 45%;
      opacity: 1;
    }
    .popped {
      animation: popBurst 0.25s forwards !important;
    }
    @keyframes popBurst {
      0% { transform: scale(1.1); }
      50% { transform: scale(1.4); opacity: 0.8; }
      100% { transform: scale(0); opacity: 0; visibility: hidden; }
    }
    .slide1-reveal {
      display: none;
      animation: zoomIn 0.8s cubic-bezier(0.17, 0.89, 0.32, 1.25) forwards;
      background: rgba(255, 255, 255, 0.08);
      border: 1px solid rgba(255, 255, 255, 0.2);
      border-radius: 24px;
      padding: 25px 35px;
      backdrop-filter: blur(15px);
      box-shadow: 0 15px 35px rgba(0, 0, 0, 0.4);
    }
    @keyframes zoomIn {
      from { transform: scale(0.5); opacity: 0; }
      to { transform: scale(1); opacity: 1; }
    }

    /* ========================================================
       SLIDE 2: INTERACTIVE CAKE & CANDLE
       ======================================================== */
    .cake-container {
      position: relative;
      width: 280px;
      height: 250px;
      margin: 25px auto;
      cursor: pointer;
    }
    .cake-base {
      position: absolute;
      bottom: 10px;
      left: 50%;
      transform: translateX(-50%);
      width: 220px;
      height: 90px;
      background: linear-gradient(to right, #6d4c41, #8d6e63, #6d4c41);
      border-radius: 12px 12px 24px 24px;
      box-shadow: 0 10px 25px rgba(0,0,0,0.6);
      border-top: 8px solid #ff7675;
    }
    .cake-top-layer {
      position: absolute;
      bottom: 95px;
      left: 50%;
      transform: translateX(-50%);
      width: 160px;
      height: 75px;
      background: linear-gradient(to right, #ffeaa7, #fab1a0, #ffeaa7);
      border-radius: 10px 10px 18px 18px;
      border-top: 7px solid #ffffff;
      box-shadow: 0 6px 15px rgba(0,0,0,0.4);
    }
    /* Candle */
    .candle {
      position: absolute;
      bottom: 170px;
      left: 50%;
      transform: translateX(-50%);
      width: 14px;
      height: 48px;
      background: repeating-linear-gradient(45deg, #ff4b72, #ff4b72 6px, #ffffff 6px, #ffffff 12px);
      border-radius: 4px;
    }
    .flame {
      position: absolute;
      top: -22px;
      left: 50%;
      transform: translateX(-50%);
      width: 16px;
      height: 24px;
      background: radial-gradient(circle, #fff 15%, #ffd166 60%, #ff4b72 100%);
      border-radius: 50% 50% 20% 20%;
      box-shadow: 0 0 20px #ffd166, 0 0 35px #ff7675;
      animation: flicker 0.2s infinite alternate;
      transition: all 0.3s;
    }
    @keyframes flicker {
      0% { transform: translateX(-50%) scale(1) rotate(-1deg); }
      100% { transform: translateX(-50%) scale(1.12) rotate(2deg); }
    }
    .flame.blown-out {
      opacity: 0;
      transform: translateX(-50%) scale(0.2);
    }
    .smoke {
      position: absolute;
      top: -30px;
      left: 50%;
      width: 6px;
      height: 6px;
      background: rgba(220, 220, 220, 0.6);
      border-radius: 50%;
      opacity: 0;
      pointer-events: none;
    }
    .candle.blown .smoke {
      animation: riseSmoke 1.2s forwards;
    }
    @keyframes riseSmoke {
      0% { opacity: 0.8; transform: translate(-50%, 0) scale(1); }
      100% { opacity: 0; transform: translate(-30px, -45px) scale(3.5); }
    }
    .knife-indicator {
      position: absolute;
      top: 70px;
      right: 15px;
      font-size: 2.8rem;
      transform: rotate(-35deg);
      transition: all 0.5s ease;
      filter: drop-shadow(0 5px 10px rgba(0,0,0,0.5));
    }
    .knife-indicator.cut {
      transform: translate(-65px, 60px) rotate(15deg);
    }
    .cake-cut-gap {
      position: absolute;
      bottom: 10px;
      left: 50%;
      width: 4px;
      height: 160px;
      background: rgba(0, 0, 0, 0.5);
      opacity: 0;
      transition: opacity 0.4s;
    }
    .cake-cut-gap.active {
      opacity: 1;
    }

    /* ========================================================
       SLIDE 3: 5 POPPING BALLOONS WITH WISHES
       ======================================================== */
    .balloons-row {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 22px;
      margin: 30px 0;
      max-width: 680px;
    }
    .small-balloon-wrap {
      display: flex;
      flex-direction: column;
      align-items: center;
      cursor: pointer;
      position: relative;
    }
    .pop-balloon {
      width: 78px;
      height: 102px;
      border-radius: 50% 50% 50% 50% / 40% 40% 60% 60%;
      position: relative;
      box-shadow: inset 5px 5px 12px rgba(255,255,255,0.4), 0 8px 20px rgba(0,0,0,0.3);
      animation: floatSmall 2.8s ease-in-out infinite alternate;
      display: flex;
      align-items: center;
      justify-content: center;
      font-weight: 700;
      font-size: 1.4rem;
      color: #ffffff;
      text-shadow: 0 1px 4px rgba(0,0,0,0.4);
      transition: transform 0.2s;
    }
    .pop-balloon:hover {
      transform: scale(1.12);
    }
    .pop-balloon::after {
      content: '';
      position: absolute;
      bottom: -6px;
      left: 50%;
      transform: translateX(-50%);
      border-left: 6px solid transparent;
      border-right: 6px solid transparent;
      border-bottom: 8px solid inherit;
    }
    .b-string {
      width: 2px;
      height: 35px;
      background: rgba(255,255,255,0.4);
      margin-top: 4px;
    }
    @keyframes floatSmall {
      0% { transform: translateY(0px) rotate(-3deg); }
      100% { transform: translateY(-12px) rotate(3deg); }
    }
    .wish-modal {
      display: none;
      position: fixed;
      top: 0;
      left: 0;
      width: 100vw;
      height: 100vh;
      background: rgba(0,0,0,0.7);
      backdrop-filter: blur(8px);
      z-index: 200;
      align-items: center;
      justify-content: center;
      padding: 20px;
    }
    .wish-card {
      background: linear-gradient(145deg, #1e1b4b, #2d2a6a);
      border: 2px solid #ffd166;
      border-radius: 24px;
      max-width: 480px;
      width: 100%;
      padding: 30px;
      text-align: center;
      box-shadow: 0 20px 45px rgba(0,0,0,0.6);
      animation: modalPop 0.4s cubic-bezier(0.18, 0.89, 0.32, 1.28);
    }
    @keyframes modalPop {
      from { transform: scale(0.6); opacity: 0; }
      to { transform: scale(1); opacity: 1; }
    }

    /* ========================================================
       SLIDE 4: 5 CUSTOMIZED PHOTOS (POLAROID GALLERY)
       ======================================================== */
    .gallery-grid {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 20px;
      margin: 25px 0;
      max-width: 780px;
    }
    .polaroid {
      background: #ffffff;
      padding: 10px 10px 22px 10px;
      border-radius: 8px;
      box-shadow: 0 10px 25px rgba(0,0,0,0.45);
      width: 140px;
      transform: rotate(calc(-4deg + 8deg * var(--r, 0.5)));
      transition: all 0.3s ease;
      cursor: pointer;
    }
    .polaroid:hover {
      transform: scale(1.15) rotate(0deg);
      z-index: 20;
      box-shadow: 0 18px 35px rgba(255, 75, 114, 0.4);
    }
    .polaroid img {
      width: 100%;
      height: 130px;
      object-fit: cover;
      border-radius: 4px;
      background: #eee;
    }
    .polaroid-caption {
      color: #2d3436;
      font-size: 0.8rem;
      margin-top: 8px;
      font-family: 'Dancing Script', cursive;
      font-weight: 700;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }

    /* ========================================================
       SLIDE 5: 700-WORD EMOTIONAL PARCHMENT LETTER
       ======================================================== */
    .parchment {
      background: linear-gradient(135deg, #fffbf0 0%, #faebd7 100%);
      color: #3e2723;
      border-radius: 16px;
      padding: 35px 30px;
      max-width: 680px;
      max-height: 520px;
      overflow-y: auto;
      text-align: left;
      line-height: 1.85;
      font-size: 1.02rem;
      border: 2px solid #e0c9a6;
      box-shadow: 0 15px 40px rgba(0,0,0,0.5), inset 0 0 40px rgba(184, 134, 11, 0.15);
      position: relative;
    }
    .parchment::-webkit-scrollbar {
      width: 6px;
    }
    .parchment::-webkit-scrollbar-thumb {
      background: #c5a880;
      border-radius: 10px;
    }
    .letter-title {
      font-family: 'Great Vibes', cursive;
      font-size: 2.6rem;
      color: #8b4513;
      text-align: center;
      margin-bottom: 20px;
      line-height: 1.2;
    }
    .letter-body {
      white-space: pre-line;
      font-family: 'Georgia', serif;
    }

    /* ========================================================
       CUSTOMIZER / CREATOR MODAL
       ======================================================== */
    #customizer-modal {
      display: none;
      position: fixed;
      top: 0;
      left: 0;
      width: 100vw;
      height: 100vh;
      background: rgba(8, 9, 23, 0.85);
      backdrop-filter: blur(12px);
      z-index: 300;
      overflow-y: auto;
      padding: 30px 15px;
    }
    .modal-box {
      background: #191b38;
      border: 1px solid var(--border);
      border-radius: 20px;
      max-width: 680px;
      margin: 0 auto;
      padding: 30px 25px;
      color: #fff;
      box-shadow: 0 25px 60px rgba(0, 0, 0, 0.6);
    }
    .form-group {
      margin-bottom: 18px;
      text-align: left;
    }
    .form-group label {
      display: block;
      font-size: 0.9rem;
      font-weight: 600;
      color: #ffd166;
      margin-bottom: 6px;
    }
    .form-group input, .form-group textarea {
      width: 100%;
      padding: 10px 14px;
      border-radius: 10px;
      border: 1px solid rgba(255,255,255,0.2);
      background: rgba(255,255,255,0.06);
      color: #fff;
      font-size: 0.95rem;
      outline: none;
    }
    .form-group input:focus, .form-group textarea:focus {
      border-color: var(--primary);
    }
    .grid-2 {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 12px;
    }
    @media (max-width: 600px) {
      .grid-2 { grid-template-columns: 1fr; }
      h1 { font-size: 2.5rem; }
      h2 { font-size: 2.1rem; }
      .slides-wrapper { padding-top: 60px; }
      .polaroid { width: 110px; padding: 6px 6px 14px 6px; }
      .polaroid img { height: 95px; }
    }
  </style>
</head>
<body>

  <canvas id="sparkle-canvas"></canvas>

  <!-- Progress Bar / Dots -->
  <div class="nav-container">
    <div class="dot active" onclick="jumpToSlide(1)" title="Slide 1: Arrow Pop"></div>
    <div class="dot" onclick="jumpToSlide(2)" title="Slide 2: Cake Cutting"></div>
    <div class="dot" onclick="jumpToSlide(3)" title="Slide 3: 5 Wishes"></div>
    <div class="dot" onclick="jumpToSlide(4)" title="Slide 4: Photo Gallery"></div>
    <div class="dot" onclick="jumpToSlide(5)" title="Slide 5: Emotional Letter"></div>
  </div>

  <div class="slides-wrapper">

    <!-- ================= SLIDE 1 ================= -->
    <div class="slide active" id="slide-1">
      <div id="slide1-start">
        <p class="subtitle" style="letter-spacing: 2px; text-transform: uppercase; font-size: 0.85rem; color: #ffd166;">
          ✨ A Magical Surprise Awaits You ✨
        </p>
        <h2 style="font-size: 2.4rem;">Pop The Mystery Balloon!</h2>
        <p class="subtitle">Neeche diye button ko dabayein ya balloon par click karein aur surprise dekhein!</p>

        <div class="balloon-stage">
          <div class="arrow-container" id="arrow">🏹</div>
          <div class="main-balloon" id="big-balloon" onclick="popFirstBalloon()">
            <div class="main-balloon-string"></div>
          </div>
        </div>

        <button class="action-btn" onclick="popFirstBalloon()">
          <span>Shoot Arrow & Pop 🎯</span>
        </button>
      </div>

      <div class="slide1-reveal" id="slide1-reveal">
        <h1 id="hero-title">Happy Birthday!</h1>
        <h2 id="hero-name" style="color: #ff7675; font-size: 3rem;">Dear Friend</h2>
        <p class="subtitle" id="hero-quote">
          "On this special day, may your heart be full of joy, your face filled with smiles, and your life showered with boundless blessings!"
        </p>
        <button class="action-btn" onclick="jumpToSlide(2)">
          <span>Next: Cut The Birthday Cake 🎂 &rarr;</span>
        </button>
      </div>
    </div>

    <!-- ================= SLIDE 2 ================= -->
    <div class="slide" id="slide-2">
      <h2>Make A Wish & Cut The Cake</h2>
      <p class="subtitle" id="cake-instruction">Pehle Candle ko Blow karein, fir Cake Slice cut karein!</p>

      <div class="cake-container" id="cake-box" onclick="interactCake()">
        <div class="candle" id="candle">
          <div class="flame" id="flame"></div>
          <div class="smoke"></div>
        </div>
        <div class="cake-top-layer"></div>
        <div class="cake-base"></div>
        <div class="cake-cut-gap" id="cut-gap"></div>
        <div class="knife-indicator" id="knife">🔪</div>
      </div>

      <div id="cake-status-text" style="font-weight: 600; font-size: 1.15rem; color: #ffd166; min-height: 30px;">
        Tap on the Candle to Blow it out! 🕯️
      </div>

      <button class="action-btn" id="cake-next-btn" style="display: none;" onclick="jumpToSlide(3)">
        <span>Next: Pop 5 Surprise Balloons 🎈 &rarr;</span>
      </button>
    </div>

    <!-- ================= SLIDE 3 ================= -->
    <div class="slide" id="slide-3">
      <h2>5 Balloons, 5 Special Wishes!</h2>
      <p class="subtitle">Har ek balloon ko tap karke phodein aur chhupi hui wishes padhein ✨</p>

      <div class="balloons-row" id="balloons-row">
        <!-- Rendered dynamically -->
 
