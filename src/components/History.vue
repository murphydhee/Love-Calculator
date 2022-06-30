<script>
export default {
  data() {
    return {
        historys: [],
    }

  },
  methods: {
    getHistory() {
      this.historys = JSON.parse(localStorage.getItem('resultsArray'));
      console.log(this.historys)
    }
  },
  beforeMount() {
    this.getHistory()
  }
}
</script>

<template>
  <section>
    <p v-for="history in historys" :key="index">{{ history.msg }}</p>
  </section>
</template>