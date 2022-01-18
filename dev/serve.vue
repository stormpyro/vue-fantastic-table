<template>
  <div id="app">
    <vue-fantastic-table :headers="headers" :rows="rows" />
  </div>
</template>
<script>
import Vue from "vue";
import VueFantasticTable from "@/vue-fantastic-table.vue";

export default Vue.extend({
  name: "ServeDev",
  components: {
    VueFantasticTable,
  },
  data: () => ({
    headers: [{ label: "Test" }, { label: "Animal" }],
    rows: [
      { test: "123", animal: "50" },
      { test: "45", animal: 150 },
    ],
  }),
});
</script>

