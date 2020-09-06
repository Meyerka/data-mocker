<template>
  <tr>
    <td>
      <input type="text" id="fieldName" />
    </td>
    <td>
      <select name="fieldType">
        <option value>--Type--</option>
        <option value="text">Text</option>
        <option value="date">Date</option>
        <option value="number">Number</option>
        <option value="Duration"></option>
      </select>
    </td>
    <td>
      <input type="number" name="range" id />
    </td>
    <td>
      <input type="number" name="range" id />
    </td>
  </tr>
</template>

<script>
export default {
  name: "FieldRow",
  props: {},
};
</script>

<style>
</style>