<template>
  <header>
    <h1 className="text-3xl font-bold md:text-5xl">Welcome to <span>Drive Wave</span></h1>
  </header>

  <main>
    <h4 className="text-l md:text-2xl">This is a tech challenge for RoadSurfer</h4>
    <auto-complete @result-selected="handleResultSelected" className="mt-36" />
  </main>
</template>

<script>
import autoComplete from './components/auto-complete.vue'
export default {
  components: {
    autoComplete
  },
  methods: {
    handleResultSelected(result) {
      console.log('Selected result:', result)
    }
  }
}
</script>

<style></style>
