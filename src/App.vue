<template>
  <div id="app">
    <dispatch-plan></dispatch-plan>
  </div>
</template>

<script>
import dispatchPlan from '@/components/dispatchPlan/dispatchPlan'

export default {
  name: 'App',
  data() {
    return {
      
    }
  },
  components: {
    dispatchPlan
  },
  created () {
    
  },
  methods: {
   
  },
  watch: {
    
  }
}
</script>

<style lang="scss" type="text/css">
@import './assets/css/reset.css';
body{
  &::-webkit-scrollbar{
    display:none;
  }
}
#app{
  width: 100%;
  height: 100%;
}
</style>
