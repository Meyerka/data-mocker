<template>
  <div class="ma-6">
    <v-alert
      :value="fileAlert"
      type="error"
      transition="scale-transition"
    >The file name you entered is not valid.</v-alert>
    <v-row>
      <v-col cols="12">
        <v-btn @click="addField(-1)" class="mx-2" fab dark small color="error">
          <v-icon dark>mdi-minus</v-icon>
        </v-btn>
        <v-btn @click="addField(1)" class="mx-2" fab dark small color="success">
          <v-icon dark>mdi-plus</v-icon>
        </v-btn>
        <v-container fluid v-for="field in fieldNumber" :key="field">
          <FieldRow
            v-model="dataGrid"
            :rowNumber="parseInt(rowNumber)"
            @update-records="updateGrid"
            ref="child"
          />
        </v-container>
      </v-col>
      <v-col cols="6">
        <v-container>
          <v-card>
            <v-card-title class="primary dark">
              <span class="headline white--text">File options</span>

              <v-spacer></v-spacer>
            </v-card-title>
            <v-card-text>
              <v-row>
                <v-col>
                  <v-text-field label="Number of rows" type="number" v-model="rowNumber"></v-text-field>
                </v-col>
              </v-row>
              <v-row>
                <v-col cols="8">
                  <v-select
                    v-if="fileExtension!='json'"
                    :items="fieldSelect"
                    v-model="fieldSeparator"
                    label="Field seperator"
                  ></v-select>
                </v-col>
                <v-col>
                  <v-checkbox v-model="isHeaderIncluded" persistent-hint hint="Include header" />
                </v-col>
              </v-row>
              <v-row>
                <v-col cols="8">
                  <v-text-field label="File name" v-model="fileName" @keyup="validateFileName"></v-text-field>
                </v-col>
                <v-col cols="4">
                  <v-select :items="fileExtensionSelect" v-model="fileExtension" label="Extension"></v-select>
                </v-col>
              </v-row>
              <v-row>
                <v-btn
                  v-if="!fileAlert"
                  @click="downloadFile()"
                  color="primary"
                  absolute
                  right
                  class="my-6"
                >Generate file</v-btn>
              </v-row>
            </v-card-text>
          </v-card>
        </v-container>
      </v-col>
    </v-row>
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
    fileAlert: false,
    fieldNumber: 3,
    dataGrid: [],
    rowNumber: 1,
    fileName: "file name",
    fileExtension: "csv",
    isHeaderIncluded: true,
    fieldSeparator: ";",
    fieldSelect: [
      { text: "Semicolon ;", value: ";" },
      { text: "Comma ,", value: "," },
      { text: "Tab", value: " " },
    ],
    fileExtensionSelect: [
      { text: ".txt", value: "txt" },
      { text: ".csv", value: "csv" },
      { text: ".json", value: "json" },
    ],
  }),

  methods: {
    addField(number) {
      this.fieldNumber += number;
    },
    updateGrid(records) {
      this.dataGrid.push(records);
    },
    downloadFile() {
      this.generateGrid();
      let generatedFile;
      this.fileExtension === "json"
        ? (generatedFile = this.generateJSON())
        : (generatedFile = this.generateCSV());
      this.dataGrid = [];
      let link = document.createElement("a");
      link.setAttribute("href", generatedFile);
      link.setAttribute("download", `${this.fileName}.${this.fileExtension}`);
      document.body.appendChild(link);
      link.click();
    },
    async generateGrid() {
      for (let i = 0; i < this.fieldNumber; i++) {
        this.$refs.child[i].generateObject();
      }
      await this.alignGroupedFields();
    },
    generateCSV() {
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
      return encodeURI(csvContent);
    },
    generateJSON() {
      return (
        "data:text/json;charset=utf-8," +
        encodeURIComponent(JSON.stringify(this.dataGrid))
      );
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
        this.fileAlert = true;
      } else {
        this.fileAlert = false;
      }
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
.container {
  height: 4rem;
}
</style>

