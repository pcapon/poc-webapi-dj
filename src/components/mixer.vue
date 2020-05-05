<template>
  <v-card>
    <circle-slider v-model="val1"></circle-slider>
    <div>{{ val1 }}</div>
    <circle-slider v-model="val1"></circle-slider>
    <div>{{ val1 }}</div>
    <circle-slider v-model="val1"></circle-slider>
    <div>{{ val1 }}</div>
  </v-card>
</template>

<script>
export default {
  name: 'mixer',
  data: () => ({
    val1: 50
  })
};
</script>

<style scoped>

</style>
