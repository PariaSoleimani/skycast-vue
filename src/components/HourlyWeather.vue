<template>
  <div class="mt-7 h-fit w-full self-end md:col-span-2 md:col-start-5 md:mt-0">
    <h2 class="mb-5 flex items-center gap-1 text-lg font-semibold opacity-80">
      <i class="ph-fill ph-clock"></i>
      <span>3-Hour Forecast</span>
    </h2>
    <ul class="flex items-center justify-between">
      <li class="flex flex-col items-center" v-for="hour in threeHourForecast" :key="hour.time">
        <span>{{ hour.temp }}°C</span>
        <img
          class="size-10 scale-125"
          :src="showWeatherIcon(hour.description)"
          alt="weather icon"
        />
        <span>{{ hour.time }}</span>
      </li>
    </ul>
  </div>
</template>

<script>
  export default {
    props: ['weatherData', 'timeOfDay'],
    inject: ['weatherIcons'],
    computed: {
      threeHourForecast() {
        if (!this.weatherData?.list) return [];
        return this.weatherData.list.slice(0, 5).map(item => ({
          time: new Date(item.dt_txt).toLocaleTimeString([], {
            hour: '2-digit',
            minute: '2-digit',
            hour12: false,
          }),
          temp: (item.main.temp - 273.15).toFixed(),
          description: item.weather[0]?.main || 'Unknown',
        }));
      },
    },
    methods: {
      showWeatherIcon(weatherType) {
        const defaultIcon = '/src/assets/default.png';
        const isDay =
          this.timeOfDay === 'morning' ||
          this.timeOfDay === 'noon' ||
          this.timeOfDay === 'afternoon' ||
          this.timeOfDay === 'evening';

        const icons = isDay ? this.weatherIcons.day : this.weatherIcons.night;
        return icons?.[weatherType] || defaultIcon;
      },
    },
  };
</script>
