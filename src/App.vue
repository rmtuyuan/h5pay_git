<template>
  <div id="app">
    <div class="id-box">
      <div>
        <h4>充值账户:</h4>
        <el-row align="center" type="flex">
          <el-col :span="18">
            <el-input class="inputstyle" onkeypress="return event.keyCode>=48&&event.keyCode<=57" v-model="id" type="tel" v-if="idCheck"
              placeholder="请输入全民ID" round></el-input>

            <div class="container app-family-list-con clearfloat" v-if="idChange">
              <div class="app-family-left"> <img src="http://qmwlzb-one.oss-cn-beijing.aliyuncs.com/img_1509602356819.jpg" alt=""></div>
              <div class="app-family-mid">
                <p>龙天下</p><span>ID1</span></div>

            </div>

          </el-col>
          <el-col :span="6">
            <el-button type="" v-if="idCheck" @click="handleIDCheck" round>确认</el-button>
            <el-button type="primary" v-if="idChange" @click="handleIDChange" round>切换账户</el-button>
          </el-col>
        </el-row>
      </div>
    </div>
    <div class="momey-list-box">
      <div class="momey-list">
        <el-row class="momey-list-row">
          <el-col :span="8" v-for="item in moneyList" :key="item.id">
            <div class="momey-list-col" :class="{active:item.id===moneyId}" @click="checkMoney(item.id,item.rmb)">
              <span v-text="item.id?item.diamond+'钻石':'自定义'"></span>
              <p v-if="item.id">￥{{item.rmb}}</p>
            </div>
          </el-col>

        </el-row>
        <el-row class="momey-list-row" v-if="money==0||moneyId==0">
          <el-col :span="6">
            <el-button type="text"><span class="text-gray">充值金额:</span></el-button>
          </el-col>
          <el-col :span="8" class="money-input">
            <el-input :maxlength="8" onkeypress="return event.keyCode>=48&&event.keyCode<=57" v-model.number="money" @keyup.native="moneyInput"
              type="tel" placeholder="请输入金额" round></el-input>
          </el-col>
          <el-col :span="2">
            <el-button type="text"><span class="text-gray">元</span></el-button>
          </el-col>
        </el-row>
        <el-row>
          <el-col :span="6">
            <el-button type="text"><span class="text-gray">应付金额:</span></el-button>
          </el-col>

          <el-col :span="2">
            <el-button type="text">{{money}} <span class="text-gray">元</span></el-button>
          </el-col>
        </el-row>
      </div>
    </div>
    <div class="pay-way-box">
      <div class="pay-way-box-list">
        <div class="pay-way">
          <el-row class="pay-way-list">
            <el-col :span="4" class="pay-way-img-box">
              <img src="./assets/weixin.png" alt="">
            </el-col>
            <el-col :span="16" class="pay-way-title">
              <dl>
                <dt>微信支付</dt>
                <dd>微信安全支付</dd>
              </dl>
            </el-col>
            <el-col :span="4" class="radio-box">

              <label class="demo--label">
                   <input class="demo--radio erroed af-input" checked type="radio" value="" name="gender">
                   <span class="demo--radioInput" ></span>
              </label>
            </el-col>
          </el-row>
        </div>
      </div>
      <div class="pay-way-box-list">
        <div class="pay-way">
          <el-row class="pay-way-list">
            <el-col :span="4" class="pay-way-img-box">
              <img src="./assets/zhifubao.png" alt="">
            </el-col>
            <el-col :span="16" class="pay-way-title">
              <dl>
                <dt>支付宝支付</dt>
                <dd>支付宝安全支付</dd>
              </dl>
            </el-col>
            <el-col :span="4" class="radio-box">

              <label class="demo--label">
                   <input class="demo--radio erroed af-input" type="radio" value="" name="gender">
                   <span class="demo--radioInput" ></span>
              </label>
            </el-col>
          </el-row>
        </div>
      </div>
    </div>
    <el-row>
      <el-col :span="24">
        <el-button type="" @click="open" :class="{active:money>0&&idChange}" class="submit-button">提交订单 ￥{{money}}</el-button>
      </el-col>
    </el-row>
    <el-row>
      <el-col :span="24">
        <p class="tel">充值如有问题请联系客服官方电话: <a href="tel:4001791118">400-179-1118</a> </p>
      </el-col>
    </el-row>


  </div>
</template>

