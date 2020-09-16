<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <HelpIcon />
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
    //
  }),
};
</script>
