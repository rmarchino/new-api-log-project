<template>
    <v-row class="text-center containe-modal-deatail">
      <v-col cols="auto">
        <v-dialog
          v-model="dialog"
          width="800px"
        >
          <template #activator="{ on, attrs }">
            <v-btn
              color="primary"
              dark
              v-bind="attrs"
              class="button-ver-mas"
              v-on="on"
            >Ver más</v-btn>
          </template>
          <v-card>
            <v-toolbar
              dark
              color="primary"
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
          <v-card class="card-container">

            <!-- Company -->
            <v-list-item class="card-company">
              <v-list-item-content>
                <v-list-item-title>Company</v-list-item-title>
              </v-list-item-content>

              <v-list-item-content>
                <v-list-item-title>Company Id</v-list-item-title>
                <v-list-item-subtitle>{{ item.idComany }}</v-list-item-subtitle>
              </v-list-item-content>

              <v-list-item-content>
                <v-list-item-title>Company Business Name</v-list-item-title>
                <v-list-item-subtitle>{{ item.companyBusinessName }}</v-list-item-subtitle>
              </v-list-item-content>

              <v-list-item-content>
                <v-list-item-title>Company Document Type Document Identity</v-list-item-title>
                <v-list-item-subtitle>{{ item.companyTypeDocumentIdentity }}</v-list-item-subtitle>
              </v-list-item-content>

              <v-list-item-content>
                <v-list-item-title>Company Document Identity Number</v-list-item-title>
                <v-list-item-subtitle>{{ item.companyDocumentIdentityNumber }}</v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>

            <!-- Device and Local -->
            <v-list
              two-line
              subheader
            >
              <v-list-item>
                <v-list-item-content>
                  <v-list-item-title>Device</v-list-item-title>
                </v-list-item-content>

                <v-list-item-content>
                  <v-list-item-title>Device Id</v-list-item-title>
                  <v-list-item-subtitle>{{ item.idDevice }}</v-list-item-subtitle>
                </v-list-item-content>

                <v-list-item-content>
                  <v-list-item-title>Device Name</v-list-item-title>
                  <v-list-item-subtitle>{{ item.deviceName }}</v-list-item-subtitle>
                </v-list-item-content>
              </v-list-item>
              
              <v-list-item>
                <v-list-item-content>
                  <v-list-item-title>Local</v-list-item-title>
                </v-list-item-content>

                <v-list-item-content>
                  <v-list-item-title>Id Establishment</v-list-item-title>
                  <v-list-item-subtitle>{{ item.idEstablishment }}</v-list-item-subtitle>
                </v-list-item-content>

                <v-list-item-content>
                  <v-list-item-title>Establishment Name</v-list-item-title>
                  <v-list-item-subtitle>{{ item.establishmentName }}</v-list-item-subtitle>
                </v-list-item-content>
              </v-list-item>
            </v-list>

            <!-- Request Header and Reqest Body -->
            <v-list-item>
              <v-list-item-content>
                <v-list-item-title>Request Body and Request Header</v-list-item-title>
              </v-list-item-content>

              <v-list-item-content>
                <v-list-item-title>Endpoint</v-list-item-title>
                <v-list-item-subtitle>{{ item.endpoint }}</v-list-item-subtitle>
              </v-list-item-content>

              <v-list-item-content>
                <v-list-item-title>Method</v-list-item-title>
                <v-list-item-subtitle>{{ item.method }}</v-list-item-subtitle>
              </v-list-item-content>

              <v-list-item-content>
                <v-list-item-title>Duration</v-list-item-title>
                <v-list-item-subtitle>{{ item.duration | formatDuration }}</v-list-item-subtitle>
              </v-list-item-content>

              <v-list-item-content>
                <v-list-item-title>IssueDate</v-list-item-title>
                <v-list-item-subtitle>{{ item.issueDate |formatIssueDate }}</v-list-item-subtitle>
              </v-list-item-content>

              <v-list-item-content>
                <v-list-item-title>StartDate</v-list-item-title>
                <v-list-item-subtitle>{{ item.startDate | formatStartDate }}</v-list-item-subtitle>
              </v-list-item-content>

              <v-list-item-content>
                <v-list-item-title>EndDate</v-list-item-title>
                <v-list-item-subtitle>{{ item.endDate | formatEndDate }}</v-list-item-subtitle>
              </v-list-item-content>

              <v-list-item-content v-show="parsedRequestHeader">
                <v-list-item-title>Request Header</v-list-item-title>
                <v-list-item-subtitle>
                  <v-treeview :data="parsedRequestHeader"></v-treeview>
                </v-list-item-subtitle>
              </v-list-item-content>

              <v-list-item-content v-show="parsedRequestBody">
                <v-list-item-title>Request Body</v-list-item-title>
                <v-list-item-subtitle>
                  <v-treeview :data="parsedRequestBody"></v-treeview>
                </v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>

            <!-- Response Header and Response Body -->
            <v-list-item>
              <v-list-item-content>
                <v-list-item-title>Response Header and Response Body</v-list-item-title>
              </v-list-item-content>
              
              <v-list-item-content>
                <v-list-item-title>Response Code</v-list-item-title>
                <v-list-item-subtitle>{{ item.responseCode }}</v-list-item-subtitle>
              </v-list-item-content>

              <v-list-item-content v-show="parseResponseHeader">
                <v-list-item-title>Response Header</v-list-item-title>
                <v-list-item-subtitle>
                  <v-treeview :data="parsedResponseHeader"></v-treeview>
                </v-list-item-subtitle>
              </v-list-item-content>

              <v-list-item-content v-show="parseResponseBody">
                <v-list-item-title>Response Body</v-list-item-title>
                <v-list-item-subtitle>
                  <v-treeview :data="parseResponseBody"></v-treeview>
                </v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>

            <!-- Aplication -->
            <v-list-item>
              <v-list-item-content>
                <v-list-item-title>Application</v-list-item-title>
              </v-list-item-content>

              <v-list-item-content>
                <v-list-item-subtitle>Id App</v-list-item-subtitle>
                <v-list-item-subtitle>{{ item.idApp }}</v-list-item-subtitle>
              </v-list-item-content>

              <v-list-item-content>
                <v-list-item-subtitle>Id Channel</v-list-item-subtitle>
                <v-list-item-subtitle>{{ item.idChannel }}</v-list-item-subtitle>
              </v-list-item-content>

              <v-list-item-content>
                <v-list-item-subtitle>Version Code</v-list-item-subtitle>
                <v-list-item-subtitle>{{ item.versionCode }}</v-list-item-subtitle>
              </v-list-item-content>

              <v-list-item-content>
                <v-list-item-subtitle>Version Name</v-list-item-subtitle>
                <v-list-item-subtitle>{{ item.versionName }}</v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>
          </v-card>
        </v-dialog> 
      </v-col>
    </v-row>
</template>
  
<script>
  import Vue from "vue";
  import TreeView from "vue-json-tree-view"
  import { format } from 'date-fns';
  
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
  /** SectioN Company**/

  
  /** SectioN Device AND LOCAL**/
 
  /** SectioN Request AND Response **/
 
  
  /**Aplication **/

  
</style>