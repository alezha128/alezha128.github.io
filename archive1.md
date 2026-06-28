<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>lock tf in - Quiz</title>

  <style>
    body {
      font-family: system-ui, -apple-system, 'Segoe UI', Roboto, Arial;
      background: #000;
      color: #e6e6e6;
      display: flex;
      align-items: center;
      justify-content: center;
      min-height: 100vh;
      margin: 0;
    }

    .card {
      background: #111;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 6px 24px rgba(0,0,0,0.7);
      width: 820px;
      max-width: 95%;
    }

    pre.console {
      background: #0b1220;
      color: #e6f1ff;
      padding: 12px;
      border-radius: 6px;
      min-height: 120px;
      margin: 0 0 12px 0;
      white-space: pre-wrap;
      font-family: monospace;
    }

    .prompt {
      font-weight: 600;
      margin: 8px 0;
    }

    input[type="text"] {
      width: 100%;
      padding: 8px 10px;
      font-size: 16px;
      border-radius: 6px;
      border: 1px solid #333;
      background: #000;
      color: #e6e6e6;
      box-sizing: border-box;
    }

    button {
      margin-top: 8px;
      padding: 8px 12px;
      border-radius: 8px;
      border: 0;
      background: #0b6cff;
      color: white;
      cursor: pointer;
    }

    button:hover {
      background: #2b82ff;
    }

    .meta {
      color: #aaa;
      font-size: 13px;
      margin-bottom: 8px;
    }
  </style>
</head>

<body>
  <div class="card">
    <div class="meta">v 1.3</div>

    <pre class="console" id="console"></pre>

    <div class="prompt" id="question"></div>

    <input type="text" id="answerInput" autocomplete="off" />

    <div style="display:flex; gap:8px;">
      <button id="submitBtn">Submit Answer</button>
      <button id="skipBtn">Skip (show answer)</button>
    </div>

    <div style="margin-top:12px; font-size:14px; color:#aaa;">
      Progress: <span id="progress">0</span>/<span id="total">0</span>
    </div>
  </div>

  <script>
    function toLower(s){ return s.toLowerCase(); }

    const quiz = [
      {prompt: "Satire comes from the Latin word 'satura' meaning ______.", answer: "poetic medley"},
      {prompt: "When did the Simpsons premiere? (1989 / 1999 / 1979 / 2009)", answer: "1989"},
      {prompt: "One of the satirist’s central tools has always been __________.", answer: "exaggeration"}
      // (keep your full list here unchanged)
    ];

    const TOTAL = quiz.length;
    let correctCount = 0;
    let pool = quiz.slice();

    const consoleEl = document.getElementById('console');
    const questionEl = document.getElementById('question');
    const inputEl = document.getElementById('answerInput');
    const submitBtn = document.getElementById('submitBtn');
    const skipBtn = document.getElementById('skipBtn');
    const progressEl = document.getElementById('progress');
    const totalEl = document.getElementById('total');

    totalEl.textContent = TOTAL;

    function appendConsole(text){
      consoleEl.textContent += text + "\n";
      consoleEl.scrollTop = consoleEl.scrollHeight;
    }

    function rand(n){
      return Math.floor(Math.random() * n);
    }

    let current = null;

    function nextQuestion(){
      if(pool.length === 0){
        questionEl.textContent = "Done.";
        appendConsole(`Finished: ${correctCount}/${TOTAL}`);
        inputEl.disabled = true;
        submitBtn.disabled = true;
        skipBtn.disabled = true;
        return;
      }

      const idx = rand(pool.length);
      current = {index: idx, item: pool[idx]};

      questionEl.textContent = current.item.prompt;
      inputEl.value = "";
      inputEl.focus();

      progressEl.textContent = TOTAL - pool.length;
    }

    submitBtn.addEventListener('click', ()=>{
      const val = inputEl.value.trim();

      if(toLower(val) === toLower(current.item.answer)){
        appendConsole("correct");
        pool.splice(current.index, 1);
        correctCount++;
      } else {
        appendConsole("incorrect: " + current.item.answer);
      }

      nextQuestion();
    });

    inputEl.addEventListener('keydown', (e)=>{
      if(e.key === "Enter") submitBtn.click();
    });

    skipBtn.addEventListener('click', ()=>{
      appendConsole("skipped: " + current.item.answer);
      nextQuestion();
    });

    appendConsole("LOCK IN MODE");
    appendConsole("not case sensitive");

    nextQuestion();
  </script>
</body>
</html>
