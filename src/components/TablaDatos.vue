<template>
  <v-row justify="center">
    <v-dialog
      v-model="dialog"
      persistent
      width="1024"
    >
      <template v-slot:activator="{ props }">
        <v-row justify="center">
          <v-col class="text-right">
            <v-btn
              color="primary"
              v-bind="props"
            >
              Nuevo Post
            </v-btn>
          </v-col>
        </v-row>

      </template>
      <v-card>
        <v-card-title>
          <span class="text-h5">User Profile</span>
        </v-card-title>
        <v-card-text>
          <v-container>
            <v-row>
              <v-col
                cols="12"
                sm="6"
                md="4"
              >
                <v-text-field
                  label="Legal first name*"
                  required
                ></v-text-field>
              </v-col>
              <v-col
                cols="12"
                sm="6"
                md="4"
              >
                <v-text-field
                  label="Legal middle name"
                  hint="example of helper text only on focus"
                ></v-text-field>
              </v-col>
              <v-col
                cols="12"
                sm="6"
                md="4"
              >
                <v-text-field
                  label="Legal last name*"
                  hint="example of persistent helper text"
                  persistent-hint
                  required
                ></v-text-field>
              </v-col>
              <v-col cols="12">
                <v-text-field
                  label="Email*"
                  required
                ></v-text-field>
              </v-col>
              <v-col cols="12">
                <v-text-field
                  label="Password*"
                  type="password"
                  required
                ></v-text-field>
              </v-col>
              <v-col
                cols="12"
                sm="6"
              >
                <v-select
                  :items="['0-17', '18-29', '30-54', '54+']"
                  label="Age*"
                  required
                ></v-select>
              </v-col>
              <v-col
                cols="12"
                sm="6"
              >
                <v-autocomplete
                  :items="['Skiing', 'Ice hockey', 'Soccer', 'Basketball', 'Hockey', 'Reading', 'Writing', 'Coding', 'Basejump']"
                  label="Interests"
                  multiple
                ></v-autocomplete>
              </v-col>
            </v-row>
          </v-container>
          <small>*indicates required field</small>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="blue-darken-1"
            variant="text"
            @click="dialog = false"
          >
            Close
          </v-btn>
          <v-btn
            color="blue-darken-1"
            variant="text"
            @click="dialog = false"
          >
            Save
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-row>
  <!-- <template> -->
    <!-- <v-data-table
      v-model:items-per-page="itemsPerPage"
      :headers="headers"
      :items="desserts"
      item-value="name"
      class="elevation-1"
    ></v-data-table> -->
  <!-- </template> -->
</template>

<script>
  export default {
    data: () => ({
      dialog: false,
    }),
  }
//   export default {
//   data () {
//     return {
//       itemsPerPage: 5,
//       headers: [
//         {
//           title: 'Dessert (100g serving)',
//           align: 'start',
//           sortable: false,
//           key: 'name',
//         },
//         { title: 'Calories', align: 'end', key: 'calories' },
//         { title: 'Fat (g)', align: 'end', key: 'fat' },
//         { title: 'Carbs (g)', align: 'end', key: 'carbs' },
//         { title: 'Protein (g)', align: 'end', key: 'protein' },
//         { title: 'Iron (%)', align: 'end', key: 'iron' },
//       ],
//       desserts: [
//         {
//           name: 'Frozen Yogurt',
//           calories: 159,
//           fat: 6.0,
//           carbs: 24,
//           protein: 4.0,
//           iron: '1',
//         },
//         {
//           name: 'Jelly bean',
//           calories: 375,
//           fat: 0.0,
//           carbs: 94,
//           protein: 0.0,
//           iron: '0',
//         },
//         {
//           name: 'KitKat',
//           calories: 518,
//           fat: 26.0,
//           carbs: 65,
//           protein: 7,
//           iron: '6',
//         },
//         {
//           name: 'Eclair',
//           calories: 262,
//           fat: 16.0,
//           carbs: 23,
//           protein: 6.0,
//           iron: '7',
//         },
//         {
//           name: 'Gingerbread',
//           calories: 356,
//           fat: 16.0,
//           carbs: 49,
//           protein: 3.9,
//           iron: '16',
//         },
//         {
//           name: 'Ice cream sandwich',
//           calories: 237,
//           fat: 9.0,
//           carbs: 37,
//           protein: 4.3,
//           iron: '1',
//         },
//         {
//           name: 'Lollipop',
//           calories: 392,
//           fat: 0.2,
//           carbs: 98,
//           protein: 0,
//           iron: '2',
//         },
//         {
//           name: 'Cupcake',
//           calories: 305,
//           fat: 3.7,
//           carbs: 67,
//           protein: 4.3,
//           iron: '8',
//         },
//         {
//           name: 'Honeycomb',
//           calories: 408,
//           fat: 3.2,
//           carbs: 87,
//           protein: 6.5,
//           iron: '45',
//         },
//         {
//           name: 'Donut',
//           calories: 452,
//           fat: 25.0,
//           carbs: 51,
//           protein: 4.9,
//           iron: '22',
//         },
//       ],
//     }
//   },
// }
</script>
