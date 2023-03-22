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
      @keyup.enter="addToList"
      type="text"
      class="w-[550px] p-4 pl-12 text-sm text-gray-900 border border-gray-300 rounded-lg bg-gray-50 focus:outline-none focus:ring-1 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
      placeholder="Add Tasks..."
    />
    <button
      type="button"
      class="absolute inset-y-0 right-0 flex items-center pr-3"
    >
      <svg
        @click="startSpeechRecognition"
        aria-hidden="true"
        class="w-4 h-4 absolute right-20 text-gray-500 dark:text-gray-400 hover:text-gray-900 dark:hover:text-white"
        fill="currentColor"
        viewBox="0 0 20 20"
        xmlns="http://www.w3.org/2000/svg"
      >
        <path
          fill-rule="evenodd"
          d="M7 4a3 3 0 016 0v4a3 3 0 11-6 0V4zm4 10.93A7.001 7.001 0 0017 8a1 1 0 10-2 0A5 5 0 015 8a1 1 0 00-2 0 7.001 7.001 0 006 6.93V17H6a1 1 0 100 2h8a1 1 0 100-2h-3v-2.07z"
          clip-rule="evenodd"
        ></path>
      </svg>
    </button>
    <button
      @click="addToList"
      type="button"
      class="text-white absolute right-2.5 bottom-2.5 bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-4 py-2"
    >
      Add
    </button>
  </div>
  <article
    :class="articleHidden ? 'hidden' : ''"
    class="rounded-xl bg-gray-700 p-4 mx-5 text-white w-[550px]"
  >
    <input type="checkbox" @change="(e) => selectAll(e.target.checked)" />
    <span class="mx-3">Select All</span>
    <div
      :class="item.isHidden ? 'hidden' : ''"
      class="mt-4 space-y-2"
      v-for="(item, index) in items"
      :key="index"
    >
      <div class="flex justify-between items-center">
        <div class="flex">
          <input
            type="checkbox"
            v-model="item.isDone"
            :checked="item.isDone"
            @change="() => !item.isDone"
          />

          <div
            class="block mx-3 w-full h-full rounded-lg border border-gray-700 p-2 hover:border-pink-600"
          >
            <p
              class="text-lg font-medium text-gray-300 w-full"
              :class="
                item.isDone
                  ? 'line-through decoration-4 decoration-pink-200'
                  : ''
              "
            >
              {{ item.message }}
            </p>
          </div>
        </div>
        <svg
          @click="() => remove(index)"
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 24 24"
          stroke-width="1.5"
          stroke="currentColor"
          class="w-6 h-6 text-white cursor-pointer hover:text-red-700 ml-5"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            d="M14.74 9l-.346 9m-4.788 0L9.26 9m9.968-3.21c.342.052.682.107 1.022.166m-1.022-.165L18.16 19.673a2.25 2.25 0 01-2.244 2.077H8.084a2.25 2.25 0 01-2.244-2.077L4.772 5.79m14.456 0a48.108 48.108 0 00-3.478-.397m-12 .562c.34-.059.68-.114 1.022-.165m0 0a48.11 48.11 0 013.478-.397m7.5 0v-.916c0-1.18-.91-2.164-2.09-2.201a51.964 51.964 0 00-3.32 0c-1.18.037-2.09 1.022-2.09 2.201v.916m7.5 0a48.667 48.667 0 00-7.5 0"
          />
        </svg>
      </div>
    </div>
  </article>
  <div>
    <ul
      class="fixed bottom-10 justify-center flex right-[35%] left-[35%] border p-5"
    >
      <li class="mx-6">Done : {{ count.Done }}</li>
      <li class="mx-6">Not Done : {{ count.toBeDone }}</li>
      <li class="mx-6">total : {{ count.Done + count.toBeDone }}</li>
    </ul>
  </div>
</template>
<script>
// import List from "./components/list.vue";
export default {
  data() {
    return {
      message: "",
      items: [],
      console: console,
      articleHidden: true,
      transcribedText: "",
    };
  },
  methods: {
    selectAll(value) {
      this.items.forEach((item) => (item.isDone = value));
    },
    addToList() {
      this.items.push({
        message: this.message,
        isHidden: false,
        isDone: false,
      });
      this.articleHidden = false;
      this.message = "";
    },
    remove(index) {
      this.items[index].isHidden = true;
      this.count.AllCount < 1
        ? (this.articleHidden = true)
        : (this.articleHidden = false);
    },
    startSpeechRecognition() {
      const speechRecognition =
        window.SpeechRecognition || window.webkitSpeechRecognition;
      const recognition = new speechRecognition();
      recognition.start();
      recognition.lang = "en-US";
      recognition.onresult = (event) => {
        this.message = event.results[0][0].transcript;
      };
    },
  },
  computed: {
    count() {
      return {
        Done: this.items.filter((e) => !e.isHidden && e.isDone).length,
        toBeDone: this.items.filter((e) => !e.isHidden && !e.isDone).length,
        AllCount: this.items.filter((e) => !e.isHidden).length,
      };
    },
  },
};
</script>
