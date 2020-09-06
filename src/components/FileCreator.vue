<template>
  <div>
    <h1>Select fields</h1>
    <button @click="addRow(1)">+</button>
    <button @click="addRow(-1)">-</button>
    <table>
      <thead>
        <tr>
          <th>Field name</th>
          <th>Type</th>
          <th>Range</th>
          <th>Unicity</th>
        </tr>
      </thead>
      <tbody v-for="row in rowNumber" :key="row">
        <FieldRow />
      </tbody>
    </table>

    <div>
      Number of rows
      <input type="number" name="rowNumber" />
    </div>

    <div>
      Include header
      <input type="checkbox" name="isHeaderIncluded" />
    </div>
    <div>
      Field seperator
      <select name="fieldSeparator">
        <option value>--Please choose an option--</option>
        <option value="semicolon">Semicolon ;</option>
        <option value="comma">Comma ,</option>
        <option value="tab">Tab</option>
      </select>
    </div>
  </div>
</template>

<script>
import FieldRow from "./FieldRow.vue";
export default {
  name: "FileCreator",
  components: {
    FieldRow,
  },
  data: () => ({
    rowNumber: 4,
  }),
  methods: {
    addRow(rows) {
      this.rowNumber += rows;
    },
  },
};
</script>

<style>
</style>
