<template>
  <div>
    <label>Search for your station here: </label>
    <input v-model="inputValue" @input="search" />
    <ul v-if="showResults">
      <li v-for="result in results" :key="result.id" @click="selectResult(result)">
        {{ result.name }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      inputValue: '',
      results: [],
      showResults: false
    }
  },
  methods: {
    search() {
      const api = `https://605c94c36d85de00170da8b4.mockapi.io/stations?search=${this.inputValue}`

      fetch(api)
        .then((response) => response.json())
        .then((data) => {
          this.results = data
          this.showResults = true
        })
        .catch((error) => {
          console.log('Error message:', error)
        })
    },
    selectResult(result) {
      this.$emit('result-selected', result)
      this.showResults = false
    }
  }
}
</script>

<style></style>
