<template>
  <div>
    <header class="time-keeper-header">
      <nav class="time-keeper-nav">
        <ul class="time-keeper-nav__menu">
          <!-- <li class="time-keeper-nav__menu__item"><a href="">Auto</a></li>
          <li class="time-keeper-nav__menu__item"><a @click="startTimer">Start</a></li>
          <li class="time-keeper-nav__menu__item"><a @click="pauseTimer">Pause</a></li> -->
          <li class="time-keeper-nav__menu__item"><button v-on:click="logout">LogOut</button></li>
        </ul>
      </nav>
      <h1 class="site-logo">Vue Time Keeper</h1>
    </header>
    <div class="time-keeper-container">
      <div class="time-keeper-box">
        <p class="current-time time-keeper-paragraph">{{ nowTime }}</p>
        <p class="remaining-time time-keeper-paragraph">{{'- ' + restTime }}</p>
        <p class="presentator time-keeper-paragraph">by {{ presentator }}</p>
        <p class="title time-keeper-paragraph">{{ presentator }}</p>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue, Prop } from 'vue-property-decorator';

@Component
export default class timeKeeperFront extends Vue {
  @Prop( {default: []} )
  spreadSheet!: Array<Array<any>>;

  // タイマーが初期化された時間を格納するDataオブジェクト
  startTimeInner: Date = new Date();
  // タイマーが終了する時間を表すオブジェクト
  finishTimeInner: Date = new Date();
  // 現在の時間を格納するDataオブジェクト
  nowTimeInner: Date =  new Date();
  timerObject: number = 0;
  presentator: string = 'hoge fuga';
  title: string =  'Story about Vue.js';
  // 発表時間が15分だと仮定
  sessionIndex: number = -1;

  /** methods **/
  // 1秒ごとにnowTimeInnerのSecondsを更新するmethod
  mounted() : void {
    const self = this;
    try{
      const newScoundDate = new Date();
      const h = newScoundDate.getHours();
      const m = newScoundDate.getMinutes();

      const firstSession = this.spreadSheet[0][0];
      const minHour = Number(firstSession.split(':')[0]);
      const minMinutes = Number(firstSession.split(':')[1]);

      const rowLength = Object.keys(this.spreadSheet).length
      const lastSession = this.spreadSheet[rowLength - 1][0];
      const maxHour = Number(lastSession.split(':')[0]);
      const maxMinutes = Number(lastSession.split(':')[1]);
    
      if ((h < minHour) || (h === minHour && m < minHour)){
        alert("現在の時刻がイベント開始前です");
      } else if((h > maxHour) || (h === maxHour && m > maxHour)) {
        alert("現在の時刻がイベント終了後です");
      } else {
        let tf = true;
        Object.keys(this.spreadSheet).forEach((key) => {
          const session = self.spreadSheet[Number(key)][0];
          const tmpHour = Number(session.split(':')[0]);
          const tmpMinutes = Number(session.split(':')[1]);

          if ( ( (h < tmpHour) || (h === tmpHour && m < tmpMinutes) ) && tf){
            self.sessionIndex = Number(key) - 1;
            tf = false;
            self.setTimer();
          }
        });
      }

    } catch (e) {
      alert("スプレッドシートの値が正しくありません")
      console.log(e)
    }
  }

  setTimer () {
    const self = this;

    this.presentator = this.spreadSheet[this.sessionIndex][1];
    this.title = this.spreadSheet[this.sessionIndex][2];

    // 現在進行中のSessionの開始時間と終了時間を求める
    const tmpDate = new Date();
    const todayDateString = tmpDate.getFullYear() + "/" + ("00" + (tmpDate.getMonth()+1)).slice(-2) + "/" + ("00" + tmpDate.getDate()).slice(-2);
    this.startTimeInner = new Date(todayDateString + " " + this.spreadSheet[this.sessionIndex][0] + ":00")
    this.finishTimeInner = new Date(todayDateString + " " + this.spreadSheet[this.sessionIndex + 1][0] + ":00")
    this.timerObject = setInterval(function() {self.count()}, 1000);
    this.sessionIndex += 1;
  }

