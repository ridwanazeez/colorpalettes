<template>
  <div class="w-48 rounded-2xl bg-white p-4 py-6 shadow-lg dark:bg-gray-800">
    <div class="flex flex-col justify-center">
      <h1 class="text-left text-xl font-medium text-gray-800 dark:text-white">
        {{ title }}
      </h1>
      <p class="mb-2 text-left text-xs dark:text-white">
        {{ author }}
      </p>
      <div
        v-for="(colour, index) of colours"
        :key="index"
        class="text-center text-xs dark:text-white"
      >
        <div
          class="mt-2 rounded p-4 shadow-md transition duration-300 hover:scale-105"
          :style="{
            backgroundColor: colour.hex,
          }"
        >
          <div class="flex items-center justify-center" @click="copyToClipboard(colour.hex)">
            <p class="font-bold" :style="{ color: getContrastingTextColor(colour.hex) }">
              {{ colour.hex }}
            </p>
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke-width="1.5"
              :stroke="getContrastingTextColor(colour.hex)"
              class="ml-2 h-4 w-4"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="M9 12h3.75M9 15h3.75M9 18h3.75m3 .75H18a2.25 2.25 0 002.25-2.25V6.108c0-1.135-.845-2.098-1.976-2.192a48.424 48.424 0 00-1.123-.08m-5.801 0c-.065.21-.1.433-.1.664 0 .414.336.75.75.75h4.5a.75.75 0 00.75-.75 2.25 2.25 0 00-.1-.664m-5.8 0A2.251 2.251 0 0113.5 2.25H15c1.012 0 1.867.668 2.15 1.586m-5.8 0c-.376.023-.75.05-1.124.08C9.095 4.01 8.25 4.973 8.25 6.108V8.25m0 0H4.875c-.621 0-1.125.504-1.125 1.125v11.25c0 .621.504 1.125 1.125 1.125h9.75c.621 0 1.125-.504 1.125-1.125V9.375c0-.621-.504-1.125-1.125-1.125H8.25zM6.75 12h.008v.008H6.75V12zm0 3h.008v.008H6.75V15zm0 3h.008v.008H6.75V18z"
              />
            </svg>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    title: { type: String, default: "" },
    author: { type: String, default: "" },
    tags: { type: Array, default: () => [] },
    colours: { type: Array, default: () => [] },
  },
  data() {
    return {};
  },
  computed: {
    textColor() {
      if (this.colours.length > 0) {
        return this.getContrastingTextColor(this.colours[0].hex);
      }
      return "black";
    },
  },
  methods: {
    copyToClipboard(text) {
      const textField = document.createElement("textarea");
      textField.innerText = text;
      document.body.appendChild(textField);
      textField.select();
      document.execCommand("copy");
      alert("Copied " + text + " to clipboard!");
      textField.remove();
    },
    getContrastingTextColor(backgroundColor) {
      // Determine the luminance of the background color
      const r = parseInt(backgroundColor.slice(1, 3), 16);
      const g = parseInt(backgroundColor.slice(3, 5), 16);
      const b = parseInt(backgroundColor.slice(5, 7), 16);
      const luminance = (0.299 * r + 0.587 * g + 0.114 * b) / 255;

      // Use a simple threshold to determine if text should be black or white
      return luminance > 0.5 ? "black" : "white";
    },
  },
};
</script>
