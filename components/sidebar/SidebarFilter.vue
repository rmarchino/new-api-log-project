<template>
   <v-navigation-drawer
      :value="drawer"
      app
      absolute
      temporary
      width="350"
   >
      <div class="button-close-sidebar">
         <v-btn class="button-close fs-1" @click="toggleDrawer">X</v-btn>
      </div>
      <h2 class="title-form-sedebar">Búsqueda Avanzada</h2>

      <template>
         <v-form>
            <v-container>
               <v-row>
                  <v-col cols="12">
                     <v-text-field
                        v-model="idCompany"
                        label="idComany"
                        clearable
                        outlined
                        color="#68c6e8"
                     />
                  </v-col>
                  <v-col cols="12">
                     <v-text-field
                        v-model="idDevice"
                        label="idDevice"
                        clearable
                        outlined
                        color="#68c6e8"
                     />
                  </v-col>
                  <v-col cols="12">
                     <v-text-field
                        v-model="endpoint"
                        label="endpoint"
                        clearable
                        outlined
                        color="#68c6e8"
                     ></v-text-field>
                  </v-col>
                  <v-col cols="12">
                     <v-menu
                        ref="startDateMenu"
                        v-model="startDateMenu"
                        :close-on-content-click="false"
                        :nudge-width="200"
                     >
                        <template #activator="{ on }">
                           <v-text-field
                              v-model="dateRange"
                              label="Rango de Fechas"
                              prepend-icon="mdi-calendar"
                              readonly
                              clearable
                              outlined
                              color="#68c6e8"
                              v-on="on"
                           ></v-text-field>
                        </template>
                        <v-date-picker
                           v-model="dateRange" 
                           no-title 
                           range 
                           scrollable
                           color="#68c6e8"
                        >
                           <div class="cotainer-buttons-close-save">
                              <v-btn class="button-save" @click="saveDateRange">Save</v-btn>
                              <v-btn class="button-cancel" @click="cancelDateRange">Cancel</v-btn>
                           </div>
                        </v-date-picker>
                     </v-menu>
                  </v-col>
                  <v-col cols="12" class="container__button-search">
                     <v-btn class="button__search" @click="search">Buscar</v-btn>
                     <LoadingVue :loading="isLoading"/> 
                  </v-col>
               </v-row>
            </v-container>
         </v-form>
      </template>
   </v-navigation-drawer>
</template>


<script>
import { mapGetters, mapState } from 'vuex'
import LoadingVue from '../loading/Loading.vue'

export default {
   components: {
      // eslint-disable-next-line vue/no-unused-components
      LoadingVue,
   },
   // eslint-disable-next-line vue/require-prop-types
   props: ['drawer'],
   data() {
      return {
         overlay: false,
         isLoading: false,
         loading: false,
         startDateMenu: false,
         endDateMenu: false,
         dateRange: [],
      }
   },
   computed: {
    ...mapGetters(['isSearchDataComplete']),
    ...mapState(['state']),

    idCompany: {
      set(val) {
        this.$store.commit('setIdCompany', val)
      },
      get() {
        return this.$store.state.searchData.idCompany
      },
    },
    idDevice: {
      set(val) {
        this.$store.commit('setIdDevice', val)
      },
      get() {
        return this.$store.state.searchData.idDevice
      },
    },
    startDate: {
      set(val) {
        this.dateRange[0] = val 
      },
      get() {
        return this.dateRange[0] 
      },
    },
    endDate: {
      set(val) {
        this.dateRange[1] = val
      },
      get() {
        return this.dateRange[1]
      },
    },
    endpoint: {
      set(val) {
        this.$store.commit('setEndpoint', val)
      },
      get() {
        return this.$store.state.searchData.endpoint
      },
    },
   },
   methods: {
      toggleDrawer() {
         this.$emit('toggleDrawer')
       },
       cancelDateRange() {
         this.startDateMenu = false
         this.dateRange = [this.startDate, this.endDate]
       },
       saveDateRange() {
         this.startDateMenu = false
         this.$store.commit('setDateRange', { startDate: this.startDate, endDate: this.endDate });
      },
      async search() {
         try {
            // Activa el Loader
            this.isLoading = true;
            await this.$store.dispatch('searchData')
            
         } catch (error) {
            // eslint-disable-next-line no-console
            console.error('Error al obtener datos:', error)
         } finally {
            setTimeout(() => {
               this.isLoading = false;
            }, 3000);
         }
      }
   }
}
</script>


<style>
h2 {
  color: #545d61;
  font-size: 1.25rem;
}
.title-form-sedebar{
   padding: 12px;
}

/**Button cerrar */
.button-close-sidebar {
   display: flex;
   flex-direction: row;
   justify-content: flex-end;
   padding: 10px;
}
.button-close {
   background: #fff !important;
   color: #545d61 !important;
   font-size: 20px !important;
}
.v-btn--is-elevated {
   box-shadow: none !important;
}

/**Container Filter */
.v-navigation-drawer__content  {
    background-color: #fff;
 }
 
 /**Label */
 .theme--dark.v-label {
    color: #545d61;
    font-size: 0.875rem !important;
}
 
 /**Textarea */
 .theme--dark.v-input input, .theme--dark.v-input textarea {
    color: #545d61 !important;
    font-size: 0.875rem;
 }
 
 /**texfield-border */
 .theme--dark.v-text-field--outlined:not(.v-input--is-focused):not(.v-input--has-state) > .v-input__control > .v-input__slot fieldset{
    color: #545d61 !important;
 }
 
 .v-icon.v-icon {
   color: #68c6e8;
 }

 /**Date Picker */
 .theme--dark.v-date-picker-header .v-date-picker-header__value:not(.v-date-picker-header__value--disabled) button:not(:hover):not(:focus){
   color: #545d61;
 }

 .theme--dark.v-date-picker-table th, .theme--dark.v-date-picker-table .v-date-picker-table--date__week {
   color: #545d61;
 }

 .v-date-picker-header {
   background-color: #fff;
 }
 .v-btn__content {
   color: #545d61;
 }
 .v-date-picker-table {
   background: #fff;
 }
 tr {
   color: #545d61;
 }
 .v-card > *:last-child:not(.v-btn):not(.v-chip):not(.v-avatar) {
   background: #fff;
 }

 .v-date-picker-years.v-date-picker-years {
   background: #fff;
 }
 .v-date-picker-years {
   color: #545d61;
 }

 .button-save {
   background: #68c6e8 !important;
 }
 .button-cancel{
   background: #ef5555 !important;
 }

 .v-menu__content {
   box-shadow: none !important;
 }

 /** Button search **/
 .container__button-search{
   display: flex;
   flex-direction: row;
   justify-content: center;
   align-items: center;
 }

 .button__search {
   background: #68c6e8 !important;
   color: #fff !important;
   font-size: 0.875rem !important;
   height: 45px !important;
   width: 150px !important;
 }

</style>