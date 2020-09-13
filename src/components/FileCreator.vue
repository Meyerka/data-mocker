<template>
  <div>
    <button @click="seedData()">Seed data for test</button>
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
          <th>Group</th>
        </tr>
      </thead>
      <tbody v-for="field in fieldNumber" :key="field">
        <FieldRow
          v-model="dataGrid"
          :rowNumber="parseInt(rowNumber)"
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
      <input type="checkbox" v-model="isHeaderIncluded" />
    </div>
    <div>
      Field seperator
      <select v-model="fieldSeparator">
        <option value=";">Semicolon ;</option>
        <option value=",">Comma ,</option>
      </select>
    </div>

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
    fieldNumber: 3,
    dataGrid: [],
    rowNumber: 1,
    fileName: "file name",
    fileExtension: "csv",
    isHeaderIncluded: true,
    fieldSeparator: ";",
  }),
  methods: {
    addField(number) {
      this.fieldNumber += number;
    },
    updateGrid(records) {
      this.dataGrid.push(records);
    },
    async downloadCsv() {
      await this.generateGrid();
      let csvContent = "data:text/csv;charset=utf-8,";
      const sep = this.fieldSeparator;
      if (this.isHeaderIncluded) {
        this.dataGrid.forEach(function (field) {
          csvContent += field.name + sep;
        });
        csvContent = csvContent.substring(0, csvContent.length - 1);
        csvContent += "\r\n";
      }
      for (let row = 0; row < this.rowNumber; row++) {
        this.dataGrid.forEach(function (field) {
          csvContent += field.values[row] + sep;
        });
        csvContent = csvContent.substring(0, csvContent.length - 1);
        csvContent += "\r\n";
      }
      let encodedUri = encodeURI(csvContent);
      var link = document.createElement("a");
      link.setAttribute("href", encodedUri);
      link.setAttribute("download", `${this.fileName}.${this.fileExtension}`);
      document.body.appendChild(link);
      console.log("must before");
      link.click();
    },
    generateGrid() {
      for (let i = 0; i < this.fieldNumber; i++) {
        this.$refs.child[i].generateObject();
      }
      this.alignGroupedFields();
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
    seedData() {
      this.$refs.child[0].field.name = "Matricule";
      this.$refs.child[0].field.type = "number";
      this.$refs.child[0].field.range.type = "range";
      this.$refs.child[0].field.range.content.from = 0;
      this.$refs.child[0].field.range.content.to = 1000;
      this.$refs.child[1].field.name = "Nom";
      this.$refs.child[1].field.type = "text";
      this.$refs.child[1].field.range.type = "list";
      this.$refs.child[1].field.range.content.list =
        "Karl;Mathie;Jean;Marc;Pierre";
      this.$refs.child[2].field.name = "N° badge";
      this.$refs.child[2].field.type = "number";
      this.$refs.child[2].field.range.type = "range";
      this.$refs.child[2].field.range.content.from = 10000;
      this.$refs.child[2].field.range.content.to = 1000000;
    },
    // TODO - refactor this mess
    alignGroupedFields() {
      //build the list of field indexes to group
      let groupedFieldsKeys = [];
      for (let i = 0; i < this.dataGrid.length; i++) {
        if (this.dataGrid[i].isGrouped) {
          groupedFieldsKeys.push(i);
        }
      }
      //consider first grouped field as key for lining up grouped fields and filter out unique values
      const uniqueValues = this.dataGrid[groupedFieldsKeys[0]].values.filter(
        (v, i, a) => a.indexOf(v) === i
      );
      //build mapping table to map unique key values to other fields' values
      let mappingTable = [];
      mappingTable.push([]);
      mappingTable[0].push(uniqueValues);
      const keyField = groupedFieldsKeys[0];
      for (let i = 1; i < groupedFieldsKeys.length; i++) {
        const fieldIndex = groupedFieldsKeys[i];
        mappingTable.push([]);
        for (const val of uniqueValues) {
          mappingTable[i].push(
            this.dataGrid[fieldIndex].values[
              this.dataGrid[keyField].values.indexOf(val)
            ]
          );
        }
      }
      for (let row = 0; row < this.dataGrid[0].values.length; row++) {
        const lookUpValue = this.dataGrid[0].values[row];
        for (let j = 1; j < groupedFieldsKeys.length; j++) {
          const fieldIndex = groupedFieldsKeys[j];
          this.dataGrid[fieldIndex].values[row] =
            mappingTable[j][uniqueValues.indexOf(lookUpValue)];
        }
      }
    },
  },
};
</script>

<style>
</style>

