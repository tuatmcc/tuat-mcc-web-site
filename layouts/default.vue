<template>
  <v-app>
    <v-app>
      <v-navigation-drawer v-model="drawer" app>
        <navigation-menu
      /></v-navigation-drawer>
      <v-app-bar color="blue-grey darken-4" dark app :value="$vuetify.breakpoint.mobile">
        <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
        <!-- <v-toolbar-title></v-toolbar-title> -->
      </v-app-bar>
      <v-main class="container">
        <div class="pt-7 px-4">
          <Nuxt />
        </div>
      </v-main>
    </v-app>
  </v-app>
</template>

<script lang="ts">
import Vue from 'vue'
import navigationMenu from '~/components/navigationMenu.vue'

export type DataType = { drawer: boolean | null }
export default Vue.extend({
  components: {
    navigationMenu,
  },
  data(): DataType {
    return {
      drawer: null,
    }
  },
})
</script>
<style lang="scss" scoped></style>
