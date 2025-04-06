<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Quel X-Men êtes-vous ?</title>
  <style>
    body {
      font-family: 'Courier New', monospace;
      background-color: #001f3f;
      color: #fff;
      margin: 0;
      padding: 20px;
      text-align: center;
    }

    h1 {
      color: #7FDBFF;
      text-shadow: 2px 2px 5px #0074D9;
      font-size: 40px;
      margin-bottom: 20px;
    }

    img.logo {
      max-width: 200px;
      margin-bottom: 20px;
    }

    .question {
      margin: 20px 0;
      background-color: #0074D9;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(127, 219, 255, 0.7);
    }

    .options button {
      background-color: #39CCCC;
      color: #001f3f;
      padding: 10px 20px;
      border: none;
      margin: 5px;
      font-size: 18px;
      cursor: pointer;
      border-radius: 5px;
      transition: all 0.3s ease;
    }

    .options button:hover {
      background-color: #3D9970;
      transform: scale(1.1);
    }

    .result {
      background-color: #0074D9;
      padding: 20px;
      border-radius: 10px;
      margin-top: 20px;
      display: none;
      box-shadow: 0 0 10px rgba(127, 219, 255, 0.7);
    }

    .score {
      font-size: 18px;
      margin-top: 10px;
      color: #7FDBFF;
    }
  </style>
</head>
<body>
  <img class="logo" src="https://zupimages.net/up/25/14/udu5.png" alt="Logo X-Men">
  <h1>Quel X-Men êtes-vous ?</h1>

  <div id="quiz-container">
    <div class="question" id="question-container">
      <h2 id="question-text"></h2>
      <div class="options" id="options-container"></div>
    </div>
  </div>

  <div class="result" id="result-container">
    <h2>Vous êtes...</h2>
    <p id="result-text"></p>
    <p class="score">Votre résultat est basé sur vos choix !</p>
    <button onclick="startQuiz()">Recommencer le quiz</button>
  </div>

  <script>
    const questions = [
      { question: "Quel est votre plus grand atout ?", options: ["Intelligence", "Force brute", "Vitesse", "Adaptabilité"], result: [0, 1, 2, 3] },
      { question: "Quel rôle jouez-vous dans une équipe ?", options: ["Leader", "Protecteur", "Éclaireur", "Stratège"], result: [4, 5, 6, 7] },
      { question: "Quelle est votre peur la plus profonde ?", options: ["Perdre le contrôle", "Être incompris", "La solitude", "L'échec"], result: [8, 9, 0, 1] },
      { question: "Quel environnement préférez-vous ?", options: ["Ville", "Forêt", "Laboratoire", "Champ de bataille"], result: [2, 3, 4, 5] },
      { question: "Comment gérez-vous un conflit ?", options: ["Dialogue", "Fuite", "Combat", "Manipulation"], result: [6, 7, 8, 9] },
      { question: "Quelle mutation vous attire le plus ?", options: ["Télépathie", "Métamorphose", "Vol", "Super-vitesse"], result: [0, 1, 2, 3] },
      { question: "Votre défaut principal ?", options: ["Trop méfiant", "Trop impulsif", "Trop distant", "Trop sensible"], result: [4, 5, 6, 7] },
      { question: "Quel type de pouvoir préférez-vous ?", options: ["Mental", "Physique", "Énergétique", "Technologique"], result: [8, 9, 0, 1] },
      { question: "Comment vos amis vous décrivent ?", options: ["Fidèle", "Solitaire", "Audacieux", "Complexe"], result: [2, 3, 4, 5] },
      { question: "Quel est votre objectif ultime ?", options: ["Un monde meilleur", "La paix intérieure", "La vérité", "La liberté"] , result: [6, 7, 8, 9] }
    ];

    const characters = [
      { name: "Professeur X", description: "Leader visionnaire, doté d'une immense intelligence et de pouvoirs mentaux incroyables." },
      { name: "Cyclope", description: "Tacticien né, loyal et courageux, souvent le cœur de l'équipe." },
      { name: "Wolverine", description: "Solitaire au passé trouble, il est animé par une féroce volonté de protéger." },
      { name: "Mystique", description: "Changeforme rusée et indépendante, experte en infiltration." },
      { name: "Tornade", description: "Maîtresse du climat, calme et puissante, guidée par un fort sens de la justice." },
      { name: "Jean Grey", description: "Dotée d'un pouvoir télékinétique et télépathique redoutable, mais aussi d'une grande sensibilité." },
      { name: "Beast", description: "Brillant scientifique à l'apparence bestiale, entre cerveau et muscles." },
      { name: "Gambit", description: "Charismatique et imprévisible, il manie l'énergie cinétique avec style." },
      { name: "Magnéto", description: "Maître du magnétisme, il lutte pour les droits des mutants, parfois à l'extrême." },
      { name: "Diablo", description: "Téléporteur au grand cœur, il mêle foi, humour et agilité surhumaine." }
    ];

    let currentQuestion = 0;
    let userAnswers = [];

    function startQuiz() {
      currentQuestion = 0;
      userAnswers = [];
      document.getElementById('result-container').style.display = 'none';
      document.getElementById('quiz-container').style.display = 'block';
      showQuestion();
    }

    function showQuestion() {
      const question = questions[currentQuestion];
      document.getElementById('question-text').innerText = question.question;

      const optionsContainer = document.getElementById('options-container');
      optionsContainer.innerHTML = '';

      question.options.forEach((option, index) => {
        const button = document.createElement('button');
        button.innerText = option;
        button.onclick = () => {
          userAnswers.push(question.result[index]);
          currentQuestion++;
          if (currentQuestion < questions.length) {
            showQuestion();
          } else {
            showResult();
          }
        };
        optionsContainer.appendChild(button);
      });
    }

    function showResult() {
      const resultIndex = userAnswers.reduce((a, b) => a + b, 0) % characters.length;
      const result = characters[resultIndex];

      document.getElementById('result-text').innerText = `${result.name} - ${result.description}`;
      document.getElementById('quiz-container').style.display = 'none';
      document.getElementById('result-container').style.display = 'block';
    }

    startQuiz();
  </script>
</body>
</html>
