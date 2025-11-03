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
    <p v-if="selectedDate">Вы выбрали: {{ selectedDate }}</p>
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
  border: 1px solid #ff5252;
  color: #ff5252;
  cursor: pointer;
  border-radius: 6px;
  transition: 0.3s;
}
.lang-switch button:hover {
  background-color: #ff5252;
  color: white;
}
</style>
