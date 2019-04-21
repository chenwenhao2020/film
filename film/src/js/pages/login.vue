<template>
    <div class='login_box'>
        <div class="title">请登录</div>
        <input class='enter' type='text' placeholder="请输入用户名/手机号" v-model='username' />
        <input class='enter' type='password' placeholder="请输入密码"  v-model='password'>
        <div class="size">
            <wxc-button :disabled="false" style="margin: 10px" text="注册" size='medium' type='white' @wxcButtonClicked="register"></wxc-button>
            
            <wxc-button :disabled='disabledBtn' style="margin: 10px" :text='loginText' size='medium' type='yellow' @wxcButtonClicked="login"></wxc-button>
        </div>

        <wxc-mask height="200"
            width="702"
            border-radius="0"
            duration="200"
            mask-bg-color="#FFFFFF"
            :has-animation="hasAnimation"
            :has-overlay="true"
            :show-close="true"
            :show="show"
            @wxcMaskSetHidden="wxcMaskSetHidden">
            <div class="content">
                <div class="demo-title">
                    <text class="title">{{message}}</text>
                </div>
            </div>
        </wxc-mask>

        <wxc-mask height="800"
            width="702"
            border-radius="0"
            duration="200"
            mask-bg-color="#FFFFFF"
            :has-animation="hasAnimation"
            :has-overlay="false"
            :show-close="false"
            :show="register_show"
            :mask-bg-color='register_color'>
            <div class="content">
                <div class="demo-title">
                    <text class="title">欢迎注册</text>
                </div>
                <input class='register_enter' type='text' placeholder="请输入用户名" v-model='register_username' />
                <input class='register_enter' type='password' placeholder="请输入密码"  v-model='register_password'>
                
                <wxc-grid-select
                    :single="true"
                    :cols="3"
                    :customStyles="customStyles"
                    :list="testData1"
                    style="margin-left: 180px"
                    @select="params => onSelect('res3', params)">
                </wxc-grid-select>
                <input class='register_enter' type='text' placeholder="请输入手机号"  v-model='register_phonenumber'>
                <div class='code_size'>
                    <input class='code_enter' type='text' placeholder="请输入验证码"  v-model='register_code_enter'>
                    <wxc-button :disabled="false" style="margin: 10px" text="点击获取验证码" size='medium' type='white' @wxcButtonClicked="get_code"></wxc-button>
                    
                </div>
                <div style='margin-left: 50px;' v-if='is_show'>
                    <text>验证码错误</text>
                </div>
        
                <div class='size'>
                    <wxc-button :disabled="false" style="margin: 10px" text="取消" size='medium' type='white' @wxcButtonClicked="register_exit"></wxc-button>
                    <wxc-button :disabled='false' style="margin: 10px" :text='reg_button_msg' size='medium' type='white' @wxcButtonClicked="register_commit"></wxc-button>
                </div>
            </div>
        </wxc-mask>

    </div>
</template>

<script>
    import { WxcButton, WxcMask, WxcGridSelect } from 'weex-ui'

    export default {
        components: { WxcButton, WxcMask, WxcGridSelect },
        data () {
            return {
                username: '',
                password: '',
                loginText: '登录',
                disabledBtn: false,
                // 注册账号
                register_username: '',
                register_password: '',
                register_phonenumber: '',
                register_code: '',
                register_code_enter: '',
                register_sex: '',
                reg_button_msg: '注册',
                // 弹出层
                show: false,
                overlayCanClose: true,
                isFalse: false,
                hasAnimation: true,
                message: '',
                // 注册弹出层
                register_show: false,
                register_overlayCanClose: true,
                register_isFalse: false,
                register_hasAnimation: true,
                register_message: '',
                register_color:'#FFC900',
                // 单选
                customStyles: {
                    lineSpacing: '14px',
                    width: '120px',
                    height: '50px',
                    icon: '',
                    color: '#333333',
                    checkedColor: '#ffffff',
                    disabledColor: '#eeeeee',
                    borderColor: '#666666',
                    checkedBorderColor: '#ffb200',
                    backgroundColor: '#ffffff',
                    checkedBackgroundColor: '#ffb200',
                },
                testData1: [
                    {
                    'title': '男'
                    },
                    {
                    'title': '女',
                    },
                ],
                // 显示-隐藏
                is_show: false,
            }
        },

        methods: {
            wxcButtonClicked (e) {
                this.disabledBtn = true;
            },
            wxcMaskSetHidden () {
                this.show = false;
            },
            register () {
                this.register_show = true
                this.register_hasAnimation= true
            },
            register_exit () {
                this.register_show = false;
            },
            onSelect (res, {selectIndex, checked, checkedList}) {
                this.register_sex = checkedList.map(item => item.title)[0]
            },
            register_commit () {
                var _this = this;
                this.reg_button_msg = '注册中...'
                if(this.register_code == this.register_code_enter){
                    this.$fetch({
                    method: 'GET',    // 大写
                    url: 'http://111.231.204.71:5000/Register',
                    data: {
                        name: this.register_username,
                        password: this.register_password,
                        phonenumber: this.register_phonenumber,
                        sex: this.register_sex,       
                    },
                    }).then(resData => {
                        // 成功回调
                        this.reg_button_msg = '注册成功'
                        this.register_show = false

                    }, error => {
                        console.log(error)
                    })    
                }
                else{
                    this.is_show = true
                }
                
            },
            get_code () {
                var _this = this;
                this.$fetch({
                    method: 'GET',    // 大写
                    url: 'http://111.231.204.71:5000/Code',
                    data: {
                        phonenumber: this.register_phonenumber             
                    },
        
                }).then(resData => {
                    // 成功回调
                    this.register_code = resData.code
                }, error => {
                    console.log(error)
                })
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

                    if(resData.message == '登录成功'){

                        this.$router.open({
                            name: resData.url,
                            params: resData.user,
                        })
                         _this.loginText = "登录";
                    }else{
                        this.show = true;
                        this.message = resData.message
                         _this.loginText = "登录";
                    }

                }, error => {
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

    .code_size {
        justify-content: space-around;
        flex-direction: row;
        margin-top: 0px;
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

    .register_enter{
        border-style: solid;
        border-width: 1px;
        border-color: #FFFFFF;
        border-radius: 5px;
        background-color: #FFFFFF;
        width: 600px;
        height: 60px;
        margin: 10px;
    }

    .code_enter{
        border-style: solid;
        border-width: 1px;
        border-color: #FFFFFF;
        border-radius: 5px;
        background-color: #FFFFFF;
        width: 300px;
        height: 60px;
        margin: 10px;
    }

    .content {
        padding: 30px;
    }

    .demo-title {
        align-items: center;
        margin-bottom: 20px;
        margin-top: 40px;
    }
    .title {
        color: #333333;
        font-size: 40px;
    }
</style>