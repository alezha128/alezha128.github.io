layout: default
title: sigma
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>lock tf in - Quiz</title>
  <style>
    body { font-family: system-ui, -apple-system, 'Segoe UI', Roboto, Arial; background:#f7f7f8; color:#111; display:flex; align-items:center; justify-content:center; min-height:100vh; margin:0; }
    .card { background:#fff; padding:20px; border-radius:10px; box-shadow:0 6px 24px rgba(0,0,0,0.08); width:820px; max-width:95%; }
    pre.console { background:#0b1220; color:#e6f1ff; padding:12px; border-radius:6px; min-height:120px; margin:0 0 12px 0; white-space:pre-wrap; font-family:monospace; }
    .prompt { font-weight:600; margin:8px 0; }
    input[type="text"]{ width:100%; padding:8px 10px; font-size:16px; border-radius:6px; border:1px solid #ddd; box-sizing:border-box; }
    button{ margin-top:8px; padding:8px 12px; border-radius:8px; border:0; background:#0b6cff; color:white; cursor:pointer; }
    .meta { color:#666; font-size:13px; margin-bottom:8px; }
  </style>
</head>
<body>
  <div class="card">
    <div class="meta">v 1.4</div>
    <pre class="console" id="console"></pre>

    <div class="prompt" id="question"></div>
    <input type="text" id="answerInput" autocomplete="off" />
    <div style="display:flex; gap:8px;">
      <button id="submitBtn">Submit Answer</button>
      <button id="skipBtn">Skip (show answer)</button>
    </div>
    <div style="margin-top:12px; color:#444; font-size:14px;">Progress: <span id="progress">0</span>/<span id="total">0</span></div>
  </div>

  <script>
    function toLower(s){ return s.toLowerCase(); }

    const quiz = [
      {prompt: "Leader of Russia prior to 1917 Revolution", answer: "Czar Nicholas II"},
      {prompt: "Group that overthrew the traditional leader", answer: "Bolsheviks"},
      {prompt: "Leader of #2", answer: "Vladimir Lenin"},
      {prompt: "Enforcers of the new Communist regime", answer: "Secret police"},
      {prompt: "Confessions of guilt led to executions", answer: "Blood purges"},
      {prompt: "“Inventor” of communism", answer: "Karl Marx"},
      {prompt: "Leader of #2 after Lenin", answer: "Joseph Stalin"}
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
    progressEl.textContent = TOTAL - pool.length;

    appendConsole("LOCK IN\n");
    appendConsole("btw not case sensitive");
    appendConsole("use enter to submit");
    appendConsole("be aware of errors");

    function appendConsole(text){
      consoleEl.textContent += text + "\n";
      consoleEl.scrollTop = consoleEl.scrollHeight;
    }

    function randomIndex(n){
      return Math.floor(Math.random()*n);
    }

    function nextQuestion(){
      if(pool.length === 0){
        appendConsole(`ok ${correctCount} / ${TOTAL}`);
        appendConsole("lock in");
        questionEl.textContent = "Done.";
        inputEl.disabled = true;
        submitBtn.disabled = true;
        skipBtn.disabled = true;
        return;
      }
      const idx = randomIndex(pool.length);
      current = {index: idx, item: pool[idx]};
      questionEl.textContent = current.item.prompt;
      inputEl.value = "";
      inputEl.focus();
      progressEl.textContent = TOTAL - pool.length;
    }

    let current = null;

    submitBtn.addEventListener('click', ()=>{
      if(!current) return;
      const userAnswer = inputEl.value.trim();
      if(toLower(userAnswer) === toLower(current.item.answer)){
        appendConsole('correct\n');
        pool.splice(current.index, 1);
        correctCount++;
      } else {
        appendConsole('incorrect. correct answer was ' + current.item.answer + '\n');
      }
      nextQuestion();
    });

    inputEl.addEventListener('keydown', (e)=>{
      if(e.key === 'Enter'){
        e.preventDefault();
        submitBtn.click();
      }
    });

    skipBtn.addEventListener('click', ()=>{
      if(!current) return;
      appendConsole('incorrect. correct answer was ' + current.item.answer + '\n');
      nextQuestion();
    });

    nextQuestion();
  </script>
</body>
</html>
