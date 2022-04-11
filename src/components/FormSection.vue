<template>
  <v-card color="basil" elevation="0">
    <v-toolbar color="blue" class="mb-4">
      <v-app-bar-nav-icon class="hidden-sm-and-down"></v-app-bar-nav-icon>
      <v-toolbar-title class="text-h6 mr-6 hidden-sm-and-down">
        Beer
      </v-toolbar-title>
      <v-autocomplete
        v-model="model"
        :items="items"
        :loading="isLoading"
        :search-input.sync="search"
        chips
        clearable
        hide-details
        hide-selected
        item-text="name"
        item-value="symbol"
        label="Search for a beer..."
        solo
      >
        <template v-slot:no-data>
          <v-list-item>
            <v-list-item-title>
              Search for your favorite
              <strong>Beer</strong>
            </v-list-item-title>
          </v-list-item>
        </template>
        <template v-slot:selection="{ attr, on, item, selected }">
          <v-chip
            v-bind="attr"
            :input-value="selected"
            color="blue-grey"
            class="white--text"
            v-on="on"
          >
            <v-icon left> mdi-glass-mug-variant </v-icon>
            <span v-text="item.name"></span>
          </v-chip>
        </template>
        <template v-slot:item="{ item }">
          <v-list-item-avatar
            color="indigo"
            class="text-h5 font-weight-light white--text"
          >
            {{ item.name.charAt(0) }}
          </v-list-item-avatar>
          <v-list-item-content>
            <v-list-item-title v-text="item.name"></v-list-item-title>
            <v-list-item-subtitle v-text="item.symbol"></v-list-item-subtitle>
          </v-list-item-content>
          <v-list-item-action>
            <v-icon>mdi-glass-mug-variant</v-icon>
          </v-list-item-action>
        </template>
      </v-autocomplete>
      <template v-slot:extension>
        <v-tabs v-model="tab" color="blue-grey" slider-color="blue-grey">
          <v-tab> All</v-tab>

        </v-tabs>
      </template>
    </v-toolbar>

    <v-tabs-items v-model="tab">
      <v-tab-item>
        <v-container>
          <v-row>
            <v-col v-for="item in items " :key="item.id" sm="4" lg="3">
              <card-component
                :tagline="item.tagline"
                :image_url="item.image_url"
                :name="item.name"
                :brewer_tips="item.brewers_tips"
              />
            </v-col>
          </v-row>
        </v-container>
      </v-tab-item>

      
    </v-tabs-items>
  </v-card>
</template>

<script>
import { debounce } from "lodash";
import CardComponent from "./CardComponent.vue";
export default {
  components: { CardComponent },
  name: "FormSection",

  data: () => ({
    isLoading: false,
    items: [],
    model: null,
    search: null,
    tab: 0,
    show: false,
  }),

  watch: {
    model(val) {
      if (val != null) this.tab = 0;
      else this.tab = null;
    },
    search() {
      this.searchApi();
    },
  },

  methods: {
    searchApi: debounce(function () {
      this.fetchData();
    }, 500),

    fetchData() {
      this.isLoading = true;
      let path = "https://api.punkapi.com/v2/beers";
      if (this.search) {
        path += `?beer_name=${this.search}`;
      }
      // Lazily load input items
      fetch(path)
        .then((res) => res.clone().json())
        .then((res) => {
          this.items = res;
        })
        .catch((err) => {
          console.log(err);
        })
        .finally(() => (this.isLoading = false));
    },
  },

  created() {
    this.fetchData();
  },
};
</script>

<style scoped>
::v-deep .v-toolbar__content {
  padding-top: 24px;
}
</style>
