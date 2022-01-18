<template>
  <table>
    <thead>
      <tr>
        <th :key="index" v-for="({ label }, index) in headers">
          {{ label }}
        </th>
      </tr>
    </thead>

    <tbody>
      <tr :key="index" v-for="(data, index) in rows">
        <td
          :key="index + prop_idx"
          v-for="(property, prop_idx) in Object.keys(data)"
        >
          {{ data[property] }}
        </td>
      </tr>
    </tbody>
  </table>
</template>
<script>
import "materialize-css/dist/css/materialize.css";
export default /*#__PURE__*/ {
  name: "VueFantasticTable", // vue component name
  props: {
    rows: {
      type: Array,
      required: true,
    },
    headers: {
      type: Array,
      required: true,
    },
  },
};
</script>