<template>
  <v-container>
    <v-row justify="space-around">
      <v-card width="400">
        <v-img
          height="200"
          :src="url"
          cover
          class="text-white"
        >
          <v-toolbar
            color="rgba(0, 0, 0, 0)"
            theme="dark"
          >
            <template v-slot:prepend>
              <v-btn icon="$menu"></v-btn>
            </template>

            <v-toolbar-title class="text-h6">
              Messages
            </v-toolbar-title>

            <template v-slot:append>
              <v-btn icon="mdi-dots-vertical"></v-btn>
            </template>
          </v-toolbar>
        </v-img>

        <v-card-text>
            <v-progress-linear v-if="cargando"
              color="red"
              indeterminate
              
            ></v-progress-linear>
        </v-card-text>
        <v-card-actions>
          <v-btn  @click="recargaImagen"  :color="red">
            Reload
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-row>
  </v-container>
</template>

<script setup>
  import { ref, onMounted }  from 'vue';
    const messages = ref ([]);
    const cargando = ref(false);
    const red = ref("blue");
    const url = ref ("");
    onMounted(()=>{
      recargaImagen();
    });
    async function recargaImagen(){
        cargando.value = true;
        let r = await fetch("https://random.imagecdn.app/500/150");
        url.value=r.url
        cargando.value = false;
    }
</script>
