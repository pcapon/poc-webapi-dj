<template>
  <v-container fluid>
    <v-layout row>
      <v-flex xs6>
        <v-text-field
          id="testing"
          name="input-1"
          label="Label Text"
          v-model="url"
          @keyup.enter="getAudio()"
        ></v-text-field>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>

export default {
  name: 'urlInput',
  data: () => ({
    url: ''
  }),
  methods: {
    getAudio () {
      console.log('pute');
    }
  }
};
</script>
