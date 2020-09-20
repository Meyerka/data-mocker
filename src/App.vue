<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <v-btn v-if="!overlay" class="white--text" color="teal" @click="overlay = !overlay">Help</v-btn>
      <v-btn v-else class="white--text" color="red" @click="overlay = !overlay">Hide help</v-btn>
    </v-app-bar>

    <v-main>
      <HelpOverlay :overlay="overlay" />
      <v-container fluid>
        <FileCreator />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import FileCreator from "./components/FileCreator";
import HelpOverlay from "./components/FileComponents/HelpOverlay";
export default {
  name: "App",
  components: {
    FileCreator,
    HelpOverlay,
  },

  data: () => ({
    overlay: false,
  }),
};
</script>
