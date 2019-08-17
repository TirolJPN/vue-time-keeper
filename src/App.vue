<template>
  <div id="app">
    <displayInfoOfSpreadSheet  v-if="!timeKeeperFlag" />
    <timeKeeperFront v-if="timeKeeperFlag" />
  </div>
</template>

<script>
import displayInfoOfSpreadSheet from './components/displayInfoOfSpreadSheet'
import timeKeeperFront from './components/timeKeeperFront'

export default {
  name: 'app',
  data () {
    return {
      // for debug
      timeKeeperFlag:true,
    }
  },
  components: {
    displayInfoOfSpreadSheet,
    timeKeeperFront
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

body {
  margin: 0px;
}
</style>
