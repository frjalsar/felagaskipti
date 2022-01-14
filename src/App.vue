<template>
  <main class="container">
   <component :is="steps[step]" :data="data" @next="next" />
  </main>
</template>

<script>
import Intro from './components/Intro.vue'
import Greeting from './components/Greeting.vue'
import Select from './components/Select.vue'
import Confirm from './components/Confirm.vue'

export default {
  components: {
    Intro,
    Greeting,
    Select,
    Confirm
  },
  data() {
    return {  
      data: undefined,          
      step: 0,
      steps: ['Intro', 'Greeting', 'Select', 'Confirm']
    }
  },
  methods: {
    updateSession(newData) {
      const oldData = JSON.parse(sessionStorage.getItem('FRI_FELAGASKIPTI'))
      this.data = {
        ...oldData,
        ...newData,
        step: this.step,
      }      

      sessionStorage.setItem('FRI_FELAGASKIPTI', JSON.stringify(this.data))
    },
    next(data) {            
      this.step = this.step + 1
      this.updateSession(data)      
    },
  },
  created() {
    this.data = JSON.parse(sessionStorage.getItem('FRI_FELAGASKIPTI'))
    this.step = this.data ? this.data.step : 0
  } 
};
</script>

<style>
</style>
