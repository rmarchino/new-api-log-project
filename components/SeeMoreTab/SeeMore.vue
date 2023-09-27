<!-- eslint-disable vue/no-lone-template -->
<template>
  <v-row class="text-center containe-modal-deatail">
    <v-col cols="auto">
      <v-dialog
        v-model="dialog"
        width="750px"
      >
        <template #activator="{ on, attrs }">
          <v-btn
            color="#68c6e8"
            dark
            v-bind="attrs"
            class="button-ver-mas"
            v-on="on"
          >Ver más</v-btn>
        </template>
        <v-card>
          <v-toolbar
            dark
            color="#68c6e8"
            class="card-modal"
          >
            <v-btn
              icon
              dark
              @click="dialog = false"
            >
              <v-icon class="button-close-modal">mdi-close</v-icon>
            </v-btn>
            <v-toolbar-title class="text-center">Campos Adicionales</v-toolbar-title>
          </v-toolbar>
        </v-card>
        
        <template>
          <v-card
            class="mx-auto"
            max-width="750"
          >
            <v-container>
              <v-row dense>
                <!-- Company -->
                <v-col cols="12">
                  <v-card class="card-company">
                    <v-list-item >
                      <v-list-item-content>
                        <h3 class="mb-3">Company</h3>
                        <v-list-item-title>Company Id</v-list-item-title>
                        <v-list-item-subtitle class="mb-2">{{ item.idCompany }}</v-list-item-subtitle>
                      
                        <v-list-item-title>Company Business Name</v-list-item-title>
                        <v-list-item-subtitle class="mb-2">{{ item.companyBusinessName }}</v-list-item-subtitle>
                      
                        <v-list-item-title>Company Document Type Document Identity</v-list-item-title>
                        <v-list-item-subtitle class="mb-2">{{ item.companyTypeDocumentIdentity }}</v-list-item-subtitle>
                      
                        <v-list-item-title>Company Document Identity Number</v-list-item-title>
                        <v-list-item-subtitle class="mb-2">{{ item.companyDocumentIdentityNumber }}</v-list-item-subtitle>
                      </v-list-item-content>
                    </v-list-item>
                  </v-card>
                </v-col>

                <!-- Device and Local -->
                <v-col cols="12" class="container-card-device-local">
                  <v-card class="card-device">
                    <v-list-item>
                      <v-list-item-content>
                        <h3>Device</h3>
                        <v-list-item-title>Device Id</v-list-item-title>
                        <v-list-item-subtitle>{{ item.idDevice }}</v-list-item-subtitle>
                    
                        <v-list-item-title>Device Name</v-list-item-title>
                        <v-list-item-subtitle>{{ item.deviceName }}</v-list-item-subtitle>
                      </v-list-item-content>
                    </v-list-item>
                  </v-card>

                  <v-card class="card-local">
                    <v-list-item>
                      <v-list-item-content>
                        <h3>Local</h3>
                        <v-list-item-title>Id Establishment</v-list-item-title>
                        <v-list-item-subtitle>{{ item.idEstablishment }}</v-list-item-subtitle>

                        <v-list-item-title>Establishment Name</v-list-item-title>
                        <v-list-item-subtitle>{{ item.establishmentName }}</v-list-item-subtitle>
                      </v-list-item-content>
                    </v-list-item>
                  </v-card>
                </v-col> 

                <!-- Request Header and Reqest Body -->
                <v-col cols="12">
                  <v-card class="card-request-heaader-body">
                    <v-list-item>
                      <v-list-item-content>
                        <h3 class="mb-3">Request Body and Request Header</h3>
                        <v-list-item-title>Endpoint</v-list-item-title>
                        <v-list-item-subtitle>{{ item.endpoint }}</v-list-item-subtitle>

                        <v-list-item-title>Method</v-list-item-title>
                        <v-list-item-subtitle>{{ item.method }}</v-list-item-subtitle>

                        <v-list-item-title>Duration</v-list-item-title>
                        <v-list-item-subtitle>{{ item.duration | formatDuration }}</v-list-item-subtitle>

                        <v-list-item-title>IssueDate</v-list-item-title>
                        <v-list-item-subtitle>{{ item.issueDate |formatIssueDate }}</v-list-item-subtitle>

                        <v-list-item-title>StartDate</v-list-item-title>
                        <v-list-item-subtitle>{{ item.startDate | formatStartDate }}</v-list-item-subtitle>

                        <v-list-item-title>EndDate</v-list-item-title>
                        <v-list-item-subtitle>{{ item.endDate | formatEndDate }}</v-list-item-subtitle>

                        <div v-show="parsedRequestHeader">
                          <span>Request Header</span>
                          <v-treeview class="texto" :data="parsedRequestHeader"></v-treeview>
                        </div>

                        <div v-show="parsedRequestBody">
                          <span>Request Body</span>
                          <v-treeview class="texto" :data="parsedRequestBody"></v-treeview>
                        </div>
                      </v-list-item-content>
                    </v-list-item>
                  </v-card>
                </v-col>

                <!-- Response Header and Response Body -->
                <v-col cols="12">
                  <v-card class="card-response-header-body">
                    <v-list-item>
                      <v-list-item-content>
                        <h3 class="mb-3">Response Header and Response Body</h3>
                        <div v-show="item.responseCode">
                          <v-list-item-title>Response Code</v-list-item-title>
                          <v-list-item-subtitle>{{ item.responseCode }}</v-list-item-subtitle>
                        </div>

                        <div v-show="parsedResponseHeader">
                          <span>Response Header</span>
                          <v-treeview class="texto" :data="parsedResponseHeader"></v-treeview>
                        </div>

                        <div v-show="parsedResponseBody">
                          <span>Response Body</span>
                          <v-treeview class="texto" :data="parsedResponseBody"></v-treeview>
                        </div>
                      </v-list-item-content>
                    </v-list-item>
                  </v-card>
                </v-col> 

                <!-- Aplication -->
                <v-col>
                  <v-card class="card-application">
                    <v-list-item>
                      <v-list-item-content>
                        <h3 class="mb-3">Application</h3>
                        
                        <v-list-item-title>Id App</v-list-item-title>
                        <v-list-item-subtitle class="mb-2">{{ item.idApp }}</v-list-item-subtitle>
                      
                        <v-list-item-title>Id Channel</v-list-item-title>
                        <v-list-item-subtitle class="mb-2">{{ item.idChannel }}</v-list-item-subtitle>
                      
                        <v-list-item-title>Version Code</v-list-item-title>
                        <v-list-item-subtitle class="mb-2">{{ item.versionCode }}</v-list-item-subtitle>
                      
                        <v-list-item-title>Version Name</v-list-item-title>
                        <v-list-item-subtitle class="mb-2">{{ item.versionName }}</v-list-item-subtitle>
                      </v-list-item-content>
                    </v-list-item>
                  </v-card>
                </v-col>
              </v-row>
            </v-container> 
          </v-card>
        </template>
      </v-dialog> 
    </v-col>
  </v-row>
