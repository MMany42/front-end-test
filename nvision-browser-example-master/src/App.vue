<template>
  <div id="app">
    <v-app>
      <!-- <v-navigation-drawer app>
        <v-list-item>
          <v-list-item-content>
            <v-list-item-title class="title">NipaCloud Nvision</v-list-item-title>
            <v-list-item-subtitle>JavaScript SDK Example </v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>

        <v-divider></v-divider>

        <v-list dense nav>
          <v-list-item to="/restful">
            <v-list-item-icon>
              <v-icon>mdi-http</v-icon>
            </v-list-item-icon>
            <v-list-item-content>
              <v-list-item-title>Http REST Example</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
          <v-list-item to="/websocket">
            <v-list-item-icon>
              <v-icon>mdi-compare_arrows</v-icon>
            </v-list-item-icon>
            <v-list-item-content >
              <v-list-item-title>Http WebSocket Example</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-list>
      </v-navigation-drawer> -->

      <!-- Sizes your content based upon application components -->
      <v-content>
        <!-- Provides the application the proper gutter -->

        <v-row justify="center">
          <v-card width="1150" flat class="mt-10">
            <router-link to="/" style="text-decoration: none">
              <v-btn class="" style="font-size: 20px" depressed color="primary"
                ><v-icon left> mdi-select </v-icon>Http REST Example
              </v-btn>
            </router-link>

            <router-link to="/websocket" style="text-decoration: none" class="mx-1">
              <v-btn class="" style="font-size: 20px" depressed color="primary"
                ><v-icon left> mdi-camera </v-icon>Http WebSocket Example
              </v-btn>
            </router-link>
          </v-card>
        </v-row>

        <v-row justify="center">
          <router-view></router-view>
        </v-row>
      </v-content>
    </v-app>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import HelloWorld from "./components/HelloWorld.vue";

@Component({
  components: {
    HelloWorld,
  },
})
export default class App extends Vue {}
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 20px;
}
</style>
