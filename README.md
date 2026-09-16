<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <title>Quiz Master</title>

  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    body {
      min-height: 100vh;
      font-family: Arial, Helvetica, sans-serif;
      background: linear-gradient(135deg, #667eea, #764ba2);
      display: flex;
      justify-content: center;
      align-items: center;
      padding: 18px;
    }

    .app {
      width: 100%;
      max-width: 520px;
    }

    .quiz-card {
      background: rgba(255, 255, 255, 0.97);
      border-radius: 25px;
      padding: 25px;
      box-shadow: 0 20px 50px rgba(0, 0, 0, 0.25);
    }

    /* START SCREEN */

    .start-screen {
      text-align: center;
      padding: 25px 10px;
    }

    .logo {
      font-size: 70px;
      margin-bottom: 10px;
    }

    .start-screen h1 {
      font-size: 38px;
      color: #333;
      margin-bottom: 10px;
    }

    .start-screen p {
      color: #666;
      font-size: 16px;
      line-height: 1.5;
      margin-bottom: 25px;
    }

    .start-info {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 10px;
      margin-bottom: 25px;
    }

    .info-box {
      background: #f3f4ff;
      padding: 15px 5px;
      border-radius: 15px;
    }

    .info-box strong {
      display: block;
      font-size: 20px;
      color: #667eea;
    }

    .info-box span {
      font-size: 12px;
      color: #666;
    }

    /* BUTTON */

    .main-btn {
      width: 100%;
      padding: 16px;
      border: none;
      border-radius: 14px;
      background: linear-gradient(135deg, #667eea, #764ba2);
      color: white;
      font-size: 18px;
      font-weight: bold;
      cursor: pointer;
      transition: transform 0.2s;
    }

    .main-btn:hover {
      transform: translateY(-2px);
    }

    /* GAME */

    .game {
      display: none;
    }

    .top-bar {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 15px;
    }

    .question-number {
      font-weight: bold;
      color: #444;
    }

    .streak {
      color: #ff7a00;
      font-weight: bold;
    }

    .progress-container {
      width: 100%;
      height: 9px;
      background: #e7e7e7;
      border-radius: 20px;
      overflow: hidden;
      margin-bottom: 22px;
    }

    .progress {
      height: 100%;
      width: 0%;
      background: linear-gradient(90deg, #667eea, #764ba2);
      transition: width 0.3s ease;
    }

    .timer-box {
      display: flex;
      justify-content: center;
      margin-bottom: 18px;
    }

    .timer {
      width: 70px;
      height: 70px;
      border-radius: 50%;
      background: #f3f4ff;
      display: flex;
      justify-content: center;
      align-items: center;
      font-size: 22px;
      font-weight: bold;
      color: #667eea;
      border: 5px solid #667eea;
    }

    .timer.warning {
      color: #e53935;
      border-color: #e53935;
    }

    #question {
      font-size: 23px;
      line-height: 1.4;
      text-align: center;
      color: #222;
      margin-bottom: 22px;
    }

    .options {
      display: grid;
      gap: 12px;
    }

    .option {
      width: 100%;
      padding: 15px;
      border: 2px solid #e5e5e5;
      background: white;
      border-radius: 13px;
      text-align: left;
      font-size: 16px;
      cursor: pointer;
      transition: 0.2s;
    }

    .option:hover:not(:disabled) {
      border-color: #667eea;
      background: #f7f7ff;
      transform: translateX(3px);
    }

    .option:disabled {
      cursor: default;
    }

    .option.correct {
      background: #d9f8df;
      border-color: #32a852;
      color: #18752f;
    }

    .option.wrong {
      background: #ffe0e0;
      border-color: #e53935;
      color: #b71c1c;
    }

    .feedback {
      min-height: 28px;
      text-align: center;
      font-weight: bold;
      margin-top: 15px;
    }

    .next-btn {
      display: none;
      width: 100%;
      margin-top: 15px;
      padding: 14px;
      border: none;
      border-radius: 13px;
      background: #333;
      color: white;
      font-size: 16px;
      font-weight: bold;
      cursor: pointer;
    }

    /* RESULT */

    .result {
      display: none;
      text-align: center;
      padding: 20px 5px;
    }

    .result-icon {
      font-size: 70px;
      margin-bottom: 10px;
    }

    .result h2 {
      font-size: 30px;
      color: #333;
      margin-bottom: 10px;
    }

    .final-score {
      font-size: 42px;
      font-weight: bold;
      color: #667eea;
      margin: 15px 0;
    }

    .result-message {
      color: #666;
      margin-bottom: 25px;
    }

    .stats {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 12px;
      margin-bottom: 25px;
    }

    .stat {
      background: #f5f5f5;
      padding: 15px;
      border-radius: 13px;
    }

    .stat strong {
      display: block;
      font-size: 22px;
      color: #333;
    }

    .stat span {
      color: #777;
      font-size: 13px;
    }

    /* MOBILE */

    @media (max-width: 480px) {
      .quiz-card {
        padding: 20px;
      }

      .start-screen h1 {
        font-size: 32px;
      }

      #question {
        font-size: 20px;
      }

      .option {
        padding: 14px;
      }
    }
  </style>
</head>

<body>

<div class="app">

  <div class="quiz-card">

    <!-- START SCREEN -->

    <div class="start-screen" id="startScreen">

      <div class="logo">🧠</div>

      <h1>Quiz Master</h1>

      <p>
        Test your knowledge and see how high you can score!
      </p>

      <div class="start-info">

        <div class="info-box">
          <strong>10</strong>
          <span>Questions</span>
        </div>

        <div class="info-box">
          <strong>15s</strong>
          <span>Per Question</span>
        </div>

        <div class="info-box">
          <strong>🏆</strong>
          <span>High Score</span>
        </div>

      </div>

      <button class="main-btn" onclick="startGame()">
        Start Quiz 🚀
      </button>

    </div>


    <!-- GAME SCREEN -->

    <div class="game" id="game">

      <div class="top-bar">

        <div class="question-number">
          Question <span id="questionNumber">1</span>/10
        </div>

        <div class="streak">
          🔥 Streak: <span id="streak">0</span>
        </div>

      </div>

      <div class="progress-container">
        <div class="progress" id="progress"></div>
      </div>

      <div class="timer-box">
        <div class="timer" id="timer">
          15
        </div>
      </div>

      <h2 id="question">
        Question
      </h2>

      <div class="options" id="options"></div>

      <div class="feedback" id="feedback"></div>

      <button class="next-btn" id="nextBtn" onclick="nextQuestion()">
        Next Question ➡️
      </button>

    </div>


    <!-- RESULT SCREEN -->

    <div class="result" id="result">

      <div class="result-icon">
        🏆
      </div>

      <h2>Quiz Complete!</h2>

      <div class="final-score" id="finalScore">
        0/10
      </div>

      <p class="result-message" id="resultMessage">
        Great job!
      </p>

      <div class="stats">

        <div class="stat">
          <strong id="correctAnswers">0</strong>
          <span>Correct</span>
        </div>

        <div class="stat">
          <strong id="wrongAnswers">0</strong>
          <span>Wrong</span>
        </div>

      </div>

      <button class="main-btn" onclick="restartGame()">
        Play Again 🔄
      </button>

    </div>

  </div>

</div>


<script>

  /* =========================
     QUESTIONS
  ========================= */

  const questions = [

    {
      question: "What is the capital of India?",
      options: [
        "Mumbai",
        "New Delhi",
        "Pune",
        "Nagpur"
      ],
      answer: 1
    },

    {
      question: "Which planet is known as the Red Planet?",
      options: [
        "Earth",
        "Mars",
        "Venus",
        "Jupiter"
      ],
      answer: 1
    },

    {
      question: "How many days are there in a week?",
      options: [
        "5",
        "6",
        "7",
        "8"
      ],
      answer: 2
    },

    {
      question: "What is 12 × 5?",
      options: [
        "50",
        "60",
        "70",
        "80"
      ],
      answer: 1
    },

    {
      question: "Which is the largest ocean on Earth?",
      options: [
        "Indian Ocean",
        "Atlantic Ocean",
        "Pacific Ocean",
        "Arctic Ocean"
      ],
      answer: 2
    },

    {
      question: "Which language makes websites interactive?",
      options: [
        "HTML",
        "CSS",
        "JavaScript",
        "SQL"
      ],
      answer: 2
    },

    {
      question: "How many continents are there?",
      options: [
        "5",
        "6",
        "7",
        "8"
      ],
      answer: 2
    },

    {
      question: "Which animal is commonly called the King of the Jungle?",
      options: [
        "Tiger",
        "Lion",
        "Elephant",
        "Wolf"
      ],
      answer: 1
    },

    {
      question: "Which star is closest to Earth?",
      options: [
        "Sirius",
        "Polaris",
        "The Sun",
        "Vega"
      ],
      answer: 2
    },

    {
      question: "What is the largest planet in our Solar System?",
      options: [
        "Earth",
        "Mars",
        "Saturn",
        "Jupiter"
      ],
      answer: 3
    }

  ];


  /* =========================
     GAME VARIABLES
  ========================= */

  let currentQuestion = 0;
  let score = 0;
  let streak = 0;
  let timeLeft = 15;
  let timerInterval;
  let answered = false;


  /* =========================
     START GAME
  ========================= */

  function startGame() {

    currentQuestion = 0;
    score = 0;
    streak = 0;

    document.getElementById("startScreen").style.display = "none";
    document.getElementById("result").style.display = "none";
    document.getElementById("game").style.display = "block";

    document.getElementById("streak").textContent = "0";

    showQuestion();
  }


  /* =========================
     SHOW QUESTION
  ========================= */

  function showQuestion() {

    clearInterval(timerInterval);

    answered = false;
    timeLeft = 15;

    const questionData = questions[currentQuestion];

    document.getElementById("questionNumber").textContent =
      currentQuestion + 1;

    document.getElementById("question").textContent =
      questionData.question;

    document.getElementById("timer").textContent =
      timeLeft;

    document.getElementById("timer").classList.remove("warning");

    document.getElementById("feedback").textContent = "";

    document.getElementById("nextBtn").style.display = "none";

    /* Progress */

    const progress =
      ((currentQuestion) / questions.length) * 100;

    document.getElementById("progress").style.width =
      progress + "%";


    /* Options */

    const optionsContainer =
      document.getElementById("options");

    optionsContainer.innerHTML = "";

    questionData.options.forEach((option, index) => {

      const button =
        document.createElement("button");

      button.className = "option";

      button.textContent =
        String.fromCharCode(65 + index) + ". " + option;

      button.onclick = function () {
        checkAnswer(index, button);
      };

      optionsContainer.appendChild(button);

    });


    /* Start timer */

    startTimer();
  }


  /* =========================
     TIMER
  ========================= */

  function startTimer() {

    timerInterval = setInterval(() => {

      timeLeft--;

      document.getElementById("timer").textContent =
        timeLeft;

      if (timeLeft <= 5) {

        document.getElementById("timer")
          .classList.add("warning");

      }

      if (timeLeft <= 0) {

        clearInterval(timerInterval);

        timeUp();

      }

    }, 1000);

  }


  /* =========================
     TIME UP
  ========================= */

  function timeUp() {

    if (answered) return;

    answered = true;

    streak = 0;

    document.getElementById("streak").textContent =
      streak;

    document.getElementById("feedback").textContent =
      "⏰ Time's up!";

    document.getElementById("feedback").style.color =
      "#e53935";

    showCorrectAnswer();

    disableOptions();

    document.getElementById("nextBtn").style.display =
      "block";

  }


  /* =========================
     CHECK ANSWER
  ========================= */

  function checkAnswer(selectedIndex, button) {

    if (answered) return;

    answered = true;

    clearInterval(timerInterval);

    const correctIndex =
      questions[currentQuestion].answer;

    if (selectedIndex === correctIndex) {

      score++;

      streak++;

      button.classList.add("correct");

      document.getElementById("feedback").textContent =
        "✅ Correct!";

      document.getElementById("feedback").style.color =
        "#32a852";

    } else {

      streak = 0;

      button.classList.add("wrong");

      document.getElementById("feedback").textContent =
        "❌ Wrong answer!";

      document.getElementById("feedback").style.color =
        "#e53935";

      showCorrectAnswer();

    }

    document.getElementById("streak").textContent =
      streak;

    disableOptions();

    document.getElementById("nextBtn").style.display =
      "block";

  }


  /* =========================
     SHOW CORRECT ANSWER
  ========================= */

  function showCorrectAnswer() {

    const correctIndex =
      questions[currentQuestion].answer;

    const buttons =
      document.querySelectorAll(".option");

    if (buttons[correctIndex]) {

      buttons[correctIndex].classList.add("correct");

    }

  }


  /* =========================
     DISABLE OPTIONS
  ========================= */

  function disableOptions() {

    const buttons =
      document.querySelectorAll(".option");

    buttons.forEach(button => {

      button.disabled = true;

    });

  }


  /* =========================
     NEXT QUESTION
  ========================= */

  function nextQuestion() {

    currentQuestion++;

    if (currentQuestion < questions.length) {

      showQuestion();

    } else {

      showResult();

    }

  }


  /* =========================
     RESULT
  ========================= */

  function showResult() {

    clearInterval(timerInterval);

    document.getElementById("game").style.display =
      "none";

    document.getElementById("result").style.display =
      "block";

    document.getElementById("finalScore").textContent =
      score + "/" + questions.length;

    document.getElementById("correctAnswers").textContent =
      score;

    document.getElementById("wrongAnswers").textContent =
      questions.length - score;


    let message = "";

    const percentage =
      (score / questions.length) * 100;

    if (percentage === 100) {

      message = "🌟 Perfect score! Amazing work!";

    } else if (percentage >= 80) {

      message = "🔥 Excellent! You really know your stuff!";

    } else if (percentage >= 60) {

      message = "👏 Good job! Keep practicing!";

    } else if (percentage >= 40) {

      message = "💪 Nice try! You can do even better!";

    } else {

      message = "📚 Keep learning and try again!";

    }

    document.getElementById("resultMessage").textContent =
      message;

  }


  /* =========================
     RESTART
  ========================= */

  function restartGame() {

    document.getElementById("result").style.display =
      "none";

    document.getElementById("game").style.display =
      "block";

    currentQuestion = 0;
    score = 0;
    streak = 0;

    showQuestion();

  }

</script>

</body>
</html>
