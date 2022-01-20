<template>
<div>
  <p class="lead mb-4" v-if="error">
    Við náum ekki tengingu við kerfin okkar. Vinsamlegast prófaður aftur síðar.
  </p> 
  <p class="lead mb-4" v-else>
    Samkvæmt okkar núverandi skráningu ert þú skráður í <strong>{{ club?.fullName }}</strong> 
    <span v-if="lastCompeted">
      og kepptir seinast {{ competitionDate }}.
      <span v-if="sameYear">
          Þar sem þú hefur keppt fyrir {{ club?.fullName }} á þessu almanaksári þá tekur umsókn þín ekki gildi fyrr en á nýju ári.
      </span>
      <span v-else>
        Þar sem þú hefur ekki keppt fyrir {{ club?.fullName }} á þessu almanaksári þá tekur umsókn þín gildi um leið og þú lýkur við greiðslu í seinasta skrefinu.
      </span>
    </span>
    <span v-else>
      og hefur enga skráða keppni. Umsókn þín tekur því gildi um leið og þú lýkur við greiðslu í seinasta skrefinu.
    </span>
  </p>

  <div class="d-grid gap-2 d-sm-flex justify-content-sm-center">
    <button type="button" class="btn btn-primary btn-lg py-3 px-4 my-3" @click="next" autofocus="autofocus">
      Já passar
    </button>
    <button type="button" class="btn btn-outline-secondary btn-lg py-3 px-4 my-3">Nei það er rangt</button>
  </div>

</div>
</template>

<script>
import { format} from 'date-fns'
import { is } from 'date-fns/locale'
import agent from 'superagent'
const apiUrl = import.meta.env.VITE_FRI_API_URL

export default {
  props: ['data'],
  data() {
    return {
      club: undefined,
      lastCompeted: undefined,
      error: false
    }
  },
  computed: {
    competitionDate() {      
      if (!this.lastCompeted) {
        return ''
      }
      return format(new Date(this.lastCompeted), 'd. MMMM yyyy', { locale: is })
    },
    sameYear() {
      if (!this.lastCompeted) {
        return undefined
      }

      return new Date().getFullYear() === new Date(this.lastCompeted).getFullYear()
    }
  },
  methods: {    
    next() {                 
      this.$emit('next', {
        currentClub: this.club,  
        lastCompeted: this.lastCompeted,
      })
    }
  },
  mounted() {
    const kt = this.data.user?.personalId
    agent
      .get(apiUrl + '/athletes')
      .query({ kt })
      .then(res => {        
        if (res.body.length === 1) {
          this.club = res.body[0].club
          this.lastCompeted = res.body[0].lastCompeted
        }
      })
      .catch(e => {
        this.error = true
      })
  },
};
</script>