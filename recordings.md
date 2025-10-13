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
    <div class="meta">v 1.3</div>
    <pre class="console" id="console"></pre>

    <div class="prompt" id="question"></div>
    <input type="text" id="answerInput" autocomplete="off" />
    <div style="display:flex; gap:8px;"><button id="submitBtn">Submit Answer</button><button id="skipBtn">Skip (show answer)</button></div>
    <div style="margin-top:12px; color:#444; font-size:14px;">Progress: <span id="progress">0</span>/<span id="total">0</span></div>
  </div>

  <script>
    function toLower(s){ return s.toLowerCase(); }

    const quiz = [
        // ------------------------- SATIRE QUESTIONS -------------------------
        {prompt: "Satire comes from the Latin word 'satura' meaning ______.", answer: "poetic medley"},
        {prompt: "When did the Simpsons premiere? (1989 / 1999 / 1979 / 2009)", answer: "1989"},
        {prompt: "One of the satirist’s central tools has always been __________.", answer: "exaggeration"},
        {prompt: "The aim of satire is to ______.", answer: "alert and educate the public of a problem"},
        {prompt: "“satire trains you to .....” (check all that apply)", answer: "think critically"},
        {prompt: "The plastic limits of Springfield…", answer: "circumscribe American social relations in their entirety"},
        {prompt: "The show Daria is known for its nihilism. As a result, viewers are becoming ______.", answer: "nihilistic"},

        // ------------------------- EARNEST VOCAB -------------------------
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

        // ------------------------- LORD OF THE FLIES VOCAB -------------------------
        {prompt: "__________they squared up to each other but kept just out of fighting distance.", answer: "truculently"},
        {prompt: "The _______ of MLK Jr is celebrated on his birthday every January.", answer: "martyrdom"},
        {prompt: "She pushed over the _______, causing the fragile statue to fall.", answer: "plinth"},
        {prompt: "She was once more the center of __________ when she kept saying she'd never get back with her boyfriend.", answer: "derision"},
        {prompt: "She threw herself on the grenade to protect her comrades; she was remembered as a _____.", answer: "martyr"},
        {prompt: "The _________ winds were blowing the homework all over the place.", answer: "tempestuous"},
        {prompt: "The girl was _______ when her boyfriend bought her flowers.", answer: "ebullient"},
        {prompt: "We're the only people who are ________ to psychoanalysis.", answer: "impervious"},
        {prompt: "The boy, finding himself ___________ in controversy, slid the cheat sheet to his friend and ran away.", answer: "blatantly"},
        {prompt: "yummmm, this is the most ______ baby I've ever eaten!", answer: "succulent"},
        {prompt: "OJ Simpson committed a heinous crime but he didn't feel ______ at all.", answer: "contrite"},
        {prompt: "After the first sunny day of the year, Mr. Marple's ______ skin becomes very red.", answer: "pallid"},
        {prompt: "Mr. Marple was _______ when the Raiders lost their 1st game.", answer: "embroiled"},
        {prompt: "The king was himself indeed a(n) _________ idiot.", answer: "obtuse"},
        {prompt: "John Cena's ______ body was able to carry the heavy desk.", answer: "sinewy"},
        {prompt: "The ______ girl, blushed when Suhan asked her out on a date.", answer: "demure"},
        {prompt: "One of my favorite books as a kid was The ________ Snowman of Pasadena.", answer: "abominable"},
        {prompt: "The ________ ninja snuck into his room while he was sleeping.", answer: "furtive"},
        {prompt: "Ralph blew into the ______ to gather the boys for the meeting.", answer: "conch"},

        // ------------------------- CROSSWORD VOCAB -------------------------
        {prompt: "Gaston’s _______ is LeFou.", answer: "sycophant"},
        {prompt: "sweet sounding", answer: "mellifluous"},
        {prompt: "The ______ unit was overrun during Covid-19.", answer: "triage"},
        {prompt: "The ______ president gave his children high-ranking positions in the government.", answer: "nepotistic"},
        {prompt: "extremely harsh punishment", answer: "draconian"},
        {prompt: "I’m trying to grow your _____ by giving you new vocab words.", answer: "lexicon"},
        {prompt: "The exiled king became a _______ in his homeland.", answer: "pariah"},
        {prompt: "Hitler is the ________ anti-Semite.", answer: "quintessential"},
        {prompt: "College should be a _____", answer: "meritocracy"},
        {prompt: "Problems that figuratively & literally take ahold of the boys", answer: "creepers"},
        {prompt: "The pirates _______ the ship and killed the victims.", answer: "commandeered"},
        {prompt: "characterized by strong and turbulent or conflicting emotion", answer: "tempestuous"},
        {prompt: "modest; shy", answer: "demure"},
        {prompt: "opposing, differing from", answer: "antithetical"},
        {prompt: "41717 ____ Avenue, Fremont, CA", answer: "palm"},
        {prompt: "I was sick and tired of her ________, so I didn’t give her what she wanted.", answer: "groveling"},
        {prompt: "The beautiful _____ brought tears to my eyes at the funeral.", answer: "dirge"},
        {prompt: "a seashell", answer: "conch"},
        {prompt: "deceitful and untrustworthy", answer: "perfidious"},
        {prompt: "completely obvious", answer: "blatant"},
        {prompt: "where the plane crashed", answer: "scar"},
        {prompt: "causing moral revulsion; detestable; hateful", answer: "abominable"},
        {prompt: "a person that is the direct opposite of someone else", answer: "antithesis"},
        {prompt: "contemptuous; ridicule", answer: "derision"},
        {prompt: "I always _______ a protein shake after my workout.", answer: "imbibe"},
        {prompt: "My ______ appetite led me to eat 4 cheeseburgers in one sitting.", answer: "voracious"},
        {prompt: "The tender meat was so _____ that it practically melted in my mouth!", answer: "succulent"},
        {prompt: "The ______ promised to bring down grocery prices but did nothing.", answer: "charlatan"},
        {prompt: "slow to understand", answer: "obtuse"},
        {prompt: "The soldier became a _____ when he threw himself on the grenade.", answer: "martyr"},
        {prompt: "juicy; tasty", answer: "succulent"},
        {prompt: "Hiring practice of one’s friends", answer: "cronyism"},
        {prompt: "strong; muscular", answer: "sinewy"},
        {prompt: "skeptical; doubtful", answer: "incredulous"},
        {prompt: "extremely pale", answer: "pallid"},
        {prompt: "feeling or expressing regret for an offense", answer: "contrite"},
        {prompt: "The crowd cheered _______ for their team.", answer: "vociferously"},
        {prompt: "one’s vocabulary", answer: "lexicon"},
        {prompt: "exuberant; cheerful", answer: "ebullient"},
        {prompt: "Capital of Burkina Faso", answer: "ouagadougou"},
        {prompt: "evoking a keen sense of sadness", answer: "poignant"},
        {prompt: "The _______ only hired his friends.", answer: "crony"},
        {prompt: "Hitler was the _____ of peace, love, and acceptance.", answer: "antithesis"},
        {prompt: "represents mankind’s savageness destroying the world", answer: "scar"},
        {prompt: "the most perfect example (adj)", answer: "quintessential"},
        {prompt: "The ______ student was caught cheating.", answer: "perfidious"},
        {prompt: "combative; pugnacious", answer: "truculent"},
        {prompt: "a fraud; imposter", answer: "charlatan"},
        {prompt: "a song for the dead", answer: "dirge"},
        {prompt: "Her bf’s _______ text brought tears to her eyes.", answer: "poignant"},
        {prompt: "a heavy base supporting a statue or vase", answer: "plinth"},
        {prompt: "sneaky; secretive", answer: "furtive"},
        {prompt: "obsequious behavior aimed at obtaining forgiveness or favor", answer: "contrition"},
        {prompt: "I was ______ when the winning lotto numbers matched my own!", answer: "incredulous"},
        {prompt: "vehement; outspoken; earnest", answer: "vociferous"}
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

    appendConsole("lock tf in\n");
    appendConsole("btw not case sensitive" + TOTAL + "\n");

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
