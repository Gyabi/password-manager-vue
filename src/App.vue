<template>
  <v-app>
    <div v-if="editMode===true">
      <v-app-bar app color="secondary" dark>
        <v-toolbar-title>Password Manager</v-toolbar-title>
        <v-spacer></v-spacer>
        <v-toolbar-title>Edit Mode</v-toolbar-title>
      </v-app-bar>
    </div>
    <div v-else>
      <v-app-bar app color="primary" dark>
        <v-toolbar-title>Password Manager</v-toolbar-title>
        <v-spacer></v-spacer>
        <v-toolbar-title>Main Mode</v-toolbar-title>
      </v-app-bar>
    </div>

    <v-main>
      <!-- <v-img alt="test image" contain src="@/assets/logo.png" class="shrink mr-2" transition="scale-transition" width="40"/> -->
      <div v-if="editMode === false">
        <Main :jsondata="jsondata" @changeEdit="changeEdit"></Main>
      </div>
      <div v-else>
        <Edit :jsondata="jsondata" @changeEdit="changeEdit" @update-Json="updateJson"></Edit>
      </div>
    </v-main>

  </v-app>
</template>

<script>
import Main from './components/Main';
import Edit from './components/Edit';
const fs = require('fs');
const remote = require('electron').remote;
const path = require('path');


const rootpath = remote.app.getAppPath();
console.log(rootpath);
var jsonpath;
if(process.env.NODE_ENV === 'development'){
  // 開発時
  jsonpath = rootpath + "/../src/password/password.json";
}else{
  // 本番環境（app.asarのpathが来るので）
  jsonpath = path.dirname(rootpath) + "/src/password/password.json";
}

export default {
  name: 'App',

  components: {
    Main:Main,
    Edit:Edit
  },

  data: () => ({
    //
    jsondata:[],
    editMode: false,
  }),

  methods:{
    // jsonファイルの読み込み
    reloadJson(){
      this.jsondata = JSON.parse(fs.readFileSync(jsonpath, 'utf8'));
      // this.jsondata = JSON.parse(fs.readFileSync('C:/Users/buyuu/Programming/005_Electron/password-manager-vue/src/password/password.json', 'utf8'));
    },
    // editモードとの切り替え
    changeEdit(){
      console.log("changeEdit");
      this.editMode = !this.editMode;
    },
    // jsonファイルの保存
    saveJson(){
      console.log("saveJson");
      // fs.writeFileSync('C:/Users/buyuu/Programming/005_Electron/password-manager-vue/src/password/password.json', JSON.stringify(this.jsondata, null, 2));
      fs.writeFileSync(jsonpath, JSON.stringify(this.jsondata, null, 2));
    },
    // jsonファイルの更新（Edit.vueからコールされる）
    updateJson(jsondata){
      console.log("updateJson");
      this.jsondata = jsondata;
      // jsonの保存
      this.saveJson();
      // 一応リロード
      this.reloadJson();
      // this.editMode = false;
    }
},
  mounted(){
    // this.jsondata = JSON.parse(fs.readFileSync('@/src/password/password.json', 'utf8'));
    // 準備ができたらjsonファイルを読み込む
    this.jsondata = JSON.parse(fs.readFileSync(jsonpath, 'utf8'));
    // this.jsondata = JSON.parse(fs.readFileSync('C:/Users/buyuu/Programming/005_Electron/password-manager-vue/src/password/password.json', 'utf8'));
  }
  
};


</script>

<style>
/* .v-application{
    font-family: "M Plus 1p" !important;
} */
</style>
