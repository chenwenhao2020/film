<template>
    <div class='login_box'>
        <div class="title">请登录</div>
        <input class='enter' type='text' placeholder="请输入账号" v-model='username' />
        <input class='enter' type='password' placeholder="请输入密码"  v-model='password'>
        <div class="size">
            <wxc-button style="margin: 10px" text="注册" size='medium' type='white' @wxcButtonClicked="wxcButtonClicked"></wxc-button>
            
            <wxc-button :disabled='disabledBtn' style="margin: 10px" :text='loginText' size='medium' type='yellow' @wxcButtonClicked="login"></wxc-button>
        </div>
    </div>
</template>

<script>
    import { WxcButton } from 'weex-ui'

    export default {
        components: { WxcButton },
        data () {
            return {
                username: 'admin',
                password: '123456',
                loginText: '登录',
                disabledBtn: false,
            }
        },

        methods: {
            wxcButtonClicked (e) {
                this.disabledBtn = true;
            },
            login(e) {
                var _this = this;
                // this.disabledBtn = true;
                this.loginText = '登录中...';
                console.log(1)
                // ajax 验证 用户名 密码
                this.$fetch({
                    method: 'GET',    // 大写
                    url: 'http://111.231.204.71:5000/check',
                    data: {
                        username: this.username,
                        password: this.password,
                    },
                    
                }).then(resData => {
                    // 成功回调
                    // _this.disablebtn = false;
                    _this.loginText = "登录成功";

                    this.$router.open({
                        name: 'Hello',
                    })
                }, error => {
                    // 错误回调
                    // _this.disabled = false;
                    _this.loginText = "登录失败"
                    _this.loginText = error
                    console.log(error)
                })
            }
        },

    }

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .login_box{
        align-items: center;
        justify-content: center;
    }

    .title{
        margin-bottom: 50px;
    }

    .size {
        justify-content: space-around;
        flex-direction: row;
        margin-top: 50px;
    }

    .enter{
        border-style: solid;
        border-width: 1px;
        border-color: #FFC900;
        border-radius: 5px;
        width: 600px;
        height: 60px;
        margin: 10px;
    }
</style>