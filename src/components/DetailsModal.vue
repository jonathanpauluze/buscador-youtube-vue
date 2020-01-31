<template>
  <v-dialog
    v-model="dialog"
    fullscreen
    v-if="show"
  >
    <v-card>
      <v-layout justify-center align-center>
        <v-flex xs12 sm10 md8>
          <v-container cols="12">
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn
                text
                @click="dialog = false"
              >
                <v-icon>mdi-close</v-icon>
              </v-btn>
            </v-card-actions>

            <v-divider></v-divider>

            <iframe
              class="embed-video"
              :src="embedUrl"
              frameborder="0"
              allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture"
              allowfullscreen
            ></iframe>

            <v-card-title
              primary-title
            >
              {{ this.videoInfo.items[0].snippet.title }}
            </v-card-title>

            <div class="statistic-container">
              <div class="views">
                <v-icon>mdi-eye</v-icon>
                <span>{{ this.videoInfo.items[0].statistics.viewCount }}</span>
              </div>
              <div class="likes">
                <v-icon>mdi-thumb-up</v-icon>
                <span>{{ this.videoInfo.items[0].statistics.likeCount }}</span>
              </div>
              <div class="dislikes">
                <v-icon>mdi-thumb-down</v-icon>
                <span>{{ this.videoInfo.items[0].statistics.dislikeCount }}</span>
              </div>
            </div>

            <v-card-text>
              {{ this.videoInfo.items[0].snippet.description }}
            </v-card-text>

          </v-container>
        </v-flex>
      </v-layout>
    </v-card>
  </v-dialog>
</template>

<script>
import { EventBus } from '@/event-bus.js';

export default {
  data() {
    return {
      show: false,
      dialog: false,
      videoInfo: Object,
      embedUrl: ''
    }
  },

  created() {
    EventBus.$on('receive-details', res => {
      this.show = true;
      this.videoInfo = res.data;
      this.embedUrl = 'https://www.youtube.com/embed/' + res.data.items[0].id;

      setTimeout(() => {
        this.dialog = true;
      }, 50)
    })
  }
}
</script>

<style lang="sass" scoped>
.embed-video
  display: block
  margin: 0 auto
  width: 100%
  height: 315px
  padding: 16px 16px 0
</style>