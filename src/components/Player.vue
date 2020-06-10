<template>
    <div class="my-side">
      <div class="text-center">Your Score</div>
      <div class="stars">
        <div class="star-one">
          <i 
            :class="{ fas: summary.proponent.score.one, far: !summary.proponent.score.one }"
            class="fa-star" 
            id="myScoreOne"
            ></i>
        </div>
        <div class="star-two">
          <i 
            :class="{ fas: summary.proponent.score.two, far: !summary.proponent.score.two }"
            class="fa-star" 
            id="myScoreSecond"
            ></i>
        </div>
        <div class="star-three">
          <i 
            :class="{ fas: summary.proponent.score.three, far: !summary.proponent.score.three }"
            class="fa-star" 
            id="myScoreThird">
            </i>
        </div>
      </div>

      <div class="selecting-items">
        <div class="stone item-placement">
          <img 
            @click="playerOption('stone')"
            v-if="items.stone"
            id="stone" 
            src="../assets/images/stone.svg" 
            alt=""
            >
        </div>
        <div class="paper item-placement">
          <img 
            @click="playerOption('paper')"
            v-if="items.paper"
            id="paper" 
            src="../assets/images/paper.svg" 
            alt=""
            >
        </div>
        <div class="scissors item-placement">
          <img 
            @click="playerOption('scissors')"
            v-if="items.scissors"
            id="scissors" 
            src="../assets/images/scissors.svg" 
            alt=""
            >
        </div>
      </div>

    <div class="status-my text-center">
      {{ (move) ? summary.proponent.made  : summary.proponent.make}}
    </div>

    <!-- <div class="status-my text-center" v-if="!move">{{ summary.proponent.make }}</div> -->
    <!-- <div class="status-my text-center" v-if="move">{{ summary.proponent.made }}</div> -->
  </div>
</template>

<script>
  import { eventBus } from '../main.js';
  export default {
    props: ['summary','items'],
    data(){
      return{
        playerSelection: '',
        move: false,
        isCalculating: false
      }
    },
    methods: {  
      playerOption(item){
        if(this.isCalculating == false){
          (item == 'stone') ? (this.items.scissors = false, this.items.paper = false) : (this.items.stone = false);
          (item == 'paper') ? (this.items.scissors = false, this.items.stone = false) : (this.items.paper = false);
          (item == 'scissors') ? (this.items.stone = false, this.items.paper = false) : (this.items.scissors = false);
          this.playerSelection = item;
          
          eventBus.$emit('selection', this.playerSelection);
          this.move = true;
          this.isCalculating = true;
        }
      }
    },
    mounted(){
      eventBus.$on('calculatedResult', () => {
        setTimeout(() => {
          this.items.scissors = true;
          this.items.stone = true;
          this.items.paper = true;
          this.move = false;
          this.isCalculating = false;
        }, 1500);
      })
    }
  }
</script>

<style scoped>
  .my-side{
    width: 50%;
  }
  .item-placement{
    margin: 0 auto;
  }
</style>