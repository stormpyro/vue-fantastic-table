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

#### Dark Theme

You can use a dark theme of VFT using the prop "dark":

```
<vue-fantastic-table dark responsive :headers="headers" :rows="callRequest" />
```

#### Custom slots

You can change the content that is displayed for each cell in the body. Only need to use slots
with the name of the field that you set in headers prop.

Example:

```
<vue-fantastic-table
      :headers="headers"
      :rows="callRequest"
      class="dark"
      responsive
    >
      <template #university="{ name, university }">
        <div>{{ name + " studies in " + university }}</div>
      </template>
</vue-fantastic-table>
...
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
    ],
...
```

#### Search

You can add the prop "search". That will add a search input at the top of VFT.

```
<vue-fantastic-table
      :headers="headers"
      :rows="callRequest"
      dark
      responsive
      search
    />
```
