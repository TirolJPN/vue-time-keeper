<template>
    <div>
        <header class="time-keeper-header">
            <nav class="time-keeper-nav">
                <ul class="time-keeper-nav__menu">
                    <li class="time-keeper-nav__menu__item"><a href="">About</a></li>
                    <li class="time-keeper-nav__menu__item"><a href="">Works</a></li>
                    <li class="time-keeper-nav__menu__item"><a href="">Recruit</a></li>
                    <li class="time-keeper-nav__menu__item"><a href="">News</a></li>
                </ul>
            </nav>
            <h1 class="site-logo">Vue Time Keeper</h1>
        </header>
        <div class="time-keeper-container">
            <div class="time-keeper">
                <p class="msg">DEMO MESSAGE</p>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'timeKeeperFront',
    data () {
        return {
        };
    },
    methods: {
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
    padding-right:100px;
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
    margin-left: 20px;
}
.time-keeper-nav__menu__item a{
    color: #333;
    font-size: 20px;
    text-decoration: none;
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

    /* 上下中央寄せ */
    -webkit-box-align: center;
    -ms-flex-align: center;
    align-items: center;
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

.time-keeper{
    position:relative;
}

.msg {
    color: #fff;
    font-size: 80px;
    font-family: 'Pacifico', cursive;
}
</style>
