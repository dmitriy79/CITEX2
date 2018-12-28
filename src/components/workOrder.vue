<template>
    <div class="order">
        <div class="m-title tc bg-part bod_bc">我的工单</div>
        <div class="left bod_rc">
            <uporder></uporder>
        </div>
        <div class="right">
            <hisorder></hisorder>
        </div>
    </div>
</template>
<script>
import uporder from '@/view/uporder'
import hisorder from '@/view/hisorder'
export default {
    name:'workOrder',
    components:{uporder,hisorder},
    data(){
        return{

        }
    }
}
</script>
<style lang="scss">
.order{
    width:100%;
    min-width:1200px;
    margin: 0 auto;
    .m-title{
        height: 42px; 
        width: 100%;
        font-size: 20px;
        // border-bottom: 1px solid #c1d0cd;
        margin: 10px 0 20px;
        line-height: 46px;
    }
    .left{
        width: 50%;
        min-height: 800px;
        float: left;
        padding: 0px 30px;
    }
    .right{
        width:50%;
        min-height: 800px;
        float: right;
        padding: 0px 30px;
    }
}
</style>

