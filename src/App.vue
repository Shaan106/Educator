<template>
  <div id="app">
    <div class="coolWords">
      <h1>{{speltWord}}</h1>
      <hr> 
      <h1>ted</h1>
    </div>
    <div class="Centre">
      <div>
        <div class="circle" v-bind:class="{'selected':selectedIndex===i,'betterselected':includesLetter(letter)}" v-for="(letter,i) in letters" v-bind:key="letter">
          {{letter}}
        </div>
      </div>
    </div>
    <div>
      <div class="arrows">
        <ArrowLeftIcon size="4x" v-on:click="addStep('left')"/>
        <div class="specialButtons">
        <ArrowUpIcon size="4x" v-on:click="addStep('up')"/>
        <XCircleIcon size="4x" v-on:click="addStep('select')"/>
        <ArrowDownIcon size="4x" v-on:click="addStep('down')"/>
        </div>
        <ArrowRightIcon size="4x" v-on:click="addStep('right')"/>
      </div>
      <div class="steps">
        <div class="step" v-bind:class="{'selected':selectedStep===i}" v-for="(step,i) in steps" v-bind:key="step">
        <ArrowLeftIcon size="4x" v-if="step==='left'"/>
        <ArrowUpIcon size="4x" v-if="step==='up'"/>
        <XCircleIcon size="4x" v-if="step==='select'"/>
        <ArrowDownIcon size="4x" v-if="step==='down'"/>
        <ArrowRightIcon size="4x" v-if="step==='right'"/>
        </div>
        <div class="step">
           <PlayIcon size="4x" v-on:click="startStep()"/>
        </div>
      </div>  
    </div>
  </div>
</template>

<script>
import {PlayIcon,ArrowUpIcon,ArrowDownIcon,ArrowLeftIcon,ArrowRightIcon,XCircleIcon} from "vue-feather-icons";
function shuffleArray(array) {
    for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
    }
}
function index2Coord(i) {
  return {x: i%5, y: Math.floor(i/5)}
}
function coord2Index(coord) {
  return(coord.y * 5 + coord.x)
}
export default {
  name: 'app',
  components: {PlayIcon,ArrowUpIcon,ArrowDownIcon,ArrowLeftIcon,ArrowRightIcon,XCircleIcon
  },
  data: function(){
    return{letters: ["a","b","c","d","e",
              "f","g","h","i","j",
              "k","l","m","n","o",
              "p","q","r","s","t",
              "z","u","v","y","x",
              ],
              steps:[],
              selectedCoord: {x:2,y:2},
              selectedStep: -1,
              speltWord: ''
              }
  },
  created: function(){
    shuffleArray(this.letters);
  },
  methods:{
    includesLetter: function(letter) {
      return this.speltWord.includes(letter);
    },
    addStep: function(press){
      this.steps.push(press);
    },
    startStep: function() {
      this.selectedStep = -1;
      this.speltWord = '';
      this.nextStep();
    },
    nextStep: function() {
      this.selectedStep += 1;
      if(this.selectedStep=== this.steps.length) {
        this.selectedStep = -1;
        this.selectedCoord.x = 2;
        this.selectedCoord.y = 2;
        return;
      }
      let step = this.steps[this.selectedStep];
      if(step==='left') {
        this.selectedCoord.x -= 1;
      }
      if(step==='right') {
        this.selectedCoord.x += 1;
      }
      if(step==='up') {
        this.selectedCoord.y -= 1;
      }
      if(step==='down') {
        this.selectedCoord.y += 1;
      }
      if(step==='select') {
        this.speltWord += this.letters[this.selectedIndex];
      }
        setTimeout(this.nextStep, 1000);
    }
  },
  computed:{
    selectedIndex: function(){
      return coord2Index(this.selectedCoord);
    }
  }
}
</script>

<style>
body {
  margin:0;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  display: flex;
  flex-direction: row;
  width: 100vw;
  height: 100vh;
}
#app>div:not(.Centre){
  width: 200px;
  background-color: cornflowerblue;
}
#app>div.Centre {
  flex-grow: 1;
  background-color: lightgray;
  display: flex;
  align-items: center;
  justify-content: center;
}
#app>div.Centre>div {
  display: grid;
  grid-template-columns: repeat(5,auto);
  grid-gap: 32px;
}
.circle {
  width: 75px;
  height: 75px;
  background-color: lightgoldenrodyellow;
  text-align: center;
  line-height: 75px;
  border-radius: 40.5px;
  font-size: 23px;
  border:black solid 3px;
}
.circle.selected {
  border-color: red;
  color: red;
}
.circle.betterselected {
  background-color:black;
  color:whitesmoke;
}
.step.selected {
  color: red;
}
.arrows {
  width: 200px;
  height: 200px;
  background-color: lightcoral;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.feather-play {
  color:lightgreen;
}
.arrows .feather:hover {
  color:lightpink;
  cursor: pointer;
}
.specialButtons {
  display: flex;
  align-self: space-between;
  justify-self: center;
  flex-direction: column;
}
.steps {
  display: flex;
  justify-content: flex-start;
  flex-wrap: wrap;
  padding: 3.5px;
}
.coolWords {
  text-align: center;
  color: rebeccapurple;
}
.coolWords h1 {
  font-size: 53px;
}
</style>
