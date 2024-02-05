<template>
  <div class="booking-detail">
    <h2>Booking Details</h2>
    <p><strong>Customer Name:</strong> {{ booking.customerName }}</p>
    <p><strong>Booking Start Date:</strong> {{ booking.startDate }}</p>
    <p><strong>Booking End Date:</strong> {{ booking.endDate }}</p>
    <p>
      <strong>Booking Duration:</strong> {{ calculateDuration(booking.startDate, booking.endDate) }}
    </p>
    <p><strong>Pickup-Return Station Name:</strong> {{ selectedStation.name }}</p>

    <button @click="returnToCalendar">Return to Calendar View</button>
  </div>
</template>

<script>
export default {
  props: {
    booking: Object, // Pass the booking details as a prop
    selectedStation: Object // Pass the selected station as a prop
  },
  methods: {
    calculateDuration(startDate, endDate) {
      const start = new Date(startDate)
      const end = new Date(endDate)

      const durationInMilliseconds = end - start
      const durationInDays = durationInMilliseconds / (1000 * 60 * 60 * 24)

      return `${Math.floor(durationInDays)} days`
    },
    returnToCalendar() {
      // Emit an event to signal returning to the calendar view
      this.$emit('return-to-calendar')
    }
  }
}
</script>

<style>
/* Add styling as needed */
</style>
