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
             this.timer = useTimer(this.time, false);

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
        <h1>Quests for the Quest Gods!</h1>
        <p>Go on a Quest and collect items as offering to the Quest Gods!</p>
        <p>Look for the requested item and scan the code!</p>
        <p>Collect as many items as possible within the given time!</p>
        <p>Compare yourself to others and find the Quest God's favorite!</p>
        <button @click="startQuest">Start Quest</button>
    </div>
    <div v-if="state.state == 1">
        <h2>Questing!</h2>
        <p><span>Time left: </span><strong>{{timer.minutes}}:{{timer.seconds}}</strong><span></span></p>
        <p>Items collected: <strong>{{ state.score }}</strong></p>
        <p>Your next quest item is:<br><br>
            <strong class="big">{{state.current}}</strong></p>
        <form @submit="checkIfCorrect" ><input type="text" name="item" autofocus v-model="state.guess" /></form>
        <p><small>(Make sure input field is in focus when scanning)</small></p>
    </div>
    <div v-if="state.state == 2">
        <h2>Your quest is over!</h2>
        <p>You've collected <strong>{{ state.score }}</strong> items to offer to the quest gods.</p>
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
 .big {
     font-size: 1.3em;
 }

</style>
