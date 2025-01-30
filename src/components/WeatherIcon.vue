<template>
  <img alt="Current Weather icon" :src="showWeatherIcon" :style="iconMove" />
</template>

<script>
  export default {
    props: ['timeOfDay', 'weatherData', 'mouse'],
    inject: ['weatherIcons'],
    computed: {
      iconMove() {
        const offsetX = this.mouse.mouseX * 0.05;
        const offsetY = this.mouse.mouseY * 0.05;
        return {
          transform: `translate(${offsetX}px, ${offsetY}px)`,
          transition: 'transform 0.1s ease-out',
        };
      },
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
