<template>
    <v-app-bar
        absolute
        prominent
        shrink-on-scroll
    >
    <v-app-bar-nav-icon @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
    <v-toolbar-title>Arisale</v-toolbar-title>

    <template v-slot:extension>
        <v-tabs align-with-title>
           <v-tab>Exceptions</v-tab> 
           <v-tab>Pinpad Events</v-tab> 
           <v-tab>Web Services</v-tab> 
        </v-tabs>
    </template>

    </v-app-bar>
</template>


<script>
  export default {
    data: () => ({
      drawer: false,
      group: null,
    }),

    watch: {
      group () {
        this.drawer = false
      },
    },
  }
</script>
