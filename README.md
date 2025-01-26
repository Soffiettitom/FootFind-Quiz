# FootFind-Quiz
Jeux de quiz football 
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Jeu de Quiz Football</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div id="app">
        <h1>Jeu de Quiz Football</h1>
        <p id="niveau">Niveau : 1</p>
        <p id="score">Score : 0</p>
        <div id="question">
            <p>Ce joueur a joué dans ces clubs :</p>
            <ul id="clubs-list"></ul>
        </div>
        <input type="text" id="player-input" placeholder="Devinez le joueur">
        <button id="submit-answer">Soumettre</button>
        <p id="result"></p>
        <button id="next-question">Prochaine question</button>
    </div>

    <script src="script.js"></script>
</body>
</html>
body {
    font-family: Arial, sans-serif;
    text-align: center;
    margin: 0;
    padding: 0;
    background-color: #f5f5f5;
    color: #333;
}

#app {
    max-width: 600px;
    margin: 50px auto;
    padding: 20px;
    background: white;
    border-radius: 10px;
    box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
}

h1 {
    color: #007bff;
}

#clubs-list {
    list-style-type: none;
    padding: 0;
    font-size: 18px;
}

#player-input {
    width: 80%;
    padding: 10px;
    margin-top: 10px;
    font-size: 16px;
}

button {
    margin-top: 10px;
    padding: 10px 20px;
    font-size: 16px;
    background: #007bff;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

button:hover {
    background: #0056b3;
}

#result {
    font-size: 18px;
    margin-top: 10px;
}
// Base de données des joueurs
const players = [
    { name: "Lionel Messi", clubs: ["Barcelona", "PSG", "Inter Miami"] },
    { name: "Cristiano Ronaldo", clubs: ["Manchester United", "Real Madrid", "Al Nassr"] },
    { name: "Zlatan Ibrahimović", clubs: ["Ajax", "Inter Milan", "AC Milan"] },
    { name: "Kylian Mbappé", clubs: ["Monaco", "PSG"] },
    { name: "Neymar", clubs: ["Santos", "Barcelona", "PSG"] },
    { name: "Erling Haaland", clubs: ["Molde", "Borussia Dortmund", "Manchester City"] },
    { name: "Robert Lewandowski", clubs: ["Lech Poznan", "Bayern Munich", "Barcelona"] },
    { name: "Vinícius Júnior", clubs: ["Flamengo", "Real Madrid"] },
    { name: "Jude Bellingham", clubs: ["Birmingham City", "Borussia Dortmund", "Real Madrid"] },
    { name: "Harry Kane", clubs: ["Tottenham", "Bayern Munich"] },
    { name: "Victor Osimhen", clubs: ["Charleroi", "Lille", "Napoli"] },
    { name: "Marcus Rashford", clubs: ["Manchester United"] },
    { name: "Mohamed Salah", clubs: ["Basel", "Chelsea", "Liverpool"] },
    { name: "Rafael Leão", clubs: ["Sporting CP", "Lille", "AC Milan"] },
    { name: "Trent Alexander-Arnold", clubs: ["Liverpool"] },
    { name: "Gavi", clubs: ["Barcelona"] },
    { name: "Lautaro Martínez", clubs: ["Racing Club", "Inter Milan"] },
    { name: "Achraf Hakimi", clubs: ["Real Madrid", "Inter Milan", "PSG"] },
    { name: "Christopher Nkunku", clubs: ["PSG", "RB Leipzig", "Chelsea"] },
    // Ajoutez autant de joueurs que nécessaire ici...
];

// Sélection des éléments HTML
const clubsList = document.getElementById("clubs-list");
const playerInput = document.getElementById("player-input");
const result = document.getElementById("result");
const submitButton = document.getElementById("submit-answer");
const nextButton = document.getElementById("next-question");
const scoreDisplay = document.getElementById("score");
const niveauDisplay = document.getElementById("niveau");

// Variables du jeu
let currentPlayer = null;
let score = 0;
let level = 1;

// Fonction pour choisir un joueur aléatoire et un niveau
function pickRandomPlayer() {
    const randomIndex = Math.floor(Math.random() * players.length);
    currentPlayer = players[randomIndex];
    displayClubs(currentPlayer.clubs);
    result.textContent = "";
    playerInput.value = "";
}

// Fonction pour afficher les clubs en fonction du niveau
function displayClubs(clubs) {
    clubsList.innerHTML = "";
    const shuffledClubs = [...clubs].sort(() => Math.random() - 0.5); // Mélange les clubs

    let displayedClubs;
    if (level === 1) {
        displayedClubs = shuffledClubs;
    } else if (level === 2) {
        displayedClubs = [shuffledClubs[0], "Un club moins connu", shuffledClubs[2]];
    } else {
        displayedClubs = ["Un club peu connu", "Un autre club peu connu", shuffledClubs[2]];
    }

    displayedClubs.forEach(club => {
        const li = document.createElement("li");
        li.textContent = club;
        clubsList.appendChild(li);
    });
}

// Vérification de la réponse
submitButton.addEventListener("click", () => {
    const userGuess = playerInput.value.trim();
    if (userGuess.toLowerCase() === currentPlayer.name.toLowerCase()) {
        result.textContent = "Correct ! C'était bien " + currentPlayer.name + " !";
        result.style.color = "green";
        score++;
        if (score % 5 === 0) level++; // Augmente le niveau tous les 5 points
        scoreDisplay.textContent = "Score : " + score;
        niveauDisplay.textContent = "Niveau : " + level;
    } else {
        result.textContent = "Incorrect. Réessayez !";
        result.style.color = "red";
    }
});

// Passer à la prochaine question
nextButton.addEventListener("click", pickRandomPlayer);

// Initialisation du jeu
pickRandomPlayer();
