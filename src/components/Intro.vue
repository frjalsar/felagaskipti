<template>
<div>

  <p class="lead mb-4">
    Frjálsíþróttasamband Íslands býður þér núna um að sækja um félagaskipti
    á netinu með aðstoð <a href="https://taktikal.is/" target="_blank">Taktial</a>. Þú þarft að hafa rafræn skilríki og kreditkort við höndina.
    Þú skráir þig inn og kerfið leiðir þig í gegnum ferlið. Að lokum skrifar þú undir og greiðir.
  </p>

  <div class="col-sm-12 col-md-9 col-lg-7 col-xl-8 col-xxl-7 mx-auto">
    <DigitInput :inputs="7" :dashes="[4]" @confirmed="next" @digit="setPhoneNumber" :autofocus="true" />
  </div>

  <div class="d-grid gap-2 d-sm-flex justify-content-sm-center">
    <button
      type="button"
      class="btn btn-primary btn-lg py-3 px-4 my-3"
      @click="next"
      :disabled="working">

      <span v-if="!working">Skrá mig inn</span>
      <span v-if="working" class="spinner-border spinner-border-sm" role="status" aria-hidden="true"></span>
    </button>
  </div>
  
  <div>
    <small v-if="working">Beiðni var send í símann</small>
  </div>

</div>
</template>

<script>
import DigitInput from './DigitInput.vue'
// import agent from 'superagent'
export default {
  components: {
    DigitInput
  },
  data() {
    return {
      working: false,
      phoneNumber: '',
    }
  },
  methods: {
    login() {
      this.working = true
      return new Promise((res) => {
        setTimeout(() => res({
          flowKey: "54ab65385c5b",
          name: "Bergur Hallgrímsson",
          phoneNumber: "6908848",
          ssn: "1907834139",
          token: "00ad345646e24ba99637",
        }), 1500)
      })
    },
    next() {
      this.login().then(res => {
        const user = {
          name: res.name,
          phoneNumber: res.phoneNumber,
          personalId: res.ssn,
        }
        this.working = false
        this.$emit('next', { user })
      })
    },
    setPhoneNumber(digit) {
      this.phoneNumber = digit
    }
  },
};
</script>

<style scoped>
button.btn {
  min-width: 160px;

}
</style>