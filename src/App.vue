<template>
  <v-app>
    <Navbar />
    <v-content class="grey lighten-3 mx-3 mb-1">
      <router-view></router-view>
    </v-content>
  </v-app>
</template>

<script>
import Navbar from "./components/Navbar";
import myaudioplayerX from "./components/myaudioplayerX";

export default {
  name: "App",
  components: {
    Navbar,
    myaudioplayerX
  },
  data: () => ({
    //
  })
};
</script>