<script>
  export default {
    name: 'app',
    data() {
      return {
        id: '', //userId
        wxpay: 1,
        zfbpay: 2,
        moneyId: 2, //moneyid
        idCheck: true, //
        idChange: false,
        money: 60, //money
        moneyList: [{
            id: 1,
            diamond: '60',
            rmb: '6'
          },
          {
            id: 2,
            diamond: '600',
            rmb: '60'
          },
          {
            id: 3,
            diamond: '6000',
            rmb: '600'
          },
          {
            id: 4,
            diamond: '60000',
            rmb: '6000'
          },
          {
            id: 0,
            diamond: '0',
            rmb: '0'
          }
        ],

      }

    },
    methods: {
      open: function () {
        if (this.idChange) {
          if (this.money > 0) {
            this.$confirm('确定充值', '', {
              confirmButtonText: '确定',
              $message: ({
                type: 'info'
              }),
              showCancelButton: false

            })
          } else {
            this.$message({
              message: '请选择或自定义充值金额',
              type: 'error',
              duration: 1000
            });
          }

        } else {
          this.$message({
            message: '请确认全民ID',
            type: 'error',
            duration: 1000
          });
        }

      },
      //切换账号
      handleIDChange() {
        //
        this.idChange = !this.idChange;
        this.idCheck = !this.idCheck
      },
      // 确认账号
      handleIDCheck() {
        let userID = this.id;
        let idReg = /(^[1-9]\d*$)/;
        if (userID.length > 0) {
          if (idReg.test(userID)) {
            //$http 成功赋值 切换 
            this.idChange = !this.idChange;
            this.idCheck = !this.idCheck
          } else {
            //不是正整数
            this.$message({
              message: '请输入正确全民ID',
              type: 'error',
              duration: 1000
            });
          }
        } else {
          this.$message({
            message: '请输入全民ID',
            type: 'error',
            duration: 1000
          });
        }
      },
      //选择套餐 
      checkMoney(id, rmb) {
        this.moneyId = id;
        this.money = rmb;
      },
      //自定义输入金额
      moneyInput() {


      }

    }
  }

</script>

