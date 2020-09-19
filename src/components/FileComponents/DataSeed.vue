<template>
  <v-btn @click="seedData()" primary>Seed data for test</v-btn>
</template>

<script>
export default {
  name: "DataSeed",
  methods: {
    seedData() {
      this.$refs.child[0].field.name = "Matricule";
      this.$refs.child[0].field.type = "number";
      this.$refs.child[0].field.range.type = "range";
      this.$refs.child[0].field.range.content.from = 0;
      this.$refs.child[0].field.range.content.to = 1000;
      this.$refs.child[1].field.name = "Nom";
      this.$refs.child[1].field.type = "text";
      this.$refs.child[1].field.range.type = "list";
      this.$refs.child[1].field.range.content.list = [
        "Karl",
        "Mathie",
        "Jean",
        "Marc",
        "Pierre",
      ];
      this.$refs.child[2].field.name = "N° badge";
      this.$refs.child[2].field.type = "number";
      this.$refs.child[2].field.range.type = "range";
      this.$refs.child[2].field.range.content.from = 10000;
      this.$refs.child[2].field.range.content.to = 1000000;
    },
  },
};
</script>

<style>
</style>