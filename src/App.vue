<template>
  <div id="app">
    <v-app>
        <v-toolbar-title class="headline text-uppercase">
          <span class="font-weight-bold">Monster</span>
          <span class="font-weight-light">Project</span>
          <br>
          <span class="font-weight-light">Darrin Harvard | DGM3790</span>
        </v-toolbar-title>

  <v-content>
    <MonsterProject></MonsterProject>
  </v-content>
</v-app>
</div>
</template>

<script>
import MonsterProject from './components/MonsterProject';
import {VApp, VToolbarTitle, VContent, } from 'vuetify/lib'

export default {
  name: 'App',
  components: {
    MonsterProject,
    VApp,  
    VToolbarTitle, 
    VContent,
  },
  data: () => ({
   //
  }),
};
</script>