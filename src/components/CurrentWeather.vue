<template>
  <div class="flex w-full flex-col">
    <h3 class="text-center text-xl font-semibold md:text-right">
      {{ cityName }}, {{ countryName }}
    </h3>
    <h4 class="mb-3 text-center text-lg md:mb-0 md:text-right">{{ description }}</h4>
    <h2 class="mb-3 text-center text-6xl font-bold md:text-right md:text-8xl">{{ temp }}°C</h2>
    <div class="mt-4 flex w-full items-center justify-around md:mt-0 md:w-fit md:flex-col">
      <h4>
        <span class="font-semibold">Real Feel</span>
        {{ feelsLike }}°C
      </h4>
      <h4>
        <span class="font-semibold">Sunrise</span>
        {{ sunrise }}
      </h4>
      <h4>
        <span class="font-semibold">Sunset</span>
        {{ sunset }}
      </h4>
    </div>
  </div>
</template>

<script>
  export default {
    props: ['weatherData'],
    computed: {
      cityName() {
        return this.weatherData?.city?.name || 'City not available';
      },
      countryName() {
        return this.weatherData?.city?.country || 'Country not available';
      },
      temp() {
        if (this.weatherData?.list?.[0]?.main?.temp) {
          const kelvin = this.weatherData.list[0].main.temp;
          return (kelvin - 273.15).toFixed();
        }
        return 'Temp not available';
      },
      feelsLike() {
        if (this.weatherData?.list?.[0]?.main?.feels_like) {
          const kelvin = this.weatherData.list[0].main.feels_like;
          return (kelvin - 273.15).toFixed();
        }
        return 'Feels Like not available';
      },
      description() {
        return (
          this.weatherData?.list?.[0]?.weather?.[0]?.description
            ?.split(' ')
            .map(word => word.charAt(0).toUpperCase() + word.slice(1))
            .join(' ') || 'Description not available'
        );
      },
      sunrise() {
        if (this.weatherData?.city?.sunrise) {
          const timestamp = this.weatherData.city.sunrise;
          return this.formatTime(timestamp);
        }
        return 'Sunrise not available';
      },
      sunset() {
        if (this.weatherData?.city?.sunset) {
          const timestamp = this.weatherData.city.sunset;
          return this.formatTime(timestamp);
        }
        return 'Sunset not available';
      },
    },
    methods: {
      formatTime(timestamp) {
        const date = new Date((timestamp + this.weatherData.city.timezone) * 1000);
        const hours = date.getUTCHours().toString().padStart(2, '0');
        const minutes = date.getUTCMinutes().toString().padStart(2, '0');
        return `${hours}:${minutes}`;
      },
    },
  };
</script>
