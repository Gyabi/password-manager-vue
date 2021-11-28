<template>
<v-container>
    <v-row justify="center">
        <div v-for="(item,i) in jsondata" :key="i">
            <v-col cols="12" sm="8" md="6" lg="4" xl="3" class="cards" >
                <v-card min-width="300px" min-height="300" height="100%" elevation="17">
                    <v-card-title>{{item.service}}</v-card-title>
                    <v-divider></v-divider>
                    <v-card-text>{{item.pass}}</v-card-text>
                </v-card>
            </v-col>
        </div>
            <v-col cols="12" sm="8" md="6" lg="4" xl="3" class="cards" >
                <v-img :src="require('../assets/edit.png')" class="edit-icon" contain height="200" />
            </v-col>
    </v-row>
</v-container>

        <!-- <v-row justify="center">
        <v-expansion-panels popout>
        <v-expansion-panel
            v-for="(item,i) in jsondata"
            :key="i"
        >
            <v-expansion-panel-header>{{item.service}}</v-expansion-panel-header>
            <v-expansion-panel-content>
            <v-card>
                <v-card-text>{{item.pass}}</v-card-text>
            </v-card>
            </v-expansion-panel-content>
        </v-expansion-panel>
        </v-expansion-panels>
    </v-row> -->


</template>
<script>
export default {
    data:() =>({
    }),
    props:{
        jsondata:{
            type:Array,
            default:() => []
        }
    },
};
</script>
<style>

</style>