<template>
    <div>
        <template v-if="gameFinish">
            <h1>G A M E  O V E R</h1>
            <button @click="createBoard">New Game</button>
        </template>

        <h3>Match Count {{matchCount}}</h3>
        <div class="memory-game">
            <div class="board">
                <div class="card" v-for="(item, index) in cards" :key="index" @click="onClick(item)">
                    <img :src="item.image" v-if="flip(item)" v-on:click.stop/>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
import { UNSPLASH_APPLICATION, UNSPLASH_SECRET } from '../config/unsplash.js'
import lodash from 'lodash'

export default {
    data() {
        return {
            cards: [],
            element1 : null,
            element2 : null,
            matchCount: 0
        }
    },
    props: ['columns'],
   computed: {
       elements () {
           return this.columns ** 2 / 2
       },
       gameFinish() {
           return this.cards.reduce((acc, act) => acc && act.discovered, true)
       }
   },
   methods: {
       async createBoard () {
           let res = await axios.get(
                `https://api.unsplash.com/photos/random?count=${this.elements}&orientation=squarish&client_id=${UNSPLASH_APPLICATION}&client_secret=${UNSPLASH_SECRET}`
            )

            this.cards = []
            res.data.forEach( i => {
                this.cards.push({
                    image: i.urls.thumb,
                    discovered: false
                })
                this.cards.push({
                    image: i.urls.thumb,
                    discovered: false
                })
            })

            this.cards = lodash.shuffle(this.cards)
            this.matchCount = 0
       },
       flip(item) {
           return item.discovered || item === this.element1 || item === this.element2
       },
       onClick(item) {
           if (!this.element1) {
               this.element1 = item;
           } else {
               if (!this.element2) {
                   this.element2 = item;
                   if (this.element1.image === this.element2.image){
                       this.element1.discovered = true;
                       this.element2.discovered = true;
                       
                       this.matchCount++
                   } 
               } else {
                   this.element1 = item,
                   this.element2 = null
               }
           }
       }
   },
   created () {
       this.createBoard();
   }
}
</script>

<style scoped>
.card {
    width: 200px;
    height: 200px;
    background-color: orangered;;
}

.board {
    display: grid;
    grid-template-columns: 200px 200px 200px 200px;
    gap: 20px;
}

.memory-game{
    display: flex;
    justify-content: center;
}

.card img {
    width: 100%;
    height: 100%;
}
</style>