<style lang="scss">
  // $base-color:#fe416e;
  html,
  p {
    padding: 0;
    margin: 0;
  }

  body {
    margin: 0;
    padding: 0;
    width: 7.5rem;
    background: #f0f0f0;
    font-size: .16rem;
  }

  .id-box {
    background: #fff;
    width: 100%;
    margin: .2rem auto;
    padding: .2rem 0;
  }

  .id-box button.el-button.is-round {
    width: 80%;
    padding: 12px 0;
    text-align: center;
  }

  .id-box>div,
  .momey-list {
    width: 96%;
    margin: 0 auto;
  }

  .id-box h4 {
    font-size: .26rem;
    padding: 0;
    margin: 0;
    text-align: left;
    margin-bottom: .2rem;
  }

  .el-message {
    min-width: 80%;
    width: 80%;
  }

  .id-box button:focus {
    background: #fff;
  }

  .app-family-list-con {
    overflow: hidden;
    height: 100%;
    display: flex;
    align-items: flex-end;
  }

  .app-family-left {
    width: 44px;

    text-align: center;
    display: flex;
    display: -webkit-box;
    /* 老版本语法: Safari, iOS, Android browser, older WebKit browsers. */
    display: -moz-box;
    /* 老版本语法: Firefox (buggy) */
    display: -ms-flexbox;
    /* 混合版本语法: IE 10 */
    display: -webkit-flex;
    /* 新版本语法: Chrome 21+ */
    flex-direction: column;
    -moz-flex-direction: column;
    -webkit-flex-direction: column;
  }

  .app-family-icon {
    display: block;
    width: 24px;
    height: 16px;
  }

  .app-family-list-con {
    position: relative;
  }

  .app-family-list-con>div {
    float: left;
  }

  .app-family-left img {
    display: block;
    width: 41px;
    height: 41px;
    border-radius: 50%;
  }

  .app-family-mid {

    height: 41px;
    line-height: 0;
    margin-left: .1rem;

    text-align: left;
  }

  .text-gray {
    color: #666;
  }

  .text-red {
    color: #fe416e;
  }

  .app-family-mid p {
    width: 100%;
    margin: 0;
    font-size: .28rem;
    color: rgb(64, 64, 64);
    margin-bottom: .35rem;
    margin-top: .2rem;
    font-weight: 600;
  }

  .app-family-mid span {
    font-size: .24rem;
    color: #999;
  }

  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    width: 100%;
    margin: 0 auto;

    /*margin-top: 60px;*/
  }

  .el-message-box {
    width: 80%; // background: #fe416e;
  }

  .momey-list-box {
    margin-top: .2rem;
    background: #fff;
    padding: .2rem 0;
  }

  .momey-list-col {
    width: 85%;
    height: .88rem;
    color: #fe416e;
    border-radius: .44rem;
    font-size: .28rem;
    text-align: center;
    margin: 0;
    border: 1px solid #fe416e;
    padding: 0;
    display: flex;
    display: -webkit-box;
    /* 老版本语法: Safari, iOS, Android browser, older WebKit browsers. */
    display: -moz-box;
    /* 老版本语法: Firefox (buggy) */
    display: -ms-flexbox;
    /* 混合版本语法: IE 10 */
    display: -webkit-flex;
    /* 新版本语法: Chrome 21+ */
    align-items: center;
    justify-content: center;
    -webkit-box-align: center;
    -moz-align-items: center;
    -webkit-align-items: center;
    flex-direction: column;
    -moz-flex-direction: column;
    -webkit-flex-direction: column;

    -webkit-box-pack: center;
    -moz-justify-content: center;
    -webkit-justify-content: center;
    margin: .1rem 0;
  }

  .momey-list-col p {
    color: #ff7998;
    font-size: .24rem;
  }

  span {
    font-size: .28rem;
  }

  .momey-list-col.active {
    background: #fe416e;
    color: #fff;
    border: 1px solid #fe416e;
  }

  .momey-list-col.active p {
    color: #fff;
    font-size: .2rem;
  }

  .momey-list-row {
    margin: .3rem 0;
  }

  .submit-button {
    width: 90%;
    margin: .3rem;
    font-size: .36rem;
    box-sizing: border-box;
  }

  .submit-button {
    background: #ccc;
    border-radius: .4rem;
  }

  .submit-button:focus ,  .submit-button:hover{
    background-color: #ccc;
    border-radius: .4rem;
    color: #666;
    border-color:  #ccc;
  }

  .submit-button.active {
    background: #fe416e;
    color: #fff;
  }

  .inputstyle {
    height: 100%;
    min-height: 44px;
  }

  .inputstyle input {
    height: 100%;
    font-size: .24rem;
  }

  .money-input {
    height: 100%;
  }

  .money-input input {
    min-height: 40px;
    font-size: .28rem;
  } //选择方式 
  .pay-way-box {
    margin-top: .2rem;
    background: #fff; // height: 300px;
  }

  .pay-way {

    width: 96%;
    margin: 0 auto;
  }

  .pay-way-box-list:last-child {
    border-top: 1px solid #f0f0f0;
  }

  .pay-way-title {
    text-align: left;
  }

  .pay-way-title dl {

    color: #333;
    font-size: .32rem;
    margin: 0;
    padding: 0;
  }

  .pay-way-title dd {
    margin: 0;
    padding: 0;
    color: #000;
    font-size: .2rem;
    opacity: 0.3;
  }

  .pay-way-img-box img {
    height: .6rem;
    width: .66rem;
  }

  .pay-way-list {
    padding: .15rem 0;
  }
  /*单选按钮*/

  .radio-box {
    text-align: right;
  }

  .demo--label {

    display: inline-block;
    margin-top: .18rem;
  }

  .demo--radio {
    display: none
  }

  .demo--radioInput {
    background: url("./assets/unchecked.png") no-repeat;
    background-color: #fff;
    background-size: 100% 100%;
    border-radius: 100%;
    display: inline-block;
    height: .57rem;
    vertical-align: middle;
    width: .6rem;
    line-height: 1
  }

  .demo--radio:checked+.demo--radioInput:after {
    background: url("./assets/checked.png") no-repeat;
    background-size: 100% 100%;
    border-radius: 100%;
    content: "";
    display: inline-block;
    height: 100%;
    width: 100%;
  }

  .demo--checkbox.demo--radioInput,
  .demo--radio:checked+.demo--checkbox.demo--radioInput:after {
    border-radius: 0
  }

  .tel {
    color: #999;
    padding-bottom: .15rem;
  }

  .tel a {
    color: #fe416e;
  }

</style>
