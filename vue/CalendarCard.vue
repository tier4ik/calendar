<template>
  <div class="calendar__day">
    <h1 class="calendar__date">{{ monthDate }}</h1>
  </div>
</template>

<script>
export default {
  props: ['monthDate']
}
</script>

<style>
  .calendar__day {
    height: 80px;
    width: 80px;
    border-radius: .5rem;
    background-color: #22b2ea;
    box-shadow: 5px 5px #42445a;
  }
  .calendar__date {
    text-align: center;
    color: #ddeaee;
    cursor: default;
  }
  .calendar__day_past {
    background-color: #42445a;
    box-shadow: 5px 5px #595c79;
  }
  .calendar__day_current {
    border: 2px solid tomato;
    background-color: #d7b5a1;
  }
  
</style>
