<template>
  <div id="user-center">
    <!-- <div class="title">个人中心</div> -->
    <div class="content">
      <ul class="content-l">
        <li>
          <!-- <img src="../assets/images/logo.png" alt=""> -->
          <router-link to="/userCenter">账户设置</router-link>
        </li>
        <!-- <li>
          <img src="../assets/images/logo.png" alt="">
          <router-link to="/">账户设置</router-link>
        </li>
        <li>
          <img src="../assets/images/logo.png" alt="">
          <router-link to="/dealCenter">账户设置</router-link>
        </li> -->
      </ul>
      <div class="content-r">
        <router-view></router-view>
      </div>

    </div>
  </div>
</template>

<script>
export default {};
</script>

<style lang='scss'>
#user-center {
  // margin: 0 auto;
  min-width: 1200px;
  width: 100%;
  .router-link-active{
    color: #d45858;
  }
  >.content{
    position: relative;
    // margin-top: 50px;
    // padding-left: 180px;
    >ul,>div{
      padding: 0 30px;
      background: #f8f8f8;
    }
    >.content-l{
      // position: absolute;
      // left: 0;
      // top: 0;
      // width: 150px;
      width: 23%;
      float: left;
      line-height: 40px;
      text-align: center;
      height: 1080px;
      border-right: 1px solid #ddd;

      img{
        width: 20px;
        height: 20px;
        vertical-align: sub;
      }
    }
    >.content-r{
      width: 77%;
      float: right;
    }
  }
}
</style>
