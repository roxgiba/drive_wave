<template>
  <header>
    <h1 className="text-3xl font-bold md:text-5xl bg-[#6bbbae] p-5 text-white">
      Welcome to <span class="italic">DriveWave</span>
    </h1>
  </header>

  <main>
    <h4 className="text-sm md:text-lg">*This is a tech challenge for RoadSurfer</h4>
    <auto-complete @station-selected="handleStationSelected" className="mt-36" />
  </main>
</template>

<script>
import autoComplete from './components/auto-complete.vue'
export default {
  components: {
    autoComplete
  },
  methods: {
    handleSelectedSelected(selectedStation) {
      console.log('Selected station:', selectedStation)
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Encode+Sans:wght@300;400;700&display=swap');
</style>
