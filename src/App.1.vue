<template>
  <div id="app" >
    <el-steps :active="instructPlanSteps.length" simple align-center>
      <el-step v-for="item in instructPlanSteps" 
        :title="stepText(item)"  
        :key="item.sort"
        :description="item.remark"
      >
      </el-step>
    </el-steps>
  </div>
</template>

<script>


export default {
  name: 'App',
  data() {
    return {
      typeId: 'a0a541f9-887b-4f53-8434-7154252189fe',
      instructPlanSteps: []
    }
  },
  components: {
    
  },
  created () {
    this.initStep ()
  },
  methods: {
    initStep () {
      this.$axios({
        method: 'get',
        url: this.ajaxURLms 
          + 'api/jp-BIO-Order-ms/instructions/plans/'
          + this.typeId
      })
      .then((response) => {
        this.instructPlanSteps = response.data.instructPlanSteps
        this.instructPlanSteps.sort(function (a, b){ //按照‘sort’属性排序
          return a.sort - b.sort
        })
        console.log(this.instructPlanSteps)
      })
    },
    stepText (item){
      return '步骤'+(item.sort+1)+': '+item.planStepDisplayname
    }
  }
}
</script>

<style lang="scss" type="text/css">

</style>
