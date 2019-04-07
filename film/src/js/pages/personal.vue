<template>
  <div class="container">
    <div class="demo">
      <text class="demo-title">列表list展示</text>

      <wxc-cell label="标题"
                title="Weex Ui"
                :has-arrow="true"
                @wxcCellClicked="wxcCellClicked"
                :has-margin="true"></wxc-cell>

      <wxc-cell label="标题"
                title="带链接"
                :has-arrow="true"
                link="https://h5.m.taobao.com/trip/home/index.html"
                @wxcCellClicked="wxcCellClicked"
                spm="181.12312312.12312.d01"
                :has-top-border="false"></wxc-cell>

      <wxc-cell label="标题"
                title="Weex Ui"
                :has-arrow="true"
                @wxcCellClicked="wxcCellClicked"
                :has-top-border="false"></wxc-cell>

    </div>
    <div class="demo">
      <text class="demo-title">不配置label</text>
      <wxc-cell title="标题"
                :has-arrow="true"
                @wxcCellClicked="wxcCellClicked"
                :has-top-border="true"></wxc-cell>
    </div>

    <div class="demo">
      <text class="demo-title">配置附加信息</text>
      <wxc-cell title="标题"
                desc="这里是附加信息"
                :has-arrow="true"
                @wxcCellClicked="wxcCellClicked"
                :has-top-border="true"></wxc-cell>
    </div>

    <div class="demo">
      <text class="demo-title">不显示箭头</text>
      <wxc-cell title="标题"
                :has-arrow="false"
                @wxcCellClicked="wxcCellClicked"
                :has-top-border="true"></wxc-cell>
    </div>
    <div class="demo">
      <text class="demo-title">自定义子元素</text>
      <wxc-cell title="这里是标题"
                :has-arrow="false"
                :has-top-border="true">
        <switch slot="value"></switch>
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
    width: 80px;
    height: 80px;
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
    methods: {
      wxcCellClicked (e) {
        console.log(e)
      }
    }
  };
</script>