<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Quiz Game</title>
  <style>
    body {
      font-family: monospace;
      background-color: #111;
      color: #0f0;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100vh;
      margin: 0;
    }
    #console {
      width: 90%;
      max-width: 800px;
      background: #000;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 0 10px #0f0;
      overflow-y: auto;
      height: 70vh;
    }
    input {
      width: 100%;
      padding: 10px;
      margin-top: 10px;
      background: #222;
      border: 1px solid #0f0;
      color: #0f0;
      border-radius: 5px;
    }
    button {
      margin-top: 10px;
      background: #0f0;
      color: #000;
      padding: 10px 20px;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      font-weight: bold;
    }
    button:hover {
      background: #9f9;
    }
  </style>
</head>
<body>
  <div id="console"></div>
  <input type="text" id="answerInput" placeholder="Your answer here..." />
  <button id="submitBtn">Submit</button>

  <script>
    const consoleDiv = document.getElementById('console');
    const answerInput = document.getElementById('answerInput');
    const submitBtn = document.getElementById('submitBtn');

    function println(text = '') {
      consoleDiv.innerHTML += text + '<br>';
      consoleDiv.scrollTop = consoleDiv.scrollHeight;
    }

    function toLower(s) {
      return s.toLowerCase();
    }

    const quiz = [
      { prompt: "Satire comes from the Latin word 'satura' meaning ______.", answer: "poetic medley" },
      { prompt: "When did the Simpsons premiere? (1989 / 1999 / 1979 / 2009)", answer: "1989" },
      { prompt: "One of the satirist’s central tools has always been __________.", answer: "exaggeration" }
      // ... all other questions would continue here ...
    ];

    let total = quiz.length;
    let correctCount = 0;

    println("lock tf in");
    println("btw not case sensitive");
    println('');

    let currentIndex = Math.floor(Math.random() * quiz.length);

    function askQuestion() {
      if (quiz.length === 0) {
        println(`ok ${correctCount} / ${total}`);
        println('lock in');
        submitBtn.disabled = true;
        answerInput.disabled = true;
        return;
      }
      println(quiz[currentIndex].prompt);
    }

    submitBtn.addEventListener('click', () => {
      const userAnswer = answerInput.value.trim();
      if (!userAnswer) return;

      if (toLower(userAnswer) === toLower(quiz[currentIndex].answer)) {
        println('correct\n');
        quiz.splice(currentIndex, 1);
        correctCount++;
      } else {
        println(`incorrect. correct answer was ${quiz[currentIndex].answer}\n`);
      }

      answerInput.value = '';

      if (quiz.length > 0) {
        currentIndex = Math.floor(Math.random() * quiz.length);
        askQuestion();
      } else {
        println(`ok ${correctCount} / ${total}`);
        println('lock in');
      }
    });

    askQuestion();
  </script>
</body>
</html>
