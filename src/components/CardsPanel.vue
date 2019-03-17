<template>
    <v-card>
        <v-container
          fluid
          grid-list-md>
              <v-layout row wrap>
                  <v-flex
                    v-for="card in items"
                    xs3
                    md2
                  >
                    <v-card class="custom-card" @click="flip(card)">
                        <v-img class="back" v-if="card.flipped" :src="card.src"
                               aspect-ratio="0.703"
                               />
                        <v-img class="front" v-else src="https://opengameart.org/sites/default/files/card%20back%20orange.png"
                               aspect-ratio="0.703"
                               />
                    </v-card>
                  </v-flex>
            </v-layout>
        </v-container>
    </v-card>
</template>

<script>
import {cards} from './cards-items'
import {bus} from './bus'
export default {
    name: 'CardsPanel',
    data() {
      return {
         cards: cards,
         items: [],
         gameIsRunning: false,
         checkedCards: [],
      }
    },
    created() {
        this.resetGame()
    },
    mounted() {
        bus.$on('reset-game', this.resetGame)
    },
    methods: {
        resetGame() {
            this.items = [];
            this.cards.forEach(card => {
               this.items.push(Object.assign({}, card));
               this.items.push(Object.assign({}, card));
            });
            for (let i = this.items.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [this.items[i], this.items[j]] = [this.items[j], this.items[i]];
            }
            this.gameIsRunning = false;

        },

        flip(item) {
            if(item.checked !== true) {
                item.flipped = !item.flipped
                if(!this.gameIsRunning) {
                    bus.$emit('start-time');
                    this.gameIsRunning = true;
                }
                item.checked = true
                setTimeout(() => {this.checkIfEqual(item)}, 1000);
            }
        },

        checkIfEqual(item) {
          this.checkedCards.push(item);
          if(this.checkedCards.length > 1) {
              for(let i=0; i<this.checkedCards.length; i++) {
                if (this.checkedCards[i].src !== this.checkedCards[i+1].src) {
                    this.checkedCards[i].flipped = this.checkedCards[i+1].flipped = false
                    this.checkedCards[i].checked = this.checkedCards[i+1].checked = false
                  }
                break;
              }
              this.checkedCards = []
          }
          console.log(this.checkedCards)
          this.checkIfFinished()
        },

        checkIfFinished() {
            console.log(this.items)
            if(this.items.every((card) => card.checked === true)) {
                bus.$emit('stop-time');
                console.log('Game stopped')
                bus.$emit('show-dialog', true);
            }
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    @keyframes flipCard {
        0%{ transform: rotateY(0deg) }
        100%{ transform: rotateY(90deg) }
    }
    .cards {
        border: 2px solid white;
        border-radius: 10px;
    }

    .custom-card:click .front{
        animation: flipCard 0.2s easy-in forwards;
    }

    .custom-card:click .front{
        animation: flipCard 0.2s 0.2s easy-out reverse;
    }
</style>
