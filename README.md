<html lang="th">
 <head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>เกมฉันเกิดมาจากใคร</title>
  <script src="/_sdk/data_sdk.js"></script>
  <script src="/_sdk/element_sdk.js"></script>
  <style>
    body {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: 'Sarabun', 'Noto Sans Thai', sans-serif;
      width: 100%;
      height: 100%;
    }

    * {
      box-sizing: border-box;
    }

    .game-wrapper {
      width: 100%;
      min-height: 100%;
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      padding: 20px;
    }

    .game-container {
      max-width: 900px;
      margin: 0 auto;
      background: white;
      border-radius: 20px;
      padding: 30px;
      box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
    }

    .header {
      text-align: center;
      margin-bottom: 30px;
    }

    .game-title {
      font-size: 32px;
      font-weight: bold;
      color: #667eea;
      margin-bottom: 10px;
    }

    .subtitle {
      font-size: 18px;
      color: #666;
    }

    .mode-selection {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 20px;
      margin-bottom: 30px;
    }

    .mode-card {
      background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
      padding: 25px;
      border-radius: 15px;
      cursor: pointer;
      transition: all 0.3s ease;
      border: 3px solid transparent;
      text-align: center;
    }

    .mode-card:hover {
      transform: translateY(-5px);
      box-shadow: 0 10px 25px rgba(0, 0, 0, 0.15);
    }

    .mode-card.active {
      border-color: #667eea;
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      color: white;
    }

    .mode-title {
      font-size: 20px;
      font-weight: bold;
      margin-bottom: 10px;
    }

    .mode-description {
      font-size: 14px;
      opacity: 0.8;
    }

    .game-area {
      background: #f8f9fa;
      border-radius: 15px;
      padding: 25px;
      margin-bottom: 20px;
      min-height: 400px;
    }

    .question-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 20px;
      padding: 15px;
      background: white;
      border-radius: 10px;
    }

    .question-number {
      font-size: 18px;
      font-weight: bold;
      color: #667eea;
    }

    .score-display {
      font-size: 18px;
      font-weight: bold;
      color: #28a745;
    }

    .salt-display {
      text-align: center;
      padding: 30px;
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      color: white;
      border-radius: 15px;
      margin-bottom: 25px;
    }

    .salt-formula {
      font-size: 36px;
      font-weight: bold;
      margin-bottom: 10px;
    }

    .salt-name {
      font-size: 20px;
      opacity: 0.9;
    }

    .drop-zones {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 20px;
      margin-bottom: 25px;
    }

    .drop-zone {
      background: white;
      border: 3px dashed #ccc;
      border-radius: 15px;
      padding: 30px;
      min-height: 120px;
      text-align: center;
      transition: all 0.3s ease;
    }

    .drop-zone.drag-over {
      border-color: #667eea;
      background: #f0f4ff;
    }

    .drop-zone.filled {
      border-style: solid;
      border-color: #667eea;
      background: #e8f0ff;
    }

    .drop-zone.correct {
      border-color: #28a745;
      background: #d4edda;
      animation: pulse 0.5s ease;
    }

    .drop-zone.incorrect {
      border-color: #dc3545;
      background: #f8d7da;
      animation: shake 0.5s ease;
    }

    .drop-zone-label {
      font-size: 18px;
      font-weight: bold;
      color: #666;
      margin-bottom: 15px;
    }

    .dropped-item {
      display: inline-block;
      padding: 12px 24px;
      background: #667eea;
      color: white;
      border-radius: 10px;
      font-size: 18px;
      font-weight: bold;
    }

    .choices-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(140px, 1fr));
      gap: 12px;
      margin-bottom: 25px;
    }

    .choice-item {
      padding: 15px;
      background: white;
      border: 2px solid #e0e0e0;
      border-radius: 10px;
      cursor: pointer;
      transition: all 0.3s ease;
      text-align: center;
      font-weight: bold;
      user-select: none;
    }

    .choice-item:hover:not(.used):not(.disabled) {
      transform: translateY(-3px);
      box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
      border-color: #667eea;
    }

    .choice-item.dragging {
      opacity: 0.5;
    }

    .choice-item.used {
      opacity: 0.3;
      cursor: not-allowed;
      background: #f5f5f5;
    }

    .choice-item.disabled {
      pointer-events: none;
      opacity: 0.3;
    }

    .choice-item.acid {
      color: #dc3545;
      border-color: #dc3545;
    }

    .choice-item.base {
      color: #007bff;
      border-color: #007bff;
    }

    .choice-item.salt {
      color: #28a745;
      border-color: #28a745;
    }

    .result-message {
      text-align: center;
      padding: 20px;
      border-radius: 10px;
      margin-top: 20px;
      font-size: 18px;
      font-weight: bold;
      animation: slideIn 0.5s ease;
    }

    .result-message.correct {
      background: #d4edda;
      color: #155724;
      border: 2px solid #c3e6cb;
    }

    .result-message.incorrect {
      background: #f8d7da;
      color: #721c24;
      border: 2px solid #f5c6cb;
    }

    .leaderboard {
      margin-top: 30px;
      background: white;
      border-radius: 15px;
      padding: 25px;
    }

    .leaderboard-title {
      font-size: 24px;
      font-weight: bold;
      color: #667eea;
      margin-bottom: 20px;
      text-align: center;
    }

    .leaderboard-list {
      list-style: none;
      padding: 0;
      margin: 0;
    }

    .leaderboard-item {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 15px;
      background: #f8f9fa;
      border-radius: 10px;
      margin-bottom: 10px;
    }

    .leaderboard-rank {
      font-size: 24px;
      font-weight: bold;
      color: #667eea;
      margin-right: 15px;
    }

    .leaderboard-name {
      flex: 1;
      font-size: 18px;
      font-weight: bold;
    }

    .leaderboard-score {
      font-size: 18px;
      font-weight: bold;
      color: #28a745;
    }

    .player-input-section {
      background: white;
      padding: 25px;
      border-radius: 15px;
      margin-bottom: 25px;
      text-align: center;
    }

    .player-input {
      padding: 12px 20px;
      border: 2px solid #e0e0e0;
      border-radius: 10px;
      font-size: 16px;
      width: 300px;
      margin-right: 10px;
    }

    .player-character {
      position: fixed;
      bottom: 20px;
      right: 20px;
      width: 120px;
      height: 120px;
      z-index: 100;
      transition: transform 0.3s ease;
    }

    .player-character:hover {
      transform: scale(1.1) rotate(5deg);
    }

    .animal-body {
      width: 100%;
      height: 100%;
      position: relative;
    }

    /* แมว */
    .animal-head.cat {
      width: 70px;
      height: 60px;
      border-radius: 50% 50% 45% 45%;
      position: absolute;
      top: 15px;
      left: 25px;
      border: 3px solid #333;
    }

    .animal-ear.cat {
      width: 0;
      height: 0;
      border-left: 15px solid transparent;
      border-right: 15px solid transparent;
      border-bottom: 25px solid;
      position: absolute;
      top: -5px;
    }

    .animal-ear.cat.left {
      left: 5px;
      transform: rotate(-20deg);
    }

    .animal-ear.cat.right {
      right: 5px;
      transform: rotate(20deg);
    }

    /* สุนัข */
    .animal-head.dog {
      width: 65px;
      height: 65px;
      border-radius: 50%;
      position: absolute;
      top: 15px;
      left: 27px;
      border: 3px solid #333;
    }

    .animal-ear.dog {
      width: 25px;
      height: 35px;
      border-radius: 50% 50% 0 50%;
      position: absolute;
      top: 0px;
      border: 3px solid #333;
    }

    .animal-ear.dog.left {
      left: -5px;
      transform: rotate(-30deg);
    }

    .animal-ear.dog.right {
      right: -5px;
      transform: rotate(30deg);
    }

    /* กระต่าย */
    .animal-head.rabbit {
      width: 65px;
      height: 60px;
      border-radius: 50% 50% 45% 45%;
      position: absolute;
      top: 25px;
      left: 27px;
      border: 3px solid #333;
    }

    .animal-ear.rabbit {
      width: 15px;
      height: 45px;
      border-radius: 50% 50% 0 0;
      position: absolute;
      top: -25px;
      border: 3px solid #333;
    }

    .animal-ear.rabbit.left {
      left: 8px;
      transform: rotate(-10deg);
    }

    .animal-ear.rabbit.right {
      right: 8px;
      transform: rotate(10deg);
    }

    /* หมี */
    .animal-head.bear {
      width: 70px;
      height: 68px;
      border-radius: 50%;
      position: absolute;
      top: 20px;
      left: 25px;
      border: 3px solid #333;
    }

    .animal-ear.bear {
      width: 20px;
      height: 20px;
      border-radius: 50%;
      position: absolute;
      top: 5px;
      border: 3px solid #333;
    }

    .animal-ear.bear.left {
      left: 0px;
    }

    .animal-ear.bear.right {
      right: 0px;
    }

    .animal-eye {
      width: 10px;
      height: 10px;
      background: #333;
      border-radius: 50%;
      position: absolute;
      top: 22px;
    }

    .animal-eye.left {
      left: 15px;
    }

    .animal-eye.right {
      right: 15px;
    }

    .animal-nose {
      width: 12px;
      height: 10px;
      background: #333;
      border-radius: 50% 50% 50% 50%;
      position: absolute;
      bottom: 18px;
      left: 50%;
      transform: translateX(-50%);
    }

    .animal-mouth {
      width: 20px;
      height: 8px;
      border: 2px solid #333;
      border-top: none;
      border-radius: 0 0 50% 50%;
      position: absolute;
      bottom: 10px;
      left: 50%;
      transform: translateX(-50%);
    }

    .animal-whiskers {
      position: absolute;
      width: 100%;
      height: 100%;
    }

    .whisker {
      width: 25px;
      height: 2px;
      background: #333;
      position: absolute;
      top: 50%;
    }

    .whisker.left-1 {
      left: -20px;
      transform: rotate(-10deg);
    }

    .whisker.left-2 {
      left: -22px;
      top: 55%;
      transform: rotate(10deg);
    }

    .whisker.right-1 {
      right: -20px;
      transform: rotate(10deg);
    }

    .whisker.right-2 {
      right: -22px;
      top: 55%;
      transform: rotate(-10deg);
    }

    .animal-body-main {
      width: 50px;
      height: 45px;
      border-radius: 50% 50% 40% 40%;
      position: absolute;
      bottom: 10px;
      left: 35px;
      border: 3px solid #333;
    }

    .animal-leg {
      width: 12px;
      height: 25px;
      border-radius: 0 0 30% 30%;
      position: absolute;
      bottom: 8px;
      border: 2px solid #333;
    }

    .animal-leg.front-left {
      left: 28px;
    }

    .animal-leg.front-right {
      left: 45px;
    }

    .animal-leg.back-left {
      left: 55px;
    }

    .animal-leg.back-right {
      left: 72px;
    }

    .animal-tail {
      width: 8px;
      height: 35px;
      border-radius: 50%;
      position: absolute;
      bottom: 20px;
      right: 15px;
      border: 2px solid #333;
      transform: rotate(45deg);
    }

    .color-btn,
    .hair-btn {
      width: 50px;
      height: 50px;
      border: 3px solid #e0e0e0;
      border-radius: 10px;
      cursor: pointer;
      transition: all 0.3s ease;
      background: white;
    }

    .color-btn:hover,
    .hair-btn:hover {
      transform: scale(1.1);
      box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
    }

    .color-btn.active,
    .hair-btn.active {
      border-color: #667eea;
      border-width: 4px;
      box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.3);
    }

    .player-name-badge {
      background: white;
      padding: 8px 15px;
      border-radius: 20px;
      border: 2px solid #667eea;
      position: absolute;
      top: -10px;
      left: 50%;
      transform: translateX(-50%);
      white-space: nowrap;
      font-weight: bold;
      font-size: 14px;
      color: #667eea;
      box-shadow: 0 2px 8px rgba(0,0,0,0.1);
    }

    .btn {
      padding: 15px 35px;
      border: none;
      border-radius: 10px;
      font-size: 16px;
      font-weight: bold;
      cursor: pointer;
      transition: all 0.3s ease;
    }

    .btn-primary {
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      color: white;
    }

    .btn-primary:hover:not(:disabled) {
      transform: translateY(-2px);
      box-shadow: 0 8px 20px rgba(102, 126, 234, 0.4);
    }

    .btn:disabled {
      opacity: 0.5;
      cursor: not-allowed;
    }

    .hidden {
      display: none;
    }

    @keyframes slideIn {
      from {
        opacity: 0;
        transform: translateY(-20px);
      }
      to {
        opacity: 1;
        transform: translateY(0);
      }
    }

    @keyframes pulse {
      0%, 100% { transform: scale(1); }
      50% { transform: scale(1.05); }
    }

    @keyframes shake {
      0%, 100% { transform: translateX(0); }
      25% { transform: translateX(-10px); }
      75% { transform: translateX(10px); }
    }

    .loading {
      text-align: center;
      padding: 20px;
      font-size: 18px;
      color: #666;
    }

    @media (max-width: 768px) {
      .mode-selection {
        grid-template-columns: 1fr;
      }

      .drop-zones {
        grid-template-columns: 1fr;
      }

      .choices-grid {
        grid-template-columns: repeat(auto-fill, minmax(120px, 1fr));
      }

      .game-title {
        font-size: 24px;
      }

      .salt-formula {
        font-size: 28px;
      }
    }
  </style>
  <style>@view-transition { navigation: auto; }</style>
  <script src="https://cdn.tailwindcss.com" type="text/javascript"></script>
 </head>
 <body>
  <div class="game-wrapper">
   <div class="game-container">
    <div class="header">
     <h1 class="game-title" id="gameTitle">สนุกกับกรด-เบส</h1>
     <p class="subtitle" id="subtitle">กรด เบส และเกลือ</p>
    </div>
    <div id="playerInputSection" class="player-input-section">
     <h3>กรุณากรอกชื่อผู้เล่น</h3><input type="text" id="playerNameInput" class="player-input" placeholder="ชื่อของคุณ">
     <div style="margin-top: 30px;">
      <h3 style="margin-bottom: 20px;">🐾 เลือกสัตว์ของคุณ</h3>
      <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 30px; max-width: 600px; margin: 0 auto;">
       <div><label style="display: block; margin-bottom: 10px; font-weight: bold; color: #667eea;">สีสัตว์:</label>
        <div style="display: flex; gap: 10px; flex-wrap: wrap; justify-content: center;"><button class="color-btn active" data-color="#FFB347" onclick="selectAnimalColor('#FFB347')" style="background: #FFB347;"></button> <button class="color-btn" data-color="#87CEEB" onclick="selectAnimalColor('#87CEEB')" style="background: #87CEEB;"></button> <button class="color-btn" data-color="#FFB6C1" onclick="selectAnimalColor('#FFB6C1')" style="background: #FFB6C1;"></button> <button class="color-btn" data-color="#90EE90" onclick="selectAnimalColor('#90EE90')" style="background: #90EE90;"></button> <button class="color-btn" data-color="#DDA0DD" onclick="selectAnimalColor('#DDA0DD')" style="background: #DDA0DD;"></button> <button class="color-btn" data-color="#F0E68C" onclick="selectAnimalColor('#F0E68C')" style="background: #F0E68C;"></button>
        </div>
       </div>
       <div><label style="display: block; margin-bottom: 10px; font-weight: bold; color: #667eea;">ประเภทสัตว์:</label>
        <div style="display: flex; gap: 10px; flex-wrap: wrap; justify-content: center;"><button class="hair-btn active" onclick="selectAnimalType('cat')" style="font-size: 32px;">🐱</button> <button class="hair-btn" onclick="selectAnimalType('dog')" style="font-size: 32px;">🐶</button> <button class="hair-btn" onclick="selectAnimalType('rabbit')" style="font-size: 32px;">🐰</button> <button class="hair-btn" onclick="selectAnimalType('bear')" style="font-size: 32px;">🐻</button>
        </div>
       </div>
      </div>
      <div style="margin-top: 30px; padding: 20px; background: #f8f9fa; border-radius: 15px;">
       <p style="margin-bottom: 15px; font-weight: bold;">ตัวอย่าง:</p>
       <div id="animalPreview" style="width: 120px; height: 120px; margin: 0 auto; position: relative;">
        <div class="animal-body">
         <div class="animal-head cat" id="previewHead" style="background: #FFB347;">
          <div class="animal-ear cat left" id="previewEarLeft" style="border-bottom-color: #FFB347;"></div>
          <div class="animal-ear cat right" id="previewEarRight" style="border-bottom-color: #FFB347;"></div>
          <div class="animal-eye left"></div>
          <div class="animal-eye right"></div>
          <div class="animal-nose"></div>
          <div class="animal-mouth"></div>
          <div class="animal-whiskers">
           <div class="whisker left-1"></div>
           <div class="whisker left-2"></div>
           <div class="whisker right-1"></div>
           <div class="whisker right-2"></div>
          </div>
         </div>
         <div class="animal-body-main" id="previewBody" style="background: #FFB347;"></div>
         <div class="animal-leg front-left" id="previewLeg1" style="background: #FFB347;"></div>
         <div class="animal-leg front-right" id="previewLeg2" style="background: #FFB347;"></div>
         <div class="animal-leg back-left" id="previewLeg3" style="background: #FFB347;"></div>
         <div class="animal-leg back-right" id="previewLeg4" style="background: #FFB347;"></div>
         <div class="animal-tail" id="previewTail" style="background: #FFB347;"></div>
        </div>
       </div>
      </div>
     </div><button class="btn btn-primary" onclick="startGame()" style="margin-top: 30px;">เริ่มเกม</button>
    </div>
    <div id="gameContent" class="hidden">
     <div class="mode-selection">
      <div class="mode-card" id="mode1Card" onclick="selectMode(1)">
       <div class="mode-title" id="mode1Title">
        โหมด 1: หาต้นกำเนิดเกลือ
       </div>
       <div class="mode-description">
        กำหนดเกลือให้ ลากกรดและเบสมาคู่กัน (10 ข้อ)
       </div>
      </div>
      <div class="mode-card" id="mode2Card" onclick="selectMode(2)">
       <div class="mode-title" id="mode2Title">
        โหมด 2: ทายเกลือที่เกิด
       </div>
       <div class="mode-description">
        กำหนดกรดและเบส ทายเกลือที่เกิดขึ้น (10 ข้อ)
       </div>
      </div>
     </div>
     <div id="gameArea" class="game-area hidden">
      <div class="question-header"><span class="question-number" id="questionNumber">คำถามที่ 1/10</span> <span class="score-display" id="scoreDisplay">คะแนน: 0</span>
      </div>
      <div id="mode1Content" class="hidden">
       <div class="salt-display">
        <div class="salt-formula" id="saltFormula">
         NaCl
        </div>
        <div class="salt-name" id="saltName">
         โซเดียมคลอไรด์
        </div>
       </div>
       <div class="drop-zones">
        <div class="drop-zone" id="acidDropZone">
         <div class="drop-zone-label">
          กรด (Acid)
         </div>
         <div id="acidDropped"></div>
        </div>
        <div class="drop-zone" id="baseDropZone">
         <div class="drop-zone-label">
          เบส (Base)
         </div>
         <div id="baseDropped"></div>
        </div>
       </div>
       <div class="choices-grid" id="choicesGrid"></div>
      </div>
      <div id="mode2Content" class="hidden">
       <div class="salt-display">
        <div class="salt-formula" id="reactantsFormula">
         HCl + NaOH →
        </div>
        <div class="salt-name">
         เกลือที่เกิดขึ้นคืออะไร?
        </div>
       </div>
       <div class="choices-grid" id="saltChoicesGrid"></div>
      </div>
      <div id="resultMessage"></div>
     </div>
     <div class="leaderboard">
      <h2 class="leaderboard-title">🏆 กระดานคะแนน</h2>
      <ul class="leaderboard-list" id="leaderboardList">
       <li class="loading">กำลังโหลดข้อมูล...</li>
      </ul>
     </div>
    </div>
    <div id="playerCharacter" class="player-character hidden">
     <div class="player-name-badge" id="characterName">
      ผู้เล่น
     </div>
     <div class="animal-body">
      <div class="animal-head cat" id="characterHead" style="background: #FFB347;">
       <div class="animal-ear cat left" id="characterEarLeft" style="border-bottom-color: #FFB347;"></div>
       <div class="animal-ear cat right" id="characterEarRight" style="border-bottom-color: #FFB347;"></div>
       <div class="animal-eye left"></div>
       <div class="animal-eye right"></div>
       <div class="animal-nose"></div>
       <div class="animal-mouth"></div>
       <div class="animal-whiskers">
        <div class="whisker left-1"></div>
        <div class="whisker left-2"></div>
        <div class="whisker right-1"></div>
        <div class="whisker right-2"></div>
       </div>
      </div>
      <div class="animal-body-main" id="characterBody" style="background: #FFB347;"></div>
      <div class="animal-leg front-left" id="characterLeg1" style="background: #FFB347;"></div>
      <div class="animal-leg front-right" id="characterLeg2" style="background: #FFB347;"></div>
      <div class="animal-leg back-left" id="characterLeg3" style="background: #FFB347;"></div>
      <div class="animal-leg back-right" id="characterLeg4" style="background: #FFB347;"></div>
      <div class="animal-tail" id="characterTail" style="background: #FFB347;"></div>
     </div>
    </div>
   </div>
  </div>
  <script>
    const defaultConfig = {
      game_title: 'สนุกกับกรด-เบส',
      subtitle: 'กรด เบส และเกลือ',
      mode1_button: 'ฉันเกิดมาจากใคร',
      mode2_button: 'ทำนายทายเกลือ'
    };

    const questions = [
      // โหมด 1: กำหนดเกลือ หากรดและเบส (10 ข้อ)
      { mode: 1, salt: 'NaCl', saltName: 'โซเดียมคลอไรด์', acid: 'HCl', base: 'NaOH', decoys: ['H₂SO₄', 'KOH', 'Ca(OH)₂', 'HNO₃', 'NH₃', 'CH₃COOH'] },
      { mode: 1, salt: 'KNO₃', saltName: 'โพแทสเซียมไนเตรต', acid: 'HNO₃', base: 'KOH', decoys: ['HCl', 'NaOH', 'H₂SO₄', 'Ca(OH)₂', 'NH₃', 'H₃PO₄'] },
      { mode: 1, salt: 'CaSO₄', saltName: 'แคลเซียมซัลเฟต', acid: 'H₂SO₄', base: 'Ca(OH)₂', decoys: ['HCl', 'NaOH', 'HNO₃', 'KOH', 'Mg(OH)₂', 'HBr'] },
      { mode: 1, salt: 'MgCl₂', saltName: 'แมกนีเซียมคลอไรด์', acid: 'HCl', base: 'Mg(OH)₂', decoys: ['H₂SO₄', 'Ca(OH)₂', 'HNO₃', 'NaOH', 'KOH', 'H₃PO₄'] },
      { mode: 1, salt: 'K₂SO₄', saltName: 'โพแทสเซียมซัลเฟต', acid: 'H₂SO₄', base: 'KOH', decoys: ['HCl', 'NaOH', 'HNO₃', 'Ca(OH)₂', 'NH₃', 'CH₃COOH'] },
      { mode: 1, salt: 'NH₄Cl', saltName: 'แอมโมเนียมคลอไรด์', acid: 'HCl', base: 'NH₃', decoys: ['H₂SO₄', 'NaOH', 'HNO₃', 'KOH', 'Ca(OH)₂', 'CH₃COOH'] },
      { mode: 1, salt: 'CH₃COONa', saltName: 'โซเดียมอะซิเตต', acid: 'CH₃COOH', base: 'NaOH', decoys: ['HCl', 'KOH', 'H₂SO₄', 'Ca(OH)₂', 'HNO₃', 'NH₃'] },
      { mode: 1, salt: 'Na₂SO₄', saltName: 'โ���เดียมซัลเฟต', acid: 'H₂SO₄', base: 'NaOH', decoys: ['HCl', 'KOH', 'HNO₃', 'Ca(OH)₂', 'NH₃', 'Mg(OH)₂'] },
      { mode: 1, salt: 'Ca(NO₃)₂', saltName: 'แคลเซียมไนเตรต', acid: 'HNO₃', base: 'Ca(OH)₂', decoys: ['HCl', 'NaOH', 'H₂SO₄', 'KOH', 'NH₃', 'CH₃COOH'] },
      { mode: 1, salt: 'MgSO₄', saltName: 'แมกนีเซียมซัลเฟต', acid: 'H₂SO₄', base: 'Mg(OH)₂', decoys: ['HCl', 'NaOH', 'HNO₃', 'KOH', 'Ca(OH)₂', 'NH₃'] },

      // โหมด 2: กำหนดกรดและเบส ทายเกลือ (10 ข้อ)
      { mode: 2, acid: 'HCl', base: 'NaOH', salt: 'NaCl', decoys: ['KCl', 'Na₂SO₄', 'NaBr', 'NaNO₃', 'K₂SO₄', 'MgCl₂'] },
      { mode: 2, acid: 'H₂SO₄', base: 'KOH', salt: 'K₂SO₄', decoys: ['KCl', 'Na₂SO₄', 'KNO₃', 'K₃PO₄', 'MgSO₄', 'CaSO₄'] },
      { mode: 2, acid: 'HNO₃', base: 'Ca(OH)₂', salt: 'Ca(NO₃)₂', decoys: ['CaCl₂', 'CaSO₄', 'Mg(NO₃)₂', 'KNO₃', 'NaNO₃', 'CaCO₃'] },
      { mode: 2, acid: 'HCl', base: 'Mg(OH)₂', salt: 'MgCl₂', decoys: ['MgSO₄', 'CaCl₂', 'NaCl', 'KCl', 'Mg(NO₃)₂', 'MgBr₂'] },
      { mode: 2, acid: 'H₂SO₄', base: 'NaOH', salt: 'Na₂SO₄', decoys: ['NaCl', 'K₂SO₄', 'Na₂CO₃', 'NaNO₃', 'MgSO₄', 'CaSO₄'] },
      { mode: 2, acid: 'HNO₃', base: 'KOH', salt: 'KNO₃', decoys: ['KCl', 'K₂SO₄', 'NaNO₃', 'Ca(NO₃)₂', 'KBr', 'K₃PO₄'] },
      { mode: 2, acid: 'HCl', base: 'NH₃', salt: 'NH₄Cl', decoys: ['NaCl', 'KCl', '(NH₄)₂SO₄', 'NH₄NO₃', 'MgCl₂', 'CaCl₂'] },
      { mode: 2, acid: 'CH₃COOH', base: 'NaOH', salt: 'CH₃COONa', decoys: ['NaCl', 'Na₂SO₄', 'NaNO₃', 'Na₂CO₃', 'NaBr', 'Na₃PO₄'] },
      { mode: 2, acid: 'HBr', base: 'KOH', salt: 'KBr', decoys: ['KCl', 'NaBr', 'KNO₃', 'K₂SO₄', 'CaBr₂', 'MgBr₂'] },
      { mode: 2, acid: 'H₂SO₄', base: 'Mg(OH)₂', salt: 'MgSO₄', decoys: ['MgCl₂', 'CaSO₄', 'Na₂SO₄', 'K₂SO₄', 'Mg(NO₃)₂', 'MgBr₂'] }
    ];

    let currentMode = null;
    let currentQuestion = 0;
    let score = 0;
    let playerName = '';
    let selectedAcid = null;
    let selectedBase = null;
    let selectedSalt = null;
    let allScores = [];
    let isAnswering = false;
    let selectedAnimalColor = '#FFB347';
    let selectedAnimalType = 'cat';

    function selectAnimalColor(color) {
      selectedAnimalColor = color;
      
      document.querySelectorAll('.color-btn').forEach(btn => {
        btn.classList.remove('active');
        if (btn.getAttribute('data-color') === color) {
          btn.classList.add('active');
        }
      });
      
      updatePreviewAnimal();
    }

    function selectAnimalType(type) {
      selectedAnimalType = type;
      
      document.querySelectorAll('.hair-btn').forEach(btn => {
        btn.classList.remove('active');
      });
      event.target.classList.add('active');
      
      updatePreviewAnimal();
    }

    function updatePreviewAnimal() {
      const head = document.getElementById('previewHead');
      const body = document.getElementById('previewBody');
      const earLeft = document.getElementById('previewEarLeft');
      const earRight = document.getElementById('previewEarRight');
      const legs = [
        document.getElementById('previewLeg1'),
        document.getElementById('previewLeg2'),
        document.getElementById('previewLeg3'),
        document.getElementById('previewLeg4')
      ];
      const tail = document.getElementById('previewTail');

      head.style.background = selectedAnimalColor;
      body.style.background = selectedAnimalColor;
      legs.forEach(leg => leg.style.background = selectedAnimalColor);
      tail.style.background = selectedAnimalColor;

      head.className = 'animal-head ' + selectedAnimalType;
      earLeft.className = 'animal-ear ' + selectedAnimalType + ' left';
      earRight.className = 'animal-ear ' + selectedAnimalType + ' right';

      if (selectedAnimalType === 'cat') {
        earLeft.style.borderBottomColor = selectedAnimalColor;
        earRight.style.borderBottomColor = selectedAnimalColor;
        earLeft.style.background = '';
        earRight.style.background = '';
      } else {
        earLeft.style.background = selectedAnimalColor;
        earRight.style.background = selectedAnimalColor;
        earLeft.style.borderBottomColor = '';
        earRight.style.borderBottomColor = '';
      }
    }

    const dataHandler = {
      onDataChanged(data) {
        allScores = data.sort((a, b) => b.score - a.score).slice(0, 10);
        renderLeaderboard();
      }
    };

    async function initSDKs() {
      const initResult = await window.dataSdk.init(dataHandler);
      if (!initResult.isOk) {
        console.error('Failed to initialize data SDK');
      }

      if (window.elementSdk) {
        window.elementSdk.init({
          defaultConfig: defaultConfig,
          onConfigChange: async (config) => {
            document.getElementById('gameTitle').textContent = config.game_title || defaultConfig.game_title;
            document.getElementById('subtitle').textContent = config.subtitle || defaultConfig.subtitle;
            document.getElementById('mode1Title').textContent = config.mode1_button || defaultConfig.mode1_button;
            document.getElementById('mode2Title').textContent = config.mode2_button || defaultConfig.mode2_button;
          },
          mapToCapabilities: (config) => ({
            recolorables: [],
            borderables: [],
            fontEditable: undefined,
            fontSizeable: undefined
          }),
          mapToEditPanelValues: (config) => new Map([
            ['game_title', config.game_title || defaultConfig.game_title],
            ['subtitle', config.subtitle || defaultConfig.subtitle],
            ['mode1_button', config.mode1_button || defaultConfig.mode1_button],
            ['mode2_button', config.mode2_button || defaultConfig.mode2_button]
          ])
        });
      }
    }

    function startGame() {
      const nameInput = document.getElementById('playerNameInput');
      playerName = nameInput.value.trim();

      if (!playerName) {
        showInlineMessage('กรุณากรอกชื่อผู้เล่น', 'error');
        return;
      }

      document.getElementById('playerInputSection').classList.add('hidden');
      document.getElementById('gameContent').classList.remove('hidden');
      
      const characterElement = document.getElementById('playerCharacter');
      characterElement.classList.remove('hidden');
      document.getElementById('characterName').textContent = playerName;
      
      updateCharacterAnimal();
    }

    function updateCharacterAnimal() {
      const head = document.getElementById('characterHead');
      const body = document.getElementById('characterBody');
      const earLeft = document.getElementById('characterEarLeft');
      const earRight = document.getElementById('characterEarRight');
      const legs = [
        document.getElementById('characterLeg1'),
        document.getElementById('characterLeg2'),
        document.getElementById('characterLeg3'),
        document.getElementById('characterLeg4')
      ];
      const tail = document.getElementById('characterTail');

      head.style.background = selectedAnimalColor;
      body.style.background = selectedAnimalColor;
      legs.forEach(leg => leg.style.background = selectedAnimalColor);
      tail.style.background = selectedAnimalColor;

      head.className = 'animal-head ' + selectedAnimalType;
      earLeft.className = 'animal-ear ' + selectedAnimalType + ' left';
      earRight.className = 'animal-ear ' + selectedAnimalType + ' right';

      if (selectedAnimalType === 'cat') {
        earLeft.style.borderBottomColor = selectedAnimalColor;
        earRight.style.borderBottomColor = selectedAnimalColor;
        earLeft.style.background = '';
        earRight.style.background = '';
      } else {
        earLeft.style.background = selectedAnimalColor;
        earRight.style.background = selectedAnimalColor;
        earLeft.style.borderBottomColor = '';
        earRight.style.borderBottomColor = '';
      }
    }

    function selectMode(mode) {
      currentMode = mode;
      currentQuestion = 0;
      score = 0;

      document.getElementById('mode1Card').classList.remove('active');
      document.getElementById('mode2Card').classList.remove('active');

      if (mode === 1) {
        document.getElementById('mode1Card').classList.add('active');
      } else {
        document.getElementById('mode2Card').classList.add('active');
      }

      document.getElementById('mode1Card').style.pointerEvents = 'none';
      document.getElementById('mode2Card').style.pointerEvents = 'none';
      document.getElementById('mode1Card').style.opacity = '0.6';
      document.getElementById('mode2Card').style.opacity = '0.6';
      
      if (mode === 1) {
        document.getElementById('mode1Card').style.opacity = '1';
      } else {
        document.getElementById('mode2Card').style.opacity = '1';
      }

      document.getElementById('gameArea').classList.remove('hidden');
      loadQuestion();
    }

    function loadQuestion() {
      const filteredQuestions = questions.filter(q => q.mode === currentMode);
      
      if (currentQuestion >= filteredQuestions.length) {
        endGame();
        return;
      }

      const question = filteredQuestions[currentQuestion];
      
      document.getElementById('questionNumber').textContent = `คำถามที่ ${currentQuestion + 1}/${filteredQuestions.length}`;
      document.getElementById('scoreDisplay').textContent = `คะแนน: ${score}`;
      document.getElementById('resultMessage').innerHTML = '';

      selectedAcid = null;
      selectedBase = null;
      selectedSalt = null;
      isAnswering = false;

      if (currentMode === 1) {
        document.getElementById('mode1Content').classList.remove('hidden');
        document.getElementById('mode2Content').classList.add('hidden');
        loadMode1Question(question);
      } else {
        document.getElementById('mode1Content').classList.add('hidden');
        document.getElementById('mode2Content').classList.remove('hidden');
        loadMode2Question(question);
      }
    }

    function loadMode1Question(question) {
      document.getElementById('saltFormula').textContent = question.salt;
      document.getElementById('saltName').textContent = question.saltName;

      document.getElementById('acidDropped').innerHTML = '';
      document.getElementById('baseDropped').innerHTML = '';
      document.getElementById('acidDropZone').classList.remove('filled', 'correct', 'incorrect');
      document.getElementById('baseDropZone').classList.remove('filled', 'correct', 'incorrect');
      
      document.getElementById('acidDropZone').addEventListener('click', () => {
        if (selectedAcid && !selectedBase && !isAnswering) {
          removeAnswerFromZone('acid');
        }
      });
      
      document.getElementById('baseDropZone').addEventListener('click', () => {
        if (selectedBase && !selectedAcid && !isAnswering) {
          removeAnswerFromZone('base');
        }
      });

      const allChoices = [question.acid, question.base, ...question.decoys];
      shuffleArray(allChoices);

      const choicesGrid = document.getElementById('choicesGrid');
      choicesGrid.innerHTML = '';

      allChoices.forEach(choice => {
        const choiceDiv = document.createElement('div');
        choiceDiv.className = 'choice-item';
        choiceDiv.textContent = choice;
        choiceDiv.draggable = true;
        
        if (choice.includes('H') && !choice.includes('OH')) {
          choiceDiv.classList.add('acid');
        } else if (choice.includes('OH') || choice === 'NH₃') {
          choiceDiv.classList.add('base');
        }

        choiceDiv.addEventListener('dragstart', (e) => {
          if (!isAnswering) {
            e.dataTransfer.setData('text/plain', choice);
            choiceDiv.classList.add('dragging');
          }
        });

        choiceDiv.addEventListener('dragend', () => {
          choiceDiv.classList.remove('dragging');
        });

        choiceDiv.addEventListener('click', () => {
          if (!choiceDiv.classList.contains('used') && !isAnswering && !(selectedAcid && selectedBase)) {
            handleChoiceClick(choice, choiceDiv);
          }
        });

        choicesGrid.appendChild(choiceDiv);
      });

      setupDropZones();
    }

    function setupDropZones() {
      const acidZone = document.getElementById('acidDropZone');
      const baseZone = document.getElementById('baseDropZone');

      [acidZone, baseZone].forEach(zone => {
        zone.addEventListener('dragover', (e) => {
          if (!isAnswering) {
            e.preventDefault();
            zone.classList.add('drag-over');
          }
        });

        zone.addEventListener('dragleave', () => {
          zone.classList.remove('drag-over');
        });

        zone.addEventListener('drop', (e) => {
          if (!isAnswering) {
            e.preventDefault();
            zone.classList.remove('drag-over');
            
            const data = e.dataTransfer.getData('text/plain');
            handleDrop(data, zone.id);
          }
        });
      });
    }

    function removeAnswerFromZone(zoneType) {
      const choiceElements = document.querySelectorAll('.choice-item');
      
      if (zoneType === 'acid' && selectedAcid) {
        const answerToRemove = selectedAcid;
        selectedAcid = null;
        document.getElementById('acidDropped').innerHTML = '';
        document.getElementById('acidDropZone').classList.remove('filled');
        
        choiceElements.forEach(el => {
          if (el.textContent === answerToRemove) {
            el.classList.remove('used');
          }
        });
      } else if (zoneType === 'base' && selectedBase) {
        const answerToRemove = selectedBase;
        selectedBase = null;
        document.getElementById('baseDropped').innerHTML = '';
        document.getElementById('baseDropZone').classList.remove('filled');
        
        choiceElements.forEach(el => {
          if (el.textContent === answerToRemove) {
            el.classList.remove('used');
          }
        });
      }
    }

    function handleChoiceClick(choice, element) {
      if (!selectedAcid) {
        selectedAcid = choice;
        element.classList.add('used');
        document.getElementById('acidDropped').innerHTML = `<span class="dropped-item">${choice}</span>`;
        document.getElementById('acidDropZone').classList.add('filled');
      } else if (!selectedBase) {
        selectedBase = choice;
        element.classList.add('used');
        document.getElementById('baseDropped').innerHTML = `<span class="dropped-item">${choice}</span>`;
        document.getElementById('baseDropZone').classList.add('filled');
        
        checkAnswerMode1();
      }
    }

    function handleDrop(choice, zoneId) {
      if (selectedAcid && selectedBase) {
        return;
      }

      const choiceElements = document.querySelectorAll('.choice-item');
      let choiceElement = null;
      
      choiceElements.forEach(el => {
        if (el.textContent === choice && !el.classList.contains('used')) {
          choiceElement = el;
        }
      });

      if (!choiceElement) return;

      if (zoneId === 'acidDropZone' && !selectedAcid) {
        selectedAcid = choice;
        choiceElement.classList.add('used');
        document.getElementById('acidDropped').innerHTML = `<span class="dropped-item">${choice}</span>`;
        document.getElementById('acidDropZone').classList.add('filled');
        
        if (selectedBase) {
          checkAnswerMode1();
        }
      } else if (zoneId === 'baseDropZone' && !selectedBase) {
        selectedBase = choice;
        choiceElement.classList.add('used');
        document.getElementById('baseDropped').innerHTML = `<span class="dropped-item">${choice}</span>`;
        document.getElementById('baseDropZone').classList.add('filled');
        
        if (selectedAcid) {
          checkAnswerMode1();
        }
      }
    }

    function loadMode2Question(question) {
      document.getElementById('reactantsFormula').textContent = `${question.acid} + ${question.base} →`;

      const allSalts = [question.salt, ...question.decoys];
      shuffleArray(allSalts);

      const saltGrid = document.getElementById('saltChoicesGrid');
      saltGrid.innerHTML = '';

      allSalts.forEach(salt => {
        const saltDiv = document.createElement('div');
        saltDiv.className = 'choice-item salt';
        saltDiv.textContent = salt;
        
        saltDiv.addEventListener('click', () => {
          if (!isAnswering) {
            selectedSalt = salt;
            checkAnswerMode2();
          }
        });

        saltGrid.appendChild(saltDiv);
      });
    }

    function checkAnswerMode1() {
      isAnswering = true;
      const filteredQuestions = questions.filter(q => q.mode === currentMode);
      const question = filteredQuestions[currentQuestion];
      
      const isCorrect = selectedAcid === question.acid && selectedBase === question.base;

      const resultDiv = document.getElementById('resultMessage');
      const acidZone = document.getElementById('acidDropZone');
      const baseZone = document.getElementById('baseDropZone');
      
      document.querySelectorAll('.choice-item').forEach(el => {
        el.classList.add('disabled');
      });

      if (isCorrect) {
        score += 10;
        resultDiv.className = 'result-message correct';
        resultDiv.textContent = '✅ ถูก��้อง! +10 คะแนน';
        acidZone.classList.add('correct');
        baseZone.classList.add('correct');
      } else {
        resultDiv.className = 'result-message incorrect';
        resultDiv.textContent = `❌ ไม่ถูกต้อง คำตอบที่ถูกคือ ${question.acid} + ${question.base}`;
        acidZone.classList.add('incorrect');
        baseZone.classList.add('incorrect');
      }

      document.getElementById('scoreDisplay').textContent = `คะแนน: ${score}`;

      setTimeout(() => {
        currentQuestion++;
        loadQuestion();
      }, 2000);
    }

    function checkAnswerMode2() {
      isAnswering = true;
      const filteredQuestions = questions.filter(q => q.mode === currentMode);
      const question = filteredQuestions[currentQuestion];
      
      const isCorrect = selectedSalt === question.salt;

      const resultDiv = document.getElementById('resultMessage');
      
      document.querySelectorAll('.choice-item').forEach(el => {
        el.classList.add('disabled');
      });

      document.querySelectorAll('#saltChoicesGrid .choice-item').forEach(el => {
        if (el.textContent === selectedSalt) {
          if (isCorrect) {
            el.style.background = '#d4edda';
            el.style.borderColor = '#28a745';
          } else {
            el.style.background = '#f8d7da';
            el.style.borderColor = '#dc3545';
          }
        }
        if (el.textContent === question.salt && !isCorrect) {
          el.style.background = '#d4edda';
          el.style.borderColor = '#28a745';
        }
      });

      if (isCorrect) {
        score += 10;
        resultDiv.className = 'result-message correct';
        resultDiv.textContent = '✅ ถูกต้อง! +10 คะแนน';
      } else {
        resultDiv.className = 'result-message incorrect';
        resultDiv.textContent = `❌ ไม่ถูกต้อง คำตอบที่ถูกคือ ${question.salt}`;
      }

      document.getElementById('scoreDisplay').textContent = `คะแนน: ${score}`;

      setTimeout(() => {
        currentQuestion++;
        loadQuestion();
      }, 2000);
    }

    async function endGame() {
      const modeText = currentMode === 1 ? 'โหมด 1' : 'โหมด 2';
      
      if (allScores.length >= 999) {
        showInlineMessage('ไม่สามารถบันทึกคะแนนได้ เนื่องจากข้อมูลเต็มแล้ว (999 รายการ)', 'error');
      } else {
        document.getElementById('gameArea').innerHTML = `
          <div style="text-align: center; padding: 40px;">
            <div class="loading">กำลังบันทึกคะแนน...</div>
          </div>
        `;

        const result = await window.dataSdk.create({
          id: Date.now().toString(),
          player_name: playerName,
          score: score,
          mode: modeText,
          completed_at: new Date().toISOString()
        });

        if (!result.isOk) {
          showInlineMessage('เกิดข้อผิดพลาดในการบันทึกคะแนน', 'error');
        }

        await new Promise(resolve => setTimeout(resolve, 1000));
      }

      const competitionSummary = generateCompetitionSummary();

      document.getElementById('gameArea').innerHTML = `
        <div style="text-align: center; padding: 40px;">
          <h2 style="font-size: 32px; color: #667eea; margin-bottom: 20px;">🎉 เกมจบแล้ว!</h2>
          <p style="font-size: 24px; margin-bottom: 30px;">คะแนนของคุณ: <strong>${score}</strong> คะแนน</p>
          ${competitionSummary}
          <button class="btn btn-primary" onclick="location.reload()">เล่นอีกครั้ง</button>
        </div>
      `;
    }

    function generateCompetitionSummary() {
      const modeText = currentMode === 1 ? 'โหมด 1' : 'โหมด 2';
      const sameModeScores = allScores.filter(s => s.mode === modeText);
      
      if (sameModeScores.length === 0) {
        return '<p style="font-size: 16px; color: #666; margin-bottom: 20px;">คุณเป็นคนแรกที่เล่นโหมดนี���!</p>';
      }

      const playerRank = sameModeScores.findIndex(s => 
        s.player_name === playerName && 
        Math.abs(s.score - score) < 0.01 &&
        s.mode === modeText
      ) + 1;
      
      const totalPlayers = sameModeScores.length;
      
      const topScore = sameModeScores[0].score;
      const averageScore = Math.round(sameModeScores.reduce((sum, s) => sum + s.score, 0) / totalPlayers);
      
      let rankEmoji = '🏅';
      let rankColor = '#667eea';
      let rankMessage = '';
      
      if (playerRank === 1) {
        rankEmoji = '🥇';
        rankColor = '#FFD700';
        rankMessage = 'ยอดเยี่ยม! คุณได้อันดับ 1';
      } else if (playerRank === 2) {
        rankEmoji = '🥈';
        rankColor = '#C0C0C0';
        rankMessage = 'เยี่ยมมาก! คุณได้อันดับ 2';
      } else if (playerRank === 3) {
        rankEmoji = '🥉';
        rankColor = '#CD7F32';
        rankMessage = 'ดีมาก! คุณได้อันดับ 3';
      } else if (playerRank <= 5) {
        rankEmoji = '⭐';
        rankColor = '#667eea';
        rankMessage = `เก่งมาก! คุณอยู่ใน Top 5 (อันดับ ${playerRank})`;
      } else if (playerRank > 0) {
        rankEmoji = '🏅';
        rankColor = '#666';
        rankMessage = `คุณอยู่อันดับที่ ${playerRank} จาก ${totalPlayers} คน`;
      } else {
        rankEmoji = '🏅';
        rankColor = '#666';
        rankMessage = `มีผู้เล่น ${totalPlayers} คนในโหมดนี้`;
      }

      let comparisonText = '';
      if (score > averageScore) {
        const diff = score - averageScore;
        comparisonText = `<p style="color: #28a745; font-size: 16px; margin: 10px 0;">📈 สูงกว่าค่าเฉลี่ย ${diff} คะแนน!</p>`;
      } else if (score < averageScore) {
        const diff = averageScore - score;
        comparisonText = `<p style="color: #ff9800; font-size: 16px; margin: 10px 0;">📊 ต่ำกว่าค่าเฉลี่ย ${diff} คะแนน</p>`;
      } else {
        comparisonText = `<p style="color: #667eea; font-size: 16px; margin: 10px 0;">📊 ตรงกับค่าเฉลี่ยพอดี!</p>`;
      }

      return `
        <div style="background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%); padding: 25px; border-radius: 15px; margin-bottom: 30px; box-shadow: 0 5px 15px rgba(0,0,0,0.1);">
          <h3 style="font-size: 24px; color: ${rankColor}; margin-bottom: 15px;">
            ${rankEmoji} ${rankMessage}
          </h3>
          
          <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(150px, 1fr)); gap: 15px; margin-top: 20px;">
            <div style="background: white; padding: 15px; border-radius: 10px; box-shadow: 0 2px 8px rgba(0,0,0,0.1);">
              <div style="font-size: 14px; color: #666; margin-bottom: 5px;">จำนวนผู้เล่น</div>
              <div style="font-size: 24px; font-weight: bold; color: #667eea;">${totalPlayers}</div>
            </div>
            
            <div style="background: white; padding: 15px; border-radius: 10px; box-shadow: 0 2px 8px rgba(0,0,0,0.1);">
              <div style="font-size: 14px; color: #666; margin-bottom: 5px;">คะแนนสูงสุด</div>
              <div style="font-size: 24px; font-weight: bold; color: #28a745;">${topScore}</div>
            </div>
            
            <div style="background: white; padding: 15px; border-radius: 10px; box-shadow: 0 2px 8px rgba(0,0,0,0.1);">
              <div style="font-size: 14px; color: #666; margin-bottom: 5px;">คะแนนเฉลี่ย</div>
              <div style="font-size: 24px; font-weight: bold; color: #ff9800;">${averageScore}</div>
            </div>
          </div>
          
          ${comparisonText}
          
          ${score === topScore ? '<p style="font-size: 18px; color: #28a745; font-weight: bold; margin-top: 15px;">🎊 คุณทำคะแนนสูงสุด!</p>' : ''}
        </div>
      `;
    }

    function renderLeaderboard() {
      const list = document.getElementById('leaderboardList');
      
      if (allScores.length === 0) {
        list.innerHTML = '<li class="loading">ยังไม่มีคะแนน</li>';
        return;
      }

      list.innerHTML = allScores.map((record, index) => `
        <li class="leaderboard-item">
          <span class="leaderboard-rank">${index + 1}</span>
          <span class="leaderboard-name">${record.player_name}</span>
          <span class="leaderboard-score">${record.score} คะแนน (${record.mode})</span>
        </li>
      `).join('');
    }

    function shuffleArray(array) {
      for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
      }
    }

    function showInlineMessage(message, type) {
      const messageDiv = document.createElement('div');
      messageDiv.style.cssText = `
        position: fixed;
        top: 20px;
        left: 50%;
        transform: translateX(-50%);
        padding: 15px 30px;
        background: ${type === 'error' ? '#f8d7da' : '#d4edda'};
        color: ${type === 'error' ? '#721c24' : '#155724'};
        border: 2px solid ${type === 'error' ? '#f5c6cb' : '#c3e6cb'};
        border-radius: 10px;
        font-weight: bold;
        z-index: 1000;
        animation: slideIn 0.5s ease;
      `;
      messageDiv.textContent = message;
      document.body.appendChild(messageDiv);

      setTimeout(() => {
        messageDiv.remove();
      }, 3000);
    }

    initSDKs();
  </script>
 <script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'9a7c513823039e41',t:'MTc2NDY5NDE2My4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
