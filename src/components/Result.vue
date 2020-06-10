<template>
  <div class="text-center">
      <!-- <p>{{ processResult.proponent.score.no }} : {{ processResult.opponent.score.no}}</p> -->
    <h4 
      v-if="result.won && result.overall == ''"
      id="result"
      >{{ resTag.won }} </h4>
    <h4 
      v-if="result.lose && result.overall == ''"
      id="result"
      >{{ resTag.lose }}</h4>
    <h4 
      v-if="result.tie"
      id="result"
      >{{ resTag.tie }}</h4>
    <h4 
      v-if="result.null"
      class="null"
      id="result"
      >{{ resTag.null }}</h4>
    <h4 
      v-if="result.end && result.overall == 'won'"
      id="result"
      >{{ resTag.end }} : {{ resTag.won }} 🥳</h4>
    <h4 
      v-if="result.end && result.overall == 'lose'"
      id="result"
      >{{ resTag.end }} : {{ resTag.lose }} 😞</h4>
  </div>
</template>

<script>
import { eventBus } from '../main.js';
export default {
  props: ['resTag', 'processResult', 'items', 'roundInfo'],
  data() {
    return {
      result: {
        won: false,
        lose: false,
        tie: false,
        end: false,
        overall: '',
        null: true
      }
    };
  },
  methods: {
    checkFinal(){
      if(this.processResult.proponent.score.no == 3){
        this.result.null = false;
        this.result.overall = 'won';
        this.result.end = true;
        setTimeout(() => {
          this.result.overall = '';
          this.resetGame();
        }, 1500);
      }
      if(this.processResult.opponent.score.no == 3){
        this.result.null = false;
        this.result.overall = 'lose';
        this.result.end = true;
        setTimeout(() => {
          this.result.overall = '';
          this.resetGame();
        }, 1500);
      }
    },
    resetGame(){
      this.result.end = false;  
      this.result.null = true;  
      this.roundInfo.number = 1;
      this.processResult.opponent.score.no = 0;
      this.processResult.opponent.score.one = false;
      this.processResult.opponent.score.two = false;
      this.processResult.opponent.score.three = false;
      this.processResult.proponent.score.no = 0;
      this.processResult.proponent.score.one = false;
      this.processResult.proponent.score.two = false;
      this.processResult.proponent.score.three = false;
      
    }
  },
  computed: {
    
  },
  created(){
    eventBus.$on('calculatedResult', (afterResult) => {
      this.roundInfo.number += 1;

      if(afterResult == 'won') {
        this.result.null = false;
        this.result.lose = false;
        this.result.tie = false;
        this.result.won = true;
      }
      if(afterResult == 'lose') {
        this.result.null = false;
        this.result.won = false;
        this.result.tie = false;
        this.result.lose = true;
      }
      if(afterResult == 'tie') {
        this.result.null = false;
        this.result.won = false;
        this.result.lose = false;
        this.result.tie = true;
      }
      setTimeout(() => {
        this.result.won = false;
        this.result.lose = false;
        this.result.tie = false;
        this.result.null = true;
      }, 1500);
      setInterval(() => {
        this.checkFinal();
      }, 200);
    })
  }
};
</script>

<style scoped>
#result {
  min-height: 10px;
}
.null{
  color: rgba(0,0,255,0.01) !important;
}
@media only screen and (max-width: 600px){ 
  
}
</style>