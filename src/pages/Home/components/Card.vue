<script>
  import { useUserStore } from "@/store/userStore";
  import { mapActions, mapState } from "pinia";
  import CardDetails from "@/components/CardDetails.vue";

  export default {
    props: {
      source: {
        type: Object,
        required: true,
      },
    },
    data() {
      return { isRecipeDetails: false };
    },
    computed: {
      ...mapState(useUserStore, ["favoritesIDs", "isAuthenticated"]),
      isFavorite() {
        return this.favoritesIDs.includes(this.source.id);
      },
    },
    methods: {
      ...mapActions(useUserStore, ["addFavorite", "removeFavorite"]),

      handleFavorite(id) {
        if (this.isAuthenticated && !this.favoritesIDs.includes(id)) {
          this.addFavorite(id);
        } else if (this.isAuthenticated) {
          this.removeFavorite(id);
        }
      },
      onDetailsClose() {
        this.isRecipeDetails = false;
      },
    },
  };
</script>

<template>
  <v-card
    class="card"
    width="300"
    color="background"
  >
    <v-img
      height="200px"
      :src="source.image"
      cover
      class="relative-container"
    >
      <template v-slot:placeholder>
        <div class="d-flex align-center justify-center fill-height">
          <v-progress-circular
            color="primary"
            indeterminate
          ></v-progress-circular>
        </div>
      </template>

      <div class="relative-container">
        <v-icon
          class="favorite-btn"
          :color="isFavorite === 'true' ? 'red' : 'primary'"
          @click="handleFavorite(source.id)"
        >
          {{ isFavorite ? "mdi-heart" : "mdi-heart-outline" }}
        </v-icon>
      </div>
    </v-img>

    <v-card-title class="text-wrap">{{ source.name }}</v-card-title>

    <v-card-subtitle>
      <v-icon color="primary">mdi-message-star-outline</v-icon>
      {{ source.rating }}
    </v-card-subtitle>

    <v-card-actions>
      <v-chip
        color="accent"
        :text="source.cuisine"
      ></v-chip>
      <v-chip
        :color="
          source.difficulty === 'Easy'
            ? 'green-lighten-2'
            : source.difficulty === 'Medium'
              ? 'orange-lighten-2'
              : 'red-lighten-2'
        "
        :text="source.difficulty"
      ></v-chip>

      <v-spacer></v-spacer>

      <v-btn @click="isRecipeDetails = true">
        <v-icon
          color="primary"
          size="x-large"
        >
          mdi-dots-horizontal-circle-outline
        </v-icon>
      </v-btn>
    </v-card-actions>
  </v-card>

  <CardDetails
    :source="source"
    :isRecipeDetails="isRecipeDetails"
    @on-close="onDetailsClose"
  />
</template>

<style scoped>
  .card {
    margin-top: 25px;
  }
  .location {
    text-align: center;
    text-decoration: none;
    color: #4a90e2;
  }

  .relative-container {
    position: relative;
  }

  .favorite-btn {
    position: absolute;
    top: 8px;
    right: 8px;
    z-index: 10;
    cursor: pointer;
  }
</style>
