<template>
  <v-container>
    <v-card >
      DECK n° {{ nbrdeck }}
      <div v-if="this.rawfile">
        {{ this.rawfile.name }}
      </div>
      <v-layout row>
        <v-flex>
        <horizontalSlider :step="0.001" thumb-label :min="0.92" :max="1.08" :callback="pitchtest" vmodel="pitchValue"></horizontalSlider>
        </v-flex>
        <v-flex>
          <div :id="'waveform' + nbrdeck"></div>
          <input @change="getfile" id="audio_file" type="file" accept="audio/*" />
          <div v-if="soundSource">
            <v-btn color="primary" @click="wavesurfer.playPause()">
              <v-icon v-if="wavesurfer.isPlaying()" dark>pause</v-icon>
              <v-icon v-else>play_arrow</v-icon>
            </v-btn>
            <v-btn color="error" @click="wavesurfer.stop()">
              <v-icon dark>stop</v-icon>
            </v-btn>
          </div>
        </v-flex>
       <v-flex>
          <v-card>
            <circle-slider v-model="truc"/>
          </v-card>
        </v-flex>
      </v-layout>
    </v-card>
  </v-container>
</template>

<script>
import UploadButton from 'vuetify-upload-button';
import axios from 'axios';
import WaveSurfer from 'wavesurfer.js';
import horizontalSlider from './horizontalSlider.vue';

export default {
  components: {
    'upload-btn': UploadButton,
    'horizontalSlider': horizontalSlider
  },
  watch: {
    truc: function (val) {
      this.filter.gain.value = val;
    }
  },
  props: [
    'context',
    'nbrdeck',
    'dynamicId'
  ],
  data: () => ({
    truc: 50,
    filter: null,
    urlsong: null,
    bufferSong: null,
    rawfile: null,
    pitchValue: 1,
    soundSource: null,
    wavesurfer: null,
    filters: null,
    filtervalue: 50
  }),
  methods: {
    getfile (event) {
      this.urlsong = URL.createObjectURL(event.target.files[0]);
      this.wavesurfer.load(this.urlsong);
      var _this = this;
      axios.get(this.urlsong, {
        responseType: 'arraybuffer'
      })
        .then(function (response) {
          console.log(response.data);
          _this.context.decodeAudioData(response.data, buffer => {
            _this.bufferSong = buffer;
            _this.addToPlayer();
          });
        });
      this.rawfile = event.target.files[0];
    },
    addToPlayer () {
      this.soundSource = this.context.createBufferSource();
      this.soundSource.buffer = this.bufferSong;
      this.soundSource.start();
    },
    pitchtest (value) {
      this.wavesurfer.setPlaybackRate(value);
      console.log(this.wavesurfer.getPlaybackRate());
    }
  },
  mounted () {
    this.wavesurfer = WaveSurfer.create({
      container: '#waveform' + this.nbrdeck,
      audioContext: this.context,
      waveColor: '#334860',
      progressColor: '#00be7e',
      hideScrollbar: true,
      forceDecode: true,
      responsive: true
    });
    var gainDb = -40.0;
    var bandSplit = [360, 3600];
    var _this = this;
    this.wavesurfer.on('ready', function () {
      _this.wavesurfer.zoom(150);
      var filter = _this.wavesurfer.backend.ac.createBiquadFilter();
      filter.type = 'lowshelf';

      // EQ Properties
      //
      var hBand = _this.wavesurfer.backend.ac.createBiquadFilter();
      hBand.type = 'lowshelf';
      hBand.frequency.value = bandSplit[0];
      hBand.gain.value = gainDb;

      var hInvert = _this.wavesurfer.backend.ac.createGain();
      hInvert.gain.value = -1.0;

      var mBand = _this.wavesurfer.backend.ac.createGain();

      _this.filter = _this.wavesurfer.backend.ac.createBiquadFilter();
      _this.filter.type = 'highshelf';
      _this.filter.frequency.value = bandSplit[1];
      _this.filter.gain.value = gainDb;

      var lInvert = _this.wavesurfer.backend.ac.createGain();
      lInvert.gain.value = -1.0;

      _this.wavesurfer.backend.setFilter(_this.filter);
      _this.wavesurfer.backend.setFilter(mBand);
      _this.wavesurfer.backend.setFilter(hBand);
    });
  }
};

</script>
