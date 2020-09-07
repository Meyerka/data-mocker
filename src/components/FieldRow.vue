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
      if (this.field.type === "date") {
        generatedRow.value = this.getRandomDate(
          this.field.range.content.to,
          this.field.range.content.from
        );
      }
      console.log(generatedRow);
      return generatedRow;
    },

    getRandomDate(date1, date2) {
      date1 = date1 || "01-01-1970";
      date2 = date2 || new Date().toLocaleDateString();
      date1 = new Date(date1).getTime();
      date2 = new Date(date2).getTime();
      if (date1 > date2) {
        return new Date(
          this.randomValueBetween(date2, date1)
        ).toLocaleDateString();
      } else {
        return new Date(
          this.randomValueBetween(date1, date2)
        ).toLocaleDateString();
      }
    },
    randomValueBetween(min, max) {
      return Math.random() * (max - min) + min;
    },
  },
};
</script>

<style>
</style>