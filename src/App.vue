<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <v-spacer></v-spacer>
      <v-toolbar-title> What did you drink? </v-toolbar-title>

      <v-spacer></v-spacer>
    </v-app-bar>

    <v-main>
      <v-container fill-height>
        <v-card
          class="mx-auto my-auto"
          elevation=""
          max-width=""
          tile
          width="400px"
        >
          <v-list-item v-for="drink in drinks" :key="drink.value">
            <v-list-item-content>
              <v-checkbox
                v-model="selected"
                :label="drink.label"
                :value="drink.value"
              ></v-checkbox>
            </v-list-item-content>
          </v-list-item>
          <v-card-actions>
            <v-btn tile color="info" class="mx-auto">
              <v-icon left> mdi-send </v-icon>
              Send
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: "App",

  components: {},

  data() {
    return {
      drinks: [
        {
          label: "Water",
          value: "water",
        },
        {
          label: "Coffee",
          value: "coffee",
        },
        {
          label: "Latte",
          value: "Latte",
        },
        {
          label: "Tea",
          value: "tea",
        },
        {
          label: "Alcohol",
          value: "alcohol",
        },
      ],
      selected: [],
    };
  },
};
</script>