  // nowTimeInnderのSecondsを一秒加算するmethod
  count () {
    let newScoundDate = new Date(this.nowTimeInner.getTime());
    newScoundDate.setSeconds(newScoundDate.getSeconds() + 1);
    this.nowTimeInner = newScoundDate;
    if( Math.floor( (this.finishTimeInner.getTime() - this.nowTimeInner.getTime()) /1000) === 0 ){
      clearInterval(this.timerObject);
      this.setTimer();
    }
  }

  logout (): void {
    console.log('hoge')
    this.$emit('logout')
  }

  /** computed */
  get nowTime(): string {
    // dataの各パラメータを参照して整形する
    const seconds = Math.floor( (this.nowTimeInner.getTime() - this.startTimeInner.getTime()) /1000);
    return ( '00' + Math.floor(seconds / 60) ).slice(-2) + ':' + ( '00' + seconds % 60).slice(-2);
  }

  get restTime(): string {
    const seconds = Math.floor( (this.finishTimeInner.getTime() - this.nowTimeInner.getTime()) /1000);
    return ( '00' + Math.floor(seconds / 60) ).slice(-2) + ':' + ( '00' + seconds % 60).slice(-2);
  }
}
</script>

<style>
button{
    background-color: transparent;
    border: none;
    cursor: pointer;
    outline: none;
    padding: 0;
    appearance: none;
}
/* for header */
.time-keeper-header{
    background: #fff;
    display: flex;
    /* padding: 20px 20px; */
    position: fixed;
    z-index:1000;
    justify-content: space-between;
    width: 100%;

    /* 子要素を全て縦中央に並べる */
    -webkit-box-align: center;
    -ms-flex-align: center;
    align-items: center;
}
.gnav__menu {
    display: flex;
}
.time-keeper-nav ul {
    list-style: none;
    padding:0px;
    margin:0px;
}
.site-logo{
    width: auto;
    font-family: 'Pacifico', cursive;
    padding-right:50px;
}
.time-keeper-nav__menu{
    display: flex;
}
.time-keeper-nav__menu__item{
    margin-left: 50px;
}
.time-keeper-nav__menu__item button{
    color: #333;
    font-size: 20px;
    text-decoration: none;
    cursor: pointer;
}


/* タイムキーパー本体 */
.time-keeper-container{
    width: 100vw;
    height: auto;
    min-height: 100vh;

    background-size: cover;
    background-position: center;
    background-image: url('http://placeimg.com/850/530/animals');

    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;

    /* 左右中央寄せ */
    -webkit-box-pack: center;
    -ms-flex-pack: center;
    justify-content: center;

}
.time-keeper-container:before {
    content: '';
    position: absolute;
    top: 0;
    right: 0;
    left: 0;
    bottom: 0;
    background-color: rgba(0,0,0,0.8); /*半透明のフィルターをかける*/
}
.time-keeper-box{
    position:relative;
}
.time-keeper-paragraph {
    color: #fff;
    font-size: 80px;
    font-family: 'Pacifico', cursive;
    white-space: nowrap;
    position: absolute;
}

.current-time{
    font-size: 600px;
    top: 90%;
    left: 50%;
    transform: translateY(-90%) translateX(-50%);
    -webkit-transform: translateY(-90%) translateX(-50%);
    margin: auto;
}

.remaining-time{
    font-size: 90px;
    top: 10%;
    left: 50%;
    transform: translateY(-10%) translateX(-50%);
    -webkit-transform: translateY(-10%) translateX(-50%);
    margin: auto;
}

.title{
    font-size: 60px;
    top: 90%;
    left: 100%;
    transform: translateY(-90%) translateX(-100%);
    -webkit-transform: translateY(-90%) translateX(-100%);
    margin: auto;
}

.presentator{
    font-size: 40px;
    top: 90%;
    left: -100%;
    transform: translateY(-90%) translateX(100%);
    -webkit-transform: translateY(-90%) translateX(100%);
    margin: auto;
}
</style>
