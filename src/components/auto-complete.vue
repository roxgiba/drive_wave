<template>
  <div>
    <label>Search for your station here: </label>
    <input v-model="inputValue" @input="search" />
    <ul v-if="showResults">
      <li v-for="result in results" :key="result.id" @click="selectResult(result)">
        {{ result.name }}
      </li>
    </ul>

    <div v-if="selectedStation">
      <h2>Bookings for: {{ selectedStation.name }}</h2>
      <ul>
        <li v-for="booking in bookings" :key="booking.id">
          Customer: <span className="text-lg">{{ booking.customerName }}</span> | Start date:
          <span className="text-green-600">{{ booking.startDate.slice(0, 10) }}</span> | End date:
          <span className="text-red-600">{{ booking.endDate.slice(0, 10) }}</span>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      inputValue: '',
      results: [],
      showResults: false,
      selectedStation: '',
      bookings: []
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
      this.selectedStation = result

      const bookingsApi = `https://605c94c36d85de00170da8b4.mockapi.io/stations/${result.id}/bookings`

      fetch(bookingsApi)
        .then((response) => response.json())
        .then((data) => {
          this.bookings = data
        })
        .catch((error) => {
          console.log('Error fetching the booking', error)
        })

      this.$emit('result-selected', result)
      this.showResults = false
      this.inputValue = result.name
    }
  }
}
</script>

<style></style>
