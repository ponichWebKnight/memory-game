<template>
    <v-app dark>
        <v-content>
            <v-container fluid>
                <MemoryGameHeader msg="Welcome to the Memory Game!"/>
                <ControlPanel/>
                <CardsPanel/>
            </v-container>
        </v-content>
    </v-app>
</template>

<script>
import MemoryGameHeader from './components/MemoryGameHeader.vue'
import ControlPanel from './components/ControlPanel.vue'
import CardsPanel from './components/CardsPanel.vue'

export default {
  name: 'app',
  components: {
    MemoryGameHeader,
    ControlPanel,
    CardsPanel
  }
}
</script>

<style>
    html {
        overflow: scroll;
    }
</style>
