<template>
  <div id="app">
    <div class="steps">
      <div class="step" v-for="item in instructPlanSteps" 
        :key="item.sort"
        
        >
        <el-tooltip popper-class="toolTipClass" visible-arrow="false" class="item" effect="dark"  placement="bottom">
          <div slot="content" v-html="changeStrTT(item.remark, 50)"></div>
          <div class="step-main">
            <div class="left-background-image">
              <p class="step-number">{{item.sort + 1}}</p>
              <div class="step-circle"></div>
            </div>
            <div class="body-background-image">
              <p class="step-title">{{stepText(item)}}</p>
            </div>
            <div class="right-background-image" v-if="maxSort != item.sort"></div>
            <div class="right-background-image-end" v-if="maxSort == item.sort"></div>
            <div class="step-number-wrapper" v-if="maxSort != item.sort"></div>
          </div>
        </el-tooltip>  
      </div>
    </div>
    <div class="div-textare" v-show="readyShow">
      <div class="div-text">
        <h1 class="hearder-h1">执法依据:</h1>
        <p class="div-zhifayiju">{{remark}}</p>
      </div>
      <div class="textare-bg-wrapper">
        <img class="left-top" src="./assets/img/left-top.png" alt="">
        <img class="left-bottom" src="./assets/img/left-bottom.png" alt="">
        <img class="right-top" src="./assets/img/right-top.png" alt="">
        <img class="right-bottom" src="./assets/img/right-bottom.png" alt="">
        <div class="top-line"></div>
        <div class="bottom-line"></div>
        <div class="left-line"></div>
        <div class="right-line"></div>
      </div>
    </div>
  </div>
</template>

<script>


export default {
  name: 'App',
  data() {
    return {
      planId: '',
      instructPlanSteps: [],
      Steps: undefined,
      maxSort: undefined,
      remark: '',
      readyShow: false,
    }
  },
 
  created () {
    // this.planId = callBackObj.getPlanId() //WPF方法传递案件ID
    this.planId = '200101'

    // var typeId = 'a3cc5787-dca9-4b2d-8a60-104b385e6f5b'
    var typeId = "af90ffb3-7b35-4c20-ae06-35857ed9c372"
    this.initStep(typeId)
  },
  methods: {
    findPlanId(){
      this.$axios({
        method: 'get',
        url: ajaxURLms
          + 'api/jp-BSPA-PolSituInfo-ms/polsituinfo/alarmTypeFindPlanId/'
          + this.planId
      }).then((response) => {
        var typeId = response.data[0]
        this.initStep(typeId)//通过接口找到预案ID
      })
    },
    initStep (typeId) {
      this.$axios({
        method: 'get',
        url: ajaxURLms 
          + 'api/jp-BIO-Order-ms/instructions/plans/'
          + typeId
      })
      .then((response) => {
        this.instructPlanSteps = response.data.instructPlanSteps
        this.instructPlanSteps.sort(function (a, b){ //按照‘sort’属性排序
          return a.sort - b.sort
        })
        this.Steps = this.instructPlanSteps.length
        this.maxSort = this.instructPlanSteps.length - 1
        this.remark = response.data.remark //执法依据
        this.readyShow = true //显示执法依据
      })
    },
    stepText (item){
      return item.planStepDisplayname
    },
        //指定长度换行
    changeStrTT (str,len){
        if(str==null||str==""){
            return "";
        }
        if(len==null){
            len=10;
        }
        var result="";
        var curlen=0;
        var patten= /.*[\u4e00-\u9fa5]+.*$/;
        for(var i=0;i<str.length;i++){
            if(patten.test(str[i])){
                curlen+=2;
            }else{
                curlen++;
            }
            if(curlen>=len){
                curlen=0;
                result+="<br/>";

            }
            result+=str[i];
        }
        console.log(result)
        return result;
    }
  },
  watch: {
    planId (val, OldVal){
      this.findPlanId()
    }
  }
}
</script>

