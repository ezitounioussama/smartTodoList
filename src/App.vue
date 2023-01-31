<script setup>
import "./style.css";
</script>

<template>
  <div class="relative my-8 mx-5">
    <div
      class="absolute inset-y-0 left-0 flex items-center pl-3 pointer-events-none"
    >
      <svg
        xmlns="http://www.w3.org/2000/svg"
        fill="none"
        viewBox="0 0 24 24"
        stroke-width="1.5"
        stroke="currentColor"
        class="w-6 h-6 text-blue-600"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          d="M15 15l6-6m0 0l-6-6m6 6H9a6 6 0 000 12h3"
        />
      </svg>
    </div>

    <input
      v-model="message"
      type="text"
      class="block w-full p-4 pl-12 text-sm text-gray-900 border border-gray-300 rounded-lg bg-gray-50 focus:outline-none focus:ring-1 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
      placeholder="Add Tasks..."
    />

    <button
      @click="onSubmit"
      type="button"
      class="text-white absolute right-2.5 bottom-2.5 bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-4 py-2"
    >
      Add
    </button>
  </div>
  <article class="rounded-xl border border-gray-700 bg-gray-800 p-4 mx-5">
    <List />
  </article>
</template>

<script>
import List from "./components/list.vue";
export default {
  data() {
    return {
      message: "",
      onSubmit() {
        console.log(this.message);
      },
      components: List,
    };
  },
};
</script>
