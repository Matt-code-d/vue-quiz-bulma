<template>
    <div class="container">
      <div v-if="attempts === 0">
        <!-- Sezione per il messaggio di benvenuto -->
        <p></p><br>
        <h1 class="title">Ciao,</h1><br>
        <p>Per iniziare la partita, premi il bottone "Inizia Partita".<br>
        <strong>Buon divertimento!</strong></p><br>
        <button class="button is-primary" @click="startGame">Inizia Partita</button>
      </div>
      <div v-else>
        <!-- Sezione per il gioco -->
        <section class="hero is-primary is-bold">
          <div class="hero-body">
            <div class="container">
              <h1 class="title">Quiz in VUE JS</h1>
              <h2 class="subtitle">{{ questionCount }}/{{ questionsRemaining.length }}</h2>
              <div v-if="timer > 0" class="subtitle">Tempo rimasto: {{ timer }} secondi</div>
            </div>
          </div>
        </section>
        <section class="section">
          <div class="container">
            <div v-if="attempts === 0">
              <p></p><br>
              <h1 class="title">Ciao,</h1><br>
              <p>Per iniziare la partita, premi il bottone "Inizia Partita".<br>
              <strong>Buon divertimento!</strong></p><br>
              <button class="button is-primary" @click="startGame">Inizia Partita</button>
            </div>
            <div v-else>
              <div v-if="currentQuestion !== null">
                <div class="box">
                  <h2 class="subtitle">{{ currentQuestion.question }}</h2>
                  <template v-if="currentQuestion.type === 'multipleChoice'">
                    <ul>
                      <li v-for="(option, index) in currentQuestion.options" :key="index">
                        <label class="radio">
                          <input type="radio" v-model="selectedOption" :value="option" @change="checkAnswer" />
                          {{ option }}
                        </label>
                      </li>
                    </ul>
                  </template>
                  <template v-else>
                    <div class="field">
                      <input class="input" v-model="userAnswer" type="text" placeholder="Inserisci la tua risposta" @keyup.enter="checkAnswer" />
                    </div>
                  </template>
                  <p v-if="!isCorrect" class="has-text-danger">{{ answerMessage }}</p>
                </div>
              </div>
              <div v-else>
                <div class="box">
                  <p v-if="score === 10">Hai raggiunto un punteggio di 10!</p>
                  <p class="subtitle">Partita Terminata!</p>
                  <p class="subtitle">Punteggio: {{ score }} / 10</p>
                  <p v-if="score !== 10">Non hai raggiunto il punteggio perfetto. Riprova per avere un'altra possibilità!</p><br>
                  <button v-if="score === 10" class="button is-primary" @click="showGiftCard">Scarta Regalo</button><br><br>
                  <button class="button is-primary" @click="resetGame">Ricomincia</button>
                </div>
              </div>
            </div>
          </div>
        </section>
        <div class="container is-max-desktop">
          <div v-if="showCard" class="card">
            <div class="card-content">
              <div class="media">
                <div class="media-left">
                  <figure class="image is-48x48">
                    <img src="https://cliply.co/wp-content/uploads/2020/12/392012300_WRAPPED_GIFT_EMOJI_400.png" alt="regalo">
                  </figure>
                </div>
                <div class="media-content">
                  <p class="title is-4">Nome premio</p>
                  <p class="subtitle is-6">Sottotitolo</p>
                </div>
              </div>
              <div class="content">
    Bene <strong>giocatore</strong>, te lo sei guadagnato!. Goditi il tuo premio.<br>
    <p><strong>Codice coupon:</strong></p>
    <p class="notification is-primary gamekey" @click="copyKey">COUPONCODE</p>
    <p class="notification is-success" v-if="copied">Codice Copiato!</p>
  </div>
