<template>
  <wxc-tab-page ref="wxc-tab-page"
                :tab-titles="tabTitles"
                :tab-styles="tabStyles"
                title-type="icon"
                :tab-page-height="tabPageHeight"
                @wxcTabPageCurrentTabSelected="wxcTabPageCurrentTabSelected">
    
            

            <waterfall v-for="(v,index) in tabList"
                  :key="index"
                  class="item-container"
                  column-count="2"
                  column-width="auto"
                  >
                
                <cell v-for="(demo,key) in ranking_list"
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
                <header class="footer" ref="footer"  @appear="footerAppear">
                  <text class="text">{{loading}}</text>
                </header>
            </waterfall>
  </wxc-tab-page>
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
  .footer {
    height: 200px;
    justify-content: center;
    align-items: center;
    background-color: #FFF100;
  }
</style>
<script>
  const dom = weex.requireModule('dom');
  import { WxcTabPage, WxcPanItem, Utils, BindEnv } from 'weex-ui';

  // https://github.com/alibaba/weex-ui/blob/master/example/tab-page/config.js
  import Config from './ranking_config.js'

  export default {
    components: { WxcTabPage, WxcPanItem},
    data: () => ({
      tabTitles: Config.tabTitles,
      tabStyles: Config.tabStyles,
      tabList: [],
      tabPageHeight: 1334,
      name_from_shouye : '123',
      ranking_tab_name : '动作',
      ranking_list : [],
      my_user: [],
      loading: 'loading...',
      refresh: 0,
    }),
    props: ['tab_name', 'user'],
    watch: {
        tab_name: function (Val) {
          this.name_from_shouye = Val;
          // 排行
          if(this.name_from_shouye == '排行榜'){
              this.$fetch({
                  method: 'GET',    // 大写
                  url: 'http://111.231.204.71:5000/Ranking',
                  data: {
                    my_name: this.ranking_tab_name,
                    refresh: this.refresh
                  }     
              }).then(resData => {
                  // 成功回调
                  this.ranking_list = resData
                  this.refresh = this.refresh + 1
                  console.log(JSON.stringify(resData))
              }, error => {
                  // 错误回调

                  console.log(error)
              });
          }
        },
        ranking_tab_name: function (Val) {
          this.ranking_tab_name = Val;
          // 排行
          if(this.name_from_shouye == '排行榜'){
              this.$fetch({
                  method: 'GET',    // 大写
                  url: 'http://111.231.204.71:5000/Ranking',
                  data: {
                    my_name: this.ranking_tab_name,
                    refresh: this.refresh
                  }     
              }).then(resData => {
                  // 成功回调
                  this.ranking_list = resData
                  this.refresh = this.refresh + 1
                  console.log(JSON.stringify(resData))
              }, error => {
                  // 错误回调

                  console.log(error)
              });
          }
        },

        user: function (Val) {
          this.my_user = Val;
        },
        loading: function (Val) {
          this.loading = Val;
        },
        
      },
    created () {
      this.tabPageHeight = Utils.env.getPageHeight();
      this.tabList = [...Array(this.tabTitles.length).keys()].map(i => []);
      // Vue.set(this.tabList, 0, this.ranking_list);
    },
    methods: {
      wxcTabPageCurrentTabSelected (e) {
        const self = this;
        const index = e.page;
        
        // 重置下拉刷新次数
        this.refresh = 0
        // 0 动作, 1 爱情, 2 恐怖 ,3 剧情 , 4 惊悚 , 5 冒险, 6 科幻, 7 动画, 8 家庭
        // 9 歌舞, 10 奇幻, 11 犯罪
        if(index == '0') this.ranking_tab_name = '动作'
        if(index == '1') this.ranking_tab_name = '爱情'
        if(index == '2') this.ranking_tab_name = '恐怖'
        if(index == '3') this.ranking_tab_name = '剧情'
        if(index == '4') this.ranking_tab_name = '惊悚'
        if(index == '5') this.ranking_tab_name = '冒险'
        if(index == '6') this.ranking_tab_name = '科幻'
        if(index == '7') this.ranking_tab_name = '动画'
        if(index == '8') this.ranking_tab_name = '家庭'
        if(index == '9') this.ranking_tab_name = '歌舞'
        if(index == '10') this.ranking_tab_name = '奇幻'
        if(index == '11') this.ranking_tab_name = '犯罪'

      },
      wxcPanItemPan (e) {
        if (BindEnv.supportsEBForAndroid()) {
          this.$refs['wxc-tab-page'].bindExp(e.element);
        }
      },
      changeName (titleCn) {
          
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
      footerAppear: function(e) {
        var _this = this;
        var items = [];
        // 排行
        this.$fetch({
            method: 'GET',    // 大写
            url: 'http://111.231.204.71:5000/Ranking',
            data: {
              my_name : _this.ranking_tab_name,
              refresh : this.refresh
            }
        }).then(resData => {
            // 成功回调
            items = resData
            for(let i=0;i<items.length;i++){
                this.ranking_list.push(items[i])
            }
            this.refresh = this.refresh + 1
            console.log(JSON.stringify(resData))
        }, error => {
            // 错误回调

            console.log(error)
        });

      },
    }
  };
</script>