<template>
  <div class="row">
    <div class="col g-1" v-for="nr in inputs" :key="nr">
      <input type="text" class="form-control form-control-lg" maxlength="1" @keydown.prevent="(evt) => keyHandler(evt, nr)" :ref="'DigitInput-' + nr"/>    
    </div>
  </div>
</template>

<script>
export default {
  name: 'DigitInput',
  props: ['inputs', 'dashes', 'initialFocus'],
  data() {
    return {
      digit: ''
    }
  },
  methods: {
    keyHandler(evt, nr) {      
      const currEl = this.$refs['DigitInput-' + nr]
      const nextEl = this.$refs['DigitInput-' + (nr + 1)]
      const prevEl = this.$refs['DigitInput-' + (nr - 1)]

      if (evt.key >0 || evt.key <= 9) {
        currEl[0].value = evt.key
        this.digit = this.digit + evt.key
        this.$emit('digit', this.digit)

        if (nextEl && nextEl.length === 1) {          
          nextEl[0].focus()
          nextEl[0].select()
        }
      }      

      if (evt.key === 'Backspace') {        
        currEl[0].value = ''
        this.digit = this.digit.slice(0, -1)
        this.$emit('digit', this.digit)

        if (prevEl && prevEl.length === 1 && currEl[0].value === '') {          
          prevEl[0].focus()
          prevEl[0].select()          
        }
      }

      if (evt.key === 'Enter' || evt.key === 'Space') {
        this.$emit('confirmed', this.digit)
      }
    },
  },
  computed: {
    inputWidth() {
      return 100 / this.inputs
    }    
  },
  mounted() {
    const firstEl = this.$refs['DigitInput-1']
    firstEl[0].focus()
    firstEl[0].select()
  }
}
</script>

<style scoped>
.digitInputWrapper {
  width: 500px;
}

input.form-control-lg {
  height: 100px;
  font-size: 3rem;
  text-align: center;
}
</style>
