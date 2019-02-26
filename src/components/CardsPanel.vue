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
                    <v-card @click="flip(card)">
                        <v-img v-if="card.flipped" :src="card.src"
                               aspect-ratio="0.703"
                               />
                        <v-img v-else src="https://opengameart.org/sites/default/files/card%20back%20orange.png"
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
export default {
    name: 'CardsPanel',
    data() {
      return {
         cards: cards,
         items: [],
      }
    },
    created() {
        this.resetGame()
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

        },

        flip(item) {
            console.log(item)
            item.flipped = !item.flipped
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .cards {
        border: 2px solid white;
        border-radius: 10px;
    }
    .custom-card {
    }
</style>
