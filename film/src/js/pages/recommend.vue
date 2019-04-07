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
      <div v-for='(item, index) in lists' :key='index' class='cell'>
            <div class="item">
                <text>{{item}}</text>
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
        items: [
        {
          src:'https://gw.alicdn.com/tps/TB1Jl1CPFXXXXcJXXXXXXXXXXXX-370-370.jpg',
          name: 'Thomas Carlyle',
          desc:'Genius only means hard-working all one\'s life',
          behaviourName: 'Change width',
          behaviour: 'changeColumnWidth',
        },
        {
          src:'https://gw.alicdn.com/tps/TB1Hv1JPFXXXXa3XXXXXXXXXXXX-370-370.jpg',
          desc:'The man who has made up his mind to win will never say "impossible "',
          name: 'Thomas Carlyle',
          behaviourName: 'Change gap',
          behaviour: 'changeColumnGap'
        },
        {
          src:'https://gw.alicdn.com/tps/TB1eNKuPFXXXXc_XpXXXXXXXXXX-370-370.jpg',
          desc:'There is no such thing as a great talent without great will - power',
          name: 'Thomas Carlyle',
          behaviourName: 'Change count',
          behaviour: 'changeColumnCount'
        },
        {
          src:'https://gw.alicdn.com/tps/TB1DCh8PFXXXXX7aXXXXXXXXXXX-370-370.jpg',
          name:'Addison',
          desc:'Cease to struggle and you cease to live',
          behaviourName: 'Show scrollbar',
          behaviour: 'showScrollbar',
        },
      ]
      }),
      created () {

      },
      methods: {
        
      }
    };

</script>