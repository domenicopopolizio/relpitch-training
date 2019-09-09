<template>
  <div id="app">  
    <div>
      <img id="logo" src="./assets/icon.svg">
    </div>
    <br>
    <br>
    <instrument :freq="freq1" :distlt="distlt" :distgt="distgt" @input="guessed1=$event"></instrument>
    <instrument :freq="freq2" :distlt="distlt" :distgt="distgt" @input="guessed2=$event"></instrument>
    <br><br>
    
    <b-button variant="primary" @click="getResult" v-if="!tested">Check</b-button>
    <h3 v-if="tested">
      Similar: <span :class=" {'text-danger':result>10, 'text-warning':result>=5&&result<=10, 'text-success':result<10}">{{result}}</span>% 
    </h3>
    <br><br>
    
    <b-button variant="success" @click="resetAll" v-if="tested">Try Again!</b-button>
  </div>
</template>

<script>  
import Instrument from './components/Instrument.vue'
export default {
  name: 'app',
  data() {
    return {
      notes: [246.9,261.6,277.2,293.7,311.1,329.6,349.2,370.0,392.0,415.3,440.0,466.2,493.9,523.3,554.4,587.3,622.3,659.3,698.5,740.0,784.0,830.6,880.0,932.3,987.8,1047],
      note: 0,
      freq1: 0,
      freq2: 0,
      distlt: 0,
      distgt: 0,
      guessed1: 0,
      guessed2: 0,
      result: 100,
      tested: false,
    }
  }, 
  mounted() {
    this.resetAll()
  },
  methods: {
    getResult(changeTest=true) {
      this.result = ((Math.max(this.guessed1, this.guessed2) - Math.min(this.guessed1, this.guessed2))*100/( (this.note+this.distgt) - (this.note-this.distlt) )).toFixed(2);
      this.tested = true; 
    },
    getRandomArbitrary(min, max) {
        return Math.random() * (max - min) + min;
    }, 
    getRandomInt(min, max) {
        min = Math.ceil(min);
        max = Math.floor(max);
        return Math.floor(Math.random() * (max - min + 1)) + min;
    },
    resetNotes() {
      let n = this.getRandomInt(1, this.notes.length-2);
      let note = this.notes[n];
      let lt = (note-this.notes[n-1])/2;
      let gt = (this.notes[n+1]-note)/2;

      let f1 = this.getRandomArbitrary(note-lt, note+gt);
      let f2 = this.getRandomArbitrary(note-lt, note+gt);

      this.note = note;
      this.freq1 = f1;
      this.freq2 = f2;
      this.distlt = lt;
      this.distgt = gt;

      let r = this.result = ((Math.max(this.freq1, this.freq2) - Math.min(this.freq1, this.freq2))*100/( (this.note+this.distgt) - (this.note-this.distlt) )).toFixed(2);
      
      if(r < 5) {
        this.resetNotes();
      }


    },
    resetAll() {
      this.tested=false;
      this.resetNotes();
    }
  },
  components:  {
    Instrument
  } 
}
</script>

<style lang="scss">  

@import 'node_modules/bootstrap/scss/bootstrap';
@import 'node_modules/bootstrap-vue/src/index.scss';

#app {
  text-align: center;
  padding: 30px;
}

#logo {
  width: 200px;
  height: 200px;
  box-shadow: 0px 0px 10px #c0c0c0;
  border-radius: 200px;
  padding: 20px;
}
</style>
