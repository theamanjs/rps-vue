<template>
  <div class="container">
    <div class="inner-container">
      <!-- Custom Components -->
      <round :roundInfo="round"></round>

      <div class="wrapper-playing-area">
        <player :summary="processing" :items="items"></player>
        <opponent :summary="processing"></opponent>
      </div>

      <Result :processResult="processing" :resTag="resultTag" :items="items" :roundInfo="round"></Result>
      <!-- EO Custom Components  -->

      <!-- Audio files for application  -->
      <audio src="./assets/sounds/tink.wav" id="tink"></audio>
      <audio src="./assets/sounds/lose.wav" id="lose"></audio>
      <audio src="./assets/sounds/win.wav" id="won"></audio>
      <!-- EO Audio Section -->
    </div>
  </div>
</template>

<script>
import Result from "./components/Result.vue";
import Round from "./components/Round.vue";
import Player from "./components/Player.vue";
import Opponent from "./components/Opponent.vue";
export default {
  data() {
    return {
      round: {
        number: 1
      },
      resultTag: {
        won: "You Won",
        lose: "You Lose",
        end: "Game End",
        tie: "Tie",
        null: "Null"
      },
      items: {
        stone: true,
        paper: true,
        scissors: true
      },
      processing: {
        proponent: {
          make: "Make a pick",
          made: "You made a pick",
          score: {
            no: 0,
            one: false,
            two: false,
            three: false
          }
        },
        opponent: {
          make: "Opponent is making a pick",
          made: "Opponent made a pick",
          score: {
            no: 0,
            one: false,
            two: false,
            three: false
          }
        }
      }
    };
  },
  components: {
    Result,
    Round,
    Player,
    Opponent
  }
};
</script>

<style >
body {
  margin: 0;
  padding: 0;
  color: white;
  overflow: hidden;
  font-family: sans-serif;
  font-weight: normal;
}

.container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background: linear-gradient(#0984e3, #74b9ff);
}

.inner-container {
  height: 90vh;
  width: 90vw;
  display: flex;
  flex-direction: column;
  align-content: stretch;
}

.wrapper-playing-area {
  display: flex;
  /* margin-top: 20px; */
  height: 70vh;
  align-items: stretch;
}

.text-center {
  text-align: center;
  font-size: 24px;
  font-weight: bold;
}

/* Styling for Stars which depicts score on both area  */
.stars {
  display: flex;
  justify-content: center;
}

.star-one,
.star-two,
.star-three {
  width: 24px;
  margin: 0 5px;
  height: 24px;
  font-size: 22px;
}

.selecting-items {
  display: flex;
  align-content: stretch;
  justify-content: space-around;
  height: 50%;
}

.item-placement {
  display: flex;
  justify-content: center;
  align-items: center;
  /* margin: 0 auto; */
  width: 33.3%;
}

.center {
  margin: 0 auto;
}

img {
  width: 80px;
  cursor: pointer;
}

@media only screen and (max-width: 600px) {
  .wrapper-playing-area {
    flex-direction: column-reverse;
    height: 70vh;
  }

  .stars {
    margin: 2px;
  }

  img {
    width: 60px;
  }

  .text-center {
    font-size: 18px;
  }

  h4 {
    font-size: 24px;
  }

  h1 {
    font-size: 36px;
  }

  .my-side,
  .opponent-side {
    height: 50%;
    width: 100% !important;
  }

  .status-opponent {
    display: none;
  }

  .status-my {
    display: none;
  }
}
</style>
