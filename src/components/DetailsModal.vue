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
                <v-icon size="18">mdi-eye</v-icon>
                <span class="number">{{ this.formattedViews }}</span>
              </div>
              <div class="likes">
                <v-icon size="18">mdi-thumb-up</v-icon>
                <span class="number">{{ this.formattedLikes }}</span>
              </div>
              <div class="dislikes">
                <v-icon size="18">mdi-thumb-down</v-icon>
                <span class="number">{{ this.formattedDislikes }}</span>
              </div>
            </div>

            <v-card-text
              v-html="formattedDescription"
              class="description"
            ></v-card-text>

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
      embedUrl: '',
      formattedViews: '',
      formattedLikes: '',
      formattedDislikes: ''
    }
  },

  created() {
    EventBus.$on('receive-details', res => {
      this.show = true;
      this.videoInfo = res.data;
      this.embedUrl = 'https://www.youtube.com/embed/' + res.data.items[0].id;
      this.formattedDescription = res.data.items[0].snippet.description.replace(/\n/gm, '<br>');
      this.formattedViews = new Intl.NumberFormat('pt').format(res.data.items[0].statistics.viewCount);
      this.formattedLikes = new Intl.NumberFormat('pt').format(res.data.items[0].statistics.likeCount);
      this.formattedDislikes = new Intl.NumberFormat('pt').format(res.data.items[0].statistics.dislikeCount);

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
  height: 200px
  padding: 16px 16px 0

.statistic-container
  display: flex
  justify-content: flex-end
  margin: 0 16px
  font-size: 14px
  font-weight: 500

  .views
    display: inline-block
    margin-right: auto

  .likes, .dislikes
    display: inline-block
    margin-left: 20px

  .number
    margin-left: 3px

.description 
  margin-top: 20px

@media only screen and (min-width: 500px)
  .embed-video
    height: 315px

@media only screen and (min-width: 800px)
  .embed-video
    height: 415px
</style>