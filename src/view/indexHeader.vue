<template>
  <div id="index-head" class="flex">
    <div class="header-l flex">
      <div class="logo" style="margin-right:10px;">
        <img src="../assets/images/logo2.png" alt="">
      </div>
      <span class="title">币然交易所</span>
      <router-link to="/" exact>首页</router-link>
      <router-link to="/c2c">c2c交易</router-link>
      <router-link to="/legalTrade">法币交易</router-link>
      <router-link to="/dealCenter">币币交易</router-link>
      <router-link to="/myLegalShops" v-if="isShow">我的商铺</router-link>
      <!-- <router-link to="/fiatradCenter">法币交易</router-link> -->
      <router-link to="/userSetting">安全设置</router-link>
      <router-link to="/components/noticeList">公告</router-link>
      <div class="coin-box">
        <router-link to="/currencyApply">上币申请</router-link>
        <router-link to="/currencyList" >币种列表</router-link>
      </div>
       <!-- <router-link to="/walletTransfer">转账至钱包</router-link> -->
      <div>
        <div class="download">
          <div class="curPer">app下载</div>
          <img src="../assets/images/ewm.png" alt="">
        </div>
      </div>
    </div>
    <div class="header-r flex">
      <div v-if="!account_number.length" class="flex">

        <router-link to='/components/login'>登录</router-link>
        <router-link to='/components/register'>注册</router-link>
      </div>
      <div v-if="account_number.length" class="flex">
        <div class="assets">
          <div>资产</div>
          <div class="links">
            <router-link to='/finance'>交易</router-link>
            <!-- <router-link to='/finance'>交易交易</router-link> -->
          </div>
        </div>
        <div class="links-box">
          <div class="account_number">{{account_number}} (邀请码 {{extension_code}})</div>
          <div class="links">
            <!-- <router-link to="/userCenter">个人中心</router-link> -->
            <div v-for="(item,index) in accountList" @click="goto(null,item.page)">{{item.title}}</div>
            <router-link to="/workOrder">提交工单</router-link>
            <div @click="signOut">退出登录</div>
          </div>
        </div>
      </div>
      <div class=" theme flex">
          <img src="../assets/images/light.png" @click="$changeTheme('light')" alt="">
          <img src="../assets/images/dark.png"  @click="$changeTheme('dark')" alt="">
          
        </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      account_number: "",
      extension_code: "",
      is_seller:0,
      isShow:false,
      accountList:[
        {
          title: "账户设置",
          page: "accountSet"
        },
      ]
    };
  },
  created() {
    this.token = window.localStorage.getItem("token") || "";
    this.account_number = window.localStorage.getItem("accountNum") || "";
    this.extension_code = window.localStorage.getItem("extension_code") || "";
    this.is_seller = window.localStorage.getItem("is_seller") || "";
    if(this.is_seller == 1){
        this.isShow = true;
    }
    eventBus.$on("seller", msg => {
      if(msg){
          this.is_seller = window.localStorage.getItem("is_seller") || "";
          if(this.is_seller == 1){
            this.isShow = true;
          }
      }
    });
    eventBus.$on("toHeader", msg => {
      if(msg.account){
        this.account_number = msg.account;
        this.extension_code = msg.extension_code;
      }
    });
    // this.init();
    
  },
  mounted() {
    eventBus.$on("toHeader", msg => {
      console.log(msg);
      if(msg.account){
        this.extension_code = msg.extension_code;

        this.account_number = msg.account;
      }
    });
  },
  methods: {
    signOut() {
      this.account_number = "";
      // window.localStorage.removeItem("token");
      // window.localStorage.removeItem("accountNum");
      // window.localStorage.removeItem("user_id");
      // window.localStorage.removeItem("extension_code");
      window.localStorage.clear();
      this.$router.push('/components/login');
    },
    goto(index, name) {
      this.current = index;
      this.bus.$emit('nav_name',name);
      if(name == 'userSetting'){
        this.$router.push('/userSetting')
      } else {
        this.$router.push({ name: name });
      }
    },
    init(){
      this.$http({
        url:'/api/'+'user/info',
        method:'GET',
        data:{},
        headers:{Authorization:this.token}
      }).then(res => {
        console.log(res)
        if(res.data.type == 'ok'){
            if(res.data.message.is_seller == 1){
               this.isShow = true;
            }
        }
        
      })
    }
  }
};
</script>

<style lang='scss'>
.title{
  margin-right: 30px
}
#index-head {
  justify-content: space-between;
  padding: 0 20px;
  min-width: 1200px;
  height: 45px;
  line-height: 45px;
  font-size: 14px;
  background: #181b2a;
  color: #ccc;

  a:hover,
  .router-link-active {
    color: #d45858;
  }
  .currency-list,
  .lalala {
    display: none;
  }
  > .header-l {
    align-items: center;
    > .logo {
      border-radius: 50%;
      width: 30px;
      height: 30px;
      background: #fff;
      > img {
        width: 100%;
      }
    }
    > a,
    > div {
      margin-right: 30px;
      height: 45px;

      > a {
        display: block;
      }
    }
    > .coin-box {
      position: relative;
      a:last-child {
        display: none;
        position: absolute;
        width: 130%;
        text-align: center;
        left: -15%;
      }
      &:hover {
        a:last-child {
          display: block;
          background: #181b2a;
          // text-align: center;
        }
      }
    }
    .download{
      position: relative;
      
      img{
        position: absolute;
        display: none;
          width: 100px;
          height: 100px;
        z-index: 999;
        left: -20px;
      }
      &:hover{
        color: #d45858;
        img{
          display: block;
        }
      }
    }
  }
  .header-r {
    a {
      margin: 0 20px;
    }
    .assets {
      position: relative;
      margin-right: 25px;
      padding-left: 25px;
      background: url("../assets/images/assets.png") no-repeat left center;
      background-size: 20px 20px;
      cursor: pointer;
      > div:first-child {
        padding-right: 14px;
        background: url("../assets/images/arrow0.png") no-repeat right center;
      }
      > .links {
        position: absolute;
        right: 0;
        width: 80px;
        z-index: 999;
        text-align: center;
        background: #181b2a;
        display: none;
        a {
          margin: 0;
          display: block;
        }
      }
      &:hover {
        .links {
          display: block;
        }
      }
    }
    .links-box {
      position: relative;
      cursor: pointer;
      padding-right: 20px;
      background: url("../assets/images/arrow0.png") no-repeat right center;

      &:hover {
        .links {
          display: block;
        }
      }
      .account_number {
        padding-left: 20px;
        background: url("../assets/images/icon_mine.png") no-repeat left center;
        background-size: 15px;
      }
      .links {
        position: absolute;
        right: 0;
        z-index: 999;
        background: #181b2a;
        display: none;
        a{
          border-bottom: 1px solid #383d54;
        }
        a,
        div {
          display: block;
          padding: 0 14px;
          margin: 0;
          text-align: center;
          border-bottom: 1px solid #383d54;
          &:hover {
            color: #d45858;
          }
        }
        div:last-child(){
          border-bottom: none;
        }
      }
    }
    .theme>img{
      cursor: pointer;
      margin-left: 15px;
      margin-top: 12.5px;
      width: 20px;
      height: 20px;
    }
  }
}
</style>
