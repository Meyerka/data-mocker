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
        <FieldRow
          v-model="dataGrid"
          :rowNumber="rowNumber"
          @update-records="updateGrid"
          ref="child"
        />
      </tbody>
    </table>
    <div>
      Nom du fichier
      <input type="text" v-model="fileName" @keyup="validateFileName" />
      <select v-model="fileExtension">
        <option value="csv">.csv</option>
        <option value="txt">.txt</option>
      </select>
    </div>
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

    <button @click="generateGrid()">Generate grid</button>
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
    fileName: "file name",
    fileExtension: "csv",
  }),
  methods: {
    addField(number) {
      this.fieldNumber += number;
    },
    updateGrid(records) {
      this.dataGrid.push(records);
    },
    downloadCsv() {
      let csvContent = "data:text/csv;charset=utf-8,";
      this.dataGrid.forEach(function (field) {
        csvContent += field.name + ";";
      });
      csvContent = csvContent.substring(0, csvContent.length - 1);
      csvContent += "\r\n";

      for (let row = 0; row < this.rowNumber; row++) {
        this.dataGrid.forEach(function (field) {
          csvContent += field.values[row] + ";";
        });
        csvContent = csvContent.substring(0, csvContent.length - 1);
        csvContent += "\r\n";
      }

      let encodedUri = encodeURI(csvContent);
      var link = document.createElement("a");
      link.setAttribute("href", encodedUri);
      link.setAttribute("download", `${this.fileName}.${this.fileExtension}`);
      document.body.appendChild(link);

      link.click();
    },
    generateGrid() {
      for (let i = 0; i < this.fieldNumber; i++) {
        this.$refs.child[i].generateObject();
      }
    },
    validateFileName() {
      var rg1 = /^[^\\/:*?"<>|]+$/; // forbidden characters \ / : * ? " < > |
      var rg2 = /^\./; // cannot start with dot (.)
      var rg3 = /^(nul|prn|con|lpt[0-9]|com[0-9])(\.|$)/i; // forbidden file names
      if (
        !rg1.test(this.fileName) ||
        rg2.test(this.fileName) ||
        rg3.test(this.fileName)
      ) {
        alert("Forbidden character in file name");
      }
    },
  },
};
</script>

<style>
</style>
