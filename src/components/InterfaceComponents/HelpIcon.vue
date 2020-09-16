<template>
  <v-tooltip v-model="show" top>
    <template v-slot:activator="{ on, attrs }">
      <v-btn icon v-bind="attrs" v-on="on">
        <v-icon color="grey lighten-1">mdi-cart</v-icon>
      </v-btn>
    </template>
    <span>Programmatic tooltip</span>
  </v-tooltip>
</template>

<script>
export default {
  name: "HelpIcon",
  props: {},
};
</script>

<style>
</style>