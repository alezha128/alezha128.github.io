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

    <div style="margin-top:12px; color:#aaa; font-size:14px;">
      Progress: <span id="progress">0</span>/<span id="total">0</span>
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
      {prompt: "Religious group from the 16th century", answer: "anabaptists"},
      {prompt: "Graduate of Oxford university", answer: "oxonian"},
      {prompt: "Somebody shamefully immoral", answer: "profligate"},
      {prompt: "A sick person", answer: "invalid"},
      {prompt: "The last stop on a train line", answer: "terminus"},
      {prompt: "Greek monsters with snakes for hair", answer: "gorgon"},
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
      {prompt: "Being calm", answer: "equanimity"},
      {prompt: "Donating money, property, or time", answer: "philanthropy"},
      {prompt: "Small, three-legged table", answer: "trivet"},

      // ------------------------- LOTF -------------------------
      {prompt: "__________ they squared up to each other.", answer: "truculently"},
      {prompt: "MLK Jr’s ______ is celebrated in January.", answer: "martyrdom"},
      {prompt: "She pushed over the ______.", answer: "plinth"},
      {prompt: "Center of ______ when she argued.", answer: "derision"},
      {prompt: "She was remembered as a ______.", answer: "martyr"},
      {prompt: "The ______ winds blew the papers.", answer: "tempestuous"},
      {prompt: "She was ______ when she got flowers.", answer: "ebullient"},
      {prompt: "They are ______ to psychoanalysis.", answer: "impervious"},
      {prompt: "He was ______ in controversy.", answer: "blatantly"},
      {prompt: "Most ______ baby ever", answer: "succulent"},
      {prompt: "He felt ______ after crime.", answer: "contrite"},
      {prompt: "His ______ skin turned red.", answer: "pallid"},
      {prompt: "He was ______ after loss.", answer: "embroiled"},
      {prompt: "The king was ______ idiot.", answer: "obtuse"},
      {prompt: "______ body lifted desk.", answer: "sinewy"},
      {prompt: "The ______ girl blushed.", answer: "demure"},
      {prompt: "Abominable Snowman of ______.", answer: "pasadena"},
      {prompt: "The ______ ninja snuck in.", answer: "furtive"},
      {prompt: "Ralph blew the ______.", answer: "conch"},

      // ------------------------- random -------------------------
      {prompt: "Sweet sounding", answer: "mellifluous"},
      {prompt: "Hospital unit", answer: "triage"},
      {prompt: "Nepotistic leader", answer: "nepotistic"},
      {prompt: "Harsh punishment", answer: "draconian"},
      {prompt: "Vocabulary", answer: "lexicon"},
      {prompt: "Exiled king", answer: "persona non grata"},
      {prompt: "Hitler is the ______ anti-Semite.", answer: "quintessential"},
      {prompt: "Government type ideal", answer: "meritocracy"},
      {prompt: "Pirates ______ the ship.", answer: "commandeered"},
      {prompt: "Opposing idea", answer: "antithetical"},
      {prompt: "Funeral song", answer: "dirge"},
      {prompt: "Sea shell", answer: "conch"},
      {prompt: "Untrustworthy", answer: "perfidious"},
      {prompt: "Obvious", answer: "blatant"},
      {prompt: "Crash site", answer: "scar"},
      {prompt: "Cheating student", answer: "perfidious"},
      {prompt: "Cheer loudly", answer: "vociferously"}
    ];
  </script>
</body>
</html>
