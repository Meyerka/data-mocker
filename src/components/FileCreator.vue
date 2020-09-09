<template>
  <div>
    <h1>Select fields</h1>
    <button @click="addField(1)">+</button>
    <button @click="addField(-1)">-</button>
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
      <tbody v-for="field in fieldNumber" :key="field">
        <FieldRow v-model="dataGrid" :rowNumber="rowNumber" @update-records="updateGrid" />
      </tbody>
    </table>

    <div>
      Number of rows
      <input type="number" v-model="rowNumber" />
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
    {{dataGrid}}
    <button @click="downloadCsv()">Generate csv file</button>
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
    fieldNumber: 2,
    dataGrid: [],
    rowNumber: 1,
  }),
  methods: {
    addField(number) {
      this.fieldNumber += number;
    },
    updateGrid(records) {
      this.dataGrid.push(records);
    },
    downloadCsv() {
      const rows = [
        [this.records.name, "column2", "column3"],
        [this.records.value, "city2", "more info"],
      ];

      let csvContent = "data:text/csv;charset=utf-8,";

      rows.forEach(function (rowArray) {
        let row = rowArray.join(",");
        csvContent += row + "\r\n";
      });
      let encodedUri = encodeURI(csvContent);
      var link = document.createElement("a");
      link.setAttribute("href", encodedUri);
      link.setAttribute("download", "my_data.csv");
      document.body.appendChild(link); // Required for FF

      link.click();
    },
  },
};
</script>

<style>
</style>
