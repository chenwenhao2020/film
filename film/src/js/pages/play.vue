<template>
  <div>
    <video class="video" :src="src" autoplay controls
      @start="onstart" @pause="onpause" @finish="onfinish" @fail="onfail"></video>
    <text class="info">state: {{state}}</text>
  </div>
</template>

<style scoped>
  .video {
    width: 630px;
    height: 350px;
    margin-top: 60px;
    margin-left: 60px;
  }
  .info {
    margin-top: 40px;
    font-size: 40px;
    text-align: center;
  }
</style>

<script>
  export default {
    data () {
      return {
        state: '----',
        src: 'http://www.chenwenhao97.cn/movie/' + film.titleCn + 'mkv',
        film: {},
      }
    },
    methods:{
      onstart (event) {
        this.state = 'onstart'
      },
      onpause (event) {
        this.state = 'onpause'
      },
      onfinish (event) {
        this.state = 'onfinish'
      },
      onfail (event) {
        this.state = 'onfinish'
      }
    },
    created() {
      var _this = this;
      this.$fetch({
          method: 'GET',    // 大写
          url: 'http://111.231.204.71:5000/Play',        
      }).then(resData => {
          // 成功回调
          _this.film = resData
        
          console.log(JSON.stringify(resData))
      }, error => {
          // 错误回调

          console.log(error)
      })
      this.$router.getParams().then(resData => {
        _this.film = resData
      })
    },
  }
</script>