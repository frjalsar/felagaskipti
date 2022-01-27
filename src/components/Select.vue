<template>
<div>
   
  <p class="lead mb-4">
    Glæsilegt, núna getur þú valið þér nýtt félag sem þú vilt skipta yfir í.
  </p>
      
  <div class="col-sm-12 col-md-9 col-lg-7 col-xl-8 col-xxl-7 mx-auto">
    <select class="form-select form-select-lg mb-3" v-model="selectedClub" aria-label="Veldu nýtt félag" autofocus="autofocus">
      <option></option>
      <option v-for="club in clubs" :key="club.id" :value="club">{{ club.fullName }}</option>
    </select>
  </div>

  <div class="d-grid gap-2 d-sm-flex justify-content-sm-center">
    <button type="button" class="btn btn-primary btn-lg py-3 px-4 my-3" @click="next">
      Velja félag
    </button>
    <button type="button" class="btn btn-outline-secondary btn-lg py-3 px-4 my-3">Fara til baka</button>
  </div>

</div>
</template>

<script>
import agent from 'superagent'

export default {
  props: ['data'],
  inject: ['FRI_API_URL', 'FRI_API_TOKEN'],
  data() {
    return {
      selectedClub: undefined,
      clubs: []
    }
  },
  methods: { 
    next() {
      this.$emit('next', { selectedClub: this.selectedClub })
    }
  },
  mounted() {
    const kt = this.data?.personalId
    agent
      .get(this.FRI_API_URL + '/clubs')
      .auth(this.FRI_API_TOKEN, { type: 'bearer' })      
      .query({ kt })
      .then(res => {
        this.clubs = res.body
      })
  }
};
</script>