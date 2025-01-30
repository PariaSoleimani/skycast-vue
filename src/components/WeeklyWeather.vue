<template>
  <div class="mt-7 h-fit w-full md:mt-0">
    <h2 class="mb-2 flex items-center gap-1 text-lg font-semibold opacity-80">
      <i class="ph-fill ph-calendar-dots"></i>
      <span>5-Day Forecast</span>
    </h2>
    <ul class="flex items-center justify-between">
      <li class="flex flex-col items-center" v-for="day in fiveDayForecast" :key="day.date">
        <span>{{ day.temp }}°C</span>
        <img class="size-10 scale-125" :src="showWeatherIcon(day.description)" alt="" />
        <span>{{ day.date }}</span>
      </li>
    </ul>
  </div>
</template>

<script>
  export default {
    props: ['weatherData', 'timeOfDay'],
    inject: ['weatherIcons'],
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
    computed: {
      fiveDayForecast() {
        if (!this.weatherData?.list) return [];
        const dailyData = this.weatherData.list.filter(item => item.dt_txt.includes('12:00:00'));

        return dailyData.map(item => ({
          date: new Date(item.dt_txt).toLocaleDateString(undefined, {
            weekday: 'short',
          }),
          temp: (item.main.temp - 273.15).toFixed(),
          description: item.weather[0]?.main || 'Unknown',
        }));
      },
    },
  };
</script>
