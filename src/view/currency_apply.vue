<template>
    <div class="wrap add-mycoin">
        <h1 class="title tc bold mb10">优质项目（币种）上线申请</h1>
        <p class="ft16 tc">请填写以下项目资料（前10名中提供最详细最准确的用户如被采纳，<span class="red">立即获得1ETH奖励</span>）</p>
        <div class="main">
           <div class="inp_wrap clear" v-for="(item,index) in list" :key="index">
               <p class="fl lebal ft14">{{item.lebal_name}}</p>
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
               提交上线申请
           </div>
        </div>
    </div>
</template>
<script>
export default {
    data(){
        return{
            list:[
                {lebal_name:'您的Email：',placeholder:'Email',type:'text',name:'email',val:''},
                {lebal_name:'团队联系方式：',placeholder:'Email等',type:'email',name:'tel',val:''},
                {lebal_name:'币种英文名：',placeholder:'比如Bitcoin',type:'text',name:'english_name',val:''},
                {lebal_name:'币种中文名：',placeholder:'比如 比特币',type:'text',name:'chinese_name',val:''},
                {lebal_name:'币种交易符号：',placeholder:'比如BTC',type:'text',name:'token',val:''},
                {lebal_name:'ICO日期：',placeholder:'YYYY-MM-DD',type:'text',name:'ico_time',val:''},
                {lebal_name:'可流通日期：',placeholder:'YYYY-MM-DD',type:'text',name:'circulation_time',val:''},
                {lebal_name:'币种区块网络类型：',placeholder:'ETH,QTUM,NEO,XLM,BTS或者独立链',type:'text',name:'net_type',val:''},
                {lebal_name:'代币合约地址：',placeholder:'如果没有填 无',type:'text',name:'address',val:''},
                {lebal_name:'小数点位数：',placeholder:'如Bitcoin是8',type:'text',name:'decimals',val:''},
                {lebal_name:'币种官方网站：',placeholder:'',type:'text',name:'official_website',val:''},
                {lebal_name:'币种白皮书网址：',placeholder:'',type:'text',name:'white_paper_website',val:''},
                {lebal_name:'区块浏览器：',placeholder:'',type:'text',name:'browsing_volume',val:''},
                {lebal_name:'Logo图片链接：',placeholder:'',type:'text',name:'img_src',val:''},
                // {lebal_name:'Twitter链接：',placeholder:'',type:'text',name:'twitter_src',val:''},
                // {lebal_name:'Telegram链接：',placeholder:'',type:'text',name:'telegram_src',val:''},
                {lebal_name:'币种简短中文介绍：',placeholder:'',type:'text',name:'chinese_des',val:''},
                // {lebal_name:'币种简短英文介绍：',placeholder:'',type:'text',name:'English_des',val:''},
                {lebal_name:'币种总量：',placeholder:'',type:'text',name:'currency_account',val:''},
                // {lebal_name:'币种流通量：',placeholder:'',type:'text',name:'currency_circle',val:''},
                // {lebal_name:'币种分配比例：',placeholder:'如ICO分发比例，团队预留等',type:'text',name:'proportion',val:''},
                // {lebal_name:'成本价格：',placeholder:'presale,private sale,public sale等',type:'text',name:'price',val:''},
                // {lebal_name:'已上线交易平台：',placeholder:'未上线写无',type:'text',name:'trade_terrace',val:''},
                // {lebal_name:'其他信息说明：',placeholder:'',type:'text',name:'other_msg',val:''},
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
                     layer.msg('请填写'+this.list[i].lebal_name);
                     return;
                 }
            }
            this.$http({
                url: "/api/currency/request_add_currency",
                method: "post",
                data: {
                email:this.list[0].val,
                tel:this.list[1].val,
                english_name:this.list[2].val,
                chinese_name:this.list[3].val,
                token:this.list[4].val,
                ico_time:this.list[5].val,
                circulation_time:this.list[6].val,
                net_type:this.list[7].val,
                address:this.list[8].val,
                decimals:this.list[9].val,
                official_website:this.list[10].val,
                white_paper_website:this.list[11].val,
                browsing_volume:this.list[12].val,
                logo:this.list[13].val,
                desc:this.list[14].val,
                sum:this.list[15].val
                },
                headers: { Authorization: this.token }
            }).then(res => {
                console.log(res);
                layer.msg(res.data.message);
                setTimeout(() => {
                    this.$router.push('/currencyList')
                }, 1000);
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
        text-align: right;
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
