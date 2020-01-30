<template>
  <v-layout justify-center align-center>
    <input v-model="searchTerm" type="text"/>
    <button @click="search">Pesquisar</button>
  </v-layout>
</template>

<script>
import axios from 'axios';
import { EventBus } from '@/event-bus.js';
export default {
  name: 'SearchForm',
  data() {
    return {
      apiKey: 'AIzaSyBfNv0UixraspY_Cc66VSPsm-_c-mn7H28',
      searchTerm: '',
      searchResults: '',
    }
  },
  methods: {
    search() {
      axios
        .get(`https://www.googleapis.com/youtube/v3/search?part=id,snippet&q=${this.searchTerm}&key=${this.apiKey}`)
        .then(res => (this.searchResults = res))
        .then(() => {
          EventBus.$emit('received-results', this.searchResults);
        });
    }
  }
}
</script>

<style lang="sass" scoped>
</style>