<template>
  <div>
    <h1>Home</h1>
    <img src="../assets/logo.png">
  </div>
</template>

<script>
export default {
  name: 'Home',
  mounted () {
    window.rudderanalytics.page('Home')
  }
}
</script>
