<template>
  <div id="app">
    <vue-fantastic-table
      :headers="headers"
      :rows="callRequest"
      class="dark"
      responsive
    />
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
    headers: [
      { field: "name", label: "Name" },
      { field: "age", label: "Age" },
      { field: "sex", label: "Sex" },
      { field: "district", label: "District" },
      { field: "university", label: "University" },
      { field: "carrer", label: "Carrer" },
      { field: "job", label: "Job" },
      { field: "experience", label: "Experience" },
      { field: "investor", label: "Investor" },
    ],
    rows: [],
  }),
  methods: {
    callRequest: function () {
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          resolve(this.rows);
        }, 1100);
      });
    },
  },
});
</script>

