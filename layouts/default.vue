<template>
  <v-app>
    <v-app-bar app>
      <v-app-bar-nav-icon @click="drawer = !drawer" />
      <v-toolbar-title class="pt-1">
        <logo />
      </v-toolbar-title>
    </v-app-bar>
    <v-navigation-drawer v-model="drawer" app> </v-navigation-drawer>
    <v-main>
      <nuxt />
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: "DefaultLayout",
  data: () => ({
    drawer: null
  })
};
</script>

<style></style>
