<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <v-spacer></v-spacer>
      <v-toolbar-title> {{ $t("What did you drink today?") }} </v-toolbar-title>

      <v-spacer></v-spacer>
    </v-app-bar>

    <v-main>
      <v-container>
        <LanguagePicker class="mb-5" />
        <v-stepper v-model="currentStep" width="800px" class="mx-auto my-auto">
          <v-stepper-header>
            <v-stepper-step :complete="currentStep > 1" step="1">
              {{ $t("Did you drink today?") }}
            </v-stepper-step>
            <v-divider></v-divider>
            <v-stepper-step :complete="currentStep > 2" step="2">
              {{ $t("What did you drink today?") }}
            </v-stepper-step>
            <v-divider></v-divider>
            <v-stepper-step :complete="currentStep == 3" step="3">
              {{ $t("Result") }}
            </v-stepper-step>
          </v-stepper-header>
          <v-stepper-items>
            <v-stepper-content step="1">
              <h3 class="text-center mb-4">
                {{ $t("Did you drink today?") }}
              </h3>
              <v-card-actions>
                <v-btn
                  tile
                  color="success"
                  class="mx-auto"
                  @click="currentStep++"
                >
                  <v-icon left> mdi-check </v-icon>
                  {{ $t("Yes") }}
                </v-btn>
                <v-btn tile color="error" class="mx-auto" @click="didNotDrink">
                  <v-icon left> mdi-close </v-icon>
                  {{ $t("No") }}
                </v-btn>
              </v-card-actions>
            </v-stepper-content>

            <v-stepper-content step="2">
              <h3 class="text-center mb-4">
                {{ $t("What did you drink today?") }}
              </h3>
              <v-list>
                <v-list-item v-for="drink in drinks" :key="drink.value">
                  <v-list-item-content>
                    <v-checkbox
                      v-model="selected"
                      :label="$t(drink.label)"
                      :value="drink.value"
                      :disabled="isSendingDrinks"
                    ></v-checkbox>
                  </v-list-item-content>
                </v-list-item>
              </v-list>
              <v-divider></v-divider>
              <v-card-actions>
                <v-btn
                  tile
                  color="primary"
                  class="mx-auto"
                  @click="sendDrinks"
                  :loading="isSendingDrinks"
                >
                  <v-icon left> mdi-send </v-icon>
                  {{ $t("Send") }}
                </v-btn>
              </v-card-actions>
            </v-stepper-content>

            <v-stepper-content step="3">
              <div v-if="isSuccess" class="mx-auto text-center green--text">
                <v-icon style="font-size: 100pt" color="success">
                  mdi-emoticon-happy-outline
                </v-icon>
                <h1>{{ $t("Well done!") }}</h1>
              </div>
              <div v-if="isFailure" class="mx-auto text-center red--text">
                <v-icon style="font-size: 100pt" color="error">
                  mdi-emoticon-sad-outline
                </v-icon>
                <h1>{{ $t("I am not satisfied...") }}</h1>
              </div>
              <div
                v-if="isNeedsDrink"
                class="mx-auto text-center warning--text"
              >
                <v-icon style="font-size: 100pt" color="warning">
                  mdi-emoticon-neutral-outline
                </v-icon>
                <h1>{{ $t("Go drink some water") }}</h1>
              </div>
              <v-card-actions>
                <v-btn tile color="primary" class="mx-auto" @click="repeat">
                  <v-icon left> mdi-repeat </v-icon>
                  {{ $t("Repeat") }}
                </v-btn>
              </v-card-actions>
            </v-stepper-content>
          </v-stepper-items>
        </v-stepper>
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
import drinks from "./assets/drinks.json";
import LanguagePicker from "./components/LanguagePicker.vue";

export default {
  name: "App",

  components: {
    LanguagePicker,
  },

  data() {
    return {
      currentStep: 1,

      drinks: drinks,
      selected: [],

      snackbar: {
        show: false,
        text: null,
        timeout: 1000,
      },

      isNeedsDrink: false,
      isSendingDrinks: false,
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
          this.snackbar.text = this.$t("You died of dehydration?");
          this.snackbar.show = true;
        } else if (this.selected.includes("alcohol")) {
          this.isFailure = true;
        } else {
          this.isSuccess = true;
        }
        this.currentStep++;
      }, 2000);
    },

    didNotDrink() {
      this.isNeedsDrink = true;
      this.currentStep = 3;
    },

    repeat() {
      Object.assign(this.$data, this.$options.data());
    },
  },
};
</script>
