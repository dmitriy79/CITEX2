<template>
    <div id="c2c-box" class="flex">
        <div class="c2c-l">
            
            <ul>
                <li class="flex" v-for="(item,index) in currency_list" :key="index" :class="index == active?'bg_active':''" :data-id="item.id" @click="currency_click(item.id,item.name,index,item.c2c_ratio)">
                    <div class="flex">
                        <div>{{item.name}}/CNY</div>
                        <div class="redColor">{{item.name}}/人民币</div>
                    </div>
                    <span>></span>
                </li>
            </ul>
        </div>
        <div class="c2c-r">
            <div class="top">
                <div class="top-title flex">
                    <div>
                        <router-link tag="span" to="/c2c" class="link-span">tether {{currency_name}}</router-link>
                        <span>对CNY</span>
                    </div>
                </div>
                <!-- <div class="top-price flex">
                    <div>
                        <span>实时价：</span>
                        <span>￥6.83</span>
                    </div>
                    <div>
                        <span>日涨跌：</span>
                        <span>1.2%</span>
                    </div>
                    <div>
                        <span>最高价：</span>
                        <span>￥6.83</span>
                    </div>
                    <div>
                        <span>最低价：</span>
                        <span>￥6.83</span>
                    </div>
                </div> -->
                <div class="inp-items flex">
                    <div class="inp-item">
                        <div class="inp-title flex">
                            <div>买入{{currency_name}}</div>
                            
                        </div>
                        <div class="how redColor ft14">如何买入?</div>
                        <div class="three-inp">
                            <!-- <div class="inp-box">
                                <span>买入估价CNY</span>
                                <div style="background:#f8f8f8;">6.85</div>
                            </div> -->
                            <div class="inp-box">
                                <span>买入量{{currency_name}}</span>
                                <input  v-model="num" type="number">
                            </div>
                            <div class="inp-box">
                                <span>单价CNY</span>
                                <input v-model="price" type="number">
                            </div>
                            <div class="inp-box hide">
                                <span>姓名</span>
                                <input type="text" v-model="user_name">
                            </div>
                            <div class="inp-box hide">
                                <span>详细内容</span>
                                <input type="text" v-model="content">
                            </div>
                        </div>
                        <div class="pay-opts flex hide">
                            <div>支付方式</div>
                            <div>
                    
                                <label >
                                    <input type="radio" name="pay" value="支付宝" v-model="pay">
                                    支付宝
                                </label>
                            </div>
                            <div>
                    
                                <label >
                                    <input type="radio" name="pay" value="微信" v-model="pay">
                                    微信
                                </label>
                            </div>
                            <div>
                    
                                <label >
                                    <input type="radio" name="pay" value="银行卡" v-model="pay">
                                    银行转账
                                </label>
                            </div>
                            <div class="redColor">（必须本人支付)</div>
                            <!-- <router-link tag="div" to="/c2c">《交易须知》</router-link> -->
                        </div>
                        <div class="btn-in bgRed" @click="bui_in">买入（CNY→{{currency_name}}）</div>
                    </div>
                    <div class="inp-item">
                        <div class="inp-title flex">
                            <div>
                                <span>卖出{{currency_name}}</span>
                                <span class="ft14" style="color:#666">({{currency_name}}余额)</span>
                                </div>
                            <!-- <div style='font-size:14px;color:#666'>
                                <router-link to="/c2c" style="margin-right:10px">{{currency_name}}充值</router-link>
                                <router-link to="/c2c">{{currency_name}}提现</router-link>
                            </div> -->
                        </div>
                        <div class="how redColor ft14">如何卖出?</div>
                        <div class="three-inp">
                            <!-- <div class="inp-box">
                                <span>卖出估价CNY</span>
                                <div style="background:#f8f8f8;color:#008069">6.85</div>
                            </div> -->
                            <div class="inp-box">
                                <span>卖出量{{currency_name}}</span>
                                <input type="number" v-model="num01">
                            </div>
                            <div class="inp-box">
                                <span>单价CNY</span>
                                <input type="number" v-model="price01">
                            </div>
                             <div class="inp-box hide">
                                <span>姓名</span>
                                <input type="text" v-model="user_name01">
                            </div>
                            <div class="inp-box hide">
                                <span>详细内容</span>
                                <input type="text" v-model="content01" />
                            </div>
                        </div>
                        <div class="pay-opts flex hide">
                            <div>支付方式</div>
                            <div>
                    
                                <label >
                                    <input type="radio" name="pay01" value="支付宝" v-model="pay01">
                                    支付宝
                                </label>
                            </div>
                            <div>
                    
                                <label >
                                    <input type="radio" name="pay01" value="微信" v-model="pay01">
                                    微信
                                </label>
                            </div>
                            <div>
                    
                                <label >
                                    <input type="radio" name="pay01" value="银行卡" v-model="pay01">
                                    银行转账
                                </label>
                            </div>
                            
                            <div class="redColor">（必须本人支付)</div>
                            <!-- <router-link tag="div" to="/c2c">《交易须知》</router-link> -->
                        </div>
                        <div class="btn-out" @click="sell_out">卖出（{{currency_name}}→CNY）</div>

                    </div>
                    
                </div>
            </div>
            <div class="bot">
                
                <div class="bot-title flex">
                  <div>
                    <span @click="nowList ='listIn';reloadC2c()" :class="{'active':nowList == 'listIn'}">c2c</span>
                    <span @click="nowList =  'myAdd';reloadMyAdd()" :class="{'active':nowList == 'myAdd'}">我发布的c2c</span>
                    <span @click="nowList = 'myBuySell';reloadMyBuySell()" :class="{'active':nowList == 'myBuySell'}">我交易的c2c</span>
                  </div>
                  <div class="flex" @click="showList = !showList">
                    <div :class="[{'switch-on':!showList},{'switch':showList}]"><div></div></div>
                    <span class="ft14">显示市场挂单</span>
                  </div>
                </div>
                <div class="list-title flex" v-if="showList">
                    <div>类型</div>
                    <div>价格(CNY)</div>
                    <div>数量</div>
                    <div>总计(CNY)</div>
                    <div>状态</div>
                    <div class="tr">操作</div>
                </div>
                <div class="ul-box scroll" v-if="nowList == 'listIn'">
                    <!-- c2c卖出 -->
                    <ul class="ul-out" v-if="showList&&listOut.list.length">
                      <li v-for="(item,index) in listOut.list" :key="index" :class="item.show==1?'flex':'flex deny_trade'">
                          <div class="blue">卖出</div>
                          <div>{{item.price}}</div>
                          <div>{{item.number}} {{item.token}}</div>
                          <div>{{(item.number*item.price-0).toFixed(2)}}</div>
                          <div>{{item.status_name}}</div>
                          <div class="last">
                            <div class="btn-last" @click="buySell(item.id,'buy')" v-if="item.show==1">买入</div>
                            <div class="btn-last" v-else>买入</div>
                            <span class="show-detail" @click="getDetail(item.id,'c2c',$event)" v-if="item.show==1">详情</span>
                            <span class="show-detail" v-else>详情</span>
                          </div>
                      </li>
                    </ul>
                    <!-- <div class="more"  v-if="listOut.length&&listOut.hasMore" @click="getList(1)">加载更多</div> -->
                    <!-- c2c买入 -->
                    <ul class="ul-in" v-if="showList&&listIn.list.length">
                        <li v-for="(item,index) in listIn.list" :key="index" :class="item.show==1?'flex':'flex deny_trade'">
                            <div class="red">买入</div>
                            <div>{{item.price}}</div>
                            <div>{{item.number}} {{item.token}}</div>
                            <div>{{(item.number*item.price-0).toFixed(2)}}</div>
                            <div>{{item.status_name}}</div>
                            <div class="last">
                              <div class="btn-last" @click="buySell(item.id,'sell')" v-if="item.show==1">卖出</div>
                              <div class="btn-last" v-else>卖出</div>
                              <span class="show-detail" @click="getDetail(item.id,'c2c',$event)" v-if="item.show==1">详情</span>
                              <span class="show-detail" v-else>详情</span>
                            </div>
                        </li>  
                    </ul>
                    <div class="more"  v-if="(listIn.list.length&&listIn.hasMore) || (listOut.list.length&&listOut.hasMore)" @click="getList(0);getList(1)">加载更多</div>
                </div>
                <!-- 我发布的买入卖出 -->
                <div class="ul-box scroll" v-if="nowList == 'myAdd'">
                  <ul class="ul-out" v-if="showList&&myAdd.list.length">
                    <li v-for="(item,index) in myAdd.list" :key="index" class="flex" >
                        <div :class='item.type_name=="挂买"?"red":"blue"'>{{item.type_name}}</div>
                        <div>{{item.price}}</div>
                        <div>{{item.number}} {{item.token}}</div>
                        <div>{{(item.number*item.price-0).toFixed(2)}}</div>
                        <div>{{item.status_name}}</div>
                        <div class="last">
                            <div class="btn-last" @click="cancelComplete('cancel_transaction',item.id)" v-if="item.type == 0&&item.status== 1" style="margin-right:10px;background: #ca4141;">取消交易</div>
                            <div v-if="item.status_name == '等待中'" class="btn-last" @click="cancelComplete('cancel',item.id,index)">取消发布</div>
                            <!-- <div v-if="item.status_name == '交易中'" class="btn-last" @click="cancelComplete('cancel_transaction',item.id,index)">取消交易</div> -->
                            <div v-if="item.type == 1&&item.status == 1" class="btn-last" @click="cancelComplete('complete',item.id,index)">确认收款</div>
                            <!-- <span class="show-detail" v-if="item.status_name == '已成功' ">{{item.status_name}}</span>
                            <span class="show-detail" v-if="item.status_name == '已取消' ">{{item.status_name}}</span> -->
                            <span class="show-detail" @click="getDetail(item.id,'myc2c',$event)">详情</span>
                        </div>
                    </li>
                  </ul>
                  <div class="more"  v-if="myAdd.list.length&&myAdd.hasMore&&this.showList" @click="getMy('myAdd')">加载更多</div>
                </div>
                <!-- 我交易的 -->
                <div class="ul-box scroll" v-if="nowList == 'myBuySell'">
                    <ul class="ul-out" v-if="showList&&myBuySell.list.length">
                        <li v-for="(item,index) in myBuySell.list" :key="index" class="flex" >
                            <div :class='item.transaction_name=="买入"?"red":"blue"'>{{item.transaction_name}}</div>
                            <div>{{item.price}}</div>
                            <div>{{item.number}} {{item.token}}</div>
                            <div>{{(item.number*item.price-0).toFixed(2)}}</div>
                            <div>{{item.status_name}}</div>
                            <div class="last">
                              <div class="btn-last" @click="cancelComplete('complete',item.id)" v-if="(item.type == 0)&&item.status == 1">确认</div>
                              <div class="btn-last" @click="cancelComplete('cancel',item.id)" v-if="item.type == 1||item.status == 1">取消交易</div>
                              <!-- <span class="show-detail" v-if="item.status_name == '等待中'">{{item.status_name}}</span>
                              <span class="show-detail" v-if="item.status_name == '已成功'">{{item.status_name}}</span>
                              <span class="show-detail" v-if="item.status_name == '已取消'">{{item.status_name}}</span> -->
                              <span class="show-detail" @click="getDetail(item.id,'trade',$event)">详情</span>
                            </div>
                        </li>               
                    </ul>
                    <div class="more"  v-if="myBuySell.list.length&&myBuySell.hasMore&&this.showList" @click="getMy('myBuySell')">加载更多</div>
                </div>
            </div>
        </div>
        <!-- =========详情弹窗========== -->
        <div class="mask" v-if="showDetail">
            <div class="m-content">
                <div class="title">
                    <div>详情</div>
                    <div @click="showDetail = false">x</div>
                </div>
                <div class="list">
                    <div class="create-date">
                        <span>创建时间：</span><span>{{detail['c2c']['create_date']}}</span>
                    </div>
                    
                    <div class="c2c-detail" v-if="detail.type=='c2c'">
                        <!-- <div>
                            <span>{{detail.c2c.type==0?'买家':'卖家'}}账号：</span><span>{{detail.user_info.account_number}}</span>
                        </div> -->
                        <!-- <div>
                            <span>{{detail.c2c.type==0?'买家':'卖家'}}姓名：</span><span>{{detail.c2c.name}}</span>
                        </div> -->
                        <div v-if="detail.c2c.type=1">
                          <span>手续费：</span><span>{{((detail.c2c.c2c_ratio-0)*(detail.c2c.number-0).toFixed(4))}}</span>
                        </div>
                    </div>
                    <!-- 我发布的内容 -->
                    <div class="myC2cDetail" v-if="detail.type=='myc2c'">
                        
                        <div>
                            <span>交易类型：</span><span>{{detail.c2c.type_name}}</span>
                        </div>
                        <div>
                            <span>币  种：</span><span>{{detail.c2c.token}}</span>
                        </div>
                        <div>
                            <span>价  格：</span><span>{{detail.c2c.price}}</span>
                        </div>
                        <div v-if="detail.c2c.status!=0&&detail.c2c.status!=3"> 
                          <div>
                            <span>交易人：</span><span>{{detail.transaction_account_info.account_number}}</span>
                          </div>
                          <!-- <div class="hide">
                            <span>姓 名：</span><span>{{detail.transaction_account_info.real_name}}</span>
                          </div> -->
                          <div>
                            <span>微 信:</span><span>{{detail.transaction_account_info.wechat_account}}</span>
                          </div>
                          <div>
                            <span>支付宝:</span><span>{{detail.transaction_account_info.alipay_account}}</span>
                          </div>
                          <div>
                            <span>银行卡:</span><span>{{detail.transaction_account_info.bank_name}}{{detail.transaction_account_info.bank_account}}</span>
                          </div>
                        </div>
                    </div>
                    <!-- 我交易的内容 -->
                    <div class="trade-detail" v-if="detail.type=='trade'">
                        <!-- <div>
                            <span>我的账号：</span>{{detail.transaction_user.account_number}}<span></span>
                        </div> -->
                        <div>
                            <span>交易类型：</span><span>{{detail.c2c.transaction_name}}</span>
                        </div>
                        <div>
                            <span>币  种：</span><span>{{detail.c2c.token}}</span>
                        </div>
                        <div>
                            <span>价  格：</span><span>{{detail.c2c.price}}</span>
                        </div>
                        <div>
                            <span>详  情：</span><span>{{detail.c2c.content}}</span>
                        </div>
                        <div>
                            <span>交易人：</span><span>{{detail.user_info.account_number}}</span>
                        </div>
                        <!-- <div>
                            <span>姓  名：</span><span>{{detail.c2c.name}}</span>
                        </div> -->
                        <div>
                            <span>微信账号：</span><span>{{detail.account_info.wechat_account}}</span>
                        </div>
                        <div>
                            <span>银行卡：</span><span>{{detail.account_info.bank_name}}{{detail.account_info.bank_account}}</span>
                        </div>
                        <div>
                            <span>支付宝：</span><span>{{detail.account_info.alipay_account}}</span>
                        </div>
                        <!-- <div v-if="detail.c2c=">
                          <span>手续费：</span><span>{{((detail.c2c.c2c_ratio-0)*(detail.c2c.number-0).toFixed(4))}}</span>
                        </div> -->
                    </div>
                    <div class="num">
                      <span>数  量：</span><span>{{detail.c2c.number}}</span>
                    </div>
                    <div>
                      <span>状 态：</span><span class="blue">{{detail.c2c.status_name}}</span>
                    </div>
                    <div class="evbox flex alcenter" v-if="(detail.c2c.pay_time>0)">
                      <span>付款凭证:</span>
                      <img :src="detail.c2c.pay_voucher" alt="" class="evimg" @click="evimgs(detail.c2c.pay_voucher)">
                    </div>
                    <div v-if="detail.type=='myc2c'">
                      <div class="upbox" v-if="detail.c2c.status == 1&&detail.c2c.type == 0&&detail.c2c.pay_time<=0">
                        <div class="updev">上传付款凭证</div>
                        <input id="photo" type="file" name="image" accept="image/*" multiple="" class="input_avator" @change="updev(detail.c2c.id)">
                      </div>
                    </div>
                    <div v-if="detail.type=='trade'">
                      <div class="upbox" v-if="detail.c2c.status == 1&&detail.c2c.type == 1&&detail.c2c.pay_time<=0">
                        <div class="updev">上传付款凭证</div>
                        <input id="photo" type="file" name="image" accept="image/*" multiple="" class="input_avator" @change="updev(detail.c2c.id)">
                      </div>
                    </div>
                </div>            
            </div>
        </div>
    </div>
