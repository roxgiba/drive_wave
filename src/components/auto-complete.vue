<template>
  <div id="search">
    <label class="text-base md:text-xl">Search for your station here: </label>

    <!-- INPUT FIELD  -->
    <div class="input-container p-5">
      <input v-model="inputValue" @input="search" className="border-2 border-indigo-600 rounded" />
      <span
        v-if="inputValue"
        class="clear-icon"
        @click="clearInput"
        className="text-red-600 text-lg p-1 font-bold"
        >x</span
      >
    </div>

    <!--  RESULTS  -->
    <ul v-if="showResults">
      <li v-if="isLoading" class="text-slate-500">Loading...</li>
      <li v-for="result in results" :key="result.id" @click="selectResult(result)">
        {{ result.name }}
      </li>
    </ul>

    <!-- <div v-if="selectedStation">
      <h2 className=" text-base md:text-2xl font-bold pb-2">
        Bookings for: {{ selectedStation.name }}
      </h2>
      <ul>
        <li v-if="isLoading">Loading...</li>
        <li v-for="booking in bookings" :key="booking.id">
          Customer: <span className="text-lg">{{ booking.customerName }}</span> | Start date:
          <span className="text-green-600">{{ booking.startDate.slice(0, 10) }}</span> | End date:
          <span className="text-red-600">{{ booking.endDate.slice(0, 10) }}</span> | Duration:
          <span className="text-blue-600">{{
            calculateDuration(booking.startDate, booking.endDate)
          }}</span>
          | Return station:
          <span className="text-orange-600">{{ selectedStation.name }}</span>
        </li>
      </ul>
    </div> -->

    <!-- WEEK VIEW -->
    <div v-if="selectedStation">
      <h2 class="text-base md:text-2xl font-bold pb-2">
        Week View for: {{ selectedStation.name }}
      </h2>
      <div class="week-view">
        <div v-for="day in weekDays" :key="day" class="day-tile">
          <h3 className="font-bold text-lg">{{ day }}</h3>
          <ul>
            <li v-for="booking in getBookingsForDay(day)" :key="booking.id">
              {{ booking.customerName }}
            </li>
          </ul>
        </div>
      </div>
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
      bookings: [],

      // LOADING STATE
      isLoading: false,

      // WEEK

      weekDays: ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'],
      selectedDay: null
    }
  },

  methods: {
    search() {
      this.isLoading = true

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
        .finally(() => {
          this.isLoading = false
        })
    },
    clearInput() {
      this.inputValue = ''
      this.selectedStation = ''
    },

    calculateDuration(startDate, endDate) {
      const start = new Date(startDate)
      const end = new Date(endDate)

      const durationInMilliseconds = end - start
      const durationInDays = durationInMilliseconds / (1000 * 60 * 60 * 24)

      return `${Math.floor(durationInDays)} days`
    },

    getBookingsForDay(day) {
      // Return the bookings for the selected day
      return this.bookings.filter(
        (booking) => new Date(booking.startDate).getDay() === this.weekDays.indexOf(day)
      )
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

<style>
.week-view {
  display: flex;
  justify-content: space-between;
}

.day-tile {
  border: 1px solid #ccc;
  padding: 10px;
  margin: 5px;
}

.day-tile h3 {
  text-align: center;
  margin-bottom: 5px;
}

.day-tile ul {
  list-style: none;
  padding: 0;
}

.day-tile li {
  margin-bottom: 5px;
}
</style>
