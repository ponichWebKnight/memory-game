<template>
    <v-toolbar class="control">
        <v-spacer></v-spacer>
            <span class="display-1">Time: {{counter}}</span>
        <v-spacer></v-spacer>
    </v-toolbar>
</template>

<script>
export default {
  name: 'ControlPanel',
  props: ['start', 'stop'],
  data() {
      return {
          timer: null,
          time: 0,
          startTime: null,
          secondsPassed: 0,
          minutesPassed: 0,
      }
  },
  computed: {
      counter() {
          return this.minutesPassed + ' mins ' + this.secondsPassed + ' secs'
      },
  },
  methods: {
      startGame() {
          this.startTime = new Date().getTime();
          this.timer = setInterval(() => {
              this.time = new Date().getTime() - this.startTime;
              this.secondsPassed = Math.floor(this.time / 1000);
              this.time = Math.floor(this.time % 1000);
              this.minutesPassed = this.secondsPassed / 60;
              this.secondsPassed = Math.floor(this.secondsPassed % 60);
              this.minutesPassed = Math.floor(this.minutesPassed % 60);
          }, 0);
      }
  },
  created() {
     this.startGame()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .control {
        border: 2px solid white;
        border-radius: 10px;
        margin-bottom: 20px;
    }
</style>