</template>

<script>
export default {
  data() {
    return {
      token: "",
      nowList: "listIn",
      listIn: { page: 1, list: [], hasMore: true },
      listOut: { page: 1, list: [], hasMore: true },
      myAdd: { page: 1, list: [], hasMore: true },
      myBuySell: { page: 1, list: [], hasMore: true },
      active: 0,
      currency_name: "",
      id: "",
      c2c_ratio:0,
      price: "",
      num: "",
      pay: "支付宝",
      user_name: "",
      content: "",
      price01: "",
      num01: "",
      pay01: "支付宝",
      user_name01: "",
      content01: "",
      currency_list: [],
      currency_name: "",
      showList: true,
      showDetail: false,
      src:'',
      evimg:'',
      detail: {} //li详情
    };
  },
  created() {
    this.token = window.localStorage.getItem("token") || "";
    if (this.token == "") {
      this.$router.push("/components/login");
    }
    this.get_currency();
    this.getList(1);
    this.getList(0);
    this.getMy("myAdd");
    this.getMy("myBuySell");
  },
  methods: {
    reloadC2c() {
      this.listIn = { page: 1, list: [], hasMore: true };
      this.listOut = { page: 1, list: [], hasMore: true };
      this.getList(1);
      this.getList(0);
    },
    reloadMyAdd(){
       this.myAdd= { page: 1, list: [], hasMore: true };
       this.getMy('myAdd')
    },
    reloadMyBuySell(){
        this.myBuySell= { page: 1, list: [], hasMore: true };
        this.getMy('myBuySell')
    },
    // 获取币种列表
    get_currency() {
      this.$http({
        url: "/api/currency/c2c_list",
        method: "get",
        headers: { Authorization: this.token }
      }).then(res => {
        ////console.log(res);
        if (res.data.type == "ok") {
          this.currency_list = res.data.message.legal;
          this.currency_name = res.data.message.legal[0].name;
          this.id = res.data.message.legal[0].id;
          this.c2c_ratio = res.data.message.legal[0].c2c_ratio;
        }
      });
    },
    //选择币种
    currency_click(id, name, index,c2c_ratio) {
      this.currency_name = name;
      this.active = index;
      this.id = id;
      this.c2c_ratio = c2c_ratio;
    },
    // 获取c2clist
    getList(type) {
      let page = 1;
      page = type == 1 ? this.listOut.page : this.listIn.page;
      ////console.log(type);
      let  i = layer.load();
      this.$http({
        url: "/api/c2c/list?type=" + type + "&page=" + page,

        method: "get",
        headers: { Authorization: this.token }
      }).then(res => {
        layer.close(i);
        if (res.data.type == "ok") {
          let list = res.data.message.list;
          ////console.log(list);

          if (list.length != 0) {
            // //console.log(list);

            if (type == 1) {
              this.listOut.list = this.listOut.list.concat(list);
              this.listOut.hasMore = true;
              this.listOut.page += 1;
              //   //console.log(this.listOut);
            } else {
              //   //console.log(this.listIn.list);
              this.listIn.hasMore = true;
              this.listIn.list = this.listIn.list.concat(list);
              // //console.log( this.listIn.list);

              // //console.log(this.listIn);

              this.listIn.page += 1;
              // //console.log([].concat(list));
            }
          } else {
            type == 1
              ? (this.listOut.hasMore = false)
              : (this.listIn.hasMore = false);
          }
        }
      });
    },
    // c2c列表买入卖出
    buySell(id, type) {
      var that = this;
      // this.showDetail  = false;
      if(type=='buy'){
      layer.confirm('必须在30分钟内完成付款，否则将会被冻结账号确认下单？', {
          btn: ['确认','取消'] //按钮
        }, function(){
          that.buyandsell(id,type);
        }, function(){
          layer.msg('取消成功');
        });
      }else{
        that.buyandsell(id,type);
      }
    },
    buyandsell(id,type){
      let  i = layer.load();
      this.$http({
        url: "/api/c2c/" + type,
        method: "post",
        data: { id: id },
        headers: { Authorization: this.token }
      }).then(res => {
        layer.close(i);
        layer.msg(res.data.message);
        if (res.data.type == "ok") {
          if (type == "buy") {
            this.listOut = { hasMore: true, list: [], page: 1 };
            this, getList(1);
          } else {
            this.listIn = { hasMore: true, list: [], page: 1 };
            this.getList(0);
          }
          this.myBuySell = { hasMore: true, list: [], page: 1 };
          this.getMy("myBuySell"); //更新我交易的c2c
          //   //console.log(res.data);
        }
      });
    },
    getMy(type) {
      let t = "";
      t = type == "myAdd" ? "my_add" : "my_transaction";
      let  i =layer.load();
      this.$http({
        url: "/api/c2c/" + t + "?page=" + this[type].page,

        headers: { Authorization: this.token }
      }).then(res => {
        //console.log(res);
        layer.close(i);
        if (res.data.type == "ok") {
          if (res.data.message.length == 0) {
            this[type]["hasMore"] = false;
          } else {
            let list = res.data.message;
            this[type]["hasMore"] = true;
            this[type]["list"] = this[type]["list"].concat(list);
            this[type]["page"] += 1;
            //console.log(this[type]);
          }
        }
      });
    },
    cancelComplete(type, id, index) {
      // this.showDetail = false;
      this.$http({
        url: "/api/c2c/" + type,
        method: "post",
        data: { id: id },
        headers: { Authorization: this.token }
      }).then(res => {
        layer.msg(res.data.message);
        if (res.data.type == "ok") {
          //console.log(res);
          this.listIn = { page: 1, list: [], hasMore: true };
          this.listOut = { page: 1, list: [], hasMore: true };
          this.getList(1);
          this.getList(0);
          if (type == "complete") {
            this.myBuySell = { hasMore: true, list: [], page: 1 };
            this.myAdd = { hasMore: true, list: [], page: 1 };
            this.getMy("myBuySell");
            this.getMy("myAdd");
          } else {
            this.myAdd = { hasMore: true, list: [], page: 1 };
            this.getMy("myAdd");
          }
        }
      });
    },
    getDetail(id, type, e) {
      if (e.target.className == "btn-last") {
        return;
      }

      this.$http({
        url: "/api/c2c/detail?id=" + id,
        headers: { Authorization: this.token }
      }).then(res => {
        console.log(res);

        if (res.data.type == "ok") {
          console.log(res.data.message);
          this.detail.c2c = res.data.message.c2c;
          this.detail.account_info = res.data.message.account_info;
          this.detail.user_info = res.data.message.user_info;
          this.detail.transaction_account_info = res.data.message.transaction_account_info;
          this.detail.type = type;
          if (res.data.message.transaction_user) {
            this.detail.transaction_user = res.data.message.transaction_user;
          }
          console.log(this.detail);
          this.showDetail = true;
        }
      });
    },
    //添加买入
    bui_in() {
      var that =this;
      if(this.price == ""){
        return layer.msg('请输入单价');
      }
      if(this.num == ""){
        return layer.msg('请输入数量');
      }
      // if(this.user_name == ""){
      //   return layer.msg('请输入姓名');
      // }
      // if(this.content == ""){
      //   return layer.msg('请填写详细信息');
      // }
      console.log(this.c2c_ratio,this.num)
      var c2c_rationum=(this.c2c_ratio*this.num).toFixed(4)
      layer.confirm('数量:'+that.num, {
        btn: ['确认','取消'] //按钮
        }, function(){
        let  i = layer.load();
        that.$http({
          url: "/api/c2c/add",
          method: "post",
          data: {
            price: that.price,
            number: that.num,
            name: '',
            pay_mode: that.pay,
            content: that.content,
            token: that.currency_name,
            type: 0
          },
            headers: { Authorization: that.token }
          }).then(res => {
            layer.close(i);
            layer.msg(res.data.message);
            that.price = "";
            that.num = "";
            that.user_name = "";
            that.pay = "";
            that.content = "";
            that.listIn = { page: 1, list: [], hasMore: true };
            that.getList(0);
            that.myAdd = { hasMore: true, list: [], page: 1 };
            that.getMy("myAdd");
          });
        }, function(){
          layer.msg('取消成功',{icon: 1})
      });
      // return layer.open({
      //    type: 1 //Page层类型
      //   ,area: ['500px', '300px']
      //   ,title: '你好，layer。'
      //   ,shade: 0.6 //遮罩透明度
      //   ,anim: 1 //0-6的动画形式，-1不开启
      //   ,content: '<div style="padding:50px;">这是一个非常普通的页面层，传入了自定义的html</div>',
      //   btn: ['是的'],
      //    yes: function(index){
      //       layer.close(index);
      //       console.log(123456789)
      //   }

      // });    
      
    },

    //添加卖出
    sell_out() {
      var that = this;
      if(this.price01 == ""){
        return layer.msg('请输入单价');
      }
      if(this.num01 == ""){
        return layer.msg('请输入数量');
      }
      // if(this.user_name01 == ""){
      //   return layer.msg('请输入姓名');
      // }
      // if(this.content01 == ""){
      //   return layer.msg('请填写详细信息');
      // }
      var c2c_rationum=(this.c2c_ratio*this.num01).toFixed(4)
      return layer.confirm('数量:'+that.num01+',所需手续费:'+c2c_rationum, {
        btn: ['确认','取消'] //按钮
        }, function(){
        let  i = layer.load();
         that.$http({
            url: "/api/c2c/add",
            method: "post",
            data: {
              price: that.price01,
              number: that.num01,
              name: '',
              pay_mode: that.pay01,
              content: that.content01,
              token: that.currency_name,
              type: 1
            },
            headers: { Authorization: that.token }
          })
            .then(res => {
              layer.close(i);
              layer.msg(res.data.message);
              that.price = "";
              that.num = "";
              that.user_name = "";
              that.pay = "";
              that.content = "";
              that.listOut = { page: 1, list: [], hasMore: true };
              that.getList(1);
              that.myAdd = { hasMore: true, list: [], page: 1 };
              that.getMy("myAdd");
            })
            .catch(res => {
              layer.msg(res.data.message);
            });
        }, function(){
          layer.msg('取消成功',{icon: 1})
      });

    },
    // 上传打款凭证
    updev(id){
      var that =this;
      var reader = new FileReader();
      reader.readAsDataURL(event.target.files[0]); 
      reader.onload = function(e){
        console.log(e.target.result)
        that.src=e.target.result
      } 
      var formData = new FormData();
      formData.append("file", event.target.files[0]); 
      $.ajax({
          url: '/api/'+'upload',
          type: 'post',
          data: formData,
          processData: false,
          contentType: false,
          headers: { Authorization: that.token },
          success: function (msg) {
            console.log(msg)
            // that.src=msg.message
            that.upimg(id,msg.message);
        }
      });     
    },
    upimg(id,avatar){
      var that = this;
      this.$http({
        url: '/api/'+'c2c/pay',
        method:'post',
        headers: { Authorization: that.token },
        data:{
            id:id,
            pay_voucher:avatar
        },    
      }).then(res=>{
          console.log(res);
              layer.msg(res.data.message)
          }).catch(error=>{       
      }) 
    },
    evimgs(src){
      console.log(src)
      return layer.open({
        type: 1 //Page层类型
        ,area: ['375px', '500px']
        ,title: ''
        ,shade: 0.6 //遮罩透明度
        ,anim: 1 //0-6的动画形式，-1不开启
        ,content: "<img src='"+src+"' alt='' class='openimg'>"
        ,btn: ['关闭'],
         yes: function(index){
            layer.close(index);
        }

      });   
    }
  }
};
</script>

