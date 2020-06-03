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

    <div class="status-opponent text-center" v-if='!move'>{{ summary.opponent.make }}</div>
    <div class="status-opponent text-center" v-if='move'>{{ summary.opponent.made }}</div>

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
        setTimeout(() => {
          this.source = require('./../assets/images/loading.svg');
        }, this.limit(700, 900));
        this.move = false;
        this.opponentMove = '';
        // this.selectionOpponent();
        setTimeout(() => {
          this.source = require('./../assets/images/tick.png');
          this.move = true;
        }, this.limit(901, 1300));
        // setTimeout(() => {
          //   this.source = require('./../assets/images/tick.png');
        //   this.move = true;
        //   // this.test();
        // }, this.limit(800, 1000));
      },
      afterCalculation(oppMove, result){
        // console.log(oppMove, result)
        if(this.summary.opponent.score.no >= 3) return;
        this.tempResult = result;
        console.log(this.tempResult, 'temp');
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
        // let tempSource = './../assets/images/' + oppMove + '.svg'; 
        // console.log(tempSource);
        // this.source = require('./../assets/images/stone.svg');
      }
    },
    computed: {
    },
    mounted() {
      eventBus.$on('selection', (playerMove) => {
        this.selectionOpponent();

        console.log(this.opponentMove, 'test Opp');
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
        // this.test();
        
      }, this.limit(800, 1000));
    }
  }
</script>

<style>
  .opponent-side{
    width: 50%;
  }
</style>