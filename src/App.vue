<template>
  <div id="app">
    <timeKeeperFront />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import displayInfoOfSpreadSheet from './components/displayInfoOfSpreadSheet.vue';
import timeKeeperFront from './components/timeKeeperFront.vue';

@Component({
  components: {
    displayInfoOfSpreadSheet,
    timeKeeperFront
  },
})
export default class App extends Vue {}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
