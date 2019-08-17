<template>
    <div class="time-keeper-container">
        <div class="time-keeper">
            <p class="msg">DEMO MESSAGE</p>
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
}
</style>
