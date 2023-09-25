<template>
  <v-container>
    <v-app-bar
      app
      absolute
      class="white"
    >
    <v-app-bar-nav-icon class="menu-icon" @click="toggleDrawer" ></v-app-bar-nav-icon>
    <v-list-item-avatar width="120" height="90">
      <v-img
      src="https://ari-s3bucket.s3.amazonaws.com/assets/logo/arisale_logotipo.png"
      ></v-img>
    </v-list-item-avatar>
    <template v-if="isSearchDataComplete" #extension>
      <v-tabs v-model="selectedTab" align-with-title @change="searchRefresh" >
        <v-tab v-for="(tab, index) in tabs" :key="index" class="container-tabs">
          <h2 class="card-tabs">{{ tab.name }}</h2>
        </v-tab> 
      </v-tabs>
      <v-car class="card-actualizar">
        <v-btn @click="searchRefresh">
          Actualizar
          <v-icon>mdi-history</v-icon>
        </v-btn>
      </v-car>
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
      }
    },
    computed: {
      ...mapGetters(['isSearchDataComplete', 'isLoading']),
      ...mapState(['state']),

      selectedTab: {
        set(val) {
          this.$store.commit('setSelectedTab', val);
        },
        get() {
          return this.$store.state.selectedTab;
        }
      },
    },
    methods: {
      ...mapActions(['setLoading']),

    // async search() {
    //   try {
    //     await this.$store.dispatch('searchData');
    //   } catch (error) {
    //     // eslint-disable-next-line no-console
    //     console.error('Error al obtener datos:', error);
    //   } finally {
    //     this.loading = false;
    //   }
    // },
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
      toggleDrawer() {
        this.$emit('toggleDrawer')
      }
    }
  }
</script>


<style>
  .container-tabs {
    border: 1px solid #c5d1db;
    margin-bottom: 5px;
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
</style>