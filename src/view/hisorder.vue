<template>
    <div class="hisorder">
        <h3 class="tc ft18">提交工单</h3>
        <div class="attion ft14">请严格按照分类提交问题</div>
        <ul class="tabbox flex">
            <li :class="{'ticket-active':index==current}" v-for="(item,index) in tabList" @click="change(index,item.type)">{{item.name}}</li>
        </ul>
        <div>
            <p>请提交您的截图：</p>
            <div class="mt10">
                <div class="ticket">
                    <input class="file" v-if="img" id="file" type="file" name="image" accept="image/*" multiple=""  @change="chooseimg">
                    <img :src="src" alt="" class="evimg" v-else>
                    <!-- <div class="mask">
                        <p>选择文件</p>
                    </div> -->  
                </div>
            </div>
        </div>
        <div>
            <p class="mt20">选择币种</p>
            <div class="mt10">
                <select name="" id="" v-model='selectval' class="input_bg">
                    <option :value="item.id" v-for="(item,index) in coinList">{{item.name}}</option>
                </select>
            </div>
        </div>
        <div>
            <p class="mt20">标题</p>
            <div class="mt10">
                <input type="text" placeholder="请输入标题信息" class="input input_bg" v-model="title">
            </div>
        </div>
        <div>
            <p class="mt40">您的文字信息</p>
            <div class="mt10">
                <textarea name="" id="" cols="30" rows="10" v-model="textval" class="input_bg">

                </textarea>
            </div>
        </div>
        <div class="mt20">
            <input type="button" value="已确认无误,提交" class="btn tc" @click="upbtn">
        </div>
       
    </div>
    
</template>
<script>
export default {
    name:'hisorder',
    data(){
        return{
            type:1,
            current:0,
            img:true,
            src:'',
            selectval:'',
            title:'',
            textval:'',
            tabList:[{name:'充值',type:1},{name:'提现',type:2},{name:'其它',type:0}],
            coinList:[],
            formData:new FormData()
        }
    },
    created(){
        this.token=localStorage.getItem('token');
        this.getCoin();
    },
    methods:{
        change(index,type){
            this.current=index;
            this.type=type;
        },
        getCoin(){
            var that =this;
            this.$http({
                url: '/api/' + 'currency/all',
                method:'get',
                data:{},
            }).then(res=>{
                console.log(res)
                var msg=res.data.message;
                if(res.data.type  ==='ok'){
                    that.coinList=msg;
                    console.log(that.coinList)
                    that.selectval=that.coinList[0].id;
                }else{
                    layer.msg(res.data.message);
                }
            })
            .catch(error=>{
                console.log(error)
            })
        },
         // 选图
            chooseimg () {
                var that =this;
                var reader = new FileReader();
                reader.readAsDataURL(event.target.files[0]); 
                reader.onload = function(e){
                    console.log(e.target.result)
                    that.src=e.target.result
                } 
                // var formData = new FormData();
                that.formData.append("attachment", event.target.files[0]); 
                that.img=false;
            },
            // 提交
            upbtn(){
                var that = this;
                if(that.img){
                    return layer.msg('请上传截图');
                }
                if(that.title==''||that.title.length<=0){
                    return layer.msg('请填写标题');
                }
                if(!that.textval&&that.textval.length<10){
                    return layer.msg('文字内容太少');
                }
                that.formData.append('issue_type', that.type);
                that.formData.append('currency_id', that.selectval);
                that.formData.append('title', that.title);
                that.formData.append('content', that.textval);
                $.ajax({
                    url: '/api/' + 'work_order/add',
                    type: 'POST',
                    data: that.formData,
                    processData: false, // 不处理数据
                    contentType: false, // 不设置内容类型
                    async: true,
                    beforeSend: function beforeSend(request) {
                        request.setRequestHeader("Authorization", that.token);
                    },
                    success: function (res) {
                        console.log(res)
                        layer.msg(res.message);
                        // if (data.type == 'ok') {
                            
                        // }
                    },
                    error: function () {
                        layer_msg('网络错误');
                    }
                })
      
            }
    }
}
</script>
<style lang="scss">
.hisorder{
    .attion{
        color: #00a5bc;
        margin: 10px 0 20px;
    }
    .tabbox{
        border-bottom: 1px solid #a3acb5;
        li{
            font-size: 16px;
            padding: 10px 30px;
            border: 1px solid transparent;
            margin-bottom: -1px;
            cursor: pointer;
        }
        .ticket-active {
            border: 1px solid #a3acb5;
            margin-bottom: -1px;
            border-bottom-color: #fff;
        }
    }
    .ticket {
        display: flex;
        align-items: center;
        justify-content: center;
        position: relative;
        overflow: hidden;
        text-align: center;
        padding: 10px;
        width: 115px;
        min-height: 128px;
        max-height: 128px;
        background: #f3f3f3 url(../../static/imgs/upimage.svg) no-repeat center;
        border: 1px solid #ccc;
        img{
            max-width: 115px;
        }
        .file{
            position: absolute;
            width: 100%;
            height: 100%;
            opacity: 0;
            cursor: pointer;
        }
        .mask{
            display: flex;
            align-items: center;
            justify-content: center;
            position: absolute;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, .8)
            p{
                padding: 5px;
                color: #fff;
                border-radius: 3px;
                background: #2196f3;
            }
        }
    }
    select,.input{
        width: 60%;
        height: 38px;
        padding: 0 10px;
        border: 1px solid #CCC;
        border-radius: 3px;
    }
    textarea{
        width: 100%;
        max-width: 100%;
        min-width: 100%;
        height: 120px;
        min-height: 120px;
        padding: 10px;
        box-sizing: border-box;
        font-family: 'Microsoft YaHei',sans-serif;
        border: 1px solid #ccc;
        border-radius: 3px;
        outline: none;
        margin-top: 5px;
    }
    .btn{
        width: 60%;
        background: #d45858;
        box-shadow: none;
        border: none;
        color: #fff;
        font-size: 16px;
        text-indent: 0;
        padding: 10px;
        height: auto;
        cursor: pointer;
        border-radius: 3px;
        outline: none;
    }
}
</style>

