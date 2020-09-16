<template>
  <v-app>
    <v-app-bar app color="primary" dark></v-app-bar>

    <v-main>
      <v-container fluid>
        <FileCreator />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import FileCreator from "./components/FileCreator";

export default {
  name: "App",
  components: {
    FileCreator,
  },

  data: () => ({
    //
  }),
};
</script>
