<!-- eslint-disable vue/order-in-components -->
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
        <v-row v-if="isSearchDataComplete" >
          <v-data-table
            :items="items"
            :headers="headers"
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

        <!-- pagination -->
          <v-col
            v-show="showPagination"
            cols="12"
            sm="6"
            class="container-pagination"
          >
            <v-btn elevation="1" :loading="loadingMore" @click="previousPage">Previous</v-btn>
            <v-btn elevation="1" :loading="loadingMore" @click="nextPage">Next</v-btn>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
      <LoadingVue :loading="isLoading || loadingMore" />
    </v-app>
</template>


<script>


import { mapGetters, mapState, mapActions } from 'vuex';
import { format } from 'date-fns';

import SeeMoreVue from '~/components/SeeMoreTab/SeeMore.vue';
import SidebarFilter from '~/components/sidebar/SidebarFilter.vue';
import HeaderTabsVue from '~/components/ui/Header.vue';
import LoadingVue from '~/components/loading/Loading.vue';



  export default {
    name: 'IndexPage',
    components: {
        SidebarFilter,
        HeaderTabsVue,
        SeeMoreVue,
        LoadingVue,
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
        showPagination: false,
        loadingMore: false,
      }
    },
    computed: {
      ...mapGetters(['isSearchDataComplete', 'isLoading']),
      ...mapState(['items', 'state', 'perPage', 'newItems']),

    
      selectedTab: {
        set(val) {
          this.$store.commit('setSelectedTab', val)
        },
        get() {
          return this.$store.state.selectedTab
        },
      },
    },
    
    mounted(){
      // eslint-disable-next-line no-undef
      this.debouncedHandleScroll = _.debounce(this.handleScroll, 500)
      window.addEventListener('scroll', this.debouncedHandleScroll);
    },
    beforeDestroy(){
      window.removeEventListener('scroll', this.debouncedHandleScroll);
    },

    methods: {
      ...mapActions(['nextPage', 'previousPage']),

      handleScroll() {
        // Lógica para detectar el final de la página
        if (this.isAtBottom() && !this.loadingMore && !this.isLoading) {

          // Evitar cargas adicionales si ya se están cargando datos
          this.loadingMore = true;

          // Lógica cuando se alcanza el final de la página
          this.loadMoreData();

        }
      },

      isAtBottom() {
        // Lógica para verificar si has llegado al final de la página
        return (
          window.innerHeight + window.scrollY >= document.body.offsetHeight
        );
      },

      async loadMoreData() {
        try {
          this.loadingMore = true;
        
          // Lógica para cargar más datos desde la API
          await this.$store.dispatch('nextPage');

          console.log('New Items:', this.$store.state.newItems);
        
          // Utilizar la nueva mutación para agregar elementos al final de la lista existente
          this.$store.commit('addItems', this.$store.state.newItems.value);

        
        } catch (error) {
          // eslint-disable-next-line no-console
          console.error('Error al cargar más datos', error);
        } finally {
          this.loadingMore = false;
        }
      },
      
      toggleDrawer() {
        this.drawer = !this.drawer
        this.overlay = !this.overlay
      }
    }
  };
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
  thead tr th span {
    color: #848d99 !important;
  }
  .container-pagination {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    column-gap: 10px;
    justify-content: center;
    align-content: center;
  }
  .v-text-field.v-text-field--solo:not(.v-text-field--solo-flat) > .v-input__control > .v-input__slot {
    background: #fff;
    color: #545d61;
    font-size: 0.875rem;
  }
  .v-sheet.v-list:not(.v-sheet--outlined) {
    background: #fff;
    color: #545d61;
    border-radius: 10px;
  }
  .v-select.v-input--dense .v-select__selection--comma {
    color: #545d61;
  }
  .v-application p {
    color: #545d61;
    font-size: 0.875rem;
  }
  .v-data-table  {
    width: 100%;
  }
</style>