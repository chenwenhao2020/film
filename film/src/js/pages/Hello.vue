<template>
    <wxc-tab-bar :tab-titles="tabTitles"
                :tab-styles="tabStyles"
                title-type="icon"
                @wxcTabBarCurrentTabSelected="wxcTabBarCurrentTabSelected">
        <!-- 第一个页面内容-->
        <div class="item-container" :style="contentStyle" ref="pageTitle">
            <list :style="listStyle">
                <cell>
                    <wxc-ep-slider></wxc-ep-slider>
                    <recommend></recommend>
                    <wxc-button class="btn-250" text="show eros" @wxcButtonClicked="showEros"></wxc-button>
                </cell>
            </list>
        </div>

        <!-- 第二个页面内容-->
        <div class="item-container" :style="contentStyle">
            <wxc-tab-page></wxc-tab-page>
        </div>

        <!-- 第三个页面内容-->
        <div class="item-container" :style="contentStyle">
            <wxc-searchbar></wxc-searchbar>
        </div>

        <!-- 第四个页面内容-->
        <div class="item-container" :style="contentStyle">
            <wxc-cell></wxc-cell>
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
    import { WxcTabBar, Utils, WxcButton} from 'weex-ui'
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
            WxcButton,
            'recommend': Recommend,
            },

        data: () => ({
            tabTitles: Config.tabTitles,
            tabStyles: Config.tabStyles,
            listStyle: { height: 1330 + 'px' },
            user_name: '',
        }),
        created () {
            _this = this;
            const tabPageHeight = env.deviceHeight / env.deviceWidth * 750;
            // 如果页面没有导航栏，可以用下面这个计算高度的方法
            // const tabPageHeight = env.deviceHeight / env.deviceWidth * 750;
            const { tabStyles } = this;
            this.contentStyle = { height: (tabPageHeight - tabStyles.height) + 'px' };
            this.$router.getParams().then(resData => {
                _this.user_name = resData
            })
        },
        methods: {
            wxcTabBarCurrentTabSelected (e) {
                const index = e.page;
                // console.log(index);
            },
            showEros() {
                this.$router.open({
                    name: 'Login',
                })
            },
        },
    };

</script>