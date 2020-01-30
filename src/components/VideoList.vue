<template>
  <v-layout
    v-if="searchResults !== ''"
    justify-center 
    align-center
  >
    <v-card
      v-for="(search, idx) in searchResults.data.items"
      :key="idx"
      class="mx-auto"
      max-width="400"
    >
      <v-img
        class="white--text align-end"
        height="200px"
        :src="search.snippet.thumbnails.medium.url"
      >
        <v-card-title>{{search.snippet.title}}</v-card-title>
      </v-img>

      <v-card-text class="text--primary">
        <div>{{ search.snippet.description }}</div>
      </v-card-text>

      <v-card-actions>
        <v-btn
          color="orange"
          text
        >
          Detalhes
        </v-btn>
      </v-card-actions>
    </v-card>
  </v-layout>
</template>

<script>
import { EventBus } from '@/event-bus.js';
export default {
  name: 'VideoList',
  data() {
    return {
      searchResults: ''
    }
  },
  created() {
    EventBus.$on('received-results', searchResults => {
      this.searchResults = searchResults;
    })
  }
}
</script>

<style lang="sass" scoped>
</style>