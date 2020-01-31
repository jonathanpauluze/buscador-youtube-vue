<template>
  <v-layout justify-center align-center>
    <v-flex xs12 sm10 md8>
      <v-container>
        <v-row v-if="show">
          <v-col
            v-for="(result, idx) in results"
            :key="idx"
            cols="12"
            sm="6"
            lg="4"
          >
            <v-card
              class="mb-5"
            >
              <v-img
                class="white--text align-end"
                height="200px"
                :src="result.snippet.thumbnails.high.url"
              />

              <v-card-title>{{result.snippet.title}}</v-card-title>

              <v-card-text class="text--primary">
                <div>{{ result.snippet.description }}</div>
              </v-card-text>

              <v-card-actions>
                <v-btn
                  color="#3e206d"
                  text
                >
                  Detalhes
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-col>
        </v-row>
        
      </v-container>
    </v-flex>
  </v-layout>
</template>

<script>
import axios from 'axios';
import { EventBus } from '@/event-bus.js';

export default {
  name: 'VideoList',

  data() {
    return {
      show: false,
      apiKey: 'AIzaSyBfNv0UixraspY_Cc66VSPsm-_c-mn7H28',
      term: '',
      maxPerPage: 6,
      nextPage: '',
      results: [],
    }
  },

  methods: {
    apiUrl(term, key, maxResults, nextPageToken) {
      if(nextPageToken !== undefined) {
        return `https://www.googleapis.com/youtube/v3/search?part=id,snippet&q=${term}&key=${key}&maxResults=${maxResults}&type=video&pageToken=${nextPageToken}`
      }

      return `https://www.googleapis.com/youtube/v3/search?part=id,snippet&q=${term}&key=${key}&maxResults=${maxResults}&type=video`
    }
  },

  created() {
    EventBus.$on('receive-term', term => {
      this.term = term;
      this.show = true;
      axios.get(this.apiUrl(this.term, this.apiKey, this.maxPerPage))
        .then(res => {
          this.nextPage = res.data.nextPageToken;
          
          this.results = res.data.items;
        });
    })
  }
}
</script>

<style lang="sass" scoped>
</style>