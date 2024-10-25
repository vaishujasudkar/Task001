# Task001
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Language Learning App</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <!-- Home Screen -->
  <div id="home" class="screen active">
    <h1>Language Learning App</h1>
    <button onclick="startLessons()">Start Lessons</button>
    <button onclick="startQuiz()">Take a Quiz</button>
  </div>

  <!-- Lessons Screen -->
  <div id="lessons" class="screen">
    <h2>Lessons</h2>
    <div id="lessonList"></div>
    <button onclick="goToHome()">Back to Home</button>
  </div>

  <!-- Lesson Detail Screen -->
  <div id="lessonDetail" class="screen">
    <h2 id="lessonTitle"></h2>
    <p id="lessonContent"></p>
    <ul id="lessonVocabulary"></ul>
    <button onclick="goToLessons()">Back to Lessons</button>
  </div>

  <!-- Quiz Screen -->
  <div id="quiz" class="screen">
    <h2>Quiz</h2>
    <p id="questionText"></p>
    <div id="quizOptions"></div>
    <button onclick="submitAnswer()">Next</button>
    <p id="scoreText"></p>
  </div>

  <script src="script.js"></script>
</body>
</html>
body {
  font-family: Arial, sans-serif;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  margin: 0;
  background-color: #f0f4f8;
}

.screen {
  display: none;
  text-align: center;
}

.screen.active {
  display: block;
}

button {
  margin: 10px;
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 5px;
}

button:hover {
  background-color: #45a049;
}
// Sample Lessons and Quiz Questions
const lessons = [
  { title: "Basics", content: "Learn basic greetings and phrases.", vocabulary: ["Hello", "Thank you", "Goodbye"] },
  { title: "Numbers", content: "Learn how to count in Spanish.", vocabulary: ["One - Uno", "Two - Dos", "Three - Tres"] }
];

const quizQuestions = [
  { question: "How do you say 'Hello' in Spanish?", options: ["Hola", "Bonjour", "Ciao", "Hello"], answer: "Hola" },
  { question: "How do you say 'Goodbye' in Spanish?", options: ["Goodbye", "Adios", "Ciao", "Au revoir"], answer: "Adios" }
];

let currentQuestionIndex = 0;
let score = 0;

// Functions to navigate screens
function goToHome() {
  showScreen("home");
}

function startLessons() {
  showScreen("lessons");
  loadLessons();
}

function goToLessons() {
  showScreen("lessons");
}

function startQuiz() {
  showScreen("quiz");
  currentQuestionIndex = 0;
  score = 0;
  showQuestion();
}

// Function to switch screens
function showScreen(screenId) {
  document.querySelectorAll(".screen").forEach(screen => screen.classList.remove("active"));
  document.getElementById(screenId).classList.add("active");
}

// Load lessons dynamically
function loadLessons() {
  const lessonList = document.getElementById("lessonList");
  lessonList.innerHTML = ""; // Clear previous list

  lessons.forEach((lesson, index) => {
    const button = document.createElement("button");
    button.textContent = lesson.title;
    button.onclick = () => showLessonDetail(index);
    lessonList.appendChild(button);
  });
}

// Show lesson details
function showLessonDetail(lessonIndex) {
  const lesson = lessons[lessonIndex];
  document.getElementById("lessonTitle").textContent = lesson.title;
  document.getElementById("lessonContent").textContent = lesson.content;

  const vocabList = document.getElementById("lessonVocabulary");
  vocabList.innerHTML = "";
  lesson.vocabulary.forEach(word => {
    const listItem = document.createElement("li");
    listItem.textContent = word;
    vocabList.appendChild(listItem);
  });

  showScreen("lessonDetail");
}

// Show quiz question
function showQuestion() {
  if (currentQuestionIndex >= quizQuestions.length) {
    document.getElementById("scoreText").textContent = `Quiz Complete! Your Score: ${score}/${quizQuestions.length}`;
    return;
  }

  const questionData = quizQuestions[currentQuestionIndex];
  document.getElementById("questionText").textContent = questionData.question;

  const optionsContainer = document.getElementById("quizOptions");
  optionsContainer.innerHTML = "";

  questionData.options.forEach(option => {
    const button = document.createElement("button");
    button.textContent = option;
    button.onclick = () => selectAnswer(option);
    optionsContainer.appendChild(button);
  });
}

// Select an answer and proceed to the next question
function selectAnswer(selectedOption) {
  const correctAnswer = quizQuestions[currentQuestionIndex].answer;
  if (selectedOption === correctAnswer) {
    score++;
  }
  currentQuestionIndex++;
  showQuestion();
}

// Submit answer and show score
function submitAnswer() {
  showQuestion();
}
# Output
Home Screen: Entry point with options for lessons and quizzes.
Lessons: A list of lessons, each with details and vocabulary.
Quiz: Multiple-choice questions with immediate feedback on completion.
