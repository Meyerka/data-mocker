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
      <input type="number" v-model="field.range" id />
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
      range: "",
      unicity: 50,
    },
  }),
};
</script>

<style>
</style>