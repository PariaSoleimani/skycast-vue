<template>
  <div class="message mx-auto max-w-md md:col-span-2 md:mx-0">
    <h2 class="mb-3 text-center text-3xl font-bold tracking-tight md:text-start md:text-4xl">
      {{ greetMessage }}
    </h2>
    <p class="text-center text-lg md:text-start md:text-xl">
      {{ weatherMessage }}
    </p>
  </div>
</template>

<script>
  import gsap from 'gsap';

  export default {
    props: ['weatherData', 'user', 'timeOfDay'],
    inject: ['weatherTypeMessages'],
    computed: {
      greetMessage() {
        if (this.timeOfDay !== 'unknown') {
          if (this.timeOfDay === 'morning') return `Good Morning ${this.user.name}`;
          if (this.timeOfDay === 'noon') return `Good Noon ${this.user.name}`;
          if (this.timeOfDay === 'afternoon') return `Good Afternoon ${this.user.name}`;
          if (this.timeOfDay === 'evening') return `Good Evening ${this.user.name}`;
          return `Good Night ${this.user.name}`;
        }
        return `Hello ${this.user.name}`;
      },
      weatherMessage() {
        if (!this.weatherData?.list?.[0]?.weather?.[0]?.main) {
          return 'Weather data unavailable.';
        }
        return (
          this.weatherTypeMessages[this.weatherData.list[0].weather[0].main] ||
          'Weather data unavailable.'
        );
      },
    },
    mounted() {
      gsap.from('.message', {
        duration: 1.5,
        opacity: 0,
        y: -50,
        ease: 'power4.out',
      });
    },
  };
</script>
