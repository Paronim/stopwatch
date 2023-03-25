<template>
  <div class="wrapper-stopwatch">
    <div class="stopwatch-list" v-for="( stopwatch, index ) in stopwatches" :key="index">
      <div class="stopwatch-element">
        {{ stopwatch.output }}
        <div class="stopwatch-button">
          <button v-if="stopwatch.run" @click="start(index)">Старт</button>
          <button v-else @click="pause(index)">Пауза</button>
          <button @click="stop(index)">Сброс</button>
        </div>
      </div>
    </div>
    <div>
      <button @click="addNewStopwatch()"></button>
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
    output: '0',
    interval: null,
    run: true
    }
  ])

  const outputAdjustment = (sec, min, hour, index) => {
    if(min > 0){
      stopwatches.value[index].output = `${min}:${sec}`
    } else if(hour > 0){
      stopwatches.value[index].output = `${hour}:${min}:${sec}`
    } else {
      stopwatches.value[index].output = `${sec}`
    }
    
  }

  const start = async (index) => {
    stopwatches.value[index].status = true
    stopwatches.value[index].run = false

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
    outputAdjustment(stopwatches.value[index].seconds, stopwatches.value[index].minutes, stopwatches.value[index].hours, index)
  } , 1000)
  stopwatches.value[index].interval = startInterval
  }

  const pause = (index) => { 
    clearInterval(stopwatches.value[index].interval);
    stopwatches.value[index].run = true
  }

  const stop = (index) => {
    clearInterval(stopwatches.value[index].interval);
    stopwatches.value[index].run = true
    stopwatches.value[index].output = '0'
    stopwatches.value[index].seconds = 0
    stopwatches.value[index].minutes = 0
    stopwatches.value[index].hours = 0
  }

  const addNewStopwatch = () => {
    stopwatches.value.push({
      seconds: 0,
      minutes: 0,
      hours: 0,
      output: '0',
      interval: null,
      run: true
    })
  }

</script>

<style lang="scss">
  .stopwatch-element{
    display: flex;
    flex-direction: column;
    align-items: center;
    color: #9E9E9E;
  }
</style>
