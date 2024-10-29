<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Language Learning App</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="container">
        <h1>Language Learning</h1>

        <div id="vocabulary-section">
            <h2>Vocabulary</h2>
            <ul id="word-list"></ul>
            <button id="add-word-button">Add Word</button>
            <div id="add-word-form" style="display: none;">
                <input type="text" id="new-word-input" placeholder="New Word">
                <input type="text" id="new-translation-input" placeholder="Translation">
                <button id="save-word-button">Save</button>
                <button id="cancel-word-button">Cancel</button>
            </div>
        </div>

        <div id="quiz-section">
            <h2>Quiz</h2>
            <div id="quiz-question"></div>
            <button id="answer-1"></button>
            <button id="answer-2"></button>
            <button id="answer-3"></button>
            <button id="answer-4"></button>
            <div id="quiz-result"></div>
        </div>
    </div>

    <script src="script.js"></script>
</body>
</html>
Копировать
/* style.css */
.container {
    width: 80%;
    margin: 0 auto;
    text-align: center;
}

#word-list li {
    list-style: none;
    margin: 10px 0;
}

#add-word-form {
    margin-top: 10px;
}

#quiz-question {
    font-size: 20px;
    margin-bottom: 10px;
}

#quiz-result {
    margin-top: 10px;
}
Копировать
// script.js
const wordList = document.getElementById("word-list");
const addWordButton = document.getElementById("add-word-button");
const addWordForm = document.getElementById("add-word-form");
const newWordInput = document.getElementById("new-word-input");
const newTranslationInput = document.getElementById("new-translation-input");
const saveWordButton = document.getElementById("save-word-button");
const cancelWordButton = document.getElementById("cancel-word-button");

const quizQuestion = document.getElementById("quiz-question");
const answerButtons = document.querySelectorAll(".answer-button");
const quizResult = document.getElementById("quiz-result");

let vocabulary = []; // Массив для хранения слов

// Загрузка данных (можно использовать локальное хранилище или API)
loadVocabulary();

// Отображение слов на странице
function displayVocabulary() {
    wordList.innerHTML = "";
    for (let i = 0; i < vocabulary.length; i++) {
        let listItem = document.createElement("li");
        listItem.textContent = `${vocabulary[i].word}: ${vocabulary[i].translation}`;
        wordList.appendChild(listItem);
    }
}

// Добавление нового слова
addWordButton.addEventListener("click", () => {
    addWordForm.style.display = "block";
});

saveWordButton.addEventListener("click", () => {
    let newWord = newWordInput.value.trim();
    let newTranslation = newTranslationInput.value.trim();
    if (newWord && newTranslation) {
        vocabulary.push({ word: newWord, translation: newTranslation });
        newWordInput.value = "";
        newTranslationInput.value = "";
        addWordForm.style.display = "none";
        saveVocabulary();
        displayVocabulary();
    } else {
        alert("Please enter both word and translation.");
    }
});

cancelWordButton.addEventListener("click", () => {
    addWordForm.style.display = "none";
    newWordInput.value = "";
    newTranslationInput.value = "";
});

// Сохранение данных (можно использовать localStorage или API)
function saveVocabulary() {
    // localStorage.setItem("vocabulary", JSON.stringify(vocabulary));
    // ... или отправка данных на сервер с помощью API
}

// Загрузка данных (можно использовать localStorage или API)
function loadVocabulary() {
    // vocabulary = JSON.parse(localStorage.getItem("vocabulary")) || [];
    // ... или загрузка данных с сервера с помощью API
}

// Функция для запуска викторины
function startQuiz() {
    if (vocabulary.length === 0) {
        alert("Please add words to the vocabulary first.");
        return;
    }

    // Выбираем случайное слово для вопроса
    let randomIndex = Math.floor(Math.random() * vocabulary.length);
    let questionWord = vocabulary[randomIndex].word;

    // Создаем варианты ответов (случайные)
    let correctAnswerIndex = Math.floor(Math.random() * 4);
    answerButtons[correctAnswerIndex].textContent = vocabulary[randomIndex].translation;
    for (let i = 0; i < 4; i++) {
        if (i !== correctAnswerIndex) {
            let randomIncorrectWordIndex = Math.floor(Math.random() * vocabulary.length);
            while (randomIncorrectWordIndex === randomIndex) {
                randomIncorrectWordIndex = Math.floor(Math.random() * vocabulary.length);
            }
            answerButtons[i].textContent = vocabulary[randomIncorrectWordIndex].translation;
        }
    }

    // Отображаем вопрос и варианты ответов
    quizQuestion.textContent = questionWord;
    quizResult.textContent = "";

    // Обрабатываем клики по вариантам ответов
    for (let i = 0; i < 4; i++) {
        answerButtons[i].addEventListener("click", () => {
            if (i === correctAnswerIndex) {
                quizResult.textContent = "Correct!";
            } else {
                quizResult.textContent = "Incorrect. The correct answer is " + vocabulary[randomIndex].translation;
            }
        });
    }
}

// Запускаем викторину
startQuiz();
