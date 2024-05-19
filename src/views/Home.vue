<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <div class="flex">
    <div class="container m-auto px-5 py-24">
      <div class="mb-6 flex w-full flex-col text-center">
        <h1 class="text-4xl font-bold tracking-tight text-blue-600 sm:text-6xl">Colour Palettes</h1>
        <p class="dark:text-white">v{{ version }} | Last updated: 10/12/2023</p>
        <p class="mx-auto hidden text-base leading-relaxed dark:text-white sm:block lg:w-2/3">
          To add a palette edit the
          <a
            class="text-blue-600 underline"
            href="https://github.com/ridwanazeez/colourpalettes/blob/master/public/palettes.json"
            target="_blank"
            >JSON</a
          >
          and make a pull request. No guarantees I'll add it though ¯\_(ツ)_/¯
        </p>
        <p class="mx-auto text-base leading-relaxed dark:text-white sm:hidden lg:w-2/3">
          To add a palette edit the
          <a
            class="text-blue-600 underline"
            href="https://github.com/ridwanazeez/colourpalettes/blob/master/public/palettes.json"
            target="_blank"
            >JSON</a
          >
          and make a pull request. No guarantees I'll add it though <br />
          ¯\_(ツ)_/¯
        </p>
      </div>
      <div
        class="grid grid-cols-2 justify-items-center gap-8 sm:grid-cols-3 lg:grid-cols-4 xl:grid-cols-6"
      >
        <div v-for="(theme, index) of palettes" :key="index" class="w-full">
          <PaletteCard
            class="dark:text-white"
            :title="theme.name"
            :colours="theme.colours"
            :tags="theme.tags"
            :author="theme.author"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { version } from "../../package.json";
import PaletteCard from "../components/PaletteCard.vue";

export default {
  components: {
    PaletteCard,
  },
  data() {
    return {
      palettes: null,
      version: version,
    };
  },
  async mounted() {
    this.getData();
  },
  methods: {
    async getData() {
      try {
        const response = await fetch("https://raw.githubusercontent.com/ridwanazeez/colourpalettes/master/public/palettes.json");

        if (!response.ok) {
          throw new Error(`HTTP error! Status: ${response.status}`);
        }

        const data = await response.json();

        // Sort the data by the "name" property in alphabetical order
        data.sort((a, b) => a.name.localeCompare(b.name));

        this.palettes = data;
      } catch (error) {
        console.error("Error fetching data:", error);
      }
    },
  },
};
</script>
