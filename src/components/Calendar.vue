<template>
  <div class="calendar">
    <div class="header">
      <button @click="prevMonth">‹</button>
      <div class="month-year">
        {{ monthNames[language][currentMonth] }} {{ currentYear }}
      </div>
      <button @click="nextMonth">›</button>
    </div>

    <div class="weekdays">
      <div v-for="(day, i) in weekDays[language]" :key="i">{{ day }}</div>
    </div>

    <div class="days">
      <div
          v-for="(day, index) in daysInMonth"
          :key="index"
          :class="['day', { today: isToday(day), selected: isSelected(day) }]"
          @click="selectDate(day)"
      >
        <span v-if="day">{{ day.getDate() }}</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'AppCalendar',
  props: {
    initialDate: {
      type: String,
      default: ''
    },
    language: {
      type: String,
      default: 'ru'
    }
  },
  data() {
    const init = this.initialDate
        ? new Date(this.initialDate)
        : new Date()
    return {
      currentYear: init.getFullYear(),
      currentMonth: init.getMonth(),
      selected: init,
      weekDays: {
        ru: ['Пн', 'Вт', 'Ср', 'Чт', 'Пт', 'Сб', 'Вс'],
        en: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
      },
      monthNames: {
        ru: ['Январь', 'Февраль', 'Март', 'Апрель', 'Май', 'Июнь',
          'Июль', 'Август', 'Сентябрь', 'Октябрь', 'Ноябрь', 'Декабрь'],
        en: ['January', 'February', 'March', 'April', 'May', 'June',
          'July', 'August', 'September', 'October', 'November', 'December']
      }
    }
  },
  computed: {
    daysInMonth() {
      const firstDay = new Date(this.currentYear, this.currentMonth, 1)
      const lastDay = new Date(this.currentYear, this.currentMonth + 1, 0)
      const days = []

      // добавляем пустые ячейки перед первым днем (сдвиг по неделе)
      const startDay = (firstDay.getDay() + 6) % 7
      for (let i = 0; i < startDay; i++) {
        days.push(null)
      }

      // добавляем дни месяца
      for (let d = 1; d <= lastDay.getDate(); d++) {
        days.push(new Date(this.currentYear, this.currentMonth, d))
      }

      return days
    }
  },
  methods: {
    prevMonth() {
      if (this.currentMonth === 0) {
        this.currentMonth = 11
        this.currentYear--
      } else {
        this.currentMonth--
      }
    },
    nextMonth() {
      if (this.currentMonth === 11) {
        this.currentMonth = 0
        this.currentYear++
      } else {
        this.currentMonth++
      }
    },
    selectDate(day) {
      if (!day) return
      this.selected = day
      const formatted = `${day.getFullYear()}-${(day.getMonth() + 1)
          .toString()
          .padStart(2, '0')}-${day
          .getDate()
          .toString()
          .padStart(2, '0')}`
      this.$emit('date-selected', formatted)
    },
    isToday(day) {
      if (!day) return false
      const today = new Date()
      return (
          day.getDate() === today.getDate() &&
          day.getMonth() === today.getMonth() &&
          day.getFullYear() === today.getFullYear()
      )
    },
    isSelected(day) {
      if (!day || !this.selected) return false
      return (
          day.getDate() === this.selected.getDate() &&
          day.getMonth() === this.selected.getMonth() &&
          day.getFullYear() === this.selected.getFullYear()
      )
    }
  }
}
</script>

<style scoped>
.calendar {
  width: 340px;
  margin: 20px auto;
  border: 2px solid #ff5252;
  border-radius: 10px;
  padding: 15px;
  background-color: #fff8f8;
  box-shadow: 0 4px 10px rgba(255, 82, 82, 0.3);
}
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  color: #ff5252;
  font-weight: bold;
  margin-bottom: 10px;
}
.header button {
  border: none;
  background: #ffe5e5;
  color: #ff5252;
  font-size: 18px;
  padding: 5px 10px;
  border-radius: 50%;
  cursor: pointer;
  transition: 0.3s;
}
.header button:hover {
  background: #ff5252;
  color: white;
}
.weekdays, .days {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
}
.weekdays div {
  font-weight: bold;
  padding: 5px;
  color: #ff5252;
}
.day {
  height: 40px;
  line-height: 40px;
  text-align: center;
  color: #ff5252;
  border-radius: 6px;
  cursor: pointer;
  transition: 0.3s;
}
.day:hover {
  background-color: #ffebee;
}
.today {
  background-color: #ffe5e5;
  font-weight: bold;
}
.selected {
  background-color: #ff5252;
  color: white;
  font-weight: bold;
}
</style>
