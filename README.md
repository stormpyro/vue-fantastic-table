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

---

#### Responsive mode

Similar to the responsive mode of jquery datatables. When
the screen is small VFT hide columns but when is large VFT
show columns.

To use it just add the prop "responsive".

```
<vue-fantastic-table responsive :headers="headers" :rows="rows" />
```

#### Server mode

The rows prop has 2 modes. You can pass an array or a function. If
you pass a function VFT will take that as a promise that return an
array of objects.

```
 <vue-fantastic-table :headers="headers" :rows="callRequest" />
 ...
 callRequest: function () {
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          resolve(this.rows);
        }, 1100);
      });
    },
```

While the data is being loaded VFT will show a default message "loading..."
You can customize this message with the slot "loading".

```
<vue-fantastic-table responsive :headers="headers" :rows="callRequest">
      <template #loading>
        <div>Your custom component or message</div>
      </template>
    </vue-fantastic-table>
```

#### Themes

You can change the table theme with the following classes:

- dark
- highlight
- striped

```
<vue-fantastic-table class="dark" responsive :headers="headers" :rows="callRequest" />
```