<style lang='scss'>
#c2c-box {
  .mask {
    position: fixed;
    width: 100%;
    height: 100%;
    z-index: 999;
    background: rgba(0, 0, 0, 0.7);
    > .m-content {
      border-radius: 4px;
      background: #fff;
      position: absolute;
      top: 40%;
      left: 50%;
      transform: translate(-50%, -50%);
      padding: 40px 20px 30px;
      // min-height: 400px;
      max-height: 550px;
      width: 400px;
      > .title {
        position: absolute;
        top: 0;
        left: 0;
        text-align: center;
        width: 100%;
        height: 40px;
        line-height: 40px;
        font-size: 18px;
        text-align: center;
        font-weight: 600;
        > div:last-child {
          position: absolute;
          top: 0;
          right: 0;
          padding: 0 15px;
          cursor: pointer;
        }
      }
      > div:not(.title) {
        line-height: 32px;
        // border-top: 1px solid #eaecef;
      }
      div {
        span:first-child {
          margin-right: 5px;
          display: inline-block;
          width: 70px;
          color: #ca4141;
        }
      }
    }
  }
  .more {
    color: #ca4141;
    text-align: center;
    cursor: pointer;
  }
  .bg_active {
    background: #dfe8f3;
  }
  border-top: 1px solid #ddd;
  font-size: 14px;
  > .c2c-l {
    border-right: 1px solid #ddd;
    padding: 30px;
    width: 23%;
    min-height: 900px;

    ul {
      background: #f8f8f8;
    }
    li {
      padding: 0 10px;
      justify-content: space-between;
      cursor: pointer;

      line-height: 40px;
      &:hover {
        background: #dfe8f3;
      }
      .redColor {
        margin-left: 10px;
      }
    }
  }
  > .c2c-r {
    padding: 10px 30px;
    width: 77%;
    > .top {
      > .top-title {
        font-size: 24px;
        line-height: 2;
        .link-span {
          cursor: pointer;
          &:hover {
            color: #de5959;
          }
        }
      }
      .top-price {
        > div {
          margin-right: 20px;
          // line-height: 1.8;
          > span:last-child {
            font-weight: 600;
            font-size: 16px;
          }
        }
      }
      .inp-items {
        > div:first-child {
          margin-right: 30px;
          border-right: 1px dashed #ddd;
          padding-right: 30px;
        }
        > .inp-item {
          flex: 1;
          > .inp-title {
            border-bottom: 1px solid #ddd;
            font-size: 16px;
            justify-content: space-between;
            line-height: 3;
          }
          > .how {
            line-height: 40px;
            cursor: pointer;
          }
          > .three-inp {
            > .inp-box {
              position: relative;
              height: 42px;
              line-height: 40px;
              padding-left: 112px;
              margin-bottom: 15px;
              > span {
                position: absolute;
                width: 112px;
                text-align: center;
                top: 0;
                left: 0;
                background: #eee;
              }
              > div,
              > input {
                display: block;
                width: 100%;
                line-height: 40px;
                padding: 0 20px;
              }
              > div {
                color: #ca4141;
              }
            }
            > div {
              border: 1px solid #ddd;
              > input {
                border-left: 1px solid #ddd;
              }
            }
          }
          .pay-opts {
            flex-wrap: wrap;
            > div:nth-child(n + 2) {
              margin-left: 15px;
            }
            > div {
              //   padding-right: 20px;
              line-height: 40px;
              > input {
                vertical-align: middle;
              }
            }
          }
          > .btn-in,
          .btn-out {
            margin-top: 20px;
            line-height: 42px;
            text-align: center;
            cursor: pointer;
            color: #fff;
          }
          > .btn-in {
            background: #ca4141;
          }
          > .btn-out {
            background: #25796a;
          }
        }
      }
    }
    > .bot {
      .ul-box{
        max-height: 500px;
        padding-bottom:20px;
        overflow-y: scroll;
      }
      .show-detail{
        float: right;
        margin: 0 8px;
        padding: 0 10px;
        background: #d5d6dc;
      }
      > .bot-title {
        margin: 30px 0 0;
        // border-bottom: 1px solid #ccc;
        font-size: 16px;
        line-height: 40px;
        justify-content: space-between;
        align-items: center;
        > div:first-child {
          cursor: pointer;
          span {
            font-weight: 600;
            line-height: 40px;
            margin-right: 20px;
          }
          .active {
            color: #ca4141;
            border: none;
          }
        }
        > .flex {
          height: 17px;
          line-height: 15px;
          cursor: pointer;
          > div {
            margin-right: 10px;
            border: 1px solid #ccc;
            transition: all 0.3s;
            width: 32px;
            border-radius: 7.5px;
            div {
              width: 15px;
              height: 15px;
              border-radius: 50%;
              background: #fff;
            }
          }
          .switch-on {
            padding-left: 0;
            background: #ccc;
          }
          .switch {
            transition: all 0.3s;
            background: #1cb69b;
            padding-left: 15px;
          }
        }
      }
      > .list-title {
        height: 40px;
        //   justify-content: space-between;
        line-height: 40px;
        font-weight: 600;
      }
      .list-title,
      ul li {
        justify-content: space-between;
        cursor: pointer;
        text-align: center;
        padding: 8px 5px;
        line-height: 24px;
        border-top: 1px solid #f3f3f3;
        &:hover {
          background: #f8f8f8;
        }
        > div:first-child {
          width: 7.69%;
          height: 24px;
          text-align: left;
        }
        > div:nth-child(n + 2) {
          width: 25.38%;
        }
        > .last {
          text-align: right;
          .btn-last {
            float: right;
            padding: 0 10px;
            min-width: 55px;
            // max-width: 80px;
            color: #fff;
            margin-left: 5px;

            //   margin-left: 70px;
            text-align: center !important;
            cursor: pointer;
          }
        }
      }

      .ul-out li {
        > div:first-child {
          color: #25796a;
        }

        .btn-last {
          background: #ca4141;
        }
        
      }
      .ul-in li {
        > div:first-child {
          color: #ca4141;
        }

        .btn-last {
          background: #25796a;
        }
      }
      .deny_trade{
        .btn-last {
          cursor: not-allowed !important;
        }
      }
    }
  }
}
#c2c-box > .c2c-r > .bot .ul-out li > div.red{color:#ca4141}
#c2c-box > .c2c-r > .bot .ul-out li > div.blue{color: #25796a}
.blue{color: #25796a}
.upbox{
  position: relative; 
  cursor: pointer;
  .updev{width: 80%;height: 40px;line-height: 40px;color: #fff;background:#ca4141;margin: 0 auto;text-align: center;border-radius: 4px;margin-top: 5px;cursor: pointer; }
  #photo{
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    opacity: 0;
  }
}
.evimg{width: 40px;height: 40px}
.openimg{display: block;margin: 0 auto;max-width: 100%;}
.deny_trade{
  opacity: 0.5;
  cursor: not-allowed !important;
}
</style>
