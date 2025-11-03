<template>
  <div id="app">
    <h1>Календарь</h1>
    <div class="lang-switch">
      <button @click="lang = 'ru'">Русский</button>
      <button @click="lang = 'en'">English</button>
    </div>
    <AppCalendar
        :initial-date="'2025-11-03'"
        :language="lang"
        @date-selected="onDateSelected"
    />
    <p v-if="selectedDate" class="selected-date">
      Выбранная дата: {{ formattedDate }}
    </p>
  </div>
</template>

<script>
import AppCalendar from './components/Calendar.vue'

export default {
  name: 'App',
  components: { AppCalendar },
  data() {
    return {
      selectedDate: null,
      lang: 'ru'
    }
  },
  computed: {
    formattedDate() {
      if (!this.selectedDate) return ''
      const [year, month, day] = this.selectedDate.split('-')
      return `${day}-${month}-${year}`
    }
  },
  methods: {
    onDateSelected(date) {
      this.selectedDate = date
    }
  }
}
</script>


<style>
#app {
  text-align: center;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  margin-top: 40px;
  color: #2d3436;
}
.lang-switch button {
  margin: 5px;
  padding: 6px 12px;
  background-color: #ffe5e5;
  border: 2px solid #ff5252;
  color: #ff5252;
  cursor: pointer;
  border-radius: 8px;
  transition: 0.3s;
  font-size: 16px;
  font-weight: 700;
  letter-spacing: 0.5px;
}
.lang-switch button:hover {
  background-color: #ff5252;
  color: white;
}
.selected-date {
  margin-top: 20px;
  font-size: 18px;
  font-weight: 700;
  color: #ff5252;
  background-color: #fff4f4;
  display: inline-block;
  padding: 8px 16px;
  border-radius: 8px;
  box-shadow: 0 2px 6px rgba(255, 82, 82, 0.3);
}

</style>
