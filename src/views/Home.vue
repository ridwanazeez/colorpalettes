<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <div class="flex">
    <div class="container m-auto px-5 py-24">
      <div class="mb-10 flex w-full flex-col text-center">
        <h1 class="mb-4 text-4xl font-bold tracking-tight text-blue-600 sm:text-6xl">Palettes</h1>
      </div>
      <div class="grid justify-items-center gap-8 sm:grid-cols-3 lg:grid-cols-4 xl:grid-cols-6">
        <div v-for="(theme, index) of palettes" :key="index">
          <PaletteCard
            class="dark:text-white"
            :title="theme.name"
            :colours="theme.colours"
            :tags="theme.tags"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import PaletteCard from "../components/PaletteCard.vue";

export default {
  components: {
    PaletteCard,
  },
  data() {
    return {
      palettes: null,
    };
  },
  async mounted() {
    this.getData();
  },
  methods: {
    async getData() {
      try {
        const response = await fetch(
          "https://raw.githubusercontent.com/ridwanazeez/colourpalettes-JSON/master/palettes.json",
        );

        if (!response.ok) {
          throw new Error(`HTTP error! Status: ${response.status}`);
        }

        const data = await response.json();
        this.palettes = data;
      } catch (error) {
        console.error("Error fetching data:", error);
      }
    },
  },
};
</script>
