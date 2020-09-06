<template>
  <div>
    <h1>Select fields</h1>

    <table>
      <thead>
        <tr>
          <th>Field name</th>
          <th>Range</th>
          <th>Unicity</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>
            <input type="text" />
          </td>
          <td>
            <input type="number" name="range" id />
          </td>
          <td>
            <input type="number" name="range" id />
          </td>
        </tr>
      </tbody>
    </table>
    <div>Number of rows</div>
    <div>Include header ?</div>
    <div>Field seperator</div>
  </div>
</template>

<script>
export default {
  name: "FileCreator",
  props: {},
};
</script>

<style>
</style>
