<template>

  <v-data-table
    :headers="headers"
    :items="posts"
    :sort-by="[{ key: 'id', order: 'asc' }]"
    class="elevation-1"
  >
    <template v-slot:top>
      <v-toolbar
        flat
      >
        <v-toolbar-title>My CRUD</v-toolbar-title>
        <v-divider
          class="mx-4"
          inset
          vertical
        ></v-divider>
        <v-spacer></v-spacer>
        <v-dialog
          v-model="dialog"
          max-width="500px"
        >
          <template v-slot:activator="{ props }">
            <v-btn
              color="primary"
              dark
              class="mb-2"
              v-bind="props"
            >
              New Item
            </v-btn>
          </template>
          <v-card>
            <v-card-title>
              <span class="text-h5">{{ formTitle }}</span>
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
                      v-model="editedItem.title"
                      label="Title"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.body"
                      label="Body"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.userid"
                      label="Userid"
                    ></v-text-field>
                  </v-col>
                  <!-- <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.carbs"
                      label="Carbs (g)"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.protein"
                      label="Protein (g)"
                    ></v-text-field>
                  </v-col> -->
                </v-row>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn
                color="blue-darken-1"
                variant="text"
                @click="close"
              >
                Cancel
              </v-btn>
              <v-btn
                color="blue-darken-1"
                variant="text"
                @click="save"
              >
                Save
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
        <v-dialog v-model="dialogDelete" max-width="500px">
          <v-card>
            <v-card-title class="text-h5">Are you sure you want to delete this item?</v-card-title>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue-darken-1" variant="text" @click="closeDelete">Cancel</v-btn>
              <v-btn color="blue-darken-1" variant="text" @click="deleteItemConfirm">OK</v-btn>
              <v-spacer></v-spacer>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>
    <template v-slot:item.actions="{ item }">
      <v-icon
        size="small"
        class="me-2"
        @click="editItem(item.raw)"
      >
        mdi-pencil
      </v-icon>
      <v-icon
        size="small"
        @click="deleteItem(item.raw)"
      >
        mdi-delete
      </v-icon>
    </template>
    <template v-slot:no-data>
      <v-btn
        color="primary"
        @click="initialize"
      >
        Reset
      </v-btn>
    </template>
  </v-data-table>
  <!-- <template> -->
    <!-- <v-data-table
      v-model:items-per-page="itemsPerPage"
      :headers="headers"
      :items="post"
      item-value="name"
      class="elevation-1"
    ></v-data-table> -->
  <!-- </template> -->

    <!-- <v-table
      fixed-header
      height="300px"
    >
      <thead>
        <tr>
          <th class="text-left">
            Autor
          </th>
          <th class="text-left">
            Título
          </th>
          <th class="text-left">
            Post
          </th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="item in posts"
          :key="item.id"
        >
          <td>{{ item.nombre }}</td>
          <td>{{ item.title }}</td>
          <td>{{ item.body }}</td>
        </tr>
      </tbody>
    </v-table> -->
</template>


<script>
export default {
  props:{
    id: Number,
    title: String,
    body:String,
    userid: Number

  },
  data: () => ({
    dialog: false,
    dialogDelete: false,
    headers: [
      {
        title: 'ID',
        align: 'start',
        sortable: false,
        key: 'id',
      },
      { title: 'Title', key: 'title' },
      { title: 'Body', key: 'body' },
      { title: 'User Id', key: 'userid' },
      // { title: 'Protein (g)', key: 'protein' },
      { title: 'Actions', key: 'actions', sortable: false },
    ],
    posts: [],
    editedIndex: -1,
    editedItem: {
      name: '',
      calories: 0,
      fat: 0,
      carbs: 0,
      protein: 0,
    },
    defaultItem: {
      name: '',
      calories: 0,
      fat: 0,
      carbs: 0,
      protein: 0,
    },
  }),

  computed: {
    formTitle () {
      return this.editedIndex === -1 ? 'New Item' : 'Edit Item'
    },
  },

  watch: {
    dialog (val) {
      val || this.close()
    },
    dialogDelete (val) {
      val || this.closeDelete()
    },
  },

  created () {
    this.initialize()
    //this.mounted()
  },

  methods: {
    initialize () {

      let p={};
      fetch('https://jsonplaceholder.typicode.com/posts')
          .then(response => response.json())
          .then(json => {
                this.posts=json;
                //this.posts.value = json;
                for (let p=0; p<this.posts.length; p++) {
                  fetch("https://jsonplaceholder.typicode.com/users/" + this.posts[p].userId)
                    .then(response => response.json())
                    .then(r => {
                      this.posts[p].nombre=r.name;
                    });
                }
          });
    },

    editItem (item) {
      this.editedIndex = this.posts.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialog = true
    },

    deleteItem (item) {
      console.log("Borro"+this.posts);
      this.editedIndex = this.posts.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialogDelete = true
      console.log(item.id);
      fetch('https://jsonplaceholder.typicode.com/posts/'+item.id, {
        method: 'DELETE',
      });
      // console.log("Borro"+this.posts.indexOf);
    },

    deleteItemConfirm (item) {

      this.posts.splice(this.editedIndex, 1)

      this.closeDelete()
    },

    close () {
      this.dialog = false
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      })
    },

    closeDelete () {
      this.dialogDelete = false
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      })
    },

    save () {
      if (this.editedIndex > -1) {
        console.log("Entra Put");
        let obj={
          id: this.editedItem.id,
          title: this.editedItem.title,
          body:this.editedItem.body,
          userid: this.editedItem.userid
        }
        fetch('https://jsonplaceholder.typicode.com/posts/'+this.editedItem.id, {
          method: 'PUT',
          body: JSON.stringify(obj),
          headers: {
            'Content-type': 'application/json; charset=UTF-8',
          },
        })
          .then((response) => response.json())
          .then((json) => console.log(json));
        Object.assign(this.posts[this.editedIndex], this.editedItem)
      } else {
        console.log("Entra post");
        let obj={
          id: this.editedItem.id,
          title: this.editedItem.title,
          body:this.editedItem.body,
          userid: this.editedItem.userid
        }

        fetch('https://jsonplaceholder.typicode.com/posts', {
          method: 'POST',
          body: JSON.stringify(obj),
          headers: {
            'Content-type': 'application/json; charset=UTF-8',
          },
        })
          .then((response) => response.json())
          .then((json) => console.log(json));
        this.posts.push(this.editedItem)
      }
      this.close()
    },
  },
}

</script>

<!--
//
// import { ref, onMounted} from 'vue';
//import { data} from 'vuetify';

// onMounted(()=>{
//   let p={};
//   fetch('https://jsonplaceholder.typicode.com/posts')
//       .then(response => response.json())
//       .then(json => {
//             console.log(json);
//             posts.value = json;
//             for (let p=0; p<posts.value.length; p++) {
//               fetch("https://jsonplaceholder.typicode.com/users/" + posts.value[p].userId)
//                 .then(response => response.json())
//                 .then(r => {
//                   posts.value[p].nombre=r.name;
//                 });
//             }
//       });
//   });


//const posts = ref([]);
// const props = defineProps({
//   title: String,
//   body: String
// });
// const dialogo = data([dialog:false]) -->

<style lang="">

</style>
