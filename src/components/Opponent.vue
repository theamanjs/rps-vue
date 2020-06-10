<template>
  <div class="opponent-side">
                    
    <div class="text-center">Opponent's Score</div>
    <div class="stars" > 
      <div class="star-one">
        <i 
          :class="{ fas: summary.opponent.score.one, far: !summary.opponent.score.one }"
          class="fa-star" 
          id="opponentScoreOne"
        ></i>
      </div>
      <div class="star-two">
        <i 
          :class="{ fas: summary.opponent.score.two, far: !summary.opponent.score.two }"
          class="fa-star" 
          id="opponentScoreSecond"
        ></i>
      </div>
      <div class="star-three">
        <i 
          :class="{ fas: summary.opponent.score.three, far: !summary.opponent.score.three }"
          class="fa-star" 
          id="opponentScoreThird"
        ></i>
      </div>
    </div>

    <div class="selecting-items">
      <div class="item-placement center" >
        <img :src=source alt="Selecting" id="opponentStatus">
      </div> 
    </div>

    <div class="status-opponent text-center">
      {{ (move) ? summary.opponent.made : summary.opponent.make}}
    </div>

    <!-- <div class="status-opponent text-center" v-if='!move'>{{ summary.opponent.make }}</div> -->
    <!-- <div class="status-opponent text-center" v-if='move'>{{ summary.opponent.made }}</div> -->

  </div>
</template>

<script>
  import { eventBus } from "./../main.js";
  export default {
    props: ['summary'],
    data() {
      return {
        opponentSelection : ['stone', 'paper', 'scissors', 'stone', 'paper', 'scissors', 'stone', 'paper', 'scissors'],
        source: require('./../assets/images/loading.svg'),
        opponentMove: '',
        move: false,
        tempResult: ''
      }
    },
    methods: {
      limit (max, min){
        return Math.round(Math.random() * (max - min) + min);
      },
      selectionOpponent(){
        this.opponentMove = this.opponentSelection[this.limit(8,0)];
      },
      resetOpponent(){
        if(this.opponentMove == 'stone') this.source = require('./../assets/images/stone.svg'); 
        if(this.opponentMove == 'paper') this.source = require('./../assets/images/paper.svg'); 
        if(this.opponentMove == 'scissors') this.source = require('./../assets/images/scissors.svg'); 
          
        this.move = true;
        setTimeout(() => {
          this.source = require('./../assets/images/loading.svg');
          this.move = false;
        }, this.limit(1500, 1700));
        
        this.opponentMove = '';

        setTimeout(() => {
          if(this.source == "img/loading.e5f52055.svg")
          this.source = require('./../assets/images/tick.png');
          this.move = true;
        }, this.limit(1900, 2000));
      },
      afterCalculation(oppMove, result){
        if(this.summary.opponent.score.no >= 3) return;
        this.tempResult = result;
        
        eventBus.$emit('calculatedResult', this.tempResult);
        eventBus.$emit('oppMove', oppMove);
        
        this.resetOpponent();
        
        if(result == 'lose'){
          this.summary.opponent.score.no += 1;
          
          if(this.summary.opponent.score.one == false){
            this.summary.opponent.score.one = true; 
            return;
          } 
          if(this.summary.opponent.score.one == true && this.summary.opponent.score.two == false){
            this.summary.opponent.score.two = true; 
            return;
          } 
          if(this.summary.opponent.score.one == true && this.summary.opponent.score.three == false){
            this.summary.opponent.score.three = true; 
            return;
          }
        }

        if(result == 'won'){
          this.summary.proponent.score.no += 1;
          if(this.summary.proponent.score.one == false){
            this.summary.proponent.score.one = true; 
            return;
          } 
          if(this.summary.proponent.score.one == true && this.summary.proponent.score.two == false){
            this.summary.proponent.score.two = true; 
            return;
          } 
          if(this.summary.proponent.score.one == true && this.summary.proponent.score.three == false){
            this.summary.proponent.score.three = true; 
            return;
          }
        }
      }
    },
    mounted() {
      eventBus.$on('selection', (playerMove) => {
        this.selectionOpponent();

        if(this.opponentMove == playerMove){
          this.afterCalculation(this.opponentMove, 'tie');
          return;
        }
        
        if(this.opponentMove == 'stone' && playerMove == 'paper'){
          this.afterCalculation(this.opponentMove, 'won'); 
          return;
        } 
        
        if(this.opponentMove == 'stone' && playerMove == 'scissors'){
          this.afterCalculation(this.opponentMove, 'lose'); 
          return;
        } 
        
        if(this.opponentMove == 'paper' && playerMove == 'scissors'){
          this.afterCalculation(this.opponentMove, 'won'); 
          return;
        } 
        
        if(this.opponentMove == 'paper' && playerMove == 'stone'){
          this.afterCalculation(this.opponentMove, 'lose'); 
          return;
        } 
        
        if(this.opponentMove == 'scissors' && playerMove == 'stone'){
          this.afterCalculation(this.opponentMove, 'won'); 
          return;
        } 
        
        if(this.opponentMove == 'scissors' && playerMove == 'paper'){
          this.afterCalculation(this.opponentMove, 'lose'); 
          return;
        } 
      });
      
      setTimeout(() => {
        this.source = require('./../assets/images/tick.png');
        this.move = true;
      }, this.limit(1000, 1200));
    }
  }
</script>

<style>
  .opponent-side{
    width: 50%;
  }
</style>