<template>
  <div class="mt-7 h-fit w-full md:mt-0">
    <h2 class="mb-2 flex items-center gap-1 text-lg font-semibold opacity-80">
      <i class="ph-fill ph-calendar-dots"></i>
      <span>5-Day Forecast</span>
    </h2>
    <Line
      v-if="chartData.labels.length > 0"
      :data="chartData"
      :options="chartOptions"
      ref="lineChartCanvas"
    />
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
  import {Line} from 'vue-chartjs';
  import {
    Chart as ChartJS,
    Title,
    Tooltip,
    Legend,
    LineElement,
    CategoryScale,
    LinearScale,
    PointElement,
    Filler,
  } from 'chart.js';
  ChartJS.register(
    Title,
    Tooltip,
    Legend,
    LineElement,
    CategoryScale,
    LinearScale,
    PointElement,
    Filler,
  );
  export default {
    components: {Line},
    props: ['weatherData', 'timeOfDay'],
    inject: ['weatherIcons'],
    data() {
      return {
        gradient: null,
      };
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
      chartData() {
        if (!this.fiveDayForecast.length) {
          return {
            labels: [],
            datasets: [],
          };
        }

        const labels = this.fiveDayForecast.map(day => day.date);
        const tempData = this.fiveDayForecast.map(day => day.temp);
        const canvas = this.$refs.lineChartCanvas?.$el?.getContext('2d');
        if (canvas) {
          const gradient = canvas.createLinearGradient(0, 0, 0, canvas.canvas.height);
          gradient.addColorStop(0, 'rgba(255, 255, 255, 0.2)');
          gradient.addColorStop(0.3, 'rgba(255, 255, 255, 0.1)');
          gradient.addColorStop(1, 'rgba(255, 255, 255, 0)');
          this.gradient = gradient;
        }

        return {
          labels: labels,
          datasets: [
            {
              data: tempData,
              borderColor: '#F6F6F6',
              backgroundColor: this.gradient || '#F6F6F6',
              pointBackgroundColor: '#F6F6F6',
              fill: 'start',
              tension: 0.5,
              borderWidth: 1,
              hoverRadius: 5,
            },
          ],
        };
      },
      chartOptions() {
        return {
          responsive: true,
          plugins: {
            legend: {
              display: false,
            },
            tooltip: {
              enabled: false,
            },
          },
          scales: {
            x: {
              grid: {
                display: false,
              },
              border: {
                display: false,
              },
              ticks: {
                display: false,
              },
            },
            y: {
              grid: {
                display: false,
              },
              border: {
                display: false,
              },
              ticks: {
                display: false,
              },
            },
          },
        };
      },
    },
  };
</script>
