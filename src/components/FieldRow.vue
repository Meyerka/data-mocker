<template>
  <tr>
    <td>
      <input type="text" v-model="field.name" />
    </td>
    <td>
      <select v-model="field.type">
        <option value>--Type--</option>
        <option value="text">Text</option>
        <option value="date">Date</option>
        <option value="number">Number</option>
        <option value="duration">Duration</option>
      </select>
    </td>
    <td>
      <select name="fieldSeparator" v-model="field.range.type">
        <option value>--Range--</option>
        <option value="random">Randomly generated</option>
        <option value="list">List</option>
        <option
          v-if="field.type === 'date' || field.type === 'number' || field.type === 'duration'"
          value="range"
        >Range</option>
      </select>
    </td>
    <td>
      <div v-if="field.type === 'date' && field.range.type === 'range'" id="dateRange">
        From
        <input type="date" name="from" v-model="field.range.content.from" />
        To
        <input type="date" name="to" v-model="field.range.content.to" />
      </div>
      <div v-else-if="field.type === 'duration' && field.range.type === 'range'" id="dateRange">
        Between
        <input type="time" name="from" v-model="field.range.content.from" />
        And
        <input type="time" name="to" v-model="field.range.content.to" />
      </div>
      <div v-else-if="field.type === 'number' && field.range.type === 'range'" id="numberRange">
        Between
        <input type="number" name="from" v-model="field.range.content.from" />
        And
        <input type="number" name="to" v-model="field.range.content.to" />
      </div>
      <input type="text" v-model="field.range.content.list" v-else-if="field.range.type === 'list'" />
      <select v-model="field.range.content.api" v-else-if="field.range.type === 'random'">
        <option value>--Dataset--</option>
        <option value="name">Name</option>
        <option value="city">City</option>
        <option value="job">Job</option>
      </select>
    </td>
    <td>
      <input type="range" min="0" max="100" value="50" v-model="field.unicity" />
    </td>
  </tr>
</template>

<script>
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
          list: "",
          api: "",
        },
      },
      unicity: 50,
    },
    records: { name: "", values: [] },
  }),
  methods: {
    generateObject() {
      this.records.name = this.field.name;
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

      console.log(this.records);
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
        let valuesFromList = this.field.range.content.list.split(";");
        return valuesFromList[
          Math.floor(Math.random() * valuesFromList.length)
        ];
      } else {
        let fromTimeInMinutes;
        let toTimeInMinutes;
        switch (type) {
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
            console.log("failed to generate random value based on the input");
            break;
        }
      }
    },
  },
};
</script>

<style>
</style>