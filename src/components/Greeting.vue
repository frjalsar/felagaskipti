<template>
  <div class="px-4 py-5 my-1 text-center">
    <img
      class="d-block mx-auto mb-8"
      src="../assets/logo.png"
      alt="FRI"
      width="75"
      height="67"
    />
    <h1 class="display-5 fw-bold my-4">Hæ {{ name }}</h1>
    <div class="col-lg-12 col-xl-10 mx-auto">
      <p class="lead mb-4">
        Samkvæmt okkar núverandi skráningu ert þú skráður í <br />
        <strong>{{ club?.fullName }}</strong>
      </p>
      <div class="d-grid gap-2 d-sm-flex justify-content-sm-center">
        <button type="button" class="btn btn-primary btn-lg py-3 px-4 my-3" @click="next" autofocus="autofocus">
          Já passar
        </button>
        <button type="button" class="btn btn-outline-secondary btn-lg py-3 px-4 my-3">Nei það er rangt</button>
      </div>
    </div>
  </div>
</template>

<script>
import agent from 'superagent'

export default {
  props: ['data'],
  data() {
    return {
      club: undefined      
    }
  },
  computed: {
    name() {      
      return this.data.user?.name.split(' ').slice(0, -1).join(' ')
    }
  },
  methods: {
    next() {                 
      this.$emit('next', { currentClub: this.club })
    }
  },
  mounted() {
    const kt = this.data.user?.personalId
    agent
      .get('http://local.fri.is:3000/athletes')
      .query({ kt })
      .then(res => {        
        if (res.body.length === 1) {
          this.club = res.body[0].club           
        }
      })
  },
};
</script>