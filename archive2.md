---
layout: default
title: sigma
permalink: /archive2.html
---

<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>lock tf in - Quiz</title>

  <style>
    body {
      margin: 0;
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      font-family: system-ui, -apple-system, 'Segoe UI', Roboto, Arial;
      background: #000;
      color: #eaeaea;
    }

    .card {
      width: 820px;
      max-width: 95%;
      background: #0b0b0f;
      border: 1px solid #1a1a1a;
      border-radius: 14px;
      padding: 20px;
      box-shadow: 0 0 25px rgba(0,255,255,0.08);
    }

    .meta {
      font-size: 13px;
      color: #888;
      margin-bottom: 10px;
    }

    pre.console {
      background: #05060a;
      color: #00f7ff;
      padding: 12px;
      border-radius: 8px;
      min-height: 120px;
      margin-bottom: 12px;
      font-family: monospace;
      overflow-y: auto;
      box-shadow: 0 0 10px rgba(0,247,255,0.15);
    }

    .prompt {
      font-weight: 600;
      margin: 10px 0;
      color: #ffffff;
      text-shadow: 0 0 8px rgba(0,255,255,0.25);
    }

    input[type="text"] {
      width: 100%;
      padding: 10px;
      border-radius: 8px;
      border: 1px solid #222;
      background: #000;
      color: #fff;
      outline: none;
    }

    input[type="text"]:focus {
      border-color: #00f7ff;
      box-shadow: 0 0 10px rgba(0,247,255,0.3);
    }

    button {
      margin-top: 10px;
      padding: 10px 14px;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      background: #00f7ff;
      color: #000;
      font-weight: 600;
      box-shadow: 0 0 12px rgba(0,247,255,0.25);
    }

    button:hover {
      box-shadow: 0 0 18px rgba(0,247,255,0.4);
    }

    .btn-row {
      display: flex;
      gap: 10px;
    }

    .progress-wrap {
      margin-top: 14px;
      font-size: 13px;
      color: #aaa;
    }

    .bar {
      width: 100%;
      height: 10px;
      background: #111;
      border-radius: 6px;
      overflow: hidden;
      margin-top: 6px;
      border: 1px solid #222;
    }

    .bar-fill {
      height: 100%;
      width: 0%;
      background: linear-gradient(90deg, #00f7ff, #7a5cff);
      box-shadow: 0 0 10px rgba(0,247,255,0.4);
      transition: width 0.25s ease;
    }
  </style>
</head>

<body>
  <div class="card">
    <div class="meta">v 1.4 neon</div>

    <pre class="console" id="console"></pre>

    <div class="prompt" id="question"></div>

    <input type="text" id="answerInput" autocomplete="off" />

    <div class="btn-row">
      <button id="submitBtn">Submit Answer</button>
      <button id="skipBtn">Skip</button>
    </div>

    <div class="progress-wrap">
      Progress: <span id="progress">0</span>/<span id="total">0</span>
      <div class="bar">
        <div class="bar-fill" id="barFill"></div>
      </div>
    </div>
  </div>

<script>
function toLower(s){ return s.toLowerCase(); }

const quiz = [
  // ------------------------- satire -------------------------
  {prompt: "Satire comes from the Latin word 'satura' meaning ______.", answer: "poetic medley"},
  {prompt: "When did the Simpsons premiere? (1989 / 1999 / 1979 / 2009)", answer: "1989"},
  {prompt: "One of the satirist’s central tools has always been __________.", answer: "exaggeration"},
  {prompt: "The aim of satire is to ______.", answer: "alert and educate the public of a problem"},
  {prompt: "“satire trains you to .....” (check all that apply)", answer: "think critically"},
  {prompt: "The plastic limits of Springfield…", answer: "circumscribe American social relations in their entirety"},
  {prompt: "The show Daria is known for its nihilism. As a result, viewers are becoming ______.", answer: "nihilistic"},

  // ------------------------- earnest -------------------------
  {prompt: "I have always suspected you of being a confirmed and secret ________.", answer: "bunburyist"},
  {prompt: "Why all these cups? Why ________? Why such reckless extravagance?", answer: "cucumber sandwiches"},
  {prompt: "Never met such a ______... Lady Bracknell is one.", answer: "gorgon"},
  {prompt: "Poor Bunbury is a dreadful _____", answer: "invalid"},
  {prompt: "Oh, Gwendolyn is as right as a ________", answer: "trivet"},
  {prompt: "Tell me the whole thing. I may mention that I have always suspected you of being a confirmed and secret ________.", answer: "bunburyist"},
  {prompt: "Only relatives, or creditors, ever ring in that ______________.", answer: "wagnerian manner"},
  {prompt: "I had some _______ with Lady Harbury, who seems to be living entirely for pleasure now.", answer: "crumpets"},
  {prompt: "_______ is your county, is it not?", answer: "shropshire"},
  {prompt: "I'm sure the program will be delightful after a few ___________.", answer: "expurgations"},
  {prompt: "She calls herself Cecily if she lives at ________?", answer: "tunbridge wells"},
  {prompt: "Please don't touch the ______. They are ordered specially for Aunt Augusta.", answer: "cucumber sandwiches"},
  {prompt: "India's currency", answer: "rupee"},
  {prompt: "Somebody who uses an imaginary friend as an excuse to get out of social responsibilities", answer: "bunburyist"},
  {prompt: "Religious group from the 16th century", answer: "anabaptists"},
  {prompt: "Graduate of Oxford university", answer: "oxonian"},
  {prompt: "Somebody shamefully immoral", answer: "profligate"},
  {prompt: "Food intended to be served with tea", answer: "cucumber sandwiches"},
  {prompt: "A sick person", answer: "invalid"},
  {prompt: "The last stop on a train line", answer: "terminus"},
  {prompt: "Goddess who served as advisor to a mythic Roman king", answer: "egeria"},
  {prompt: "AKA English muffin", answer: "crumpet"},
  {prompt: "Money that comes through work or trade rather than rich parents", answer: "purple of commerce"},
  {prompt: "Where men would put flowers through in their coat", answer: "buttonhole"},
  {prompt: "Somebody who hates mankind", answer: "misanthrope"},
  {prompt: "Somebody who hates women", answer: "womanthrope"},
  {prompt: "Yellow and fragrant rose", answer: "marechal niel"},
  {prompt: "Greek monsters with snakes for hair", answer: "gorgon"},
  {prompt: "Daily evening service in Anglican church", answer: "evensong"},
  {prompt: "Combining 2 separate words into 1", answer: "portmanteau"},
  {prompt: "Baby carriage", answer: "perambulator"},
  {prompt: "The act of burying a body", answer: "interment"},
  {prompt: "Swing from one side to the other", answer: "vacillate"},
  {prompt: "City in Kent, SE England", answer: "tunbridge wells"},
  {prompt: "Removing vulgar material from something", answer: "expurgation"},
  {prompt: "Caught up in the pursuit of unreachable goals", answer: "quixotic"},
  {prompt: "Two-wheeled horse-drawn vehicle", answer: "dogcart"},
  {prompt: "New word / new meaning for a word", answer: "neologistic"},
  {prompt: "Loud, demonstrative nature", answer: "wagnerian manner"},
  {prompt: "A county west of London known for its sheep", answer: "shropshire"},
  {prompt: "Being calm", answer: "equanimity"},
  {prompt: "Donating money, property, or time", answer: "philanthropy"},
  {prompt: "Small, three-legged table", answer: "trivet"},

  // ------------------------- LOTF + rando -------------------------
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
let current = null;

const consoleEl = document.getElementById("console");
const questionEl = document.getElementById("question");
const inputEl = document.getElementById("answerInput");
const submitBtn = document.getElementById("submitBtn");
const skipBtn = document.getElementById("skipBtn");
const progressEl = document.getElementById("progress");
const totalEl = document.getElementById("total");
const barFill = document.getElementById("barFill");

totalEl.textContent = TOTAL;

function appendConsole(t){
  consoleEl.textContent += t + "\n";
  consoleEl.scrollTop = consoleEl.scrollHeight;
}

function rand(n){
  return Math.floor(Math.random()*n);
}

function updateProgress(){
  const done = TOTAL - pool.length;
  progressEl.textContent = done;
  barFill.style.width = (done / TOTAL) * 100 + "%";
}

function nextQuestion(){
  if(pool.length === 0){
    appendConsole(`finished ${correctCount}/${TOTAL}`);
    questionEl.textContent = "Done.";
    inputEl.disabled = true;
    submitBtn.disabled = true;
    skipBtn.disabled = true;
    updateProgress();
    return;
  }

  const idx = rand(pool.length);
  current = {index: idx, item: pool[idx]};

  questionEl.textContent = current.item.prompt;
  inputEl.value = "";
  inputEl.focus();

  updateProgress();
}

submitBtn.onclick = () => {
  const val = inputEl.value.trim();
  if(toLower(val) === toLower(current.item.answer)){
    appendConsole("correct");
    pool.splice(current.index,1);
    correctCount++;
  } else {
    appendConsole("incorrect: " + current.item.answer);
  }
  nextQuestion();
};

skipBtn.onclick = () => {
  appendConsole("skipped: " + current.item.answer);
  nextQuestion();
};

inputEl.addEventListener("keydown",(e)=>{
  if(e.key === "Enter") submitBtn.click();
});

appendConsole("LOCK IN NEON MODE");
appendConsole("not case sensitive");

nextQuestion();
</script>

</body>
</html>
