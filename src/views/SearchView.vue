<template>
  <v-app>
    <SearchForm
      @submitted="searchVideos"
    />

    <VideoList
      :videos="videos"
      @reach-bottom="loadMoreVideos"
      @get-video-statistics="getVideoStatistics"
    />

    <DetailsModal
      :videoStatistics="videoStatistics"
    />
  </v-app>
</template>

<script>
import axios from "axios";
import SearchForm from '@/components/SearchForm';
import VideoList from '@/components/VideoList';
import DetailsModal from '@/components/DetailsModal';

export default {
  name: 'App',

  components: {
    SearchForm,
    VideoList,
    DetailsModal
  },

  data() {
    return {
      searchBaseUrl: 'https://www.googleapis.com/youtube/v3/search?part=id,snippet&type=video&q=',
      videoBaseUrl: 'https://www.googleapis.com/youtube/v3/videos?part=snippet,statistics&id=',
      apiKey: 'AIzaSyBfNv0UixraspY_Cc66VSPsm-_c-mn7H28',
      show: false,
      term: '',
      maxPerPage: 6,
      nextPage: '',
      videos: [],
      videoStatistics: {}
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

    searchVideos(term) {
      this.term = term;
      this.show = true;
      axios.get(this.searchUrl())
        .then(res => {
          this.nextPage = res.data.nextPageToken;
          
          this.videos = res.data.items;
        });
    },

    loadMoreVideos() {
      axios.get(this.searchUrl(this.nextPage))
        .then(res => {
          this.nextPage = res.data.nextPageToken;

          res.data.items.map((video) => {
            this.videos.push(video);
          });
        });
    },

    getVideoStatistics(id) {
      axios.get(this.videoUrl(id))
        .then(res => {
          let videoInfo = res.data.items[0];

          let viewCount = videoInfo.statistics.viewCount ? videoInfo.statistics.viewCount : 0;
          let likeCount = videoInfo.statistics.likeCount ? videoInfo.statistics.likeCount : 0;
          let dislikeCount = videoInfo.statistics.dislikeCount ? videoInfo.statistics.dislikeCount : 0;

          this.videoStatistics = {
            embedUrl: 'https://www.youtube.com/embed/' + videoInfo.id,
            title: videoInfo.snippet.title,
            views: new Intl.NumberFormat('pt').format(viewCount),
            likes: new Intl.NumberFormat('pt').format(likeCount),
            dislikes: new Intl.NumberFormat('pt').format(dislikeCount),
            description: videoInfo.snippet.description.replace(/\n/gm, '<br>')
          }
        });
    }
  }
};
</script>