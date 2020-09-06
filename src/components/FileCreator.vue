<template>
  <div>
    <h1>Select fields</h1>

    <table>
      <thead>
        <tr>
          <th>Field name</th>
          <th>Range</th>
          <th>Unicity</th>
        </tr>
      </thead>
      <tbody>
        <FieldRow />
        <FieldRow />
        <FieldRow />
        <FieldRow />
      </tbody>
    </table>
    <div>Number of rows</div>
    <div>Include header ?</div>
    <div>Field seperator</div>
  </div>
</template>

<script>
import FieldRow from "./FieldRow.vue";
export default {
  name: "FileCreator",
  components: {
    FieldRow,
  },
};
</script>

<style>
</style>
