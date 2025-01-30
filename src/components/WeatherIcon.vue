<template>
  <div class="flex items-center justify-center md:col-span-2 md:row-span-2">
    <img alt="Current Weather icon" :src="showWeatherIcon" class="md:scale-125" :style="iconMove" />
  </div>
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
