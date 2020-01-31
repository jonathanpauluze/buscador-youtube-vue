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
                  @click="details(result.id.videoId)"
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
      searchBaseUrl: 'https://www.googleapis.com/youtube/v3/search?part=id,snippet&type=video&q=',
      videoBaseUrl: 'https://www.googleapis.com/youtube/v3/videos?part=snippet,statistics&id=',
      apiKey: 'AIzaSyBfNv0UixraspY_Cc66VSPsm-_c-mn7H28',
      show: false,
      term: '',
      maxPerPage: 6,
      nextPage: '',
      results: [],
    }
  },

  methods: {
    searchUrl(nextPageToken) {
      if(nextPageToken !== undefined) {
        return `${this.searchBaseUrl}${this.term}&key=${this.apiKey}&maxResults=${this.maxPerPage}&pageToken=${nextPageToken}`;
      }

      return `${this.searchBaseUrl}${this.term}&key=${this.apiKey}&maxResults=${this.maxPerPage}`;
    },

    videoUrl(id) {
      return `${this.videoBaseUrl}${id}&key=${this.apiKey}`;
    },

    infiniteScroll() {
      window.onscroll = () => {
        let windowBottom = document.documentElement.scrollTop + window.innerHeight === document.documentElement.offsetHeight;

        if (windowBottom) {
          axios.get(this.searchUrl(this.nextPage))
            .then(res => {
              this.nextPage = res.data.nextPageToken;

              res.data.items.map((item) => {
                this.results.push(item);
              });
            });
        }
      };
    },

    details(id) {
      axios.get(this.videoUrl(id))
        .then(res => {
          EventBus.$emit('receive-details', res);
        });
    }
    
  },

  created() {
    EventBus.$on('receive-term', term => {
      this.term = term;
      this.show = true;
      axios.get(this.searchUrl())
        .then(res => {
          this.nextPage = res.data.nextPageToken;
          
          this.results = res.data.items;
        });
    })
  },

  mounted() {
    this.infiniteScroll();
  }
}
</script>

<style lang="sass" scoped>
</style>