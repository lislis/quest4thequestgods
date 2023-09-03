<script>
 import {  watchEffect, onMounted } from "vue";
 import { useTimer } from 'vue-timer-hook';

 const randomThing = (arr) => {
     return arr[Math.floor(Math.random() * arr.length)];
 }


 export default {
     data() {
         return {
             time: null,
             timer: null,
             state: {
                 words: [],
                 score: 0,
                 current: null,
                 guess: null,
                 state: 0
             }
         }
     },
     created() {
         this.time = new Date();
         this.time.setSeconds(this.time.getSeconds() + 180);
         this.timer = useTimer(this.time, false);

         //console.log(this.state)
         fetch(`input.txt`).then(r => r.text()).then(d => {
             this.state.words = d.split('\n').filter(x => x !== "");
             this.state.current = randomThing(this.state.words);
         });

         watchEffect(async () => {
             if(this.timer.isExpired.value) {
                 console.warn('IsExpired')
             }
         })
     },
     methods: {
         startQuest() {
             console.log("start");
             this.timer.start();
             this.state.state = 1;
             //console.log(this.state);
         },
         restartQuest() {
             this.time = new Date();
             this.time.setSeconds(this.time.getSeconds() + 180);

             this.state.state = 0;
             this.state.score = 0;

             console.log(this.state);
         },
         newQuest() {
             this.state.guess = null;
             this.state.current = randomThing(this.state.words);
         },
         checkIfCorrect(ev) {
             ev.preventDefault();
             if (this.state.current === this.state.guess) {
                 this.state.score += 1;
                 this.newQuest();
             } else {
                 this.newQuest();
             }
         }
     },
     watch: {
         "timer.isExpired": {
             handler(mew, old) {
                 if (mew) {

                     this.state.state = 2;
                 }
                 //console.log(newTitle)
             },
             immediate: true,
         }
     }
 }

</script>

<template>
    <div v-if="state.state == 0">
        <h1>Quests for the Questgods!</h1>
        <p>Collect as many quest items as possible to offer them to the Questgods!</p>
        <button @click="startQuest">Start Quest</button>
    </div>
    <div v-if="state.state == 1">
        <h2>Questing!</h2>
        <p><span>You have </span><span>{{timer.minutes}}</span>:<span>{{timer.seconds}}</span><span> left.</span></p>
        <p>You've quested {{ state.score }} items.</p>
        <p>Your next quest item is <strong>{{state.current}}</strong></p>
        <form @submit="checkIfCorrect" ><input type="text" name="item" autofocus v-model="state.guess" /></form>
    </div>
    <div v-if="state.state == 2">
        <h2>Your quest is over!</h2>
        <p>You offered {{ state.score }} items to the quest gods.</p>
        <button @click="restartQuest()">New Quest</button>
    </div>
</template>

<style>

 body {
     font-family: sans-serif;
     background-color: mintcream;
     color: dimgray;
 }

 button {
     background-color: dimgray;
     color: white;
     border: none;
     padding: 0.8em 1em 0.6em;
     font: inherit;
     border-radius: 6px;
     cursor: pointer;
 }
 button:hover,
 button:active {
     background-color: black;
 }

</style>
