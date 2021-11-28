<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
          transition="scale-transition"
          width="40"
        />

        <v-img
          alt="Vuetify Name"
          class="shrink mt-1 hidden-sm-and-down"
          contain
          min-width="100"
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-name-dark.png"
          width="100"
        />
      </div>

      <v-spacer></v-spacer>

      <v-btn
        href="https://github.com/vuetifyjs/vuetify/releases/latest"
        target="_blank"
        text
      >
        <span class="mr-2">Latest Release</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <v-img alt="test image" contain src="@/assets/logo.png" class="shrink mr-2" transition="scale-transition" width="40"/>

      <HelloWorld/>
      <Top :jsondata="jsondata"/>
      <!-- <button v-on:click="getJson">aaa</button> -->
    </v-main>
  </v-app>
</template>

<script>
import HelloWorld from './components/HelloWorld';
import Top from './components/Top';
const fs = require('fs');


export default {
  name: 'App',

  components: {
    HelloWorld,
    Top,
  },

  data: () => ({
    //
    jsondata:[]
  }),

  methods:{
    reloadJson(){
      this.jsondata = JSON.parse(fs.readFileSync('C:/Users/buyuu/Programming/005_Electron/password-manager-vue/src/password/password.json', 'utf8'));
    }
  },
  mounted(){
    // this.jsondata = JSON.parse(fs.readFileSync('@/src/password/password.json', 'utf8'));
    this.jsondata = JSON.parse(fs.readFileSync('C:/Users/buyuu/Programming/005_Electron/password-manager-vue/src/password/password.json', 'utf8'));
  }
  
};


</script>
