<template>
    <v-app
        height="auto"
        class="mx-auto overflow-hidden"
        style="position: relative;"
    >
        <SidebarFilter 
          :drawer="drawer"
          @toggleDrawer="toggleDrawer"
        />
        <HeaderTabsVue 
          :drawer="drawer"
          @toggleDrawer="toggleDrawer"
        />

    <v-main>
      <v-container fluid>
        <v-data-table
          :headers="headers"
          :items="items"
          disable-pagination
          hide-default-footer
          class="tabla-datos"
        >
          <template #item="props">
            <tr class="customer-row">
              <td class="custom-cell">{{ props.item.id }}</td>
              <td class="custom-cell">{{ props.item.issueDate | formatIssueDate }}</td>
              <td class="custom-cell">{{ props.item.duration | formatDuration }}</td>
              <td class="custom-cell">{{ props.item.establishmentName }}</td>
              <td class="custom-cell">{{ props.item.endpoint }}</td>
              <td class="custom-cell">{{ props.item.method }}</td>
              <td class="custom-cell">{{ props.item.responseCode }}</td>
              <td class="custom-cell">{{ props.item.versionName }}</td>
              <td class="custom-cell">{{ props.item.startDate | formatStartDate }}</td>
              <td class="custom-cell">{{ props.item.endDate | formatEndDate }}</td>
              <td>
                <SeeMoreVue 
                  :item="props.item"
                />
              </td>
            </tr>
          </template>
        </v-data-table>
      </v-container>
    </v-main>

    </v-app>
</template>


<script>
import { mapGetters, mapState, mapActions } from 'vuex'
import { format } from 'date-fns';

import SeeMoreVue from '~/components/SeeMoreTab/SeeMore.vue';
import SidebarFilter from '~/components/sidebar/SidebarFilter.vue';
import HeaderTabsVue from '~/components/ui/Header.vue';


  export default {
    name: 'IndexPage',
    components: {
        SidebarFilter,
        HeaderTabsVue,
        SeeMoreVue,
    },
    filters: {
      formatStartDate(value) {
        if (!value) return ''
        return format(value, 'dd/MM/yyyy HH:mm:ss.SSS')
      },
      formatEndDate(value) {
        if (!value) return ''
        return format(value, 'dd/MM/yyyy HH:mm:ss.SSS')
      },
      formatDuration(value) {
        if (!value) return ''
        return value / 1000 + 'seg'
      },
      formatIssueDate(value) {
        if (!value) return ''
        return format(value, 'dd/MM/yyyy HH:mm:ss.SSS')
      },
    },
    data() {
      return {
        headers: [
        { text: 'ID', value: 'id', align:'start', sortable: false },
        { text: 'Issue Date', value: 'issueDate', align:'start', sortable: false },
        { text: 'Duration', value: 'duration' },
        { text: 'Name Establishment', value: 'establishmentName' },
        { text: 'Endpoint', value: 'endpoint' },
        { text: 'Method', value: 'method' },
        { text: 'Response Code', value: 'responseCode' },
        { text: 'Versión Name', value: 'versionName' },
        { text: 'StartDate', value: 'startDate' },
        { text: 'EndDate', value: 'endDate' },
        { text: 'Actions', value: '' },
      ],
        drawer: false,
        loading: false,
        overlay: false,
      }
    },
    computed: {
      ...mapGetters(['isSearchDataComplete']),
      ...mapState(['items', 'state', 'perPage', 'docsCount']),

    perPage: {
      get() {
        return this.$store.state.perPage
      },
      set(val) {
        this.$store.commit('setPerPage', val)
      },
    },
    selectedTab: {
      set(val) {
        this.$store.commit('setSelectedTab', val)
      },
      get() {
        return this.$store.state.selectedTab
      },
    },
    },
    methods: {
      ...mapActions(['nextPage', 'previousPage', 'changePerPage']),

      async changePerPage() {
        try {
          await this.$store.dispatch('changePerPage', this.perPage)
        } catch (error) {
          console.log('Error al cambiar la cantidad de elementos por página:', error)
        }
      },
      toggleDrawer() {
        this.drawer = !this.drawer
        this.overlay = !this.overlay
      }
    }
  }
</script>


<style>
  .v-main {
    background-color: #ebeff5 !important;
  }
  .tabla-datos  {
    background: #fff !important;
  }
  .customer-row {
    font-size: 0.875rem;
    color: #848d99 !important;
  }
  .customer-row:hover{
    background: #ebeff5 !important;
    transition: .5s ease;
  }
  /* .v-data-table-header {
    color: #848d99 !important;
  } */
  thead tr th span {
    color: #848d99 !important;
  }
  
</style>