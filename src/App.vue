<template>
  <v-app>
    <v-main>
      <FormSection />
    </v-main>
  </v-app>
</template>

<script>
import FormSection from "./components/FormSection.vue";

export default {
  name: "App",

  components: {
    FormSection,
  },

  data: () => ({
    //
  }),
};
</script>
