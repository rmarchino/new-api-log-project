<template>
    <v-app
        height="auto"
        class="overflow-hidden"
        style="position: relative;"
    >
        <SidebarFilter />
        <HeaderTabsVue />
    </v-app>
</template>


<script>
import SidebarFilter from '~/components/sidebar/SidebarFilter.vue';
import HeaderTabsVue from '~/components/ui/Header.vue';

  export default {
    components: {
        SidebarFilter,
        HeaderTabsVue,
    },
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