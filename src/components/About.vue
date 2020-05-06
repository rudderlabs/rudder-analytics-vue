<template>
  <div>
    <h1>About</h1>
    <p class="about-content">
      Rudder is a platform for collecting, storing and routing customer event data to dozens of tools. Rudder is open-source, can run in your cloud environment (AWS, GCP, Azure or even your data-centre) and provides a powerful transformation framework to process your event data on the fly.
    </p>
  </div>
</template>

<script>
export default {
  name: 'About',
  mounted () {
    window.rudderanalytics.page('About')
  }
}
</script>

<style>
.about-content {
  font-size: 23px;
  line-height: 1.4;
  margin: 0 35%;
}
</style>
