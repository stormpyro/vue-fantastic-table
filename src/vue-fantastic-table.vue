<template>
  <table ref="fantasticTable">
    <thead>
      <tr>
        <th ref="headers" :key="index" v-for="({ label }, index) in headers">
          {{ label }}
        </th>
      </tr>
    </thead>

    <tbody>
      <tr ref="rows" :key="index" v-for="(data, index) in rows">
        <td
          ref="cells"
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
    responsive: {
      type: Boolean,
      default: true,
    },
  },
  data: () => ({
    width: document.documentElement.clientWidth,
    height: document.documentElement.clientHeight,
  }),
  mounted: function () {
    if (this.responsive) window.addEventListener("resize", this.responsiveMode);
  },
  unmounted() {
    if (this.responsive)
      window.removeEventListener("resize", this.responsiveMode);
  },
  computed: {
    tableIsGrower: function () {
      return this.$refs.fantasticTable.clientWidth > this.width;
    },
    columnCellsCounter: function () {
      return 1 + this.$refs.rows.length;
    },
  },
  methods: {
    isVisible: function (element) {
      if (
        element.offsetWidth ||
        element.offsetHeight ||
        element.getClientRects().length
      )
        return true;

      return false;
    },
    getOnlyVisibleColumns: function () {
      const columns = this.getAllColumns();
      return columns.filter((column) => {
        for (const element of column) {
          return this.isVisible(element);
        }
      });
    },
    getAllColumns: function () {
      const columns = [];
      for (const [header_idx, header] of this.$refs.headers.entries()) {
        const domCollection = [];
        domCollection.push(header);
        for (const row of this.$refs.rows) {
          domCollection.push(row.children[header_idx]);
        }
        columns.push(domCollection);
      }
      return columns;
    },
    responsiveMode: function () {
      const widthDifference = document.documentElement.clientWidth - this.width;
      this.width = document.documentElement.clientWidth;
      this.height = document.documentElement.clientHeight;

      if (this.tableIsGrower) {
        const columns = this.getOnlyVisibleColumns();
        for (const cell of columns[columns.length - 1]) {
          cell.classList.add("hide");
        }
      }

      if (widthDifference > 10 && !this.tableIsGrower) {
        const columns = this.getAllColumns();
        const visibleColumns = this.getOnlyVisibleColumns();
        if (columns[visibleColumns.length])
          for (const cell of columns[visibleColumns.length]) {
            cell.classList.remove("hide");
          }
      }
    },
  },
};
</script>
<style scoped>
.hide {
  display: none;
}

table {
  width: 100%;
  display: table;
  border-collapse: collapse;
  border-spacing: 0;
}

table.striped tr {
  border-bottom: none;
}

table.striped > tbody > tr:nth-child(odd) {
  background-color: rgba(242, 242, 242, 0.5);
}

table.striped > tbody > tr > td {
  border-radius: 0;
}

table.highlight > tbody > tr {
  -webkit-transition: background-color 0.25s ease;
  transition: background-color 0.25s ease;
}

table.highlight > tbody > tr:hover {
  background-color: rgba(242, 242, 242, 0.5);
}

table.centered thead tr th,
table.centered tbody tr td {
  text-align: center;
}

tr,
td,
th {
  border: 1px solid rgba(0, 0, 0, 0.12);
}

td,
th {
  padding: 15px 5px;
  display: table-cell;
  text-align: left;
  vertical-align: middle;
  border-radius: 2px;
}

@media only screen and (max-width: 992px) {
  table.responsive-table {
    width: 100%;
    border-collapse: collapse;
    border-spacing: 0;
    display: block;
    position: relative;
    /* sort out borders */
  }
  table.responsive-table td:empty:before {
    content: "\00a0";
  }
  table.responsive-table th,
  table.responsive-table td {
    margin: 0;
    vertical-align: top;
  }
  table.responsive-table th {
    text-align: left;
  }
  table.responsive-table thead {
    display: block;
    float: left;
  }
  table.responsive-table thead tr {
    display: block;
    padding: 0 10px 0 0;
  }
  table.responsive-table thead tr th::before {
    content: "\00a0";
  }
  table.responsive-table tbody {
    display: block;
    width: auto;
    position: relative;
    overflow-x: auto;
    white-space: nowrap;
  }
  table.responsive-table tbody tr {
    display: inline-block;
    vertical-align: top;
  }
  table.responsive-table th {
    display: block;
    text-align: right;
  }
  table.responsive-table td {
    display: block;
    min-height: 1.25em;
    text-align: left;
  }
  table.responsive-table tr {
    border-bottom: none;
    padding: 0 10px;
  }
  table.responsive-table thead {
    border: 0;
    border-right: 1px solid rgba(0, 0, 0, 0.12);
  }
}
</style>