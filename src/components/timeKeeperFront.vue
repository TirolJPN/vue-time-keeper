<template>
    <div>
        <header class="time-keeper-header">
            <nav class="time-keeper-nav">
                <ul class="time-keeper-nav__menu">
                    <li class="time-keeper-nav__menu__item"><a href="">Auto</a></li>
                    <li class="time-keeper-nav__menu__item"><a @click="startTimer">Start</a></li>
                    <li class="time-keeper-nav__menu__item"><a @click="pauseTimer">Pause</a></li>
                    <li class="time-keeper-nav__menu__item"><a href="">SignOut</a></li>
                </ul>
            </nav>
            <h1 class="site-logo">Vue Time Keeper</h1>
        </header>
        <div class="time-keeper-container">
            <div class="time-keeper-box">
                <p class="current-time time-keeper-paragraph">{{ nowTime }}</p>
                <p class="remaining-time time-keeper-paragraph">{{ defferenceTime }}</p>
                <p class="presentator time-keeper-paragraph">by {{presentator}}</p>
                <p class="title time-keeper-paragraph">{{title}}</p>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'timeKeeperFront',
    data () {
        return {
            // タイマーが初期化された時間を格納するDataオブジェクト
            initTimeInner: new Date(),
            // 現在の時間を格納するDataオブジェクト
            nowTimeInner: new Date(),
            timerOn: false,
            timerObject: null,
            startFlag: false,
            presentator: 'hoge fuga',
            title: 'Story about Vue.js',
            // 発表時間が15分だと仮定
            dummyPresentationSeconds: 900,
        };
    },
    methods: {
        // 1秒ごとにnowTimeInnerのSecondsを更新するmethod
        startTimer () {
            const self = this;
            this.timerOn = true;
            if (!this.startFlag) {
                this.startFlag = true;
                this.timerObject = setInterval(function() {self.count()}, 1000);
            }
        },
        pauseTimer () {
            this.startFlag = false;
            clearInterval(this.timerObject);
        },
        // nowTimeInnderのSecondsを一秒加算するmethod
        count () {
            let newScoundDate = new Date(this.nowTimeInner.getTime());
            newScoundDate.setSeconds(newScoundDate.getSeconds() + 1);
            this.nowTimeInner = newScoundDate;
        }
    },
    computed: {
        nowTime: function () {
            // dataの各パラメータを参照して整形する
            const seconds = Math.floor( (this.nowTimeInner.getTime() - this.initTimeInner.getTime()) /1000);
            return ( '00' + Math.floor(seconds / 60) ).slice(-2) + ':' + ( '00' + seconds % 60).slice(-2);
        },
        defferenceTime: function () {
            // dataの各パラメータを参照して整形する
            const seconds = this.dummyPresentationSeconds - Math.floor( (this.nowTimeInner.getTime() - this.initTimeInner.getTime()) /1000);
            return '-' + ( '00' + Math.floor(seconds / 60) ).slice(-2) + ':' + ( '00' + seconds % 60).slice(-2);
        },
    }
}
</script>

<style>
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
.time-keeper-nav__menu__item a{
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
