<template>
  <v-card class="mx-auto my-auto" tile width="400px">
    <v-list-item three-line>
      <v-list-item-content>
        <v-select
          v-if="$i18n"
          :items="langs"
          :label="$t('Language')"
          v-model="$i18n.locale"
        ></v-select>
      </v-list-item-content>
    </v-list-item>
  </v-card>
</template>

<script>
export default {
  data() {
    return {
      langs: ["ru", "en"],
    };
  },
};
</script>

<style></style>
