<template>
  <div class="container">
    <div class="demo">
        
        <wxc-cell :has-arrow="false"
            :cell-style="cellStyle"
            :has-top-border="false"
            :auto-accessible="false"
            >
          <image class="image"
                 slot="label"
                 :src='head_portrait'></image>
          <text class="red"
                slot="title">{{my_user.name}}
          </text>
          <text class="red"
                slot="title">{{my_user.phone}}
          </text>
        </wxc-cell>
    </div>
  </div>

</template>

<style scoped>
  .container {
    flex: 1;
  }
  .demo {
    width: 750px;
    background-color: #f2f3f4;
  }
  .category {
    margin-top: 40px;
  }
  .default {
    color: #000000;
  }
  .active {
    color: #FFC900;
  }
  .red {
    color: #C3413D;
  }
  .image {
    width: 160px;
    height: 160px;
    margin-right: 20px;
  }
  .demo-title {
    font-size: 30px;
    color: #333333;
    margin-top: 30px;
    margin-left: 30px;
    margin-bottom: 16px;
  }
</style>

<script>
  import { WxcCell } from 'weex-ui';
  export default {
    components: { WxcCell },
    data: () => ({
      my_user: [],
      head_portrait: '',
    }),
    watch: {
        user: function (Val) {
          this.my_user = Val[0];
          this.head_portrait= 'http://www.chenwenhao97.cn/user_img/' + this.my_user.head_portrait
        }
    },
    props: ['user'],
    methods: {
      wxcCellClicked (e) {
        console.log(e)
      }
    }
  };
</script>