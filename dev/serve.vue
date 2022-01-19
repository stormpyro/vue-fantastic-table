<template>
  <div id="app">
    <vue-fantastic-table responsive :headers="headers" :rows="callRequest">
      <template #loading>
        <div>Your custom component or message</div>
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
      { label: "Test" },
      { label: "Animal" },
      { label: "qw" },
      { label: "tr" },
      { label: "purchased Sale price" },
      { label: "purchased Sale price" },
      { label: "purchased Sale price" },
      { label: "purchased Sale price" },
      { label: "ownership" },
    ],
    rows: [
      {
        test: "123",
        animal: "50",
        qw: 150,
        tr: 150,
        purchased: 150,
        t: 4,
        y: 0,
        p: 0,
        f: 90,
      },
      {
        test: "45",
        animal: 150,
        qw: 150,
        tr: 150,
        purchased: 150,
        t: 4,
        y: 0,
        p: 0,
        f: 90,
      },
      {
        test: "45",
        animal: 150,
        qw: 150,
        tr: 150,
        purchased: 150,
        t: 4,
        y: 0,
        p: 0,
        f: 90,
      },
      {
        test: "45",
        animal: 150,
        qw: 150,
        tr: 150,
        purchased: 150,
        t: 4,
        y: 0,
        p: 0,
        f: 90,
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

