<template>
  <vue-slider v-bind="sliderVolume" @callback="callback"></vue-slider>
</template>

<script>
import vueSlider from 'vue-slider-component';

export default {
  components: {
    vueSlider
  },
  props: [
    'step',
    'min',
    'max',
    'vmodel',
    'callback'
  ],
  data: () => ({
    sliderVolume: {}
  }),
  methods: {},
  mounted () {
    this.sliderVolume = {
      value: 1,
      width: 4,
      height: 300,
      dotSize: 12,
      eventType: 'auto',
      min: this.min,
      max: this.max,
      interval: this.step,
      disabled: false,
      show: true,
      piecewise: false,
      tooltip: false,
      reverse: true,
      style: {
        'display': 'inline-block',
        'marginLeft': '30px'
      },
      direction: 'vertical',
      processStyle: {
        'backgroundColor': '#00be7e'
      },
      sliderStyle: {
        'backgroundColor': '#1565C0'
      }
    };
  }
};
</script>

<style scoped>

</style>
