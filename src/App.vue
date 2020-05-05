<template>
  <div id="app">
    <v-app dark>
      <v-container grid-list-md text-xs-center>
        <v-layout row>
          <v-flex xs4>
            <deck :context="context" :nbrdeck="0" :dynamicId="'audio_player' + 0"/>
          </v-flex>
          <v-flex xs4>
            <mixer></mixer>
          </v-flex>
          <v-flex xs4>
            <deck :context="context" :nbrdeck="1" :dynamicId="'audio_player' + 1"/>
          </v-flex>
        </v-layout>
      </v-container>

    </v-app>
  </div>
</template>

<script>
import urlInput from './components/urlInput';
import oscillator from './components/oscillator';
import deck from './components/deck.vue';
import Mixer from './components/mixer.vue';

export default {
  name: 'App',
  components: {
    Mixer,
    urlInput,
    oscillator,
    deck
  },
  data: () => ({
    context: ''
  }),
  created () {
    this.context = new (window.AudioContext || window.webkitAudioContext)();
    console.log(this.context);
  }
};
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
