<template>
  <div class="w-screen overflow-x-hidden font-sans">
    <div class="mx-auto flex h-full max-w-7xl flex-col gap-6 px-12">
      <app-header @select-city="selectCity"></app-header>
      <main>
        <div v-if="error" class="text-red-500">{{ error }}</div>
      </main>
    </div>
  </div>
</template>

<script>
  import axios from 'axios';
  import AppHeader from './components/AppHeader.vue';

  export default {
    components: {
      AppHeader,
    },
    data() {
      return {
        apiKey: 'd0376c716d3b1cf5731a07dddf0ed8fb',
        selectedCity: '',
        weatherData: null,
        error: null,
      };
    },
    computed: {},
    methods: {
      selectCity(city) {
        this.selectedCity = city;
      },
      async getCityData(city) {
        try {
          const apiUrl = `https://api.openweathermap.org/data/2.5/forecast?q=${city}&appid=${this.apiKey}`;
          const response = await axios.get(apiUrl);
          this.weatherData = response.data;
          console.log(this.weatherData);
          this.error = null;
        } catch (error) {
          this.error = 'Failed to fetch weather data. Please try again.';
          console.error('Error fetching data:', error);
        }
      },
    },
    watch: {
      selectedCity(newCity) {
        this.getCityData(newCity);
      },
    },
  };
</script>
