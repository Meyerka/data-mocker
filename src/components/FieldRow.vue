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
        <option value="Duration"></option>
      </select>
    </td>
    <td>
      <select name="fieldSeparator" v-model="field.range.type">
        <option value>--Range--</option>
        <option value="random">Randomly generated</option>
        <option value="list">List</option>
      </select>
    </td>
    <td>
      <div v-if="field.type === 'date'" id="dateRange">
        From
        <input type="date" name="from" v-model="field.range.content.from" />
        To
        <input type="date" name="to" v-model="field.range.content.to" />
      </div>
      <div v-else-if="field.type === 'number'" id="numberRange">
        From
        <input type="number" name="from" v-model="field.range.content.from" />
        To
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
    <button @click="generateObject()">GENERATE!</button>
  </tr>
</template>

<script>
export default {
  name: "FieldRow",
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
  }),
  methods: {
    generateObject() {
      let generatedRow = {};
      generatedRow.name = this.field.name;
      if (this.field.range.type === "list") {
        let values = this.field.range.content.list.split(";");
        generatedRow.value = values[Math.floor(Math.random() * values.length)];
      } else {
        switch (this.field.type) {
          case "date":
            generatedRow.value = this.getRandomDateBetween(
              this.field.range.content.to,
              this.field.range.content.from
            );
            break;
          case "number":
            generatedRow.value = Math.round(
              this.getRandomValueBetween(
                this.field.range.content.from,
                this.field.range.content.to
              )
            );
            break;
          default:
            break;
        }
      }
      console.log(generatedRow);
      return generatedRow;
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
  },
};
</script>

<style>
</style>