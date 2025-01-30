<template>
  <header>
    <nav class="flex items-center justify-between">
      <form
        class="group flex w-fit items-center rounded-3xl py-0.5 transition-all duration-1000 focus-within:w-72 focus-within:gap-3 focus-within:border focus-within:px-3"
        @submit.prevent="passCityName"
      >
        <input
          class="weather-input w-0 bg-transparent transition-all duration-500 group-focus-within:w-full group-focus-within:px-1 group-focus-within:py-1 focus:outline-none"
          v-model="city"
          type="text"
          @keyup.enter="passCityName"
        />
        <button class="peer cursor-pointer text-2xl md:text-3xl" type="submit">
          <i class="ph-thin ph-magnifying-glass"></i>
        </button>
      </form>

      <ul class="text-3xl md:text-4xl">
        <li>
          <button class="cursor-pointer" type="button" aria-label="Search City">
            <i class="ph-thin ph-user-circle"></i>
          </button>
        </li>
      </ul>
    </nav>
  </header>
</template>

<script>
  export default {
    emits: ['select-city'],
    data() {
      return {
        city: '',
      };
    },
    methods: {
      passCityName() {
        if (this.city?.trim()) {
          this.$emit('select-city', this.city);
          this.city = '';
        }
      },
    },
  };
</script>
