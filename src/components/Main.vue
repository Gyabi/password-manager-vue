<template>
    <v-app>
        <v-main>
            <v-container>
                <v-row justify="center">
                    <div v-for="(item,i) in jsondata" :key="i">
                        <v-col cols="12" sm="8" md="6" lg="4" xl="3" class="cards" >
                            <v-card min-width="300px" min-height="300" height="100%" elevation="17">
                                <v-card-title>{{item.service}}</v-card-title>
                                <v-divider></v-divider>
                                <div v-for="(value, name, j) in item" :key="j">
                                    <div v-if="name !== 'service'">
                                        <v-card-subtitle class="my-4 text-subtitle-1">{{name}}</v-card-subtitle>
                                        <v-card-subtitle class="text-subtitle-1" @click="copyToClipboard(value)">{{value}}</v-card-subtitle>
                                    </div>
                                </div>
                            </v-card>
                        </v-col>
                    </div>
                        <v-col cols="12" sm="8" md="6" lg="4" xl="3" class="cards" >
                            <v-img :src="require('../assets/edit.png')" class="edit-icon rounded-circle" contain height="200" @click="throwEdit()"/>
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

        showAlert(){
            this.show = true;
            setTimeout(() => {
                this.show = false;
            }, 1000);
        },
        throwEdit(){
            console.log("throwEdit");
            this.$emit('changeEdit');
        }
    }
};
</script>
<style>

</style>