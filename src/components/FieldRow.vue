<template>
  <v-row>
    <v-col cols="1" v-for="checkbox in checkboxes" :key="checkbox">
      <v-checkbox v-model="isGrouped" persistent-hint hint="Group" />
    </v-col>
    <v-col cols="2">
      <v-text-field label="Field name" v-model="field.name"></v-text-field>
    </v-col>
    <v-col cols="1">
      <v-select :items="typeSelect" v-model="field.type" label="Type"></v-select>
    </v-col>
    <v-col cols="1">
      <v-select :items="rangeTypeSelect" v-model="field.range.type" label="Range type"></v-select>
    </v-col>
    <v-col cols="4">
      <v-row v-if="field.type === 'date' && field.range.type === 'range'">
        <v-menu
          v-model="menu2"
          :close-on-content-click="false"
          :nudge-right="40"
          transition="scale-transition"
          offset-y
          min-width="290px"
        >
          <template v-slot:activator="{ on, attrs }">
            <v-text-field
              v-model="field.range.content.from"
              label="From"
              readonly
              v-bind="attrs"
              v-on="on"
            ></v-text-field>
          </template>
          <v-date-picker v-model="field.range.content.from" @input="menu2 = false"></v-date-picker>
        </v-menu>

        <v-menu
          v-model="menu3"
          :close-on-content-click="false"
          :nudge-right="40"
          transition="scale-transition"
          offset-y
          min-width="290px"
        >
          <template v-slot:activator="{ on, attrs }">
            <v-text-field
              v-model="field.range.content.to"
              label="To"
              readonly
              v-bind="attrs"
              v-on="on"
            ></v-text-field>
          </template>
          <v-date-picker v-model="field.range.content.to" @input="menu3 = false"></v-date-picker>
        </v-menu>
      </v-row>

      <v-row v-else-if="field.type === 'duration' && field.range.type === 'range'" id="dateRange">
        <v-text-field label="From" type="time" v-model="field.range.content.from"></v-text-field>
        <v-text-field label="From" type="time" v-model="field.range.content.to"></v-text-field>
      </v-row>
      <v-row
        no-gutter
        v-else-if="field.type === 'number' && field.range.type === 'range'"
        id="numberRange"
      >
        <v-text-field label="From" type="number" v-model="field.range.content.from"></v-text-field>
        <v-text-field label="To" type="number" v-model="field.range.content.to"></v-text-field>
      </v-row>
      <v-combobox
        v-else-if="field.range.type === 'list'"
        v-model="field.range.content.list"
        :chips="field.range.content.list"
        clearable
        label="List of items"
        multiple
        solo
      ></v-combobox>
      <v-select
        v-else-if="field.range.type === 'random'"
        :items="datasetSelect"
        v-model="field.range.content.api"
        label="Dataset"
        append-icon="mdi-book-search"
      ></v-select>
    </v-col>

    <v-col>
      <v-card flat color="transparent">
        <v-card-text>
          <v-slider
            v-model="field.unicity"
            color="orange"
            min="0"
            max="100"
            label="Unicity"
            thumb-label
          ></v-slider>
        </v-card-text>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
