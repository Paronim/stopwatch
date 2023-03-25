<template>
  <div class="wrapper-stopwatch">
    <div class="stopwatch-list" v-for="( stopwatch, index ) in stopwatches" :key="index">
      <div class="stopwatch-element">
        {{ stopwatch }}
      </div>
    </div>
  </div>
</template>

<script setup>
  import { ref } from 'vue'

  const stopwatches = ref([0])
</script>

<style lang="scss">

</style>
