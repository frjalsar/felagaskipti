<template>
  <main class="container">
    <div class="px-4 py-5 my-sm-5 text-center">
      <img
        class="d-block mx-auto mb-4"
        src="./assets/logo.png"
        alt="FRI"
        width="100"
        height="90"
      />
      <h1 class="display-5 fw-bold mb-4">{{ title }}</h1>
      <div class="col-lg-12 col-xl-10 mx-auto">
        <component :is="steps[step]" :data="data" @next="next" />
      </div>
    </div>
  </main>
</template>

<script>
import Intro from "./components/Intro.vue";
import Greeting from "./components/Greeting.vue";
import Select from "./components/Select.vue";
import Confirm from "./components/Confirm.vue";

export default {
  components: {
    Intro,
    Greeting,
    Select,
    Confirm,
  },
  provide: {
    FRI_API_URL: import.meta.env.VITE_FRI_API_URL,
    FRI_API_TOKEN: import.meta.env.VITE_FRI_API_TOKEN,    
  },
  data() {
    return {
      data: undefined,
      step: 0,
      steps: ['Intro', 'Greeting', 'Select', 'Confirm']
    };
  },
  computed: {
    title() {
      const generalTitles = ['Félagaskipti', 'Hæ', 'Veldu félag', 'Undirritun']

      if (this.step === 1) {
        const session = JSON.parse(sessionStorage.getItem("FRI_FELAGASKIPTI"))      
        if (session && session.user) {
          const name = session.user.name.split(' ').slice(0, -1).join(' ')
          return 'Hæ ' + name
        }
      }

      return generalTitles[this.step]
    },
  },
  methods: {
    updateSession(newData) {
      const oldData = JSON.parse(sessionStorage.getItem("FRI_FELAGASKIPTI"));
      this.data = {
        ...oldData,
        ...newData,
        step: this.step,
      };

      sessionStorage.setItem("FRI_FELAGASKIPTI", JSON.stringify(this.data));
    },
    next(data) {
      this.step = this.step + 1;
      this.updateSession(data);
    },
  },
  created() {
    this.data = JSON.parse(sessionStorage.getItem("FRI_FELAGASKIPTI"));
    this.step = this.data ? this.data.step : 0;
  },
};
</script>

<style>
</style>
