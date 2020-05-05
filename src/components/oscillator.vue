<template>
  <v-container>
    <v-text-field
      id="testing"
      name="input-1"
      label="Path To Song"
      v-model="Path"
      @keyup.enter="getAudio"
    ></v-text-field>
    <v-btn color="primary" @click="oscillator.connect(gain)">Start</v-btn>
    <v-btn color="error" @click="oscillator.disconnect(gain)">Stop</v-btn>
    <v-slider thumb-label :min="16" :max="7902" @input="freqOscil" v-model="frequencyValue"></v-slider>
    <vue-slider @callback="volumeLvl($event)" v-bind="sliderVolume"></vue-slider>
  </v-container>
</template>

<script>
import vueSlider from 'vue-slider-component';

export default {
  components: {
    vueSlider
  },
  props: [
    'context'
  ],
  data: () => ({
    Path: '',
    frequencyValue: 444,
    oscillator: null,
    gain: null,
    sliderVolume: {
      value: 2,
      width: 4,
      height: 300,
      dotSize: 12,
      eventType: 'auto',
      min: 0,
      max: 3.4,
      interval: 0.01,
      disabled: false,
      show: true,
      piecewise: false,
      tooltip: false,
      style: {
        'display': 'inline-block',
        'marginLeft': '30px'
      },
      direction: 'vertical',
      speed: 0.5,
      processStyle: {
        'backgroundColor': '#1565C0'
      },
      sliderStyle: {
        'backgroundColor': '#1565C0'
      }
    }
  }),
  methods: {
    freqOscil () {
      this.oscillator.frequency.setValueAtTime(this.frequencyValue, 0);
    },
    volumeLvl (vlm) {
      this.gain.gain.setValueAtTime(vlm, 0);
    },
    getAudio () {

    }
  },
  mounted () {
    console.log(this.context);
    this.oscillator = this.context.createOscillator();
    this.gain = this.context.createGain();

    this.oscillator.connect(this.gain);
    this.gain.connect(this.context.destination);
    this.gain.gain.setValueAtTime(this.sliderVolume.value, 0);

    this.oscillator.type = 'sine';
    this.oscillator.frequency.setValueAtTime(this.frequencyValue, 0);
    this.oscillator.start();
    this.oscillator.disconnect(this.gain);
  }
};
</script>
