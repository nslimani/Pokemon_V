<template>
  <v-app>
    <nav-bar />
    <v-main class="grey lighten-5 ">
      <router-view />
    </v-main>
    <cp-footer />
  </v-app>
</template>

<script>

export default {
  name: 'App',
  components: {
  },

  data: () => ({
    //
  }),
};
</script>
