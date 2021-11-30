<template>
    <v-app>
        <v-main>
            <v-container>
                <v-row justify="center">
                    <!-- jsonのリストをループ -->
                    <div v-for="(item,i) in jsondata" :key="i">
                        <v-col cols="12" sm="8" md="6" lg="4" xl="3" class="cards" >
                            <v-card min-width="300px" min-height="300" height="100%" elevation="17">
                                <v-card-title>{{item.service}}</v-card-title>
                                <v-divider></v-divider>
                                <!-- jsonの各要素をループ -->
                                <div v-for="(value, name, j) in item" :key="j">
                                    <div v-if="name !== 'service'">
                                        <v-card-title class="my-4 text-title-1">{{name}}</v-card-title>
                                        <v-card-subtitle class="text-subtitle-1 " @click="copyToClipboard(value)"><p class="font-weight-black text-decoration-underline">{{value}}</p></v-card-subtitle>
                                    </div>
                                </div>
                            </v-card>
                        </v-col>
                    </div>
                </v-row>
                <v-row justify="center">
                        <v-col cols="12" sm="8" md="6" lg="4" xl="3">
                                <v-card min-width="150" max-width="200" min-height="200" max-height="200" class="mx-auto">
                                    <v-card-title>Edit</v-card-title>
                                    <v-divider></v-divider>
                                    <div class="mx-auto">
                                        <p></p>
                                        <v-img contain src="@/assets/edit.png" class="edit-icon" height="100" @click="throwEdit()"/>
                                    </div>
                                </v-card>
                        </v-col>
                </v-row>


                <v-overlay :value="show">
                    <v-alert prominent type="success" v-if="show === true">Copy!!!</v-alert>
                </v-overlay>
            </v-container>
        </v-main>
    </v-app>
</template>
<script>
export default {
    data:() =>({
        show: false,
    }),
    props:{
        jsondata:{
            type:Array,
            default:() => []
        }
    },
    methods:{
        // クリップボードにテキストをコピー
        copyToClipboard(text) {
            navigator.clipboard.writeText(text)
            .then(() => {
                console.log("copied!")
            })
            .catch(e => {
                console.error(e)
            })
            this.showAlert();
        },

        // コピーした時に１秒間showをtrueにする
        showAlert(){
            this.show = true;
            setTimeout(() => {
                this.show = false;
            }, 1000);
        },
        // Appへeditモードへの変更を伝える
        throwEdit(){
            console.log("throwEdit");
            this.$emit('changeEdit');
        }
    }
};
</script>
<style>

</style>