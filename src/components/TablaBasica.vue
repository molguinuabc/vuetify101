<template>
    <v-table
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
    </v-table>
  </template>
  
<script setup>
import { ref, onMounted } from 'vue';

onMounted(()=>{
  let p={};
  fetch('https://jsonplaceholder.typicode.com/posts')
      .then(response => response.json())
      .then(json => {
            posts.value = json;
            for (let p=0; p<posts.value.length; p++) {
              fetch("https://jsonplaceholder.typicode.com/users/" + posts.value[p].userId)
                .then(response => response.json())
                .then(r => {
                  posts.value[p].nombre=r.name;
                });
            }
      });
  });
const posts = ref([]);
 
</script>
<style lang="">
    
</style>