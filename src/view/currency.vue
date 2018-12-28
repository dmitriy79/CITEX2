<template>
    <div class="currency">
        <div class="title ">币种资料</div>
        <div class="content f14 ">
            <p class="ft24 ">{{currency_name}}</p>
            <p class=" abstract ft14">简介</p>
            <p class="mt15 ft14" v-html="content"></p> 
        </div>
    </div>
</template>
<script>
export default {
    name:"currency",
    data(){
        return{
            currency_name:'',
            currency_id:'',
            content:''
        }
    },
    methods:{
        getData(){
            this.$http({
            url: '/api/' + "introduction",
            method: "post",
            data: {currency_id: this.currency_id},
            // headers: { Authorization: window.localStorage.getItem("token") || "" },
        }).then(res => {
                if (res.data.type == "ok") {
                   this.content=res.data.message.introduction[0].contents;
                }
            })
        }
    },
    mounted(){
        var that = this;
        if(window.localStorage.getItem('tradeData')){
            var localData=JSON.parse(window.localStorage.getItem('tradeData'))
            that.currency_id = localData.currency_id,
            that.legal_id = localData.legal_id;
            that.currency_name = localData.currency_name;
            that.legal_name = localData.legal_name;
            that.getData();
        }else{
            eventBus.$on("toTrade", function(data) {
                // console.log(data)
                that.currency_id = data.currency_id
                that.currency_name = data.currency_name;
                that.getData();
            });
            eventBus.$on("toTrade0", function(data0) {
                // console.log(data0)
                that.currency_id = data0.currency_id
                that.currency_name = data0.currency_name;
                that.getData();
            });
        }
        
        
    }
}
</script>
<style scoped>
.currency{border:1px solid #ccc}
.title{height: 48px;line-height: 46px;padding: 0 40px 0 30px;}
.content{padding: 15px 30px 60px;}
.ft24{font-size: 24px}
.abstract{padding:15px 0;border-bottom: 1px solid #ccc;}
</style>

