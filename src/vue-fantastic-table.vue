<template>
  <div>
    <div
      v-if="search"
      style="margin-bottom: 10px; display: flex; justify-content: flex-end"
    >
      <div>
        <input
          id="search"
          @input="globalSearch"
          placeholder="Search"
          type="text"
          v-model="searchInput"
        />
      </div>
    </div>

    <table :class="{ dark }" ref="fantasticTable">
      <thead>
        <tr>
          <th ref="headers" :key="index" v-for="({ label }, index) in headers">
            {{ label }}
          </th>
        </tr>
      </thead>
      <tbody v-if="loading">
        <tr>
          <td :colspan="headers.length">
            <slot name="loading">
              <div class="flex-center">Loading...</div>
            </slot>
          </td>
        </tr>
      </tbody>
      <tbody v-else>
        <tr v-if="!validData">
          <td :colspan="headers.length">
            <div class="flex-center">Not Valid Data</div>
          </td>
        </tr>
        <tr v-else-if="tableData.length == 0">
          <td :colspan="headers.length">
            <div class="flex-center">Empty Data</div>
          </td>
        </tr>
        <tr
          v-else
          ref="rows"
          :key="row_idx"
          v-for="(data, row_idx) in tableData"
        >
          <td
            ref="cells"
            :key="row_idx + prop_idx"
            v-for="(property, prop_idx) in Object.keys(data)"
          >
            <slot v-bind="data" :name="property">
              {{ data[property] }}
            </slot>
          </td>
        </tr>
        <tr v-if="emptyResults">
          <td :colspan="headers.length">
            <div class="flex-center">Empty Results</div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
<script>
export default /*#__PURE__*/ {
  name: "VueFantasticTable", // vue component name
  props: {
    rows: {
      type: Array | Function,
      required: true,
    },
    headers: {
      type: Array,
      required: true,
    },
    responsive: {
      type: Boolean,
      default: false,
    },
    dark: {
      type: Boolean,
      default: false,
    },
    search: {
      type: Boolean,
      default: false,
    },
  },
  data: () => ({
    emptyResults: false,
    height: document.documentElement.clientHeight,
    loading: false,
    searchInput: "",
    tableData: [],
    validData: false,
    width: document.documentElement.clientWidth,
  }),
  created: async function () {
    if (typeof this.rows == "function") {
      this.loading = true;
      const rows = await this.rows();
      this.loading = false;
      this.validData = this.validateData(rows);
      if (this.validData) this.tableData = rows;
      return;
    }
    this.validData = this.validateData(this.rows);
    if (this.validData) this.tableData = this.rows;
  },
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
  },
  methods: {
    globalSearch: function () {
      if (this.$refs.rows) {
        const visibleRows = this.$refs.rows.filter(({ children }, row_idx) => {
          const notFound = [...children].every(
            ({ textContent }) =>
              !textContent
                .trim()
                .toLowerCase()
                .startsWith(this.searchInput.toLowerCase())
          );
          if (notFound) this.$refs.rows[row_idx].classList.add("hide");
          else this.$refs.rows[row_idx].classList.remove("hide");

          return this.isVisible(this.$refs.rows[row_idx]);
        });
        if (visibleRows.length == 0) this.emptyResults = true;
        else this.emptyResults = false;
      }
    },
    validateData: function (data) {
      const globalProps = this.headers.map(({ field }) => field).sort();
      return data.every((element) => {
        const rowProps = Object.keys(element).sort();
        return (
          globalProps.length == rowProps.length &&
          rowProps.every((prop, idx) => {
            return prop == globalProps[idx];
          })
        );
      });
    },
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
      if (this.responsive) {
        const widthDifference =
          document.documentElement.clientWidth - this.width;
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
      } else {
        window.removeEventListener("resize", this.responsiveMode);
      }
    },
  },
};
</script>
<style scoped>
.hide {
  display: none;
}

.flex-center {
  align-content: center;
  display: flex;
  justify-content: center;
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

table.dark {
  background-color: #363636;
  color: #fff;
  border: 1px solid rgba(255, 255, 255, 0.2);
}
table.dark > thead {
  background-color: rgba(0, 0, 0, 0.746);
}
table.dark > thead > tr > th {
  border: 1px solid rgba(255, 255, 255, 0.281);
}

table.dark > tbody > tr > td {
  border: 1px solid rgba(255, 255, 255, 0.2);
}
</style>