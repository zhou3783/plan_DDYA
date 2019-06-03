<template>
  <div class="dispatch-plan">
    <div class="box_one">
      <div class="title_one">
      </div>
      <div class="item-wrapper">
        <steps
          :instructPlanSteps = instructPlanSteps
        ></steps>
      </div>
    </div>

  </div>
</template>


<script>
import steps from '@/components/steps/steps'

export default {
  name: 'dispatchPlan',
  data() {
    return {
      planId: '',
      instructPlanSteps: [
        {
          id: "901f806a-a00a-4dd1-bf6d-bd98fcfbed1367",
          instructPlanStepFields: [],
          isDefault: null,
          detailedDescription: "预案编制中",
          simpleDescription: "预案编制中",
          stepOrder: 0
        }
      ],
      Steps: undefined,
      maxSort: undefined,
      remark: '',
      readyShow: false,
    }
  },
  created () {
    //WPF URL alarmType取出planId
    this.getURL ()
  },
  methods: {

    initStep (typeId) {
      this.$axios({
        method: 'get',
        url: baseURL
          + '/api/jp-HCZZ-AdminWeb-app-ms/benchmark/info?pageIndex=1&pageSize=1&category=DDYA&natureDetails='
          + typeId
      })
        .then(res => {
          if(res.data.data.length <= 0){
            return
          }
          this.instructPlanSteps = res.data.data[0].benchmarkStepModelList
          this.instructPlanSteps.sort(function (a, b){ // 按照‘sort’属性排序
            return a.stepOrder - b.stepOrder
          })
          this.Steps = this.instructPlanSteps.length
        })
        .catch( err => {
          if(isAlert){
            alert('typeId')
          }
          this.instructPlanSteps = [
            {
              id: "901f806a-a00a-4dd1-bf6d-bd98fcfbed1367",
              instructPlanStepFields: [],
              isDefault: null,
              planStepCode: "ddpcsjwz",
              simpleDescription: "预案编制中",
              detailedDescription: "预案编制中",
              stepOrder:0
            }
          ];
        })
    },
    getURL (){
      this.planId = this.getQueryString('alarmType')
      this.initStep(this.planId);
      if(isAlert) {
        alert('planId'+this.planId)
        console.log(window.location.search)
      }
    },
    getQueryString (name){
      var href = window.location.href.split('?')[1];
      var reg = new RegExp("(^|&)"+ name +"=([^&]*)(&|$)");
      var r = href.match(reg);
      if (r!=null) return r[2]; return '';
    }
  },
  watch: {
    planId (val, OldVal){
      this.initStep(this.planId)
    }
  },
  components: {
    steps
  }
}
</script>

<style lang="scss" scoped>
  .dispatch-plan {

  }
</style>

