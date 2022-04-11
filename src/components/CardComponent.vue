<template>
  <v-card class="mx-auto" max-width="250">
    <v-img :src="image_url" height="150px"></v-img>

    <v-card-title> {{ name }} </v-card-title>

    <v-card-subtitle> {{ tagline }} </v-card-subtitle>

    <v-card-actions>
      <v-btn color="orange lighten-2" text> See More </v-btn>

      <v-spacer></v-spacer>

      <v-btn icon @click="show = !show">
        <v-icon>{{ show ? "mdi-chevron-up" : "mdi-chevron-down" }}</v-icon>
      </v-btn>
    </v-card-actions>

    <v-expand-transition>
      <div v-show="show">
        <v-divider></v-divider>

        <v-card-text>
          {{ brewer_tips }}
        </v-card-text>
      </div>
    </v-expand-transition>
  </v-card>
</template>

<script>
export default {
  name: "CardComponent",

  props: {
    name: String,
    tagline: String,
    brewer_tips: String,
    image_url: String,
  },

  data: () => ({
    show: false,
  }),
};
</script>

<style scoped >
::v-deep .v-image__image--cover{
    background-size: contain;
}
</style>