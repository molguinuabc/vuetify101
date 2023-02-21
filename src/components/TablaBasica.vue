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
</template>


<script setup>
import {ref, computed, watch, nextTick} from 'vue'
 const props = defineProps({
    id: Number,
    title: String,
    body:String,
    userid: Number

  });
  const dialog = ref(false);
  const dialogDelete = ref(false);
  const headers = ref([
      {
        title: 'ID',
        align: 'start',
        sortable: false,
        key: 'id',
      },
      { title: 'Title', key: 'title' },
      { title: 'Body', key: 'body' },
      { title: 'User Id', key: 'userid' },
      { title: 'Actions', key: 'actions', sortable: false },
    ]);
  const posts = ref([]);
  const editedIndex = ref(-1);
  const editedItem = ref({});
  const defaultItem = ref({});
  const formTitle = computed(() => {
      return editedIndex.value === -1 ? 'New Item' : 'Edit Item'
  });
  watch(dialog, (newDialog, oldDialog) => {
    newDialog || close();
  });
  watch(dialogDelete, (newDialogDelete, oldDialogDelete) => {
      newDialogDelete || closeDelete();
  });
  function initialize () {
      let p={};
      fetch('https://jsonplaceholder.typicode.com/posts')
          .then(response => response.json())
          .then(json => {
                posts.value=json;
                for (let p=0; p<posts.value.length; p++) {
                  fetch("https://jsonplaceholder.typicode.com/users/" + posts.value[p].userId)
                    .then(response => response.json())
                    .then(r => {
                      posts.value[p].nombre=r.name;
                    });
                }
          });
    }
    initialize();
    function editItem (item) {
      editedIndex.value = posts.value.indexOf(item);
      editedItem.value = Object.assign({}, item);
      dialog.value = true;
    }

    function deleteItem (item) {
      console.log("Borro"+this.posts);
      editedIndex.value = this.posts.indexOf(item)
      editedItem.value = Object.assign({}, item)
      dialogDelete.value = true
      console.log(item.id);
      fetch('https://jsonplaceholder.typicode.com/posts/'+item.id, {
        method: 'DELETE',
      });
      // console.log("Borro"+this.posts.indexOf);
    }

    function deleteItemConfirm (item) {

      posts.value.splice(editedIndex.value, 1);

      closeDelete();
    }

    function close () {
      dialog.value = false
      nextTick(() => {
        editedItem.value = Object.assign({}, defaultItem.value)
        editedIndex.value = -1
      })
    }

    function closeDelete () {
      dialogDelete.value = false
      nextTick(() => {
        editedItem.value = Object.assign({}, defaultItem.value)
        editedIndex.value = -1
      })
    }

    function save () {
      const obj={
          id: editedItem.value.id,
          title: editedItem.value.title,
          body:editedItem.value.body,
          userid: editedItem.value.userid
      }
      if (editedIndex.value > -1) {
        console.log("Entra Put");
        console.log(obj);
        fetch('https://jsonplaceholder.typicode.com/posts/'+editedItem.value.id, {
          method: 'PUT',
          body: JSON.stringify(obj),
          headers: {
            'Content-type': 'application/json; charset=UTF-8',
          },
        })
          .then((response) => {
            r = response.json();
            console.log("Response del put",r);
            return r;
          })
          .then((json) => console.log("Response del PUT Json",json));
        Object.assign(posts.value[editedIndex.value], editedItem.value)
      } else {
        console.log("Entra post");
        fetch('https://jsonplaceholder.typicode.com/posts', {
          method: 'POST',
          body: JSON.stringify(obj),
          headers: {
            'Content-type': 'application/json; charset=UTF-8',
          },
        })
          .then((response) => response.json())
          .then((json) => console.log(json));
        posts.value.push(editedItem.value)
      }
      close()
    }
</script>

<style lang="">

</style>