</div>
</div>
</div><br><br>
</div>
</div>
</template>
  
  <script>
  export default {
    data() {
      return {
        quizzes: [
  { type: 'multipleChoice', question: "In quale anno è stato rilasciato il videogioco 'Super Mario Bros.'?", options: ["1983", "1985", "1987", "1989"], answer: "1985" },
  { type: 'multipleChoice', question: "Qual è il nome del protagonista di 'The Witcher'?", options: ["Geralt di Rivia", "Ezio Auditore", "Sonic the Hedgehog", "Link"], answer: "Geralt di Rivia" },
  { type: 'text', question: "In che videogioco controlli un'iconica palla rosa che mangia tutto?", answer: "Kirby" },
  { type: 'multipleChoice', question: "Qual è il videogioco più venduto di tutti i tempi?", options: ["Minecraft", "Tetris", "GTA V", "Super Mario Bros."], answer: "Minecraft" },
  { type: 'text', question: "Qual è il nome del protagonista di 'The Legend of Zelda'?", answer: "Link" },
  { type: 'multipleChoice', question: "Chi è l'antagonista principale nel videogioco 'Half-Life'?", options: ["Gordon Freeman", "Barney Calhoun", "The G-Man", "Alyx Vance"], answer: "The G-Man" },
  { type: 'text', question: "Qual è il nome dell'androide di compagnia in 'Detroit: Become Human'?", answer: "Connor" },
  { type: 'multipleChoice', question: "Qual è il nome del personaggio principale in 'God of War'?", options: ["Kratos", "Atreus", "Zeus", "Athena"], answer: "Kratos" },
  { type: 'text', question: "In quale videogioco devi combattere contro demoni in un'ambientazione marziana?", answer: "Doom" },
  { type: 'multipleChoice', question: "Chi è il personaggio principale nel videogioco 'Uncharted'?", options: ["Nathan Drake", "Joel", "Ellie", "Lara Croft"], answer: "Nathan Drake" },
  { type: 'text', question: "Qual è il nome del protagonista di 'Final Fantasy VII'?", answer: "Cloud Strife" },
  { type: 'multipleChoice', question: "Qual è il nome dell'arma principale nel videogioco 'Fortnite'?", options: ["Spara tutto", "Lancia-granate", "Fucile d'assalto", "Piccone"], answer: "Fucile d'assalto" },
  { type: 'text', question: "In quale videogioco devi risolvere enigmi mentre esplori una città sotterranea chiamata Rapture?", answer: "BioShock" },
  { type: 'multipleChoice', question: "Quale videogioco è noto per il suo stile di gioco a puzzle basato su fisica e gravità?", options: ["Portal", "Call of Duty", "Overwatch", "Counter-Strike"], answer: "Portal" },
  { type: 'text', question: "Qual è il nome del protagonista di 'Assassin's Creed'?", answer: "Altair" },
  { type: 'multipleChoice', question: "Quale videogioco è ambientato in un mondo aperto post-apocalittico?", options: ["Fallout 4", "The Elder Scrolls V: Skyrim", "Red Dead Redemption 2", "The Last of Us"], answer: "Fallout 4" },
  { type: 'text', question: "Quale è la famosa sindrome di un noto gioco per GameBoy'?", answer: "Lavandonia" },
  { type: 'multipleChoice', question: "Chi è il protagonista del videogioco 'Sonic the Hedgehog'?", options: ["Sonic", "Tails", "Knuckles", "Dr. Robotnik"], answer: "Sonic" },
  { type: 'text', question: "Qual è il nome del pianeta principale in 'Mass Effect'?", answer: "Terra" },
  { type: 'multipleChoice', question: "Qual è il nome del personaggio principale nel videogioco 'The Last of Us'?", options: ["Joel", "Ellie", "Tommy", "Sarah"], answer: "Joel" },
  { type: 'text', question: "In quale videogioco controlli un'iconico idraulico italiano che salva la principessa Peach?", answer: "Super Mario" },
  { type: 'multipleChoice', question: "Quale videogioco ha un protagonista chiamato 'Master Chief'?", options: ["Halo", "Destiny", "Titanfall", "Gears of War"], answer: "Halo" },
  { type: 'text', question: "Qual è il nome del protagonista di 'Red Dead Redemption'?", answer: "John Marston" },
  { type: 'multipleChoice', question: "In quale videogioco devi affrontare mostri giganti chiamati 'Colossi'?", options: ["Shadow of the Colossus", "Monster Hunter: World", "Dark Souls", "Bloodborne"], answer: "Shadow of the Colossus" },
  { type: 'text', question: "Qual è il nome del protagonista di 'BioShock Infinite'?", answer: "Booker DeWitt" },
  { type: 'multipleChoice', question: "Quale videogioco è noto per le sue costruzioni e il combattimento 'Battle Royale'?", options: ["Fortnite", "Minecraft", "PlayerUnknown's Battlegrounds", "Apex Legends"], answer: "Fortnite" },
  { type: 'text', question: "In quale videogioco esplori un mondo subacqueo chiamato Rapture?", answer: "BioShock" },
  { type: 'multipleChoice', question: "Quale videogioco presenta un'ambientazione vittoriana steampunk e il personaggio di Booker DeWitt?", options: ["BioShock", "Deus Ex", "Bioshock Infinite", "Dishonored"], answer: "Bioshock Infinite" },
  { type: 'text', question: "Qual è il nome del protagonista di 'The Elder Scrolls V: Skyrim'?", answer: "Dragonborn" },
  { type: 'multipleChoice', question: "Quale videogioco è noto per la sua grafica pixel art e il personaggio di Steve?", options: ["Minecraft", "Terraria", "Stardew Valley", "Celeste"], answer: "Minecraft" },
  { type: 'text', question: "In quale videogioco sei alla ricerca della città perduta di El Dorado?", answer: "Uncharted" },
  { type: 'multipleChoice', question: "Qual è il nome del protagonista di 'Dark Souls'?", options: ["Solaire of Astora", "Artorias the Abysswalker", "Gwyn, Lord of Cinder", "Chosen Undead"], answer: "Chosen Undead" },
  { type: 'text', question: "In quale videogioco controlli un cuoco che deve preparare piatti in una cucina frenetica?", answer: "Overcooked" },
  { type: 'multipleChoice', question: "Quale videogioco presenta un mondo aperto ambientato nell'antica Grecia?", options: ["Assassin's Creed Odyssey", "God of War", "Spartan: Total Warrior", "Ryse: Son of Rome"], answer: "Assassin's Creed Odyssey" },
  { type: 'text', question: "Qual è il nome del protagonista di 'Half-Life'?", answer: "Gordon Freeman" },
  { type: 'text', question: "Qual'è il Pokémon clonato in laboratorio?", answer: "Mewtwo" },
  { type: 'text', question: "Un famoso team di villain presente sia in una saga videoludica sia animata", answer: "Team Rocket" },
  { type: 'multipleChoice', question: "Quale videogioco presenta un'ambientazione di guerra moderna e il personaggio di Soap MacTavish?", options: ["Call of Duty: Modern Warfare", "Battlefield 1", "Medal of Honor", "Spec Ops: The Line"], answer: "Call of Duty: Modern Warfare" },
  { type: 'text', question: "In quale videogioco controlli un cacciatore di mostri chiamato Geralt of Rivia?", answer: "The Witcher" },
  { type: 'multipleChoice', question: "Chi è il personaggio principale nel videogioco 'The Legend of Zelda: Ocarina of Time'?", options: ["Link", "Zelda", "Ganondorf", "Navi"], answer: "Link" },
  { type: 'text', question: "Qual è il nome del protagonista di 'Dead Space'?", answer: "Isaac Clarke" },
  { type: 'multipleChoice', question: "Quale videogioco è noto per i suoi veicoli da combattimento chiamati 'Sparrow'?", options: ["Destiny", "Warframe", "Anthem", "Titanfall"], answer: "Destiny" },
  { type: 'text', question: "In quale videogioco devi sopravvivere in un mondo pieno di zombie?", answer: "Resident Evil" },
  { type: 'multipleChoice', question: "Qual è il nome del personaggio principale nel videogioco 'The Last Guardian'?", options: ["Trico", "Ico", "Wander", "Agro"], answer: "Trico" },
  { type: 'text', question: "In quale videogioco sei un cacciatore di demoni chiamato Dante?", answer: "Devil May Cry" },
  { type: 'multipleChoice', question: "Quale videogioco presenta un mondo sottomarino chiamato Rapture?", options: ["BioShock", "Subnautica", "Soma", "Depth"], answer: "BioShock" },
  { type: 'text', question: "Qual è il nome del protagonista di 'Metal Gear Solid 2: Sons of Liberty'?", answer: "Raiden" },
  { type: 'multipleChoice', question: "Un famoso glitch nei primi giochi Pokémon", options: ["404 not found", "System error", "Missingn0", "Errore01"], answer: "Missingn0" },
  // Aggiungi altre domande a tema gaming
  ],
      currentQuestion: null,
      selectedOption: null,
      isCorrect: true,
      answerMessage: "",
      attempts: 0,
      score: 0,
      questionsRemaining: [],
      userAnswer: "",
      currentQuestionIndex: 0,
      showCard: false,
      questionCount: 0,
      copied: false, // Variabile per gestire il messaggio "Chiave copiata"
    };
  },
  methods: {
    startGame() {
      this.attempts++;
      this.score = 0;
      this.shuffleQuestions();
      this.nextQuestion();
    },
    shuffleQuestions() {
      this.questionsRemaining = [...this.quizzes];
      for (let i = this.questionsRemaining.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [this.questionsRemaining[i], this.questionsRemaining[j]] = [this.questionsRemaining[j], this.questionsRemaining[i]];
      }
      this.questionsRemaining = this.questionsRemaining.slice(0, 10);
    },
    nextQuestion() {
      if (this.currentQuestionIndex < this.questionsRemaining.length) {
        this.currentQuestion = this.questionsRemaining[this.currentQuestionIndex];
        this.isCorrect = true;
        this.answerMessage = "";
        this.questionCount = this.currentQuestionIndex + 1; // Incrementa il contatore
      } else {
        this.currentQuestion = null;
      }
    },
    checkAnswer() {
      const currentQuestion = this.currentQuestion;
      if (currentQuestion) {
        if (currentQuestion.type === 'multipleChoice') {
          if (this.selectedOption === currentQuestion.answer) {
            this.score++;
            this.isCorrect = true;
            this.answerMessage = "Risposta esatta!";
          } else {
            this.isCorrect = false;
            this.answerMessage = "Risposta sbagliata. Riprova.";
          }
        } else {
          if (this.userAnswer.toLowerCase() === currentQuestion.answer.toLowerCase()) {
            this.score++;
            this.isCorrect = true;
            this.answerMessage = "Risposta esatta!";
          } else {
            this.isCorrect = false;
            this.answerMessage = "Risposta sbagliata. Riprova.";
          }
        }
        this.userAnswer = "";
        this.selectedOption = null;
        if (this.currentQuestionIndex < this.questionsRemaining.length - 1) {
          this.currentQuestionIndex++;
          this.nextQuestion();
        } else {
          this.endGame();
        }
      }
    },
    resetGame() {
      this.attempts = 0;
      this.score = 0;
      this.currentQuestion = null;
      this.isCorrect = true;
      this.answerMessage = "";
      this.userAnswer = "";
      this.selectedOption = null;
      this.currentQuestionIndex = 0;
      this.shuffleQuestions();
    },
    endGame() {
      this.currentQuestion = null;
    },
    showGiftCard() {
      if (this.score === 10) {
        this.showCard = true;
      }
    },
    copyKey() {
      const key = "COUPONCODE"; // Chiave da copiare
      const tempInput = document.createElement("input");
      tempInput.value = key;
      document.body.appendChild(tempInput);
      tempInput.select();
      document.execCommand("copy");
      document.body.removeChild(tempInput);
      this.copied = true;
    },
  },
};
</script>