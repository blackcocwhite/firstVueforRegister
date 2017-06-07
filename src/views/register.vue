<template>
    <div id="reg">
        <div class="main">
            <div class='head-title'>
                <p class='icon'><img src="../assets/favicon_change.png"></p>
                <span>皖新智慧校园</span>
            </div>
            <div class='input-group input-group-lg'>
                <!-- <span class="input-group-addon" style='border-radius:0px' id="basic-addon1">+86</span> -->
                <input type='text' class='form-control' style="font-size:.85rem;border-radius:0px" name='phone' placeholder="请输入手机号" v-model="mobile" @keyup="inputVerify">
                <span class="input-group-btn">
                    <button class="btn btn-default send-button" style='border-radius:0px;font-size:.85rem;'  :disabled='disabled || time > 0' type="button" @click='sendVerifyCode'>
                        <p style="font-size:.75rem;" :class="{ sended: disabled }">{{ text }}</p>
                    </button>
                </span>
            </div>
            <div class="input-group input-group-lg">
                <input type="text" class='form-control' style="font-size:.85rem" name='code' v-model='code' placeholder="请输入5位验证码" @keyup="inputCode">
            </div>

            <button class='btn btn-lg btn-validate' style='color:white' :disabled="validating" @click="validate">注册</button>
        </div>


        <div class="copyright">
            <span>Copyright @2010-2017</span>
        </div>
    </div>
</template>
<script>
    export default{
        name: 'Register',
        props:['uid','wappid','url','fid'],
        data(){
            return{
                code:'',
                second: 60,
                mobile:'',
                time:0,
                disabled:true,
                isTrue:false,
                validating:true
            }
        },
        methods:{
            run(){
                this.time = this.second
                this.timer()
            },
            timer() {
                if (this.time > 0) {
                    this.time--;
                    setTimeout(this.timer, 1000);
                }else {
                    this.disabled = false;
                }
            },
            sendVerifyCode(){
                this.disabled = true;
                setTimeout(this.sended, 1000);
            },
            sended(){
                this.run();
                this.axios.post('http://k12.edu/laravel-sms/verify-code',{
                    mobile: this.mobile,
                    mobile_rule : 'mobile_required'
                }).then(response=>{
                    if(response.data.success != true){
                        alert(response.data.message);
                    }
                });
            },
            inputVerify(){
                if((/^1[34578]\d{9}$/.test(this.mobile))){
                    this.disabled = false;
                }else{
                    this.disabled = true;
                }
            },
            inputCode(){
                if(this.code.length == 5){
                    this.validating = false;
                }else{
                    this.validating = true;
                }
            },
            validate(){
                if(this.code == ''){
                    alert('请输入4位验证码');
                }else{
                    this.axios.post('http://k12.edu/validateCode',{
                        mobile: this.mobile,
                        verifyCode: this.code,
                        mobile_rule: 'mobile_required'
                    }).then(response=>{
                        console.log(response);
                        if(response.data.status === 0){
                            this.code = '';
                            this.validating = true;
                            alert('您输入的验证码不正确，请重试');
                        }else{
                            var url = 'http://k12.edu/v1/systemRegister/' + this.uid + '/' + this.mobile + '/' + this.wappid;
                            this.axios.get(url).then(response=>{
                                if(response.data.status == 1){
                                    location.href = this.url + "/#/" + '?uid=' +this.uid+ '&fid=' +this.fid+ '&wappid=' +this.wappid;
                                }
                                console.log(response);
                            })
                        }
                    })
                }

            }
        },
        computed: {
            text() {
                return this.time > 0 ? this.time + 's 后重新获取' : '发送验证码';
            }
        }
    }
</script>
<style>
    #reg{
        min-width: 100%;
        width:100%;
        margin: 0 auto;
        min-height: 100%;
        /*height: auto;*/
        /*height: 100%;*/
        /*position: relative;*/
    }
    .main{
        /*padding-bottom: 3.75rem;*/
    }
    .icon>img{
        margin-top: -0.25rem;
        width:3.75rem;
        height: 3.625rem;
    }
    .input-group{
        width:80%;
        margin: 0 auto;
        border-bottom:1px solid #F1F1F1;
    }
    .form-control{
        border-radius: 0px;
        border-style: none;
        /*border-bottom-style: solid;*/
        /*border-bottom-width:thin;*/
    }
    .send-button{
        font-size:.75rem;
        color:#1A73C4;
        /*height: 34px;*/
        border-style: none;
        /*border-bottom-style:solid;*/
        border-left-style:solid;
        /*border-bottom-width:thin;*/
    }
    .send-button>p{
        line-height: 1.5rem;
        color:#1A73C4;
    }
    .sended{
        color:#BDD6ED;
        border-radius: 0px;
    }
    .head-title{
        margin-top:60px;
        font-size: 1.875rem;
        color:#1A73C4;
        font-weight: bold;
    }
    .btn.disabled, .btn[disabled], fieldset[disabled] .btn{
        opacity:.45;
    }
    .head-title{
        margin-bottom: 3.125rem;
    }
    .head-title span{
        font-weight: blod;
    }
    .input-group-addon{
        background-color: white;
        border-radius: 0px;
        border-style: none;
        border-bottom-style:solid;
        border-bottom-width:thin;
    }
    .btn-validate{
        width:80%;
        color:white;
        margin-top: 2rem;
        border-radius: 0px;
        border-style: none;
        background-color:#1A73C4;
    }
    .copyright{
        font-size: .625rem;
        width:100%;
        margin: 0 auto;
        color:#828282;
        position:absolute;
        bottom:0;
        height: 3.75rem;
    }
</style>
