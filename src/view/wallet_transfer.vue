<template>
    <div class="wrap add-mycoin">
        <h1 class="title tc bold mb10">转账至钱包</h1>
        <div class="main">
           <div class="inp_wrap clear" v-for="(item,index) in list" :key="index">
               <p class="fl lebal ft14 tc">{{item.lebal_name}}：</p>
               <input class="fl inp ft14" :type="item.type" :name="item.name" value="" autocomplete="on" v-model="item.val" :placeholder="item.placeholder" />
           </div>
           <!-- <div class="img">
              <img src="../assets/images/yzm.png" />
           </div>
           <div class="inp_wrap clear">
               <p class="fl lebal ft14">图片验证码：</p>
               <input class="fl inp ft14" type="text" name="yanzhengma" value="" placeholder="" />
           </div> -->
           <div class="submit_btn" @click="submit_apply">
               确认
           </div>
        </div>
    </div>
</template>
<script>
export default {
    data(){
        return{
            list:[
                {lebal_name:'钱包地址',placeholder:'请填写钱包地址',type:'text',name:'address',val:''},
                {lebal_name:'转出金额',placeholder:'请填写转出金额',type:'number',name:'num',val:''},
                {lebal_name:'支付密码',placeholder:'请填写支付密码',type:'password',name:'pay_password',val:''}
            ]
        }
    },
    created(){
        this.token = window.localStorage.getItem("token") || "";
        if(this.token == ''){
        this.$router.push('/components/login');
    }
    },
    methods:{
        submit_apply(){
            for(var i=0;i<this.list.length;i++){
                 if(this.list[i].val == ''){
                     layer.msg(this.list[i].placeholder);
                     return;
                 }
            }
            this.$http({
                url: "/api/wallet/wallet_withdraw",
                method: "post",
                data: {
                address:this.list[0].val,
                number:this.list[1].val,
                pay_password:this.list[2].val
                },
                headers: { Authorization: this.token }
            }).then(res => {
                console.log(res);
                 layer.msg(res.data.message);
                })
                .catch(res => {
                layer.msg(res.data.message);
                });
                 
           
        }
    }
}
</script>
<style scoped>
.add-mycoin{
    min-height: 1050px;
}
    .wrap{
        padding-top: 20px;
    }
    .title{
        font-size: 28px;
    }
    .main{
        width: 1200px;
        margin: 0 auto;
        margin-top: 10px;
        border-top: 1px solid #C8DAE2;
        padding: 30px 0;
    }
    .inp_wrap{
        width: 800px;
        margin: 0 auto;
        margin-bottom: 10px;
        display: flex;
        justify-content: center;
        border: 1px solid #eee;
    }
    .lebal{
        width: 200px;
        background: #eee;
        color: #333;
        padding: 10px;
        border-right: 1px solid #ccc;
        text-align: center;
    }
    .inp{
        width: 600px;
         padding: 10px;
    }
    .img{
        width: 800px;
        margin: 0 auto;
    }
    .submit_btn{
        width: 800px;
        margin: 0 auto;
        margin-top: 50px;
        padding: 10px 0;
        font-size: 16px;
        color: #fff;
        text-align: center;
        background: #d45858;
    }
    .submit_btn:hover{
        background: #ca4141;
        cursor: pointer;
    }
</style>
