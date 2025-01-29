<template>
  <img alt="Current Weather icon" :src="showWeatherIcon" />
</template>

<script>
  export default {
    props: ['timeOfDay', 'weatherData'],
    inject: ['weatherIcons'],
    computed: {
      showWeatherIcon() {
        const iconType = this.weatherData?.list?.[0]?.weather?.[0]?.main;
        const defaultIcon = '/src/assets/default.png';
        if (!iconType) return defaultIcon;
        return this.timeOfDay === 'morning' ||
          this.timeOfDay === 'noon' ||
          this.timeOfDay === 'afternoon' ||
          this.timeOfDay === 'evening'
          ? this.weatherIcons?.day?.[iconType] || defaultIcon
          : this.weatherIcons?.night?.[iconType] || defaultIcon;
      },
    },
  };
</script>
