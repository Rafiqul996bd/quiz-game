<!DOCTYPE html>
<html lang="bn">
<head>
  <meta charset="UTF-8">
  <title>জ্ঞানমূলক গেম</title>
  <style>
    body {
      font-family: 'Arial', sans-serif;
      text-align: center;
      padding: 30px;
      background: #f0f8ff;
    }
    h1 {
      color: #333;
    }
    .question {
      font-size: 24px;
      margin: 20px 0;
    }
    .options button {
      display: block;
      margin: 10px auto;
      padding: 10px 20px;
      font-size: 18px;
      cursor: pointer;
    }
    .result {
      font-size: 22px;
      margin-top: 20px;
    }
    .category-btn {
      padding: 12px 20px;
      margin: 10px;
      font-size: 18px;
      background-color: #009688;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
    #nameInput {
      margin-bottom: 20px;
    }
    input[type="text"] {
      padding: 10px;
      font-size: 18px;
      width: 250px;
    }
    #timer {
      font-size: 20px;
      color: red;
      margin: 10px 0;
    }
  </style>
</head>
<body>
  <h1>জ্ঞানমূলক কুইজ</h1>
  <div id="nameInput">
    <p>আপনার নাম লিখুন:</p>
    <input type="text" id="playerName" placeholder="নাম দিন...">
  </div>
  <div id="categorySelect">
    <p>বিষয় বেছে নিন:</p>
    <button class="category-btn" onclick="startQuiz('sorborno')">স্বরবর্ণ</button>
    <button class="category-btn" onclick="startQuiz('alphabet')">ইংরেজি বর্ণ</button>
    <button class="category-btn" onclick="startQuiz('religion')">ধর্ম</button>
    <button class="category-btn" onclick="startQuiz('history')">ইতিহাস</button>
    <button class="category-btn" onclick="startQuiz('numbers')">সংখ্যা</button>
  </div>
  <div class="question" id="question"></div>
  <div id="timer"></div>
  <div class="options" id="options"></div>
  <div class="result" id="result"></div>

  <!-- Audio Elements -->
  <audio id="correctSound" src="https://cdn.pixabay.com/audio/2022/03/15/audio_b62bf77762.mp3"></audio>
  <audio id="wrongSound" src="https://cdn.pixabay.com/audio/2022/03/15/audio_92781e6f47.mp3"></audio>

  <script>
    const allQuestions = {
      sorborno: [
        { q: "স্বরবর্ণ কয়টি?", options: ["৭টি", "১১টি", "১০টি", "৯টি"], answer: "১১টি" },
      ],
      alphabet: [
        { q: "A এর পরে কোন বর্ণ?", options: ["C", "D", "B", "E"], answer: "B" },
      ],
      religion: [
        { q: "ইসলামের প্রথম স্তম্ভ কোনটি?", options: ["রোজা", "হজ", "কালেমা", "নামাজ"], answer: "কালেমা" },
      ],
      history: [
        { q: "বাংলাদেশ স্বাধীনতা পায় কবে?", options: ["১৯৭১", "১৯৪৭", "১৯৭৫", "১৯৬৫"], answer: "১৯৭১" },
      ],
      numbers: [
        { q: "১০ এর পর কোন সংখ্যা?", options: ["৯", "১২", "১১", "১০০"], answer: "১১" },
      ]
    };

    let current = 0;
    let score = 0;
    let questions = [];
    let player = "অতিথি";
    let timerInterval;
    let timeLeft = 10;

    function startQuiz(category) {
      const name = document.getElementById("playerName").value.trim();
      if (name !== "") player = name;

      document.getElementById("nameInput").style.display = "none";
      document.getElementById("categorySelect").style.display = "none";
      questions = allQuestions[category];
      current = 0;
      score = 0;
      showQuestion();
    }

    function showQuestion() {
      clearInterval(timerInterval);
      timeLeft = 10;

      if (current < questions.length) {
        document.getElementById("question").textContent = questions[current].q;
        document.getElementById("result").textContent = "";
        const optionsDiv = document.getElementById("options");
        optionsDiv.innerHTML = "";

        questions[current].options.forEach(option => {
          const btn = document.createElement("button");
          btn.textContent = option;
          btn.onclick = () => checkAnswer(option);
          optionsDiv.appendChild(btn);
        });

        document.getElementById("timer").textContent = `সময় বাকি: ${timeLeft} সেকেন্ড`;
        timerInterval = setInterval(() => {
          timeLeft--;
          document.getElementById("timer").textContent = `সময় বাকি: ${timeLeft} সেকেন্ড`;
          if (timeLeft === 0) {
            clearInterval(timerInterval);
            checkAnswer(null);
          }
        }, 1000);
      } else {
        document.getElementById("question").textContent = "শেষ!";
        document.getElementById("options").innerHTML = "";
        document.getElementById("timer").textContent = "";
        document.getElementById("result").textContent = `${player}, আপনার স্কোর: ${score} / ${questions.length}`;
      }
    }

    function checkAnswer(selected) {
      clearInterval(timerInterval);
      const correct = questions[current].answer;
      if (selected === correct) {
        score++;
        document.getElementById("correctSound").play();
      } else {
        document.getElementById("wrongSound").play();
      }
      current++;
      setTimeout(showQuestion, 1000);
    }
  </script>
</body>
</html># quiz-game
A Bengla educational quiz game
