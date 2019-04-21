<template>
    <wxc-tab-bar :tab-titles="tabTitles"
                :tab-styles="tabStyles"
                title-type="icon"
                @wxcTabBarCurrentTabSelected="wxcTabBarCurrentTabSelected">
        <!-- 第一个页面内容-->
        <div class="item-container" :style="contentStyle" ref="pageTitle" >
            <list :style="listStyle">
                <cell>
                    <wxc-ep-slider :user=user></wxc-ep-slider>
                    <recommend :user=user></recommend>
                </cell>
            </list>
        </div>

        <!-- 第二个页面内容-->
        <div class="item-container" :style="contentStyle">
            <wxc-tab-page :tab_name=tab_name :user=user></wxc-tab-page>
        </div>

        <!-- 第三个页面内容-->
        <div class="item-container" :style="contentStyle">
            <wxc-searchbar :tab_name=tab_name></wxc-searchbar>
        </div>

        <!-- 第四个页面内容-->
        <div class="item-container" :style="contentStyle">
            <wxc-cell :user=user></wxc-cell>
        </div>
    </wxc-tab-bar>
</template>

<style scoped>
    .item-container {
        width: 750px;
        background-color: #f2f3f4;
        align-items: center;
        justify-content: center;
    }

</style>
<script>
    import { WxcTabBar, Utils } from 'weex-ui'
    import WxcEpSlider  from './shouye.vue'
    import WxcCell from './personal.vue'
    import WxcSearchbar from './search.vue'
    import WxcTabPage from './ranking.vue'
    import Recommend from './recommend.vue'

    // https://github.com/alibaba/weex-ui/blob/master/example/tab-bar/config.js 
    import Config from './config'

    export default {
        components: { 
            WxcTabBar, 
            WxcEpSlider, 
            WxcSearchbar,
            WxcTabPage,
            WxcCell,
            'recommend': Recommend,
            },

        data: () => ({
            tabTitles: Config.tabTitles,
            tabStyles: Config.tabStyles,
            listStyle: { height: 1330 + 'px' },
            user_name: '',
            user: {},
            tab_name: '123',
        }),
        created () {
            var _this = this;
            // 获取 login传来的登录用户名
            this.$router.getParams().then(resData => {
                this.user_name = resData

                // 获取 user 
                this.$fetch({
                    method: 'GET',    // 大写
                    url: 'http://111.231.204.71:5000/GetUser',
                    data: {
                        name: this.user_name,
                    },
                }).then(resData => {
                    // 成功回调
                    var o
                    // _this.user = str(resData.user) 
                    // eval("var o = " + resData.user);
                    _this.user = resData
                    
                    console.log(JSON.stringify(resData))
                }, error => {
                    // 错误回调

                    console.log(error)
                });
            })

            const tabPageHeight = env.deviceHeight / env.deviceWidth * 750;
            // 如果页面没有导航栏，可以用下面这个计算高度的方法
            // const tabPageHeight = env.deviceHeight / env.deviceWidth * 750;
            const { tabStyles } = this;
            this.contentStyle = { height: (tabPageHeight - tabStyles.height) + 'px' };
        },

        methods: {
            wxcTabBarCurrentTabSelected (e) {
                const index = e.page;
                // console.log(index);
                if(index == '0') this.tab_name = '首页'
                if(index == '1') this.tab_name = '排行榜'
                if(index == '2') this.tab_name = '搜索'
                if(index == '3') this.tab_name = '个人中心'
            },
        },
    };

</script>