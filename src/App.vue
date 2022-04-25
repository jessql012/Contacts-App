<template>
  <v-app>
    <v-main>
      <ContactsMain/>
    </v-main>
    <v-footer app light>
    <v-spacer></v-spacer>
    Developed by Jessica Liu
    </v-footer>
  </v-app>
</template>

<script>
import ContactsMain from './components/ContactsMain.vue';

export default {
  name: 'App',

  components: {
    ContactsMain,
  },

  data: () => ({
    //
  }),
};
</script>

<style lang="scss">
@import "./scss/variables.scss";


.v-application {
  font-family: $body-font-family, sans-serif !important;
  .title { // To pin point specific classes of some components
      font-family: $title-font, sans-serif !important;
  }
}

</style>