<template>
   <v-navigation-drawer
      v-model="drawer"
      absolute
      temporary
   >
      <template>
         <h2>Filtros</h2>
      </template>
   </v-navigation-drawer>
</template>


<script>
export default {
   data: () => ({
      drawer: false,
   })
}
</script>