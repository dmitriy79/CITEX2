<template>
    <div class="account-main bgf8">
            <div class="title">
                身份认证
            </div>
            <div class="main-content mt20">
                <div v-show="review_status==0">
                    <div class="main-input">
                        <div class="flex alcenter center">
                            <span>姓名：</span>
                            <input type="text" placeholder="请输入真实姓名" id="name" v-model="name">
                        </div>
                        <div class="flex alcenter center mt20">
                            <span>身份证：</span>
                            <input type="text" placeholder="请输入身份证号" id="card" v-model="card_id">
                        </div>
                    </div>
                    <div class="mt40 fColor1 ft14 tc">请上传身份证正反面，第一张为正面，第二张为反面。</div>
                    <div class="idimg flex center mt40">
                        <div>
                            <img :src="src01" alt="">
                            <input type="file" id="file" accept="image/*" name="file" @change="file1">
                        </div>
                        <div>
                            <img :src="src02" alt="">
                            <input type="file" id="file" accept="image/*" name="file" @change="file2">
                        </div>
                    </div>
                    <div class="updata tc">
                        <input type="button" value="提交" @click="updata">
                    </div>
                </div>
                <div  v-show="review_status==1">
                    <div class="tc ft30 au-statue">
                        审核中...  
                    </div> 
                </div>
                <div v-show="review_status==2">
                    <div class="tc ft30 au-statue">
                        已认证!
                    </div>
                </div>
            </div>
    </div>
</template>
<script>
export default {
    name:'authentication',
    data(){
        return {
           name:'',
           card_id:'',
           src1:'',
           src2:'',
           src01:'',
           src02:'',
           review_status:''
        }
    },
    created(){
        this.token=localStorage.getItem('token')
    },
    methods:{
        file1(){
            var that = this;
            var reader = new FileReader();
            reader.readAsDataURL(event.target.files[0]); 
            reader.onload = function(e){
                // console.log(e.target.result)
                that.src1=e.target.result
                that.src01=e.target.result
            } 
            var formData = new FormData();
            formData.append("file", event.target.files[0]); 
            $.ajax({
                url: '/api/'+'upload',
                type: 'post',
                data: formData,
                processData: false,
                contentType: false,
                beforeSend: function beforeSend(request) {
                    request.setRequestHeader("Authorization", that.token);
                },
                success: function (msg) {
                    console.log(msg)
                    that.src1=msg.message

                }
            });     

        },
        file2(){
            var that = this;
            var reader = new FileReader();
            reader.readAsDataURL(event.target.files[0]); 
            reader.onload = function(e){
                // console.log(e.target.result)
                that.src2=e.target.result
                that.src02=e.target.result
            } 
            var formData = new FormData();
            formData.append("file", event.target.files[0]); 
            $.ajax({
                url: '/api/'+'upload',
                type: 'post',
                data: formData,
                processData: false,
                contentType: false,
                beforeSend: function beforeSend(request) {
                    request.setRequestHeader("Authorization", that.token);
                },
                success: function (msg) {
                    that.src2=msg.message
                }
            });    
        },
        updata(){
            var that = this;
            let name = this.$utils.trim(that.name);
            let card_id = this.$utils.trim(that.card_id);
            var reg = /(^\d{15}$)|(^\d{18}$)|(^\d{17}(\d|X|x)$)/;
            if(this.name.length == ''){
                layer.tips('请输入姓名!', '#name');
                return;
            }
            if(this.card_id.length == ''|| !reg.test(this.card_id)){
                layer.tips('请输入正确身份证号!', '#card');
                return;
            }
            this.$http({
                url: '/api/'+'user/real_name',
                method:'post',
                data:{
                    name:name,
                    card_id:card_id,
                    front_pic:that.src1,
                    reverse_pic:that.src2
                },  
                headers: {'Authorization':  that.token}    
            }).then(res=>{
                console.log(res);
                    layer.msg(res.data.message)

                }).catch(error=>{
                    
            })  
        },
        Info(){
            var that = this;
            this.$http({
                url: '/api/'+'user/info',
                method:'get',
                data:{},  
                headers: {'Authorization':  that.token}   
            }).then(res=>{
                that.review_status=res.data.message.review_status;
                }).catch(error=>{
                    
            })
        }
    },
    mounted(){
       this.Info();
    }
}
</script>
<style lang="scss" scoped>
.account-main{
    padding-left: 34px;
    padding-right: 34px;
    padding-top: 34px;
    width: 970px;
    min-height: 800px;
    .title{
            width: 100%;
            line-height: 60px;
            border-radius: 4px;
            padding-left: 20px;
        }
        .main-content{
            .main-input{
                margin: 0 auto;
                text-align: center;
                padding-top: 100px;
                span{
                    width:100px;
                }
                input{
                    width: 320px;
                    min-height: 46px;
                    border: 1px solid #4e5b85;
                    padding: 0 20px;
                    font-size: 14px;
                    border-radius: 3px;
                }
            }
            .idimg{
                div{
                    width: 160px;
                    height: 160px;
                    overflow: hidden;
                    position: relative;
                    background: url('../../static/imgs/addimg.png') no-repeat;
                    background-size: 100% 100%;
                    border: 1px solid #E2E2E2;
                    margin-left: 50px;
                    input{
                        position: absolute;
                        z-index: 11110;
                        opacity: 0;
                        width: 100%;
                        height: 160px;
                        top: 0;
                        cursor: pointer;
                    }
                }
                
            }
            .updata{
                input{
                    width: 300px;
                    height: 45px;
                    border-radius: 4px;
                    font-size: 14px;
                    margin: 0 auto;
                    margin-left: 100px;
                    margin-top: 60px;
                    background: #5697f4
                }
            }
            .au-statue{
                padding-top: 100px;
                font-size: 30px
            }
        }
}

</style>

