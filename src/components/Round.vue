<template>
  <div class="text-center">
    <h1>Round <span id="roundNumber">{{ roundInfo.number }}</span></h1>
  </div>
</template>

<script>
  export default {
    props: ['roundInfo'],
  }
</script>

<style scoped>

</style>