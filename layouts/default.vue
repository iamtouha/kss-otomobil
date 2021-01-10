<template>
  <v-app>
    <v-app-bar app>
      <v-app-bar-nav-icon @click="drawer = !drawer" />
      <v-toolbar-title class="pt-1">
        <logo />
      </v-toolbar-title>
    </v-app-bar>
    <v-navigation-drawer v-model="drawer" app>
      <v-list>
        <v-list-item
          v-for="route in routes"
          :key="route.path"
          :to="route.path"
          nuxt
          exact
        >
          {{ route.name }}
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-main>
      <nuxt />
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: "DefaultLayout",
  data: () => ({
    drawer: null,
    routes: [
      {
        name: "Home",
        path: "/"
      },
      {
        name: "Auction",
        path: "/auction"
      }
    ]
  })
};
</script>

<style></style>
