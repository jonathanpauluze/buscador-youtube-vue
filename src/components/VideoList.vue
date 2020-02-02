<template>
  <v-layout justify-center align-center>
    <v-flex xs12 sm10 md8>
      <v-container class="top-space">
        <v-row>
          <v-col
            v-for="(video, idx) in videos"
            :key="idx"
            cols="12"
            sm="6"
            lg="4"
          >
            <v-card
              class="mb-5"
            >
              <v-img
                class="video-list__thumbnail white--text align-end"
                height="200px"
                :src="video.snippet.thumbnails.high.url"
              />

              <v-card-title class="video-list__title">{{video.snippet.title}}</v-card-title>

              <v-card-text class="video-list__description text--primary">
                <div>{{ video.snippet.description }}</div>
              </v-card-text>

              <v-card-actions>
                <v-btn
                  class="video-list__details-btn"
                  color="#3e206d"
                  text
                  @click="getVideoStatistics(video.id.videoId)"
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
import { EventBus } from '@/event-bus.js';

export default {
  name: 'VideoList',

  props: {
    videos: Array
  },

  data() {
    return {
      
    }
  },

  methods: {
    infiniteScroll() {
      window.onscroll = () => {
        let windowBottom = document.documentElement.scrollTop + window.innerHeight === document.documentElement.offsetHeight;

        if(windowBottom) {
          this.$emit('reach-bottom')
        }
      };
    },

    getVideoStatistics(id) {
      this.$emit('get-video-statistics', id)
      EventBus.$emit('details-clicked')
    }
    
  },

  mounted() {
    this.infiniteScroll();
  }
}
</script>

<style lang="sass" scoped>
.top-space 
  margin-top: 140px

// ===== Queries =====
@media only screen and (min-width: 600px)
  .top-space 
    margin-top: 80px
</style>