<style lang="scss" type="text/css">
body{
  &::-webkit-scrollbar{
    display:none;
  }
}
#app{
  width: 100%;
  height: 100%;
  .div-textare{
    position: relative;
    margin: 4% 20% auto 20%;
    padding: 16px;
    color: #ffffff;
    font-family: 'Microsoft YaHei';
    .div-text {
      padding: 26px 26px 0 60px;
      .hearder-h1 {
        line-height: 24px;
        margin: 0;
        font-size: 24px;
        color: #ffffff;
      }
      .div-zhifayiju {
        line-height: 30px;
        font-size: 16px;
      }
    }
    
    .textare-bg-wrapper {
      position: absolute;
      top: 0;
      width: 100%;
      height: 100%;
      // background: linear-gradient(to right,rgba(54, 110, 164, 0), rgba(54, 110, 164, 0.5), rgba(54, 110, 164, 0)); /* 标准的语法（必须放在最后） */
      .left-top {
        position: absolute;
        left: 0;
        top: 0;
        width: 122px;
        height: 122px;
      }
      .left-bottom {
        position: absolute;
        left: 0;
        bottom: 0;
        width: 122px;
        height: 122px;
      }
      .right-top {
        position: absolute;
        right: 0;
        width: 122px;
        height: 122px;
      }
      .right-bottom {
        position: absolute;
        right: 0;
        bottom: 0;
        width: 122px;
        height: 122px;
      }
      .top-line {
        position: absolute;
        display: block;
        left: 118px;
        top: 0;
        width: calc(100% - 237px);
        border-top: 2px solid rgba(0, 255, 255, 0.4);
      }
      .bottom-line {
        position: absolute;
        display: block;
        left: 120px;
        bottom: 0;
        width: calc(100% - 241px);
        border-top: 2px solid rgba(0, 255, 255, 0.4);
      }
      .left-line {
        position: absolute;
        display: block;
        left: 21px;
        top: 118px;
        height: calc(100% - 238px);
        border-left: 2px solid rgba(0, 255, 255, 0.4);
      }
      .right-line {
        position: absolute;
        display: block;
        right: 30px;
        top: 119px;
        height: calc(100% - 240px);
        border-left: 2px solid rgba(0, 255, 255, 0.4);
      }
    }
  }
  .steps {

    .step {
      display: inline-block;
      margin-top: 20px;
      font-size: 0;
      .step-main {
        display: inline-block;
        text-align: center;

        .left-background-image {
          position: relative;
          vertical-align: top;
          display: inline-block;
          width: 34px;
          height: 38px;
          background: url(./assets/img/left.png) no-repeat;
          text-align: center;

          .step-number {
            margin: 0;
            line-height: 38px;
            font-size: 16px;
            color: #072540;
          }

          .step-circle {
            position: absolute;
            top: 0;
            bottom: 0;
            left: 0;
            right: 0;
            margin: auto;
            width: 20px;
            height: 20px;
            background: rgb(0, 255, 255);
            -moz-border-radius: 50px;
            -webkit-border-radius: 50px;
            border-radius: 50px;
            z-index: -1;
          }
        }

        .body-background-image {
          vertical-align: top;
          display: inline-block;
          height: 38px;
          background-image: url(./assets/img/body.png);
          text-align: center;
          .step-title {
            margin: 0;
            line-height: 38px;
            font-size: 16px;
            font-family: 'Microsoft YaHei';
            color: #ffffff;
            &:hover {
              color: #00ffff;
            }
          }
        }

        .right-background-image {
          vertical-align: top;
          display: inline-block;
          width: 34px;
          height: 38px;
          background: url(./assets/img/right.png) no-repeat;
        }
        .right-background-image-end {
          display: inline-block;
          width: 34px;
          height: 38px;
          margin-left: -1px;
          background: url(./assets/img/end.png) no-repeat;
        }
        .step-number-wrapper {
          display: inline-block;
          width: 30px;
          height: 38px;
          margin-left: -10px;
          background: url(./assets/img/arrow.png) no-repeat center;
        }
      }
    }
  }
}

.toolTipClass {
  background-color: #025cad !important;
  font-size: 20px;
}



</style>
