<template>
  <v-container>
    <v-app-bar
      app
      absolute
      class="white"
    >
      <v-app-bar-nav-icon class="menu-icon" @click="toggleDrawer" ></v-app-bar-nav-icon>
      <v-list-item-avatar width="120" height="90">
        <v-img src="https://ari-s3bucket.s3.amazonaws.com/assets/logo/arisale_logotipo.png"></v-img>
      </v-list-item-avatar>
      
      <template v-if="isSearchDataComplete" #extension>
        <v-tabs v-model="selectedTab" align-with-title @change="searchRefresh">
          <v-tab v-for="(tab, index) in tabs" :key="index" class="container-tabs">
            <h2 class="card-tabs">{{ tab.name }}</h2>
          </v-tab>

          <v-spacer></v-spacer>

          <v-col
            class="d-flex select-pagination"
          >
            <v-select
              v-model="perPage"
              :items="[10, 15, 25, 50, 100]"
              color="#545d61"
              label="Filtrar por cantidad de documentos"
              @input="changePerPage"
            ></v-select>
          </v-col>

          <v-btn class="card-button-actualizar" @click="searchRefresh" >
            <span>Actualizar</span>
            <v-icon class="icon-actualizar">mdi-history</v-icon>
          </v-btn>

          <v-spacer></v-spacer>

          <div>
            <p>Total: {{ docsCount }}</p>
          </div>
          <v-spacer></v-spacer>
        </v-tabs>
      </template>
    </v-app-bar>
  </v-container>
</template>


<script>
import { mapGetters, mapState, mapActions } from 'vuex'

  export default {
    // eslint-disable-next-line vue/require-prop-types
    props: ['drawer', 'overlay'],

    data() {
      return {
        tabs: [
          {
            name: 'Exceptions'
          },
          {
            name: 'Pinpad Events'
          },
          {
            name: 'Web Services'
          }
        ],
        loading: false,
        // elementPagination: [10, 15, 25, 50, 100],
      }
    },
    computed: {
      ...mapGetters(['isSearchDataComplete', 'isLoading']),
      ...mapState(['state', 'perPage', 'docsCount']),

      selectedTab: {
        set(val) {
          this.$store.commit('setSelectedTab', val);
        },
        get() {
          return this.$store.state.selectedTab;
        }
      },
      perPage: {
        get() {
          return this.$store.state.perPage
        },
        set(val) {
          this.$store.commit('setPerPage', val)
        },
      }
    },
    methods: {
      ...mapActions(['setLoading', 'changePerPage']),

    async searchRefresh() {
      try {
        this.setLoading(true);
        await this.$store.dispatch('searchData');
        
      } catch (error) {
        // eslint-disable-next-line no-console
        console.error('Error al obtener datos:', error);
      } finally {
        setTimeout(() => {
          this.setLoading(false);
        }, 2000)
      }
    },

    async changePerPage() {
        try {
          await this.$store.dispatch('changePerPage', this.perPage)
        } catch (error) {
          // eslint-disable-next-line no-console
          console.log('Error al cambiar la cantidad de elementos por página:', error)
        }
      },
      toggleDrawer() {
        this.$emit('toggleDrawer')
      }
    }
  }
</script>


<style>
  .container-tabs {
    border: 1px solid #c5d1db;
    margin-bottom: 8px;
  }
  .theme--dark.v-btn.v-btn--has-bg {
    background-color: #fff;
  }
  .card-actualizar {
    border: 1px solid #c5d1db;
  }
  .card-tabs {
    background: #fff !important;
    color: #545d61 !important;
    font-size: 0.875rem !important;
    text-transform: capitalize !important;
  }
  .v-sheet.v-card:not(.v-sheet--outlined) {
    box-shadow: none !important;
  }
  .v-tabs-slider {
    color: #68c6e8;
  }

  .v-select__selection--comma {
    color: #545d61;
  }

  /***Tabs actualizar and seleccionar */
  .v-btn__content {
    text-transform: capitalize;
  }
  .icon-actualizar {
    font-size: 0.875rem !important;
  }

  .card-button-actualizar {
    border: 1px solid #c5d1db;
    padding: 10px;
  }
</style>