import faker from "faker";
export default {
  name: "FieldRow",
  props: {
    dataGrid: [],
    rowNumber: { type: Number },
  },
  data: () => ({
    field: {
      name: "",
      type: "",
      range: {
        type: "",
        content: {
          from: "",
          to: "",
          list: [],
          api: "",
        },
      },
      unicity: 50,
    },
    records: { name: "", values: [] },
    checkboxes: 1,
    isGrouped: false,
    typeSelect: [
      { text: "Text", value: "text" },
      { text: "Date", value: "date" },
      { text: "Number", value: "number" },
      { text: "Duration", value: "duration" },
    ],
    datasetSelect: [
      { text: "Full Name", value: "fullName" },
      { text: "Last Name", value: "lastName" },
      { text: "First Name", value: "firstName" },
      { text: "Email address", value: "email" },
      { text: "Company", value: "company" },
      { text: "Department", value: "department" },
      { text: "Job", value: "job" },
      { text: "City", value: "city" },
    ],
  }),
  computed: {
    rangeTypeSelect: function () {
      return [
        { text: "Random", value: "random" },
        { text: "List", value: "list" },
        {
          text: "Range",
          value: "range",
          disabled: !(
            this.field.type === "date" ||
            this.field.type === "number" ||
            this.field.type === "duration"
          ),
        },
      ];
    },
  },
  methods: {
    generateObject() {
      this.records.name = this.field.name;
      this.records.isGrouped = this.isGrouped;
      let valuesPool = [];
      const uniqueValues = Math.ceil(
        (parseInt(this.rowNumber) * this.field.unicity) / 100
      );
      for (let i = 0; i < uniqueValues; i++) {
        valuesPool.push(
          this.generateValue(this.field.type, this.field.range.type)
        );
      }
      for (let j = 0; j < this.rowNumber; j++) {
        const randomValueFromPool =
          valuesPool[Math.floor(Math.random() * valuesPool.length)];
        this.records.values.push(randomValueFromPool);
      }

      this.$emit("update-records", this.records);
      return this.records;
    },

    getRandomDateBetween(date1, date2) {
      date1 = date1 || "01-01-1970";
      date2 = date2 || new Date().toLocaleDateString();
      date1 = new Date(date1).getTime();
      date2 = new Date(date2).getTime();
      if (date1 > date2) {
        return new Date(
          this.getRandomValueBetween(date2, date1)
        ).toLocaleDateString();
      } else {
        return new Date(
          this.getRandomValueBetween(date1, date2)
        ).toLocaleDateString();
      }
    },
    getRandomValueBetween(min, max) {
      return Math.random() * (max - min) + min;
    },
    hoursToMinutes(hours, separator) {
      return (
        parseInt(hours.split(separator)[0] * 60) +
        parseInt(hours.split(separator)[1])
      );
    },
    minutesToHours(minutes, separator) {
      let hourPart = "00" + Math.floor(minutes / 60);
      let minutePart = "00" + (minutes % 60);
      hourPart = hourPart.slice(hourPart.length - 2);
      minutePart = minutePart.slice(minutePart.length - 2);
      return hourPart + separator + minutePart;
    },

    generateValue(type, rangeType) {
      if (rangeType === "list") {
        return this.getValueFromList();
      } else if (rangeType === "random") {
        return this.getValueFromRandom(this.field.range.content.api);
      } else if (rangeType === "range") {
        return this.getValueFromRange(type);
      }
      console.log("failed to generate random value based on the input");
    },
    getValueFromList() {
      const list = this.field.range.content.list;
      return list[Math.floor(Math.random() * list.length)];
    },
    getValueFromRandom(fieldToRandomize) {
      switch (fieldToRandomize) {
        case "fullName":
          return faker.name.findName();
        case "email":
          return faker.internet.email();
        case "city":
          return faker.address.city();
        case "department":
          return faker.commerce.department();
        case "company":
          return faker.company.companyName();
        case "firstName":
          return faker.name.firstName();
        case "lastName":
          return faker.name.lastName();
        case "job":
          return faker.name.jobTitle();
      }
    },
    getValueFromRange(rangeType) {
      let fromTimeInMinutes;
      let toTimeInMinutes;
      switch (rangeType) {
        case "date":
          return this.getRandomDateBetween(
            this.field.range.content.to,
            this.field.range.content.from
          );
        case "number":
          return Math.floor(
            this.getRandomValueBetween(
              this.field.range.content.from,
              this.field.range.content.to
            )
          );
        case "duration":
          fromTimeInMinutes = this.hoursToMinutes(
            this.field.range.content.from,
            ":"
          );
          toTimeInMinutes = this.hoursToMinutes(
            this.field.range.content.to,
            ":"
          );

          return this.minutesToHours(
            Math.floor(
              this.getRandomValueBetween(fromTimeInMinutes, toTimeInMinutes)
            ),
            ":"
          );

        default:
          break;
      }
    },
  },
};
</script>

<style>
</style>