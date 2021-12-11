<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <v-spacer></v-spacer>
      <v-toolbar-title> {{ $t("What did you drink today?") }} </v-toolbar-title>

      <v-spacer></v-spacer>
    </v-app-bar>

    <v-main>
      <v-container fill-height>
        <div v-if="isSuccess" class="mx-auto text-center green--text">
          <v-icon style="font-size: 100pt;" color="green"> mdi-emoticon-happy-outline </v-icon>
          <h1> {{ $t("Well done!") }} </h1>
        </div>
        <div v-else-if="isFailure" class="mx-auto text-center red--text">
          <v-icon style="font-size: 100pt;" color="red"> mdi-emoticon-sad-outline </v-icon>
          <h1> {{ $t("I am not satisfied...") }} </h1>
        </div>
        <v-card v-else
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
                :label="$t(drink.label)"
                :value="drink.value"
              ></v-checkbox>
            </v-list-item-content>
          </v-list-item>
          <v-card-actions>
            <v-btn
              tile
              color="info"
              class="mx-auto"
              @click="sendDrinks"
              :loading="isSendingDrinks"
            >
              <v-icon left> mdi-send </v-icon>
              {{ $t("Send") }}
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-container>
    </v-main>
    <v-snackbar v-model="snackbar.show" :timeout="snackbar.time">
      {{ snackbar.text }}

      <template v-slot:action="{ attrs }">
        <v-btn color="pink" text v-bind="attrs" @click="snackbar.show = false">
          {{ $t("Close") }}
        </v-btn>
      </template>
    </v-snackbar>
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
      isSendingDrinks: false,
      snackbar: {
        show: false,
        text: null,
        timeout: 1000,
      },
      isSuccess: false,
      isFailure: false,
    };
  },

  methods: {
    sendDrinks() {
      this.isSendingDrinks = true;
      setTimeout(() => {
        this.isSendingDrinks = false;
        this.isSuccess = false;
        this.isFailure = false;
        if (!this.selected.length) {
          this.snackbar.text = this.$t("You died of dehydration");
          this.snackbar.show = true;
        } else if (this.selected.includes('alcohol')) {
          this.isFailure = true;
        } else {
          this.isSuccess = true;
        }
      }, 0);
    },
  },
};
</script>
