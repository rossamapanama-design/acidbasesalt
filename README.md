<html lang="th">
<head>
  <meta charset="UTF-8">
  <title>เกมฉันเกิดมาจากใคร: ปฏิกิริยาระหว่างกรดและเบส</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <!-- Main styles for the single page app -->
  <style>
    /*=== Base styles ===*/
    body {
      font-family: 'Sarabun', 'Noto Sans Thai', Arial, sans-serif;
      background: linear-gradient(135deg, #f2faff 0%, #f7e9ff 100%);
      margin: 0;
      padding: 0;
      color: #222;
    }
    .container {
      max-width: 480px;
      margin: 32px auto;
      background: white;
      border-radius: 1.5em;
      box-shadow: 0 2px 24px #bbb5f04d;
      padding: 24px 20px 32px 20px;
    }
    h1, h2 {
      text-align: center;
      margin-bottom: 0.2em;
      color: #6a1bb3;
      letter-spacing: 0.02em;
    }
    h1 {
      font-size: 2em;
      margin-bottom: 0.4em;
    }
    .subtitle {
      text-align: center;
      font-size: 1em;
      color: #3b4897cc;
      margin-bottom: 1em;
    }
    .mode-btn, .main-btn {
      display: block;
      width: 100%;
      margin: 8px 0;
      padding: 12px 0;
      font-size: 1.08em;
      font-weight: bold;
      border: none;
      border-radius: 0.8em;
      background: linear-gradient(90deg, #cdbafe 0%, #a6befa 100%);
      color: #252259;
      cursor: pointer;
      transition: background 0.15s, box-shadow 0.15s;
      box-shadow: 0 2px 8px #b5a7f04d;
    }
    .mode-btn:hover, .main-btn:hover {
      background: linear-gradient(90deg, #a6befa 0%, #cdbafe 100%);
      box-shadow: 0 2px 14px #b5a7f0aa;
    }
    .question-area {
      margin-top: 18px;
      margin-bottom: 8px;
    }
    .equation-box {
      background: #f3f1fa;
      border: 1.2px solid #dbcef5;
      border-radius: 0.7em;
      padding: 0.7em 1em;
      font-family: "Menlo", "Consolas", "monospace";
      font-size: 1.06em;
      color: #3c2786;
      margin-bottom: 1em;
      text-align: center;
    }
    .options-list {
      display: flex;
      flex-direction: column;
      gap: 0.6em;
    }
    .option-btn {
      padding: 10px 8px;
      border: 1px solid #c1c1ff;
      background: #f7f7faf7;
      border-radius: 0.7em;
      cursor: pointer;
      text-align: left;
      font-size: 1em;
      transition: background 0.16s, border 0.14s;
    }
    .option-btn:hover {
      background: #e8e4fc;
      border-color: #a38be8;
    }
    .disabled {
      pointer-events: none;
      opacity: 0.6;
    }
    .question-number {
      font-size: 0.98em;
      color: #8759da;
      margin-bottom: 0.1em;
      text-align: center;
    }
    .score {
      text-align: center;
      font-size: 1.15em;
      font-weight: bold;
      color: #279f3f;
      margin-bottom: 0.8em;
    }
    .feedback {
      text-align: center;
      font-size: 1.08em;
      font-weight: 600;
      color: #193a86;
      margin-bottom: 0.7em;
    }
    .summary-list {
      width: 100%;
      border-collapse: collapse;
      margin-top: 8px;
      font-size: 0.99em;
    }
    .summary-list th,
    .summary-list td {
      padding: 5px 8px;
      text-align: left;
      border-bottom: 1px solid #e1dffe;
    }
    .summary-list th {
      background: #ece6f9;
      font-weight: 600;
    }
    .summary-list td {
      background: #faf9fe;
    }
    .correct {
      color: #279f3f;
      font-weight: bold;
    }
    .incorrect {
      color: #b83232;
      font-weight: bold;
    }
    .footer {
      text-align: center;
      color: #aaa;
      font-size: 0.97em;
      margin-top: 2em;
    }
    @media (max-width: 520px) {
      .container {
        margin: 12px;
        padding: 14px 6px 18px 6px;
      }
      h1 {
        font-size: 1.37em;
      }
    }
  </style>
</head>
<body>
<div class="container" id="app">
  <!-- App contents rendered by JS -->
</div>
<div class="footer">
  &copy; 2025 <span style="color:#6a1bb3;">ครูเคมีไทย</span> | Educational Demo
</div>

<script>
/* ========= Chemistry Data Bank (Thai–English) ========= */
// Acid–Base pair and salt items for the game, minimum 20
const acidBasePairs = [
  // id, acid (th+en), base (th+en), salt (th+en), equation
  {
    id: 1,
    acidNameTh: "กรดไฮโดรคลอริก (Hydrochloric acid)",
    acidFormula: "HCl",
    baseNameTh: "โซเดียมไฮดรอกไซด์ (Sodium hydroxide)",
    baseFormula: "NaOH",
    saltNameTh: "โซเดียมคลอไรด์ (Sodium chloride)",
    saltFormula: "NaCl",
    equation: "HCl + NaOH → NaCl + H₂O"
  },
  {
    id: 2,
    acidNameTh: "กรดกำมะถัน (Sulfuric acid)",
    acidFormula: "H₂SO₄",
    baseNameTh: "โพแทสเซียมไฮดรอกไซด์ (Potassium hydroxide)",
    baseFormula: "KOH",
    saltNameTh: "โพแทสเซียมซัลเฟต (Potassium sulfate)",
    saltFormula: "K₂SO₄",
    equation: "H₂SO₄ + 2KOH → K₂SO₄ + 2H₂O"
  },
  {
    id: 3,
    acidNameTh: "กรดไนตริก (Nitric acid)",
    acidFormula: "HNO₃",
    baseNameTh: "แคลเซียมไฮดรอกไซด์ (Calcium hydroxide)",
    baseFormula: "Ca(OH)₂",
    saltNameTh: "แคลเซียมไนเตรต (Calcium nitrate)",
    saltFormula: "Ca(NO₃)₂",
    equation: "2HNO₃ + Ca(OH)₂ → Ca(NO₃)₂ + 2H₂O"
  },
  {
    id: 4,
    acidNameTh: "กรดอะซิติก (Acetic acid)",
    acidFormula: "CH₃COOH",
    baseNameTh: "โซเดียมไฮดรอกไซด์ (Sodium hydroxide)",
    baseFormula: "NaOH",
    saltNameTh: "โซเดียมอะซิเตต (Sodium acetate)",
    saltFormula: "CH₃COONa",
    equation: "CH₃COOH + NaOH → CH₃COONa + H₂O"
  },
  {
    id: 5,
    acidNameTh: "กรดไฮโดรคลอริก",
    acidFormula: "HCl",
    baseNameTh: "แคลเซียมไฮดรอกไซด์ (Calcium hydroxide)",
    baseFormula: "Ca(OH)₂",
    saltNameTh: "แคลเซียมคลอไรด์ (Calcium chloride)",
    saltFormula: "CaCl₂",
    equation: "2HCl + Ca(OH)₂ → CaCl₂ + 2H₂O"
  },
  {
    id: 6,
    acidNameTh: "กรดกำมะถัน",
    acidFormula: "H₂SO₄",
    baseNameTh: "โซเดียมไฮดรอกไซด์",
    baseFormula: "NaOH",
    saltNameTh: "โซเดียมซัลเฟต (Sodium sulfate)",
    saltFormula: "Na₂SO₄",
    equation: "H₂SO₄ + 2NaOH → Na₂SO₄ + 2H₂O"
  },
  {
    id: 7,
    acidNameTh: "กรดไนตริก",
    acidFormula: "HNO₃",
    baseNameTh: "โพแทสเซียมไฮดรอกไซด์",
    baseFormula: "KOH",
    saltNameTh: "โพแทสเซียมไนเตรต (Potassium nitrate)",
    saltFormula: "KNO₃",
    equation: "HNO₃ + KOH → KNO₃ + H₂O"
  },
  {
    id: 8,
    acidNameTh: "กรดฟอสฟอริก (Phosphoric acid)",
    acidFormula: "H₃PO₄",
    baseNameTh: "แคลเซียมไฮดรอกไซด์",
    baseFormula: "Ca(OH)₂",
    saltNameTh: "แคลเซียมฟอสเฟต (Calcium phosphate)",
    saltFormula: "Ca₃(PO₄)₂",
    equation: "2H₃PO₄ + 3Ca(OH)₂ → Ca₃(PO₄)₂ + 6H₂O"
  },
  {
    id: 9,
    acidNameTh: "กรดคาร์บอนิก (Carbonic acid)",
    acidFormula: "H₂CO₃",
    baseNameTh: "โซเดียมไฮดรอกไซด์",
    baseFormula: "NaOH",
    saltNameTh: "โซเดียมคาร์บอเนต (Sodium carbonate)",
    saltFormula: "Na₂CO₃",
    equation: "H₂CO₃ + 2NaOH → Na₂CO₃ + 2H₂O"
  },
  {
    id: 10,
    acidNameTh: "กรดซิตริก (Citric acid)",
    acidFormula: "C₆H₈O₇",
    baseNameTh: "โปแตสเซียมไฮดรอกไซด์",
    baseFormula: "KOH",
    saltNameTh: "โปแตสเซียมซิเตรต (Potassium citrate)",
    saltFormula: "K₃C₆H₅O₇",
    equation: "C₆H₈O₇ + 3KOH → K₃C₆H₅O₇ + 3H₂O"
  },
  {
    id: 11,
    acidNameTh: "กรดอะซิติก",
    acidFormula: "CH₃COOH",
    baseNameTh: "โพแทสเซียมไฮดรอกไซด์",
    baseFormula: "KOH",
    saltNameTh: "โพแทสเซียมอะซิเตต (Potassium acetate)",
    saltFormula: "CH₃COOK",
    equation: "CH₃COOH + KOH → CH₃COOK + H₂O"
  },
  {
    id: 12,
    acidNameTh: "กรดซัลฟิวริก (Sulfurous acid)",
    acidFormula: "H₂SO₃",
    baseNameTh: "โซเดียมไฮดรอกไซด์",
    baseFormula: "NaOH",
    saltNameTh: "โซเดียมซัลไฟต์ (Sodium sulfite)",
    saltFormula: "Na₂SO₃",
    equation: "H₂SO₃ + 2NaOH → Na₂SO₃ + 2H₂O"
  },
  {
    id: 13,
    acidNameTh: "กรดไฮโดรคลอริก",
    acidFormula: "HCl",
    baseNameTh: "แมกนีเซียมไฮดรอกไซด์ (Magnesium hydroxide)",
    baseFormula: "Mg(OH)₂",
    saltNameTh: "แมกนีเซียมคลอไรด์ (Magnesium chloride)",
    saltFormula: "MgCl₂",
    equation: "2HCl + Mg(OH)₂ → MgCl₂ + 2H₂O"
  },
  {
    id: 14,
    acidNameTh: "กรดไนตริก",
    acidFormula: "HNO₃",
    baseNameTh: "แอมโมเนีย (Ammonia)",
    baseFormula: "NH₃",
    saltNameTh: "แอมโมเนียมไนเตรต (Ammonium nitrate)",
    saltFormula: "NH₄NO₃",
    equation: "HNO₃ + NH₃ → NH₄NO₃"
  },
  {
    id: 15,
    acidNameTh: "กรดฟอสฟอริก",
    acidFormula: "H₃PO₄",
    baseNameTh: "โซเดียมไฮดรอกไซด์",
    baseFormula: "NaOH",
    saltNameTh: "โซเดียมฟอสเฟต (Sodium phosphate)",
    saltFormula: "Na₃PO₄",
    equation: "H₃PO₄ + 3NaOH → Na₃PO₄ + 3H₂O"
  },
  {
    id: 16,
    acidNameTh: "กรดคาร์บอนิก",
    acidFormula: "H₂CO₃",
    baseNameTh: "แคลเซียมไฮดรอกไซด์",
    baseFormula: "Ca(OH)₂",
    saltNameTh: "แคลเซียมคาร์บอเนต (Calcium carbonate)",
    saltFormula: "CaCO₃",
    equation: "H₂CO₃ + Ca(OH)₂ → CaCO₃ + 2H₂O"
  },
  {
    id: 17,
    acidNameTh: "กรดอะซิติก",
    acidFormula: "CH₃COOH",
    baseNameTh: "แคลเซียมไฮดรอกไซด์",
    baseFormula: "Ca(OH)₂",
    saltNameTh: "แคลเซียมอะซิเตต (Calcium acetate)",
    saltFormula: "Ca(CH₃COO)₂",
    equation: "2CH₃COOH + Ca(OH)₂ → Ca(CH₃COO)₂ + 2H₂O"
  },
  {
    id: 18,
    acidNameTh: "กรดกำมะถัน",
    acidFormula: "H₂SO₄",
    baseNameTh: "แคลเซียมไฮดรอกไซด์",
    baseFormula: "Ca(OH)₂",
    saltNameTh: "แคลเซียมซัลเฟต (Calcium sulfate)",
    saltFormula: "CaSO₄",
    equation: "H₂SO₄ + Ca(OH)₂ → CaSO₄ + 2H₂O"
  },
  {
    id: 19,
    acidNameTh: "กรดไฮโดรคลอริก",
    acidFormula: "HCl",
    baseNameTh: "แอมโมเนีย",
    baseFormula: "NH₃",
    saltNameTh: "แอมโมเนียมคลอไรด์ (Ammonium chloride)",
    saltFormula: "NH₄Cl",
    equation: "HCl + NH₃ → NH₄Cl"
  },
  {
    id: 20,
    acidNameTh: "กรดซิตริก",
    acidFormula: "C₆H₈O₇",
    baseNameTh: "แคลเซียมไฮดรอกไซด์",
    baseFormula: "Ca(OH)₂",
    saltNameTh: "แคลเซียมซิเตรต (Calcium citrate)",
    saltFormula: "Ca₃(C₆H₅O₇)₂",
    equation: "2C₆H₈O₇ + 3Ca(OH)₂ → Ca₃(C₆H₅O₇)₂ + 6H₂O"
  }
];

/* ===== App State and Utility Functions ===== */

// Shuffles an array in-place (Fisher–Yates algorithm)
function shuffle(array) {
  let m = array.length, t, i;
  while (m > 0) {
    i = Math.floor(Math.random() * m--);
    t = array[m];
    array[m] = array[i];
    array[i] = t;
  }
  return array;
}

// Pick array of distinct random items
function pickRandom(array, n) {
  if (n >= array.length) return Array.from(array);
  let arrCopy = Array.from(array);
  return shuffle(arrCopy).slice(0, n);
}

// For displaying Thai + formula together
function nameAndFormula(nameTh, formula) {
  return `${nameTh} <span style="color:#4963b9">(${formula})</span>`;
}

/* ========== Main Game App ========== */

const appDom = document.getElementById('app');

const MODES = [
  {
    key: 'whoami',
    name: 'ฉันคือใคร',
    desc: 'ดูชื่อกรดและเบส แล้วตอบว่าได้เกลือชนิดไหนจากปฏิกิริยานี้',
    btn: 'โหมด “ฉันคือใคร”'
  },
  {
    key: 'whomade',
    name: 'ฉันเกิดจากใคร',
    desc: 'ดูชื่อเกลือ แล้วตอบว่ามันเกิดจากกรดและเบสชนิดใด',
    btn: 'โหมด “ฉันเกิดจากใคร”'
  }
];

// ========== Rendering Screens ==========

// Entry screen
function renderStartScreen() {
  appDom.innerHTML = `
    <h1>เกมฉันเกิดมาจากใคร:<br>ปฏิกิริยาระหว่างกรดและเบส</h1>
    <div class="subtitle">
      เลือกโหมดเกมเพื่อเริ่มต้น:<br>
      <span style="color:#193a86;font-size:0.97em">เล่นเพื่อเรียนรู้เรื่อง “เกลือ” จากการเกิดปฏิกิริยา กรด + เบส</span>
    </div>
    <button class="mode-btn" onclick="startGame('whoami')">${MODES[0].btn}</button>
    <button class="mode-btn" onclick="startGame('whomade')">${MODES[1].btn}</button>
  `;
}

// Game round state
let state = {
  mode: null,
  questions: [],
  currentQIdx: 0,
  answers: []
};

// Start game with mode
window.startGame = function(modeKey) {
  // Setup state
  const questions = pickRandom(acidBasePairs, 10);
  state = {
    mode: modeKey,
    questions,
    currentQIdx: 0,
    answers: []
  };
  renderQuestion();
};

// Return to home/reset
window.goHome = function() {
  renderStartScreen();
};

// ========== Question Rendering ==========

// Render one question for current state
function renderQuestion() {
  const qNum = state.currentQIdx + 1;
  const q = state.questions[state.currentQIdx];
  let questionHtml = `
    <div class="question-number">ข้อที่ ${qNum} / 10</div>
    <h2>
      ${state.mode === 'whoami' ? 'ฉันคือใคร?' : 'ฉันเกิดจากใคร?'}
    </h2>
    <div class="subtitle">${state.mode === 'whoami'
      ? "กรด + เบส = เกลือชนิดใด?"
      : "เกลือนี้ เกิดจากกรดและเบสชนิดใด?"
    }</div>
    <div class="question-area">
  `;

  // Who Am I mode: show acid & base
  if (state.mode === 'whoami') {
    questionHtml += `
      <div class="equation-box" style="margin-bottom:10px;">
        <span>
          ${nameAndFormula(q.acidNameTh, q.acidFormula)}
          &nbsp;+&nbsp;
          ${nameAndFormula(q.baseNameTh, q.baseFormula)}
        </span><br>
        <span style="color:#2d3285;font-size:0.95em;">
          ${q.equation}
        </span>
      </div>
      <div><strong>เลือกชื่อเกลือที่เกิดขึ้นต่อไปนี้</strong></div>
      <div class="options-list" id="options-container">
    `;
    // Build options: correct salt + 3 random salts
    let saltOptions = [q.saltNameTh + ' (' + q.saltFormula + ')'];
    // Avoid duplicate and self
    let otherSalts = acidBasePairs.filter(ab => ab.id !== q.id)
      .map(item => item.saltNameTh + ' (' + item.saltFormula + ')');
    shuffle(otherSalts);
    // Push 3 others:
    for (let i = 0; i < 3 && i < otherSalts.length; i++) saltOptions.push(otherSalts[i]);
    saltOptions = shuffle(saltOptions); // randomize option order
    saltOptions.forEach(opt =>
      questionHtml += `<button class="option-btn" onclick="chooseOption('${escapeHtml(opt)}')">${opt}</button>`
    );
    questionHtml += `</div>`;
  }
  // Who Made Me mode: show salt, choose acid & base
  else if (state.mode === 'whomade') {
    questionHtml += `
      <div class="equation-box" style="margin-bottom:10px;">
        <span>
          ${nameAndFormula(q.saltNameTh, q.saltFormula)}
        </span><br>
        <span style="color:#2d3285;font-size:0.95em;">
          ${q.equation}
        </span>
      </div>
      <div><strong>เลือกคู่ กรด + เบส ที่ทำให้เกิดเกลือนี้</strong></div>
      <div class="options-list" id="options-container">
    `;
    // Build 4 pairs: correct + 3 random pairs
    let pairOptions = [q.acidNameTh + ' (' + q.acidFormula + ') + ' + q.baseNameTh + ' (' + q.baseFormula + ')'];
    // Others, ensuring no duplicate:
    let others = acidBasePairs.filter(ab => ab.id !== q.id);
    shuffle(others);
    for (let i = 0; i < 3 && i < others.length; i++) {
      let o = others[i];
      pairOptions.push(o.acidNameTh + ' (' + o.acidFormula + ') + ' + o.baseNameTh + ' (' + o.baseFormula + ')');
    }
    pairOptions = shuffle(pairOptions);
    pairOptions.forEach(opt =>
      questionHtml += `<button class="option-btn" onclick="chooseOption('${escapeHtml(opt)}')">${opt}</button>`
    );
    questionHtml += `</div>`;
  }

  questionHtml += `
      <button class="main-btn" onclick="goHome()" style="margin-top:16px;background:#fcddeb;color:#a03851;">กลับหน้าแรก</button>
    </div>
  `;
  appDom.innerHTML = questionHtml;
}

// Helper to escape quotes in option values
function escapeHtml(str) {
  return str.replace(/'/g, "&#39;").replace(/"/g, "&quot;");
}

// Option chosen handler
window.chooseOption = function(selectedOpt) {
  const q = state.questions[state.currentQIdx];
  // What is correct answer:
  let correctOpt;
  if (state.mode === 'whoami') {
    correctOpt = q.saltNameTh + ' (' + q.saltFormula + ')';
  } else {
    correctOpt = q.acidNameTh + ' (' + q.acidFormula + ') + ' +
                 q.baseNameTh + ' (' + q.baseFormula + ')';
  }
  // Save answer for summary; ถูก/ผิด
  state.answers.push({
    question: state.mode === 'whoami'
      ? `${nameAndFormula(q.acidNameTh, q.acidFormula)} + ${nameAndFormula(q.baseNameTh, q.baseFormula)}`
      : `${nameAndFormula(q.saltNameTh, q.saltFormula)}`,
    equation: q.equation,
    playerAnswer: selectedOpt,
    playerCorrect: selectedOpt === correctOpt,
    correctAnswer: correctOpt
  });
  // Next question or summary
  state.currentQIdx += 1;
  if (state.currentQIdx < 10) {
    renderQuestion();
  } else {
    renderSummary();
  }
};

// ========== Score & Summary ==========

function renderSummary() {
  // Compute score:
  const score = state.answers.filter(a => a.playerCorrect).length;
  // Feedback
  let feedback = '';
  if (score <= 4) {
    feedback = 'ลองทบทวนเนื้อหาอีกครั้งนะ 👍';
  } else if (score <= 7) {
    feedback = 'ทำได้ดีแล้ว ลองเล่นอีกรอบเพื่อให้แม่นยำขึ้น 💪';
  } else {
    feedback = 'ยอดเยี่ยม! เก่งมาก เข้าใจเรื่องเกลือจากกรด–เบสเป็นอย่างดี 🌟';
  }
  let summaryHtml = `
    <h2>สรุปคะแนน</h2>
    <div class="score">ได้ ${score} จาก 10 คะแนน</div>
    <div class="feedback">${feedback}</div>
    <table class="summary-list">
      <thead>
        <tr>
          <th>#</th>
          <th>คำถาม</th>
          <th style="width:110px;">คำตอบของคุณ</th>
          <th style="width:55px;">ผล</th>
          <th>คำตอบที่ถูกต้อง</th>
        </tr>
      </thead>
      <tbody>
  `;
  // List each Q/A
  state.answers.forEach((a, idx) => {
    let check = a.playerCorrect
      ? `<span class="correct">ถูก</span>`
      : `<span class="incorrect">ผิด</span>`;
    summaryHtml += `
      <tr>
        <td>${idx + 1}</td>
        <td>${a.question}<br>
          <span style="color:#6a33c2;font-size:0.95em;">${a.equation}</span>
        </td>
        <td>${a.playerAnswer}</td>
        <td>${check}</td>
        <td>${a.correctAnswer}</td>
      </tr>
    `;
  });
  summaryHtml += `
      </tbody>
    </table>
    <button class="main-btn" onclick="startGame('${state.mode}')" style="margin-top:14px;">เล่นโหมดนี้อีกครั้ง</button>
    <button class="main-btn" onclick="goHome()" style="background:#fcddeb;color:#a03851;">กลับหน้าแรก</button>
  `;
  appDom.innerHTML = summaryHtml;
}

// ========== Initial Load ==========

renderStartScreen();

</script>
</body>
</html># acidbasesalt
