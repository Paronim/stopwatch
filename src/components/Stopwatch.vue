<template>
  <div class="wrapper-stopwatch">
    <div class="stopwatch-list" v-for="( stopwatch, index ) in stopwatches" :key="index">
      <div class="stopwatch-element">
        {{ stopwatch.stopwatch }}
        <div class="stopwatch-button">
          <button @click="start(index)">Старт</button>
          <button @click="pause(index)">Пауза</button>
          <button @click="stop(index)">Сброс</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
  import { computed, ref } from 'vue'

  const stopwatches = ref([
    {
    stopwatch: '00:00:00',
    status: null
    }
  ])



  const start = async (index) => {
    stopwatches.value[index].status = true

    let hours = 0;
    let minutes = 0;
    let seconds = 0;

  const startInterval = setInterval(() => {
    if(seconds < 60){
      seconds++
    } else if(minutes < 60){
      seconds = 0;
      minutes++
    } else {
      minutes = 0;
      hours++
    }
    stopwatches.value[index].stopwatch =  `${hours}:${minutes}:${seconds}`
  } , 1000)
  stopwatches.value[index].status = startInterval
  }

  const pause = (index) => {

  }

  const stop = (index) => {
    stopwatches.value[index].status = false
    stopwatches.value[index].stopwatch = '00:00:00'
  }

</script>

<style lang="scss">
  .stopwatch-element{
    display: flex;
    flex-direction: column;
    align-items: center;
  }
</style>
