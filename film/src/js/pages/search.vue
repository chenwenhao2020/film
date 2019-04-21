<template>
  <div class="container">
    <div class="demo">

      <wxc-searchbar ref="wxc-searchbar"
      :always-show-cancel="showCancel"
      theme='yellow'
      cancel-label='搜索'
      @wxcSearchbarInputOnInput="wxcSearchbarInputOnInput"
      @wxcSearchbarCancelClicked='wxcSearchbarCancelClicked'></wxc-searchbar>

      <waterfall
            class="item-container"
            column-count="2"
            column-width="auto"
            >
          
          <cell v-for="(demo,key) in search"
                class="cell"
                :key="key"
                @click="play_clicked(v, v.titleCn, v.click, v.genreTypes, my_user)">
                <div class='item'>
                    <text class='itemName'>{{demo.titleCn}}</text>
                    <image :src='demo.coverPath' class='itemPhoto'></image>
                    <text class='itemType'>{{demo.genreTypes}}</text>
                    <text class='itemDesc'>{{demo.paragraph}}</text>
                    <text class='itemActor'>{{demo.actorNameCn1}}</text>
                </div>
          </cell>
      </waterfall>
    </div>
  </div>
  
</template>

<style scoped>
  .item-container {
    width: 750px;
    background-color: #f2f3f4;
    /* height: 1300px; */
  }

  .cell {
    padding-top: 10px;
    padding-bottom: 10px;
    
  }

  .item {
    padding: 10px;
    background-color: #FFFFFF;
    align-items: center;
  }

  .content{
    width:750px;
    height:300px;
    border-bottom-width:1px;
    align-items: center;
    justify-content: center;
  }

  .itemPhoto {
    margin-top: 18;
    width: 220px;
    height: 220px;
    margin-bottom: 18px;
  }

  .itemName {
    font-size:28px;
    color:#333333;
    line-height:42px;
    text-align:left;
    margin-top: 24px;
  }

  .itemDesc {
    font-size:24px;
    margin:12px;
    color:#999999;
    line-height:36px;
    text-align:left;
  }

  .itemType {
    font-size:20px;
    margin:6px;
    color:#00cc99;
    line-height:36px;
    text-align:left;
  }

  .itemActor {
    font-size:36px;
    color:#00cc99;
    line-height:36px;
    text-align:center;
    margin-top: 6px;
    margin-left: 24px;
    margin-right: 24px;
    margin-bottom: 30px;
  }
  .text {
    font-size: 88px;
    text-align: center;
    color: #41B883;
  }
</style>

<script>
  import { WxcSearchbar } from 'weex-ui'
  export default {
    components: { WxcSearchbar },
    data: () => ({
      value: '',
      showCancel: true,
      search: [],
      hello: '123',
    }),
    methods: {
      wxcSearchbarInputOnInput (e) {
        this.value = e.value;
      },
      wxcSearchbarCancelClicked () {
        var _this = this;

        this.$fetch({
          method: 'GET',    // 大写
          url: 'http://111.231.204.71:5000/Search',
          data: {
            film_name: this.value
          }
        }).then(resData => {
            // 成功回调
          this.search = resData
        }, error => {
            // 错误回调
            console.log(error)
        })
      },
      play_clicked (film, name, click, genreTypes, user) {
          this.$fetch({
            method: 'GET',    // 大写
            url: 'http://111.231.204.71:5000/Click',
            data: {
              click_name : name,
              click_count : click,
              click_genreTypes: genreTypes,
              click_user : user
            } 
          }).then(resData => {
              // 成功回调
              film.click = resData.click
              this.user = resData.user
              console.log(resDate)
          }, error => {
              // 错误回调
              console.log(error)
          })
          this.$router.open({
              name: 'Play',
              params: film,
          })
        },
    },
  };
</script>
