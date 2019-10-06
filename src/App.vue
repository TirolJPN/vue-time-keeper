<template>
  <div id="app">
    <displayInfoOfSpreadSheet @fetch="fetchSpreadSheet" />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import displayInfoOfSpreadSheet from './components/displayInfoOfSpreadSheet.vue';
import timeKeeperFront from './components/TimeKeeperFront.vue';

@Component({
  components: {
    displayInfoOfSpreadSheet,
    timeKeeperFront
  },
})
export default class App extends Vue {
  fetchSpreadSheet () {
    alert("hoge")
  }
}
</script>

<style>

</style>
