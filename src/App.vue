<template>
    <v-app dark>
        <v-content>
            <v-container fluid>
                <MemoryGameHeader msg="Welcome to the Memory Game!"/>
                <TimePanel @startTime="startGame"/>
                <CardsPanel/>
                <v-dialog v-model="endGameDialog" max-width="500px">
                    <v-card>
                        <v-card-title>
                          Gongratulations! Your time is {{time}}
                        </v-card-title>
                        <v-card-text>
                            <v-btn color="primary" dark @click="resetGame">START AGAIN</v-btn>
                            <v-btn color="primary" dark @click="endGameDialog=false">Close</v-btn>
                        </v-card-text>
                    </v-card>
                </v-dialog>
            </v-container>
        </v-content>
    </v-app>
</template>

<script>
import MemoryGameHeader from './components/MemoryGameHeader.vue'
import TimePanel from './components/TimePanel.vue'
import CardsPanel from './components/CardsPanel.vue'
import {bus} from './components/bus.js'

export default {
  name: 'app',
  components: {
    MemoryGameHeader,
    TimePanel,
    CardsPanel
  },
  data() {
      return {
          endGameDialog: false,
          time: 0,
      }
  },
  mounted() {
      bus.$on('show-dialog', this.showDialog)
      bus.$on('show-time', data => {this.time = data})
  },
  methods: {
      showDialog() {
          this.endGameDialog = true
      }
  }
}
</script>

<style>
    html {
        overflow: scroll;
    }
</style>
