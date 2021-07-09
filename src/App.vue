<template>
  <div id="app">
    <div class="wrapper clearfix"> 
      <!-- PLAYERS -->
      <players 
        v-bind:scoresPlayer="scoresPlayer"
        v-bind:activePlayer="activePlayer" 
        v-bind:currentPlayer="currentPlayer"
        v-bind:isWinner="isWinner"
        />
      
      <!-- CONTROLS -->
      <controls
        v-bind:isPlaying="isPlaying"
        v-bind:finalScore="finalScore"

        v-on:handleNewGame="handleNewGame"
        v-on:handleRollDice="handleRollDice" 
        v-on:handleHoldScore="handleHoldScore"
        v-on:handleChangeFinalScore="handleChangeFinalScore"
        /> 

      <!-- DICES -->
      <dices
        v-bind:numbersDice="numbersDice"
        />

      <!-- POPUP RULE -->
      <popup-rule
        v-bind:isOpenPopup="isOpenPopup"

        v-on:handleConfirm="handleConfirm"
        />
    </div>
  </div>
</template>

<script>
// IMPORT
import Players from './components/Players.vue'
import Controls from './components/Controls.vue'
import Dices from './components/Dices.vue'
import PopupRule from './components/PopupRule.vue'

export default {
  name: 'app',
  components: {
    Players,
    Controls,
    Dices,
    PopupRule
  },
  data () {
    return {
      scoresPlayer: [0, 0],
      numbersDice: [3, 4],
      activePlayer: 0,
      currentPlayer: 0,
      isPlaying: false,
      isOpenPopup: false,
      finalScore: 10
    }
  },
  methods: {
    handleNewGame() {
      this.isPlaying = true;
      this.isOpenPopup = true;
    },
    handleConfirm() {
      this.isOpenPopup = false;
      this.isPlaying = true;

      this.numbersDice = [1, 1];
    },
    handleRollDice() {
      if (this.isPlaying) {
        let numberDice1 = Math.floor(Math.random() * 6) + 1;
        let numberDice2 = Math.floor(Math.random() * 6) + 1;

        this.numbersDice = [numberDice1, numberDice2];

        if (numberDice1 == 1 || numberDice2 == 1) {
          setTimeout(() => {

            alert(`Người chơi Player ${this.activePlayer + 1} đã quay vào số 1. Đen ghê !!!`);
          }, 10);

          this.currentPlayer = 0;
          this.nextPlayer();
          console.log('nextPlayer ', this.activePlayer)
        } else {
          this.currentPlayer = numberDice1 + numberDice2;
        }
      } else {
        alert('Hãy bấm New Game để bắt đầu trò chơi !!!');
      }
    },
    handleHoldScore() {
      if (this.isPlaying) {
        let scoresPlayerClone = [...this.scoresPlayer];
        scoresPlayerClone[this.activePlayer] += this.currentPlayer;

        this.scoresPlayer = [...scoresPlayerClone];

        this.currentPlayer = 0;
        if (!this.isWinner) {
          this.nextPlayer();
        }
      } else {
        alert('Hãy bấm New Game để bắt đầu trò chơi !!!');
      }
    },
    handleChangeFinalScore(event) {
      let newScore = parseInt(event.target.value);

      if (newScore <= 0 || isNaN(newScore)) {
        this.finalScore = '';
      } else {
        this.finalScore = newScore;
      }
    },
    nextPlayer() {
      this.activePlayer = this.activePlayer == 0 ? 1 : 0; 
    }
  },
  computed: {
    isWinner() {
      if (this.scoresPlayer[this.activePlayer] >= this.finalScore) {
        this.isPlaying = false;
        return true;
      }
      return false;
    }
  }
}
</script>

<style>
  * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      
  }

  html {
    font-size: 62.5%;
  }

  .clearfix::after {
      content: "";
      display: table;
      clear: both;
  }

  body {
      background-image: linear-gradient(rgba(62, 20, 20, 0.4), rgba(62, 20, 20, 0.4)), url('./assets/back.jpg');
      background-size: cover;
      background-position: center;
      font-family: Lato;
      font-weight: 300;
      position: relative;
      height: 100vh;
      color: #555;
  }

  .wrapper {
      width: 1000px;
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      background-color: #fff;
      box-shadow: 0px 10px 50px rgba(0, 0, 0, 0.3);
      overflow: hidden;
  }
</style>
