<script>
  export default {
    data() {
      return {
        icons: [
          { name: "mdi-web", url: "" },
          { name: "mdi-twitter", url: "" },
          { name: "mdi-instagram", url: "" },
        ],
      };
    },
  };
</script>

<template>
  <v-footer
    color="primary"
    class="bg-indigo-lighten-1 text-center d-flex flex-column"
  >
    <div>
      <v-btn
        v-for="icon in icons"
        :key="`icon-${icon}`"
        :icon="icon.name"
        :href="icon.url"
        variant="text"
        class="mx-4"
        color="text"
      ></v-btn>
    </div>

    <v-divider></v-divider>

    <div class="text">
      <v-icon
        size="24"
        class="mb-2"
      >
        mdi-chef-hat
      </v-icon>

      <strong>Yum Yum</strong>
    </div>
  </v-footer>
</template>

<style scoped>
  .text {
    color: #4a3222;
  }
</style>
