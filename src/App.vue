<template>
  <v-app>
    <v-app-bar class="primary">
      <v-app-bar-title> Re:Gero </v-app-bar-title>
    </v-app-bar>

    <v-main>
      <router-view />
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: "App",

  data: () => ({
    //
  }),
};
</script>
