<template>
  <div class='container'>
    <category title="推荐"></category>
    <div class='wrap-row'>
      <div v-for='(item, index) in items' :key='item.src' class='cell'>
          <div class="item">
            <text v-if="item.name" class="itemName">{{item.name}}</text>
            <image class="itemPhoto" :src="item.src"></image>
            <text v-if="item.desc" class="itemDesc">{{item.desc}}</text>
            <text v-if="item.behaviourName" class="itemClickBehaviour"> {{item.behaviourName}}</text>
          </div>
      </div>
    </div>

    <category title="热门"></category>
    <div class='wrap-row'>
      <div v-for='(v, index) in hot' :key='index' class='cell'>
          <div class="item">
            <text v-if="v.titleCn" class="itemName">{{v.titleCn}}</text>
            <image class="itemPhoto" :src=v.coverPath></image>
            <text v-if="v.titleEn" class="itemDesc">{{v.titleEn}}</text>
            <text v-if="v.actorNameCn1" class="itemClickBehaviour"> {{v.actorNameCn1}}</text>
          </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
  .page {
    background-color: #EFEFEF;
  }

  .wrap-row{
    display: flex;
    flex-wrap: wrap;
    flex-direction: row;
    width: 750px;
  }

  .cell {
    padding-top: 10px;
    padding-bottom: 10px;
    width: 375px;
  }
  .item {
    padding: 10px;
    background-color: #FFFFFF;
    align-items: center;
  }
  .itemName {
    font-size:28px;
    color:#333333;
    line-height:42px;
    text-align:left;
    margin-top: 24px;
  }
  .itemPhoto {
    margin-top: 18;
    width: 220px;
    height: 220px;
    margin-bottom: 18px;
  }
  .itemDesc {
    font-size:24px;
    margin:12px;
    color:#999999;
    line-height:36px;
    text-align:left;
  }
  .itemClickBehaviour {
    font-size:36px;
    color:#00cc99;
    line-height:36px;
    text-align:center;
    margin-top: 6px;
    margin-left: 24px;
    margin-right: 24px;
    margin-bottom: 30px;
  }  
</style>
<script>
  import Category from './com/category.vue';

  export default {
      components: {Category},
      name: 'Recommend',
      data: () => ({
        lists: ['a', 'b', 'c'],
        user: [],
        hot: [],
        recommend: [],
      }),
      created () {
          var _this = this;
          this.$fetch({
              method: 'GET',    // 大写
              url: 'http://111.231.204.71:5000/Hot',        
          }).then(resData => {
              // 成功回调
              _this.hot = resData
            
              console.log(JSON.stringify(resData))
          }, error => {
              // 错误回调

              console.log(error)
          })
      },
      methods: {
        
      }
    };

</script>