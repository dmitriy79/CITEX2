<template>
    <div class="detail">
        <div class="title pd20 bgf8">
           工单详情
           <span class="fr curPer" @click="goBefore">&lt;&lt;返回</span>
        </div>
        <div>

        </div>
        <div class="checkbox bgf8  scroll">
           <div v-for="(item,index) in detailList" class="list">
               <div :class="item.send==0?'flex my':'flex system'">
                    <p v-if='item.send==1'>客服：</p>
                    <div :class="item.send==0?'bluebg c_content':'whitebg c_content'">
                        <p>{{item.content}}</p>
                        <img v-if='item.attachment' :src="item.attachment" alt="">
                    </div>
                    
                    <p v-if='item.send==0'>:我</p>
                </div> 
                <!-- <p>{{item.create_time}}</p> -->
            </div> 
        </div>
        <div class="enterbox">
            <div class="chossefile flex flex between alcenter">
                <div class="flex">
                   <p>
                        <img src="../../static/imgs/addimg.png" alt="">
                        <input type="file" id="file" name="attachment" accept="image/*" @change='file'>
                    </p>
                    <p @click="reload"><img src="../../static/imgs/reload.png" alt=""></p> 
                </div>
                <div class="complete">
                    <input type="button" value="完成工单" @click="complete">
                </div>
               
            </div>
            <textarea name="" id="" v-model="txt">

            </textarea>
            <input type="button" class="btn" value="发送" @click='send'>
        </div>
        
    </div>
</template>
<script>
export default {
    name:"orderDetail",
    data(){
        return{
            detailList:[],
            txt:'',
            src:''
        }
    },
    created(){
        this.id = this.$route.query.id;
        this.token=localStorage.getItem('token');
    },
    methods:{
        goBefore(){
            this.$router.back(-1);
        },
        getDetail(){
            var that =this;
            this.$http({
                url: '/api/' + 'work_order/detail/' + that.id,
                method:'get',
                data:{},
                headers: { Authorization: that.token }
            }).then(res=>{
                console.log(res)
                var msg=res.data.message.data.data;
                that.detailList=msg;
                // if(res.data.type ==='ok'){
                //     that.hisList=msg;
                // }else{
                //     layer.msg(res.data.message);
                // }
            })
            .catch(error=>{
                console.log(error)
            })
        },
        file(){
            var that = this;
            var reader = new FileReader();
            reader.readAsDataURL(event.target.files[0]); 
            reader.onload = function(e){
                that.src=e.target.result;
            } 
        },
        send(){
            var that =this;
            var form_data = new FormData();
            var txt = that.txt;
            var attachment = $('#file')[0].files;
            form_data.append('content', txt);
            if (attachment.length > 0) {
                attachment = attachment[0];
                form_data.append('attachment', attachment);
            }
            console.log(form_data)
            if(txt == ''){
               return layer.msg('内容不能为空');
            } else {
                // window.scrollTo(div.offset());
                $.ajax({
                    url:'/api/'+'work_order/reply/'+that.id,
                    type:'post',
                    data:form_data,
                    processData: false, 
                    contentType: false, 
                    async: true,
                    beforeSend:function beforeSend(request){
                         request.setRequestHeader('Authorization',that.token)
                    },
                    success:function(res) {
                        console.log(res);
                        if(res.type == 'ok'){
                            var newdata={"content":that.txt,send:0,attachment:that.src}
                            that.detailList.push(newdata);
                            that.txt='';
                            that.src='';
                            form_data='';
                        }
                    }
                })
            }
        },
        reload(){
            this.getDetail();
        },
        complete(){
            var that =this;
            this.$http({
                url: '/api/' + 'work_order/complete/' + that.id,
                method:'post',
                data:{},
                headers: { Authorization: that.token }
            }).then(res=>{
                console.log(res)
                if(res.data.type ==='ok'){
                    layer.msg(res.data.message);
                    setTimeout(function(){
                        that.$router.go(-1);
                    })

                }else{
                    layer.msg(res.data.message);
                }
            })
            .catch(error=>{
                console.log(error)
            })
        }
    },
    mounted(){
        this.getDetail()
    }
}
</script>
<style lang="scss" scoped>
.detail{
    min-height: 850px;
    width: 1000px;
    margin: 0 auto;
    padding-top: 20px;
    .title{
       height:60px;
       line-height:60px;
       padding:0 20px;
    }
    .checkbox{
        height: 500px;
        margin: 0 auto;
        margin-top: 20px;
        padding: 20px;
        overflow-y:scroll;
        .list{
            margin-top: 10px; 
            width:100%;
            .my{
                justify-content: flex-end;
            }
            .c_content{
                margin-left: 10px;
                max-width: 40%;
                border-radius:5px;
                padding: 5px 10px;
                
            }
            img{
                max-width: 100%;
            }
            .bluebg{
                background: green;
            }
            .whitebg{
                background: #fff;
            }
        }
    }
    .enterbox{
        background: #fff;
        height: 200px;
        position: relative;
        .complete{
            input{
                padding: 5px 10px;
                border-radius:5px;
                color: #fff;
                background: #de5959;
            }
            
        }
       .chossefile{
           background: #ddd;
           padding:5px 20px;
           p{
               width: 35px;
               height:35px;
               position: relative;
               cursor: pointer;
               overflow: hidden;
               input{
                position: absolute;
                width: 35px;
                height: 35px;
                opacity: 0;
                z-index: 9;
                top:0;
                left:0;
                margin-right:5px;
                cursor: pointer;

               }
                img{
                    width: 35px;
                    height:35px;
                } 
           }
          
       }
        textarea{
            width:100%;
            height:155px;
            border: 1px solid #eee;
            padding:10px;
        }
        .btn{
            width: 60px;
            padding: 5px;
            position: absolute;
            right: 15px;
            bottom:15px;
            background: #f8f8f8;
            border: 1px solid #eee;
            z-index: 9;
            cursor: pointer;
        
        }
    }
}
</style>


