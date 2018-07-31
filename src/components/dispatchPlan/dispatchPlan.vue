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
      instructPlanSteps: [],
      Steps: undefined,
      maxSort: undefined,
      remark: '',
      readyShow: false,
    }
  },
  created () {
    // this.planId = callBackObj.getPlanId() //WPF方法传递案件ID
    var typeId = "3863a5cf-2cbe-4a7c-a428-de45fa4ee0f133"
    this.initStep(typeId)

    // this.planId = '900199'
    // this.findPlanId()
  },
  methods: {
   findPlanId(){
      /**
      *警情类型id去找预案id
      *swagger 地址：http://172.17.99.30:10014/api/jp-BSPA-PolSituInfo-ms/swagger-ui.html#!/t45pa45alarm45type45controller/getDispatchFindPlanIdUsingGET
      */
      this.$axios({
        method: 'get',
        url: ajaxURLms
          + 'api/jp-BSPA-PolSituInfo-ms/polsituinfo/dispatchFindPlanId/'
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
        // this.remark = response.data.remark //执法依据
        // this.readyShow = true //显示执法依据
      })
    }
  },
  watch: {
    planId (val, OldVal){
      this.findPlanId()
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

