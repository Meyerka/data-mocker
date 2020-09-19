<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <v-btn class="white--text" color="teal" @click="overlay = !overlay">Help</v-btn>
    </v-app-bar>

    <v-main>
      <HelpOverlay />
      <v-container fluid>
        <FileCreator />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import FileCreator from "./components/FileCreator";
import HelpOverlay from "./components/InterfaceComponents/HelpOverlay";
export default {
  name: "App",
  components: {
    FileCreator,
    HelpOverlay,
  },

  data: () => ({
    overlay: false,
    //
  }),
};
</script>
