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
          <th>Range type</th>
          <th>Range content</th>
          <th>Unicity</th>
        </tr>
      </thead>
      <tbody v-for="row in rowNumber" :key="row">
        <FieldRow v-model="records" @update-records="update" />
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
    {{records}}
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
    rowNumber: 2,
    records: {},
  }),
  methods: {
    addRow(rows) {
      this.rowNumber += rows;
    },
    update(record) {
      this.records = record;
    },
  },
};
</script>

<style>
</style>
