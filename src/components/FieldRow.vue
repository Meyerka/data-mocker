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
    <button @click="generateObject()">GENERATE {{this.rowNumber}} lines !</button>
  </tr>
</template>

<script>
export default {
  name: "FieldRow",
  props: {
    dataGrid: [],
    rowNumber: Number,
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
      for (let i = 0; i < this.rowNumber; i++) {
        if (this.field.range.type === "list") {
          let valuesFromList = this.field.range.content.list.split(";");
          this.records.values.push(
            valuesFromList[Math.floor(Math.random() * valuesFromList.length)]
          );
        } else {
          let fromTimeInMinutes;
          let toTimeInMinutes;
          switch (this.field.type) {
            case "date":
              this.records.values.push(
                this.getRandomDateBetween(
                  this.field.range.content.to,
                  this.field.range.content.from
                )
              );
              break;
            case "number":
              this.records.values.push(
                Math.floor(
                  this.getRandomValueBetween(
                    this.field.range.content.from,
                    this.field.range.content.to
                  )
                )
              );
              break;
            case "duration":
              fromTimeInMinutes = this.hoursToMinutes(
                this.field.range.content.from,
                ":"
              );
              toTimeInMinutes = this.hoursToMinutes(
                this.field.range.content.to,
                ":"
              );

              this.records.values.push(
                this.minutesToHours(
                  Math.floor(
                    this.getRandomValueBetween(
                      fromTimeInMinutes,
                      toTimeInMinutes
                    )
                  ),
                  ":"
                )
              );

              break;
            default:
              break;
          }
        }
        this.record = this.generateObject;
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
  },
};
</script>

<style>
</style>