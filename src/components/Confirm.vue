<template>
<div>

  <p class="lead mb-4">
    Nú er hægt að hefja undirritun. Fyrst skaltu samt athuga hvort það sé rétt að þú ætlar að skipta frá <strong>{{ currentClub?.fullName }}</strong>
    yfir í <strong>{{ selectedClub?.fullName }}</strong>
  </p>

  <p class="lead mb-4">
    Formaður aðalstjórrnar, eða frjálsíþróttadeildar núverandi félags þarf líka að skrifa undir.<br />
    Skráðu inn símanúmerið hans.
  </p>

  <div class="col-sm-12 col-md-9 col-lg-7 col-xl-8 col-xxl-7 mx-auto mb-4">
    <DigitInput :inputs="7" :dashes="[4]" @confirmed="next" @digit="setPhoneNumber" />
  </div>

  <p class="lead mb-4">
    Þegar þú ýtir á <em>"Skrifa undir"</em> þá sendist undirritunarbeiðni á þig og símanúmerið sem þú gafst upp.
  </p>

  <div class="d-grid gap-2 d-sm-flex justify-content-sm-center">
    <button type="button" class="btn btn-primary btn-lg py-3 px-4 my-3" @click="next" autofocus="autofocus" :disabled="!selectedClub">
      Skrifa undir
    </button>
    <button type="button" class="btn btn-outline-secondary btn-lg py-3 px-4 my-3" @click="back">Til baka</button>
  </div>

</div>
</template>

<script>
import DigitInput from './DigitInput.vue'
export default {
  components: {
    DigitInput,
  },
  props: ['data'],
  data() {
    return {
      phoneNumber: '',
      currentClub: undefined,
      selectedClub: undefined
    }
  },  
  methods: {
    setPhoneNumber(nr) {
      this.phoneNumber = nr
    },
    next() {
      
    }
  },
  mounted() {
    this.currentClub = this.data.currentClub
    this.selectedClub = this.data.selectedClub
  },
};
</script>