</template>

<script>
import Vue from 'vue';
import { format } from 'date-fns';
import TreeView from "vue-json-tree-view"

Vue.use(TreeView)

export default {
    filters: {
    formatStartDate(value) {
      if(!value) return '';
        return format((value), 'dd/MM/yyyy HH:mm:ss.SSS');
    },
    formatEndDate(value){
      if(!value) return '';
      return format((value), 'dd/MM/yyyy HH:mm:ss.SSS')
    },
    formatDuration(value){
      if(!value) return '';
      return (value / 1000) + 'seg'
    },
    formatIssueDate(value) {
      if(!value) return '';
      return format((value), 'dd/MM/yyyy HH:mm:ss.SSS');
    }
  },
    props: {
      // eslint-disable-next-line vue/require-default-prop
      item: Object,  // Recibe el objeto de datos como prop
    },
    data () {
      return {
        dialog: false,
        parsedRequestBody: null,
        parsedRequestHeader: null,
        parsedResponseHeader: null,
        parsedResponseBody: null,
      }
    },
    mounted() {
      this.parseRequestBody();
      this.parseRequestHeader();
      this.parseResponseHeader();
    },

    methods: {
      parseRequestBody() {
        if(typeof this.item.requestBody === 'string' && this.item.requestBody.trim() !== ''){
          try {
            const requestBodyObj = JSON.parse(this.item.requestBody);
            this.parsedRequestBody = requestBodyObj;
          } catch (error) {
            // eslint-disable-next-line no-console
            console.error('Error al analizar requestBody:', error);
            this.parsedRequestBody = null;
          }
        } else {
          this.parsedRequestBody = null;
        }
      },

      parseRequestHeader() {
        if(typeof this.item.requestHeader === 'string' && this.item.requestHeader.trim() !==''){
          const headerPairs = this.item.requestHeader.match(/\[.*?\]/g);
          if(headerPairs){
            const headerObject = {};
            for (const pair of headerPairs) {
              const [key, value] = pair.slice(1, -1).split(':');
              headerObject[key] = value;
            }
            this.parsedRequestHeader = headerObject;
          }else {
            this.parsedRequestHeader = null;
          }
        } else {
          this.parsedRequestHeader = null;
        }
      },

      parseResponseHeader() {
        if(typeof this.item.responseHeader === 'string' && this.item.responseHeader.trim() !==''){
          const headerPairs = this.item.responseHeader.match(/\[.*?\]/g);
          if(headerPairs) {
            const headerObject = {};
            for (const pair of headerPairs) {
              const [key, value] = pair.slice(1, -1).split(':');
              headerObject[key] = value;
            }
            this.parsedResponseHeader = headerObject;
          }else {
            this.parsedResponseHeader = null;
          }
        } else {
          this.parsedResponseHeader = null;
        }
      },

      parseResponseBody() {
        if(typeof this.item.responseBody === 'string' && this.item.responseBody.trim() !==''){
          try {
            const responseBodyObj = JSON.parse(this.item.responseBody);
            this.parsedResponseBody = responseBodyObj;
          } catch (error) {
            // eslint-disable-next-line no-console
            console.error('Error al analizar responseBody', error);
            this.parsedResponseBody = null;
          }
        }else {
          this.parsedResponseBody = null;
        }
      }
    }
};
</script>

