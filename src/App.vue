<template>
  <header>
    <h1 className="text-3xl font-bold md:text-5xl bg-[#6bbbae] p-5 text-white">
      Welcome to <span class="italic">DriveWave</span>
    </h1>
    <h4 className="text-sm md:text-lg">*This is a tech challenge for RoadSurfer</h4>
  </header>

  <main>
    <div class="text-center m-5">
      <AutoComplete @result-selected="selectResult" />
      <WeekView
        :selectedStation="selectedStation"
        :weekDays="weekDays"
        :bookings="bookings"
        @open-booking-details="openBookingDetails"
      />
      <BookingDetailModal
        :selectedBooking="selectedBooking"
        :selectedStation="selectedStation"
        :bookings="bookings"
        @close-booking-details="closeBookingDetails"
      />
    </div>
  </main>
</template>

<script>
import AutoComplete from './components/AutoComplete.vue'
import WeekView from './components/WeekView.vue'
import BookingDetailModal from './components/BookingDetailModal.vue'

export default {
  components: {
    AutoComplete,
    WeekView,
    BookingDetailModal
  },
  data() {
    return {
      inputValue: '',
      results: [],
      showResults: false,
      selectedStation: null,
      bookings: [],
      weekDays: ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'],
      selectedDay: null,
      selectedBooking: null
    }
  },

  methods: {
    selectResult(result) {
      this.selectedStation = result

      const apiBooking = `https://605c94c36d85de00170da8b4.mockapi.io/stations/${result.id}/bookings`

      fetch(apiBooking)
        .then((response) => response.json())
        .then((data) => {
          this.bookings = data
        })
        .catch((error) => {
          console.log('Error fetching booking:', error)
        })

      this.$emit('result-selected', result)
      this.showResults = false
      this.inputValue = ''
    },
    openBookingDetails(booking) {
      this.selectedBooking = booking
    },

    closeBookingDetails() {
      this.selectedBooking = null
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Encode+Sans:wght@300;400;700&display=swap');
</style>
