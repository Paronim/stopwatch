<template>
  <div class="wrapper-stopwatch">

      <div class="stopwatch-element" v-for="( stopwatch, index ) in stopwatches" :key="index">
        <p class="stopwatch-content">{{ stopwatch.output }}</p>
        <div class="stopwatch-button">
          <button class="stopwatch-button-content" v-if="stopwatch.run" @click="start(index)"><img src="../assets/triangle.png" alt="start"></button>
          <button class="stopwatch-button-content" v-else @click="pause(index)"><img src="../assets/Pause.png" alt="pause"></button>
          <button class="stopwatch-button-content" @click="stop(index)"><img src="../assets/square.png" alt="square"></button>
        </div>
      </div>

    <div @click="addNewStopwatch()" class="stopwatch-button-add">
    <img src="../assets/plus.png" alt="plus">
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
  .wrapper-stopwatch{
    display: flex;
    justify-content: start;
    max-width: 843px;
    flex-wrap: wrap;
    margin: 0 auto;
  }
  .stopwatch-element{
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 225px;
    height: 120px;
    background: #696969;
    margin: 45px 28px 0 28px;
  }
  .stopwatch-content{
    color: #9E9E9E;
    font-weight: 400;
    font-size: 22px;
    height: 60px;
    display: flex;
    align-items: center;
  }
  .stopwatch-button{
    border-top: 1px solid #9E9E9E;
    width: 100%;
    display: flex;
    justify-content: center;
    height: 60px;
  }
  .stopwatch-button-add{
    height: 120px;
    background: #69696900;
    margin: 45px 28px 0 28px;
    display: flex;
    justify-content: flex-start;
  }
  .stopwatch-button-content{
    border: none;
    background: #69696900;
    margin: 0 24px 0 24px;
  }
  @media screen and (max-width: 800px) {
    .wrapper-stopwatch{
    max-width: 562px;
  }
  }
  @media screen and (max-width: 425px) {
      .wrapper-stopwatch{
      max-width: 281px;
    }
  }
</style>
