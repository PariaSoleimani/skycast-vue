<template>
  <div
    class="h-screen w-full bg-[url(/src/assets/welcome-bg1.jpg)] bg-cover bg-bottom bg-no-repeat text-white"
  >
    <div class="mx-auto flex h-full max-w-7xl items-center justify-center px-8">
      <div
        class="welcome w-lg rounded-3xl border-[0.5px] border-slate-200 px-12 py-16 shadow backdrop-blur-md"
      >
        <div class="max-w-m mx-auto h-full">
          <h1 class="mb-14 text-center text-5xl font-bold">Welcome!</h1>
          <form class="mx-auto max-w-sm space-y-3" @submit.prevent="$emit('new-user', user)">
            <div class="flex w-full items-center gap-3">
              <label class="w-16 font-bold" for="name">Name</label>
              <input
                class="w-full rounded-xl border border-white bg-transparent px-3 py-1 focus:outline-none"
                id="name"
                v-model="user.name"
                type="text"
              />
            </div>
            <div class="flex items-center gap-3">
              <label class="w-16 font-bold" for="city">City</label>
              <input
                class="w-full rounded-xl border border-white bg-transparent px-3 py-1 focus:outline-none"
                id="city"
                v-model="user.city"
                type="text"
              />
            </div>
            <button
              class="mt-4 w-full cursor-pointer rounded-lg bg-white py-1.5 text-lg text-zinc-700 duration-300 hover:shadow-sm active:scale-95"
              type="submit"
            >
              Start
            </button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    emits: ['new-user'],
    data() {
      return {
        user: {
          name: null,
          city: null,
        },
      };
    },
  };
</script>
