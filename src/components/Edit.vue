<template>
    <v-app>
        <v-main>
            <v-container>
                <v-row justify="center">
                    <div v-for="(item,i) in jsondata" :key="i">
                        <v-col cols="12" sm="8" md="6" lg="4" xl="3" class="cards" >
                            <v-card min-width="300px" min-height="300" height="100%" elevation="17" @click.stop="openEdit(i)">
                                <v-card-title>{{item.service}}</v-card-title>
                                <v-divider></v-divider>
                                <div v-for="(value, name, j) in item" :key="j">
                                    <div v-if="name !== 'service'">
                                        <v-card-title class="my-4 text-title-1">{{name}}</v-card-title>
                                        <v-card-subtitle class="text-subtitle-1"><p class="font-weight-black text-decoration-underline">{{value}}</p></v-card-subtitle>
                                    </div>
                                </div>
                            </v-card>
                        </v-col>
                    </div>
                </v-row>
                <v-row justify="center">
                        <v-col cols="12" sm="8" md="6" lg="4" xl="3" class="cards" >
                                <v-card min-width="150" max-width="200" min-height="200" max-height="200" elevation="17" class="mx-auto">
                                    <v-card-title>Edit end</v-card-title>
                                    <v-divider></v-divider>
                                    <div class="mx-auto">
                                        <p></p>
                                        <v-img src="@/assets/check.png" class="edit-icon rounded-circle" contain height="100" @click="throwEdit()"/>
                                    </div>
                                </v-card>
                        </v-col>
                        <v-col cols="12" sm="8" md="6" lg="4" xl="3" class="cards" >
                                <v-card min-width="150" max-width="200" min-height="200" max-height="200" elevation="17" class="mx-auto">
                                    <v-card-title>Add</v-card-title>
                                    <v-divider></v-divider>
                                    <div class="mx-auto">
                                        <p></p>
                                        <v-img src="@/assets/add.png" class="edit-icon rounded-circle" contain height="100" @click="showAddDialog()"/>
                                    </div>
                                </v-card>
                        </v-col>
                </v-row>

                <!-- 拡大表示用のコンポーネント。dialogは画面外を触ると勝手に閉じる -->
                <v-dialog v-model="showEdit" scrollable max-width="80%">
                    <v-card min-width="300px" min-height="300" height="100%" elevation="17" @click.stop="">
                    <v-card-title>Edit</v-card-title>
                    <v-divider></v-divider>
                    <v-container>
                        <div v-for="(value, name, j) in EditData" :key="j">
                        <v-row>
                            <v-col>
                                <v-card-title class="">{{name}}::</v-card-title>
                            </v-col>
                            <v-col>
                                <v-text-field v-model="EditData[name]"></v-text-field>
                            </v-col>
                        </v-row>
                        </div>
                        <v-row justify="center">
                            <v-col justify="center">
                                <v-btn elevation="8" color="secondary" @click="showDeleteDialog=true">delete</v-btn>
                            </v-col>
                            <v-col justify="center">
                                <v-btn elevation="8" color="primary" @click="updateJson()">submit</v-btn>
                            </v-col>
                        </v-row>
                    </v-container>
                </v-card>
                </v-dialog>


                <!-- 削除命令の確認用ダイアログ -->
                <v-dialog v-model="showDeleteDialog" scrollable max-width="60%">
                    <v-card>
                        <v-card-title>
                            <div>Delete</div>
                        </v-card-title>
                        <v-card-text>
                            <p>Are you sure you want to permanently delete this password?</p>
                        </v-card-text>

                        <v-card-actions>
                            <v-spacer></v-spacer>
                            <v-btn @click="showDeleteDialog=false">cancel</v-btn>
                            <v-btn @click="deleteJson()">DELETE</v-btn>
                        </v-card-actions>
                    </v-card>
                </v-dialog>


                <!-- 追加用ダイアログ -->
                <v-dialog v-model="showAdd" scrollable max-width="80%">
                    <v-card min-width="300px" min-height="300" height="100%" elevation="17" @click.stop="">
                    <v-card-title>Add</v-card-title>
                    <v-divider></v-divider>
                    <v-container>
                        <v-row>
                            <v-col>
                                <v-card-title class="">service</v-card-title>
                            </v-col>
                            <v-col>
                                <v-card-title class="">::</v-card-title>
                            </v-col>
                            <v-col>
                                <v-text-field v-model="addvalues[0]"></v-text-field>
                            </v-col>
                        </v-row>
                        <div v-for="i in addComponentNum" :key="i">
                            <v-row>
                                <v-col>
                                    <v-text-field v-model="addkeys[i]"></v-text-field>
                                </v-col>
                                <v-col>
                                    <v-card-title class="">::</v-card-title>
                                </v-col>
                                <v-col>
                                    <v-text-field v-model="addvalues[i]"></v-text-field>
                                </v-col>
                            </v-row>
                        </div>
                        <v-row justify="center">
                            <v-col justify="center">
                                <v-btn elevation="8" color="error" @click="DeleteComponent()">delete</v-btn>
                            </v-col>
                            <v-col justify="center">
                                <v-btn elevation="8" color="info" @click="AddComponent()">add</v-btn>
                            </v-col>
                            <v-col justify="center">
                                <v-btn elevation="8" color="primary" @click="AddJson()">submit</v-btn>
                            </v-col>
                        </v-row>
                    </v-container>
                </v-card>
                </v-dialog>
            </v-container>
        </v-main>
    </v-app>
</template>
<script>
import Vue from 'vue';
export default {
    data:() =>({
        show: false,
        showEdit: false,
        EditData:{},
        EditDataIndex:null,
        showDeleteDialog: false,

        showAdd: false,
        addComponentNum: 1,
        addkeys: [],
        addvalues: [],
    }),
    props:{
        jsondata:{
            type:Array,
            default:() => []
        }
    },
    methods:{
        showAlert(){
            this.show = true;
            setTimeout(() => {
                this.show = false;
            }, 1000);
        },
        throwEdit(){
            console.log("throwEdit");
            this.$emit('changeEdit');
        },
        openEdit(index){
            // 値渡し
            this.EditData = Vue.util.extend({},this.jsondata[index]);
            this.EditDataIndex = index;
            this.showEdit = true;
        },
        updateJson(){
            console.log("updateJson");
            this.jsondata[this.EditDataIndex] = this.EditData;
            this.$emit('update-Json', this.jsondata);
            this.showEdit = false;
        },
        deleteJson(){
            console.log("deleteJson");
            this.jsondata.splice(this.EditDataIndex, 1);
            this.$emit('update-Json', this.jsondata);
            this.showEdit = false;
            this.showDeleteDialog = false;
        },
        clearAddDialog(){
            this.addkeys = ["",""];
            this.addvalues = ["",""];
            this.addComponentNum = 1;
            this.addkeys[0] = "service";
        },
        showAddDialog(){
            this.clearAddDialog();
            this.showAdd = true;
        },
        AddComponent(){
            this.addComponentNum++;
            this.addkeys.push("");
            this.addvalues.push("");
        },
        DeleteComponent(){
            if(this.addComponentNum > 1){
                this.addComponentNum--;
                this.addkeys.pop();
                this.addvalues.pop();
            }
        },
        AddJson(){
            console.log("AddJson");
            var AddJson = {};
            for(var i=0; i<=this.addComponentNum; i++){
                console.log(this.addkeys[i]);
                AddJson[this.addkeys[i]] = this.addvalues[i];
            }
            this.jsondata.push(AddJson);
            this.$emit('update-Json', this.jsondata);
            this.showAdd = false;
        }
    }
};
</script>
<style>

</style>