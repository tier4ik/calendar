<template>
  <div class="calendar">
    <h1 class="calendar__month">
      <a href="#" type="button" class="calendar__month-change calendar__month-change_prev" @click="getPrevMonth()">&Lang;</a>
      {{ getLocaleMonth() | monthFilter}}
      <span class="calendar__year">{{ year }}</span>
      <a href="#" type="button" class="calendar__month-change calendar__month-change_next" @click="getNextMonth()">&Rang;</a>
    </h1>
    <ul class="calendar__week">
        <li class="calendar__day-name">Monday</li>
        <li class="calendar__day-name">Tuesday</li>
        <li class="calendar__day-name">Wednesday</li>
        <li class="calendar__day-name">Thursday</li>
        <li class="calendar__day-name">Friday</li>
        <li class="calendar__day-name">Saturday</li>
        <li class="calendar__day-name">Sunday</li>
      </ul>
      <transition name="fade">
        <div class="calendar__days-box" v-if="show">
          <calendar-card 
            v-for="day in daysInCurMonth" 
            :key="day" 
            :monthDate=day
            :class="{
            'calendar__day_past': day < currentDate || month < new Date().getMonth() && year <= new Date().getFullYear(), 
            'calendar__day_current': day === currentDate && month === new Date().getMonth(),
            'calendar__day_first': day === 1,
            [`calendar__day_${firstMonthDay}`]: day === 1
            }">
          </calendar-card>
        </div>
      </transition>
  </div>
</template>

<script>

import CalendarCard from './CalendarCard.vue'

export default {
  data() {
    return {
      date: new Date(),
      show: true
    }
  },
  computed: {
    currentDate() { return this.date.getDate() },
    year() { return this.date.getFullYear() },
    month() {return this.date.getMonth() },
    daysInCurMonth() { return this.daysInThisMonth() },
    firstMonthDay() { return new Date(this.year, this.month, 1).toLocaleDateString('en-EN', { weekday: 'long' }).toLowerCase() }
  },
  components: {
    "calendar-card": CalendarCard 
  },
  filters: {
    monthFilter(value) {
      if (!value) return ''
      return value.charAt(0).toUpperCase() + value.slice(1)
    }
  },
  methods: {
    getLocaleMonth() {
      return this.date.toLocaleDateString('en-EN', { month: 'long' })
    },
    daysInThisMonth() {
      const now = this.date;
      return new Date(now.getFullYear(), now.getMonth()+1, 0).getDate();
    },
    isPast(val) {
      return val < this.currentDate
    },
    getPrevMonth() {
      this.show = false;
      const now = new Date();
      const prev = new Date(this.year, this.month - 1);
      if (now.getMonth() === prev.getMonth()) {
        this.date = now
      } else {
        this.date = prev
      }
      setTimeout(() => this.show = true, 500)
    },
    getNextMonth() {
      this.show = false;
      const now = new Date();
      const next = new Date(this.year, this.month + 1);
      if (now.getMonth() === next.getMonth()) {
        this.date = now
      } else {
        this.date = next
      }
      setTimeout(() => this.show = true, 500)
    }
  }
}
</script>

<style>
  body {
    margin: 0;
    width: 100%;
    height: 100%;
    background-color: #ddeaee;
  }
  .calendar {
    font-family: Tahoma, sans-serif;
    box-sizing: border-box;
  }
  .calendar__year {
    font-size: 1.2rem;
    align-self: flex-end;
    color: #42445a;
  }
  .calendar__month {
    text-align: center;
    font-size: 3rem;
    display: flex;
    justify-content: center;
    align-items: center;
    color: #42445a;
  }
  .calendar__month-change {
    display: inline-block;
    padding: 0 1rem;
    font-size: 1.4rem;
    color: inherit;
    text-decoration: none;
    transition: all .3s ease;
  }
  .calendar__month-change_prev:hover {
    transform: translate(-.8rem);
  }
  .calendar__month-change_next:hover {
    transform: translate(.8rem);
  }
  .calendar__days-box {
    width: 80%;
    margin: 80px auto 0;
    display: grid;
    grid-template-columns: repeat(7, 1fr);
    grid-template-rows: auto;
    grid-row-gap: 40px;
    grid-auto-flow: row;
    justify-items: center;
  }
  .calendar__week {
    width: 80%;
    height: 40px;
    border-radius: .2rem;
    display: grid;
    margin: auto;
    justify-content: center;
    grid-template-columns: repeat(7, 1fr);
    justify-items: center;
    align-items: center;
    padding-left: 0;
    list-style: none;
    font-weight: bold;
    background-color: #c5a490;
    color: #dce5f0;
  }
  .calendar__day_monday {
    grid-column: 1 / 2;
  }
  .calendar__day_tuesday {
    grid-column: 2 / 3;
  }
  .calendar__day_wednesday {
    grid-column: 3 / 4;
  }
  .calendar__day_thursday {
    grid-column: 4 / 5;
  }
  .calendar__day_friday {
    grid-column: 5 / 6;
  }
  .calendar__day_saturday {
    grid-column: 6 / 7;
  }
  .calendar__day_sunday {
    grid-column: 7 / 8;
  }
  /* animation */
  .fade-enter-active, .fade-leave-active {
    transition: opacity .3s;
  }
  .fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
    opacity: 0;
  }
</style>