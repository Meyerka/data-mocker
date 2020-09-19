<template>
  <v-overlay z-index="3" :value="overlay" opacity="0.75">
    <h2>How to get started ?</h2>

    <h3>Name your fields and select the type of data.</h3>
    <h3>Type:</h3>
    <p>Select what kind of data the field will host</p>
    <h3>Range type:</h3>
    <p>Select how the data will be generated</p>
    <ul>
      <li>Range: Select the min and max values</li>
      <li>List: Select a pool of values to choose from</li>
      <li>Random: It will randomly fetch from a dataset based on faker api</li>
    </ul>

    <h3>Unicity:</h3>
    <p>
      The higher the value for the unicity, the more unique values will be present for this field.
      <br />For example, if maxed out, there will be only one value for this field, for all lines.
    </p>
    <h3>Groupe:</h3>
    <p>
      You can group 2 or more fields to "pair" their values. It will ensure the set of values will be coherent throughout the file rows.
      <br />For example, pair first name, name, and employee number to make sure the trio will always be the same for a given name.
    </p>
    <v-btn class="white--text" color="teal" @click="overlay = false">OK!</v-btn>
  </v-overlay>
</template>

<script>
export default {
  name: "HelpOverlay",
};
</script>

<style>
</style>