<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <HelpIcon />
      <v-btn class="white--text" color="teal" @click="overlay = !overlay">Show Overlay</v-btn>

      <v-overlay z-index="3" :value="overlay">
        <v-btn class="white--text" color="teal" @click="overlay = false">Hide Overlay</v-btn>
      </v-overlay>
    </v-app-bar>

    <v-main>
      <v-container fluid>
        <FileCreator />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import FileCreator from "./components/FileCreator";
import HelpIcon from "./components/InterfaceComponents/HelpIcon.vue";

export default {
  name: "App",
  components: {
    FileCreator,
    HelpIcon,
  },

  data: () => ({
    overlay: false,
    //
  }),
};
</script>
