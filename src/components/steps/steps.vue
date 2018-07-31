<template>
  <div class="steps">
    <div class="step" v-for="item in instructPlanSteps" 
        :key="item.sort">
          <div class="step-main">
            <div @mouseleave="stepMouseleave($event)"
                 @mouseenter='stepMouseenter($event)'>
              <div class="left-background-image"></div>
              <div class="step-number-wrapper">
                <p class="step-number">{{item.sort + 1}}</p>
                <div class="step-circle"></div>
              </div>
              <div class="body-background-image">
                <p class="step-title">{{stepText(item)}}</p>
              </div>
              <div class="right-background-image" v-if="maxSort != item.sort"></div>
              <div class="right-background-image-end" v-if="maxSort == item.sort"></div>
              <div class="step-arrow-wrapper" v-if="maxSort != item.sort"></div>
            </div>
            
            <div class="remark-swapper" 
              v-html="item.remark">
            </div>
          </div>
      </div>
  </div>
</template>


<script>
export default {
  name: 'stpes',
  props: {
    instructPlanSteps: Array,
  },
  computed: {
    maxSort (){
      return this.instructPlanSteps.length - 1;
    }
  },
  methods: {
        /**
         *鼠标悬浮状态文字，指定长度换行
        */
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
    },
    /**
     *步骤条显示的文字
     *如果文字前有数组和、要删除
     */
    stepText (item){
      item.planStepDisplayname = item.planStepDisplayname.replace(/^\d、/,'');
      item.planStepDisplayname = item.planStepDisplayname.replace(/[^\u4e00-\u9fa5]$/,'');
      return item.planStepDisplayname
    },
    stepMouseleave (e){
      e.srcElement.parentElement.lastElementChild.style.display = 'none';
    },
    stepMouseenter (e){
      if (e.srcElement.parentElement.lastElementChild.innerHTML != '')
      e.srcElement.parentElement.lastElementChild.style.display = 'block';
    }
  }
}
</script>

<style lang="scss" scoped>
  .steps {

    .step {
      display: inline-block;
      margin-top: 20px;
      font-size: 0;
      .step-main {
        position: relative;
        display: inline-block;
        text-align: center;

        .left-background-image {
          position: relative;
          vertical-align: top;
          display: inline-block;
          width: 10px;
          height: 38px;
          background: url(./../../assets/img/step_footer.png) no-repeat;
          text-align: center;
        }
        .step-number-wrapper {
          position: absolute;
          top: 0;
          left: 3%;
          .step-number {
            width: 16px;
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
          background-image: url(./../../assets/img/step_body.png);
          text-align: center;
          .step-title {
            margin-left: 24px;
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
          background: url(./../../assets/img/step_head.png) no-repeat;
        }
        
        .right-background-image-end {
          display: inline-block;
          width: 34px;
          height: 38px;
          background: url(./../../assets/img/end.png) no-repeat;
        }

        .step-arrow-wrapper {
          display: inline-block;
          width: 30px;
          height: 38px;
          margin-left: -22px;
          margin-right: 6px;
          background: url(./../../assets/img/arrow.png) no-repeat center;
        }

        .remark-swapper {
          position: absolute;
          display: none;
          width: calc(100% - 46px);
          top: 43px;
          padding: 4px 10px;
          text-align: left;
          color: #ffffff;
          font-size: 20px;
          border-radius: 4px;
          background-color: #025cad;
          z-index: 10;
          &:before{
            display: block;
            position: absolute;
            left: calc(50% - 6px);
            top: -6px;
            width: 0;
            height: 0;
            border-left: 8px solid transparent;
            border-right: 8px solid transparent;
            border-bottom: 6px solid #025cad;
            content: "";
          }
        }
      }
    }
  }
</style>

