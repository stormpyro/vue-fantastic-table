<template>
  <div id="app">
    <vue-fantastic-table
      :headers="headers"
      :rows="callRequest"
      dark
      responsive
      search
    >
      <template #university="{ name, university }">
        <div>{{ name + " studies in " + university }}</div>
      </template>
    </vue-fantastic-table>
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
    rows: [
      {
        name: "Renatto",
        age: 26,
        sex: "M",
        district: "La Perla",
        university: "UNFV",
        carrer: "Ing. Informatica",
        job: "Software Developer",
        experience: 2,
        investor: "Yes",
      },
      {
        name: "Danixsa",
        age: 26,
        sex: "F",
        district: "Chorrillos",
        university: "UNFV",
        carrer: "Ing. Alimentaria",
        job: "Desarrolladora de innovacion",
        experience: 3,
        investor: "No",
      },
      {
        name: "Vanessa",
        age: 25,
        sex: "F",
        district: "San Isidro",
        university: "Pacifico",
        carrer: "Adm. de empresas",
        job: "Admin",
        experience: 4,
        investor: "No",
      },
    ],
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

