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
};
</script>

<style>
</style>