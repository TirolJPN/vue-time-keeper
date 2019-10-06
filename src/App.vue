<template>
  <div id="app">
    <AutoTimeKeeper
      v-if="isLogined"
      @logout="logout"
      v-bind:spreadSheet="this.spreadSheetRows"
    />
    <displayInfoOfSpreadSheet
      v-else
      @fetch="fetchSpreadSheet" />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import displayInfoOfSpreadSheet from './components/displayInfoOfSpreadSheet.vue';
import TimeKeeper from './components/TimeKeeper.vue';
import AutoTimeKeeper from './components/AutoTimeKeeper.vue';

@Component({
  components: {
    displayInfoOfSpreadSheet,
    TimeKeeper,
    AutoTimeKeeper
  },
})
export default class App extends Vue {
  // ログイン状態を表すフラグ
  isLogined: boolean = false;
  // スプレッドシートの情報を格納する2次元Array
  spreadSheetRows: Array<Array<any>> = [];
  
  fetchSpreadSheet (rows: Array<Array<any>>) {
    this.isLogined = true;
    this.spreadSheetRows = Object.assign({}, rows);
  }

  logout () {
    this.isLogined = false;
    this.spreadSheetRows = [];
  }
}
</script>

<style>
body{
  margin: 0px;
}
</style>
