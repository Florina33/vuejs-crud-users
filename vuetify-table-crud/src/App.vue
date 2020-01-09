<template>
  <v-app>
    <v-app-bar app color="indigo" dark>
      <v-btn
        v-for="link in links"
        :key="`${link.label}-header-link`"
        text
        :to="link.url"
      >{{ link.label }}</v-btn>
      <v-spacer></v-spacer>
      <v-toolbar-title>SvetLana</v-toolbar-title>
    </v-app-bar>

    <v-content>
      <router-view></router-view>
    </v-content>

    <v-footer color="indigo" dark padless>
      <v-layout justify-center wrap>
        <v-flex
          indigo
          dark
          py-4
          text-center
          white--text
          xs12
        >VueJS/Vuetify example: Users, author SvetLana, {{ new Date().getFullYear() }}</v-flex>
      </v-layout>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      links: [
        {
          label: "Home",
          url: "/"
        },
        {
          label: "Users",
          url: "/users"
        },
        {
          label: "Login",
          url: "/login"
        }
      ]
    };
  }
};
</script>

<style>
* {
  text-transform: none !important;
}
</style>


