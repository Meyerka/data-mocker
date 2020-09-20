<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <v-toolbar-title>Data mocker</v-toolbar-title>
      <v-spacer></v-spacer>

      <a class="mx-6" href="https://github.com/Meyerka/data-mocker" target="_blank">
        <v-img src="./assets/githubLogo.png"></v-img>
      </a>
      <v-btn v-if="!overlay" class="white--text" color="teal" @click="overlay = !overlay">Help</v-btn>
      <v-btn
        v-else
        class="white--text"
        color="red"
        @click="overlay = !overlay"
        absolute
        right
      >Hide help</v-btn>
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
