<script>
import Vue from 'vue';
import VueFantasticTable from '@/vue-fantastic-table.vue';

export default Vue.extend({
  name: 'ServeDev',
  components: {
    VueFantasticTable
  }
});
</script>

<template>
  <div id="app">
    <vue-fantastic-table />
  </div>
</template>
