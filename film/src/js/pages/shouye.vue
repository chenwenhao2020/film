<template>
  <div class="wrapper">
    <wxc-ep-slider :slider-id="sliderId"
                   :card-length='cardLength'
                   :card-s="cardSize"
                   :auto-play='true'
                   :select-index="2"
                   @wxcEpSliderCurrentIndexSelected="wxcEpSliderCurrentIndexSelected">

      <!--自动生成demo-->
      <wxc-pan-item v-for="(v,index) in slide_list"
           :key="index"
           :slot="`card${index}_${sliderId}`"
           :class="['slider',`slider${index}`]"
           @wxcPanItemClicked="wxcPanItemClicked(v, v.titleCn, v.click)"
           :accessible="true">
          <text>{{v.titleCn}}</text>
          <image :src=v.coverPath style="width: 300px; height: 256px;"></image>
      </wxc-pan-item>
    </wxc-ep-slider>
  </div>
</template>

<style scoped>

  .wrapper {
    padding-top: 100px;
  }

  .slider {
    width: 400px;
    height: 300px;

    align-items: center;
    justify-content: center;
  }

</style>

<script>
  import { WxcEpSlider, WxcPanItem } from 'weex-ui';

  export default {
    components: { WxcEpSlider, WxcPanItem},
    data: () => ({
      sliderId: 1,
      cardLength: 5,
      cardSize: {
        width: 400,
        height: 300,
        spacing: 0,
        scale: 0.8
      },
      slide_list: [],
    }),
    created() {
      var _this = this;
      this.$fetch({
          method: 'GET',    // 大写
          url: 'http://111.231.204.71:5000/ShouyeSlide',        
      }).then(resData => {
          // 成功回调
          _this.slide_list = resData
        
          console.log(JSON.stringify(resData))
      }, error => {
          // 错误回调

          console.log(error)
      })
    },
    methods: {
      wxcEpSliderCurrentIndexSelected (e) {
        const index = e.currentIndex;
        console.log(index);
      },
      wxcPanItemClicked (film, name, click) {
        var _this = this;
        this.$router.open({
            name: 'Play',
            params: film,
        }),
        this.$fetch({
          method: 'GET',    // 大写
          url: 'http://111.231.204.71:5000/Click',
          data: {
            click_name : name,
            click_count : click,
          } 
        }).then(resData => {
            // 成功回调
            film.click = resData.click
            console.log(resDate)
        }, error => {
            // 错误回调
            console.log(error)
        })
      },
    }
  }
</script>