<style>
/**button vermás */
.button-ver-mas {
  font-size: 0.875rem !important;
  color: #848d99 !important;
}

/** SectioN Company**/
h3 {
  color: #545d61;
}
.card-company {
  border: 0.25px solid #848d99;
  opacity: 0.5;
  filter: alpha(opacity=50);
  zoom: 1;
}
.v-application .text-h5 {
  color: #545d61;
}
.v-list-item .v-list-item__title, .v-list-item .v-list-item__subtitle  {
  color: #545d61;
  font-size: 0.875rem;
}
.theme--dark.v-list-item:not(.v-list-item--disabled) .v-list-item__subtitle, .theme--dark.v-list-item:not(.v-list-item--disabled) .v-list-item__action-text {
  color: #848d99;
}
/** Section Device AND LOCAL**/
.container-card-device-local {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 5px;
}
.card-device {
  border: 0.25px solid #848d99;
  opacity: 0.5;
  filter: alpha(opacity=50);
  zoom: 1;
}
.card-local {
  border: 0.25px solid #848d99;
  opacity: 0.5;
  filter: alpha(opacity=50);
  zoom: 1;
}

/** Section Request AND Response **/
.card-request-heaader-body {
  border: 0.25px solid #848d99;
}
.card-response-header-body {
  border: 0.25px solid #848d99;
}

/**Aplication **/
.card-application {
  border: 0.25px solid #848d99;
}

span {
  color: #545d61;
  font-size: 0.875rem;
}


</style>
