<template>
  <div class="wrapper-stopwatch">
    <div class="stopwatch-list" v-for="( stopwatch, index ) in stopwatches" :key="index">
      <div class="stopwatch-element">
        {{ stopwatch.output }}
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
  import { ref } from 'vue'

  const stopwatches = ref([
    {
    seconds: 0,
    minutes: 0,
    hours: 0,
    output: '00:00:00',
    interval: null
    }
  ])

  const outputAdjustment = (num) => {
    if(String(num).length === 1){
      num = `0${num}`
    }
    return num
  }

  const start = async (index) => {
    stopwatches.value[index].status = true

  const startInterval = setInterval(() => {
    if(stopwatches.value[index].seconds < 60){
      stopwatches.value[index].seconds++
    } else if(stopwatches.value[index].minutes < 60){
      stopwatches.value[index].seconds = 0;
      stopwatches.value[index].minutes++
    } else {
      stopwatches.value[index].minutes = 0;
      stopwatches.value[index].hours++
    }
    stopwatches.value[index].output = `${outputAdjustment(stopwatches.value[index].hours)}:${outputAdjustment(stopwatches.value[index].minutes)}:${outputAdjustment(stopwatches.value[index].seconds)}`
  } , 1000)
  stopwatches.value[index].interval = startInterval
  }

  const pause = (index) => {

    clearInterval(stopwatches.value[index].interval);
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
