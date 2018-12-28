<template>
    <div class="uporder">
        <h3 class="tc ft18">历史工单</h3>
        <div class="attion ft14">提交工单后，客服稍后给您回复，请耐心等待。</div>
        <ul class="hislist">
            <li v-for="(item,index) in hisList" @click="godetail(item.id)" class="bg-part">
                <div class="flex between">
                    <p>{{item.title}}</p>
                    <p :class="{'colorlan':item.status_name=='已完成'}">{{item.status_name}}</p>
                </div>
                <div class="content-text">
                    {{item.content}}
                </div>
                <div class="tr">
                    <span class="color8">{{item.create_time}}</span>
                </div>
            </li>
        </ul>
    </div>
</template>
<script>
export default {
    name:'uporder',
    data(){
        return{
            hisList:[]
        }
    },
    created(){
        this.token=localStorage.getItem('token');
        this.getHist();
    },
    methods:{
        getHist(){
            var that =this;
            this.$http({
                url: '/api/' + 'work_order/lists',
                method:'get',
                data:{},
                headers: { Authorization: that.token }
            }).then(res=>{
                console.log(res)
                var msg=res.data.message.data;
                if(res.data.type ==='ok'){
                    that.hisList=msg;
                }else{
                    layer.msg(res.data.message);
                }
            })
            .catch(error=>{
                console.log(error)
            })
        },
        godetail(id){
            this.$router.push({
                name: 'orderDetail',
                query:{id:id}
            });
        }
    }
}
</script>
<style lang="scss" scoped>
.uporder{
    .attion{
        color: #00a5bc;
        margin: 10px 0 20px;
    }
    .hislist{
        li{
            padding: 10px;
            background: #eee;
            border-radius: 4px;
            margin-bottom: 10px;
            cursor: pointer;
            .content-in{
                max-height: 50px;
                overflow: hidden;
            }
            .color8{color: #888}
            .colorlan{color: #00a5bc}
        }
    }
}
</style>


