<template>
  <div class="bg w-screen overflow-x-hidden font-sans">
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
  import gsap from 'gsap';
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
    computed: {
      timeOfDay() {
        if (!this.weatherData?.city?.timezone) {
          return 'unknown';
        }
        const timeZoneOffset = this.weatherData.city.timezone;
        const now = new Date();
        const utcHours = now.getUTCHours();
        const localHours = (utcHours + timeZoneOffset / 3600) % 24;
        if (localHours >= 5 && localHours < 10) {
          return 'morning';
        } else if (localHours >= 10 && localHours < 13) {
          return 'noon';
        } else if (localHours >= 13 && localHours < 16) {
          return 'afternoon';
        } else if (localHours >= 16 && localHours < 18) {
          return 'evening';
        } else {
          return 'night';
        }
      },
    },
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
      selectedCity(oldCity, newCity) {
        if (newCity && newCity !== oldCity) {
          this.getCityData(newCity);
        }
      },
      timeOfDay(newTimeOfDay) {
        let gradient;
        if (newTimeOfDay === 'morning') {
          gradient = 'linear-gradient(to bottom, #FFD9C0, #FFB2AC, #99DFFF)';
        } else if (newTimeOfDay === 'noon') {
          gradient = 'linear-gradient(to bottom, #87CEEB, #E6F9FF)';
        } else if (newTimeOfDay === 'afternoon') {
          gradient = 'linear-gradient(to bottom, #62A1FF, #FFD700, #FFECB5)';
        } else if (newTimeOfDay === 'evening') {
          gradient = 'linear-gradient(to bottom, #FF8C42, #FF3CAC, #6A0572)';
        } else if (newTimeOfDay === 'night') {
          gradient = 'linear-gradient(to bottom, #000428, #004E92, #1F1C2C)';
        }
        gsap.to('.bg', {
          duration: 1,
          backgroundImage: gradient,
          ease: 'power2.inOut',
        });
      },
    },
  };
</script>
