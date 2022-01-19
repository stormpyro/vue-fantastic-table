# Vue Fantastic Table

A great vue table with material design and all the power of
jquery datatables features.

### Install

```bash
npm i vue-fantastic-table
```

### Usage

```bash
import VueFantasticTable from "vue-fantastic-table"

Vue.component("vue-fantastic-table", VueFantasticTable)
```

### Features

#### Responsive mode

Similar to the responsive mode of jquery datatables. When
the screen is small VFT hide columns but when is large VFT
show columns.

To use it just add the prop "responsive".

```
<vue-fantastic-table responsive :headers="headers" :rows="rows" />
```
