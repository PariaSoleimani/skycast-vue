<template>
  <div
    class="bg w-screen overflow-x-hidden font-sans"
    :class="dynamicTextColor"
    @mousemove="handleMouseMove"
  >
    <welcome-view v-if="!user" @new-user="registerNewUser"></welcome-view>
    <div v-else class="mx-auto flex h-full max-w-7xl flex-col gap-6 px-12 py-8">
      <app-header @select-city="selectCity"></app-header>
      <main class="flex w-full flex-col">
        <div v-if="error" class="text-red-500">{{ error }}</div>
        <weather-greeting
          :user="user"
          :weatherData="weatherData"
          :timeOfDay="timeOfDay"
        ></weather-greeting>
        <weather-icon :weatherData="weatherData" :timeOfDay="timeOfDay" :mouse="mouse"></weather-icon>
        <current-weather :weatherData="weatherData"></current-weather>
        <hourly-weather :weatherData="weatherData" :timeOfDay="timeOfDay"></hourly-weather>
        <weekly-weather :weatherData="weatherData" :timeOfDay="timeOfDay"></weekly-weather>
      </main>
    </div>
  </div>
</template>

<script>
  import axios from 'axios';
  import gsap from 'gsap';
  import AppHeader from './components/AppHeader.vue';
  import WeatherGreeting from './components/WeatherGreeting.vue';
  import WeatherIcon from './components/WeatherIcon.vue';
  import CurrentWeather from './components/CurrentWeather.vue';
  import HourlyWeather from './components/HourlyWeather.vue';
  import WeeklyWeather from './components/WeeklyWeather.vue';
  import WelcomeView from './components/WelcomeView.vue';

  export default {
    components: {
      AppHeader,
      WeatherGreeting,
      WeatherIcon,
      CurrentWeather,
      HourlyWeather,
      WeeklyWeather,
      WelcomeView,
    },
    provide() {
      return {
        weatherTypeMessages: this.weatherTypeMessages,
        weatherIcons: this.weatherIcons,
      };
    },
    data() {
      return {
        apiKey: 'd0376c716d3b1cf5731a07dddf0ed8fb',
        user: null,
        selectedCity: '',
        weatherData: null,
        error: null,
        weatherTypeMessages: {
          Rain: "It's rainy! You'd better take an umbrella!",
          Mist: "It's misty! Drive carefully!",
          Snow: "Grab your warm clothes! Let's make a snowman!",
          Clear: 'Look up at the clear sky! Enjoy!',
          Clouds: "It's cloudy! Don't forget your jacket!",
          Thunderstorm: "It's stormy! Prepare for the loud thunders!",
        },
        weatherIcons: {
          day: {
            Rain: '/src/assets/rain.png',
            Mist: '/src/assets/fog.png',
            Snow: '/src/assets/snow.png',
            Clear: '/src/assets/sunny.png',
            Clouds: '/src/assets/mostly-cloudy.png',
            Thunderstorm: '/src/assets/thunderstorms-sunny.png',
          },
          night: {
            Rain: '/src/assets/drizzle-night.png',
            Mist: '/src/assets/fog.png',
            Snow: '/src/assets/snow.png',
            Clear: '/src/assets/night.png',
            Clouds: '/src/assets/mostly-cloudy-night.png',
            Thunderstorm: '/src/assets/thunderstorms-night.png',
          },
        },
        mouse: {
          mouseX: 0,
          mouseY: 0,
        },
      };
    },
    computed: {
      dynamicTextColor() {
        if (
          this.timeOfDay === 'noon' ||
          this.timeOfDay === 'morning' ||
          this.timeOfDay === 'afternoon'
        ) {
          return 'text-zinc-800';
        } else {
          return 'text-white';
        }
      },
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
      registerNewUser(user) {
        this.user = user;
      },
      selectCity(city) {
        this.selectedCity = city;
      },
      handleMouseMove(event) {
        this.mouse.mouseX = event.clientX - window.innerWidth / 2;
        this.mouse.mouseY = event.clientY - window.innerHeight / 2;
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
      user(newUser) {
        this.getCityData(newUser.city);
      },
      selectedCity(newCity) {
        this.getCityData(newCity);
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
