<template>
   <v-navigation-drawer
        v-model="opciones"
        temporary
      >
        <v-list-item
          title="Opciones"
        ></v-list-item>

        <v-divider></v-divider>

        <v-list density="compact" nav>
          <v-list-item @click="$router.push('/')" 
            prepend-icon="mdi-view-dashboard" 
            title="Home" 
            value="home">
            
          </v-list-item>
          <v-list-item @click="$router.push('/tabla')" prepend-icon="mdi-table" title="Tabla" value="tabla"></v-list-item>
        </v-list>
  </v-navigation-drawer>
  <v-app-bar rounded elevation="15" >
    <template v-slot:prepend>
        <v-app-bar-nav-icon @click.stop="opciones = !opciones"></v-app-bar-nav-icon>
    </template>
    <v-app-bar-title>

    </v-app-bar-title>
  </v-app-bar>
</template>

<script setup>
import { ref } from 'vue';
const opciones = ref(false)
</script>
