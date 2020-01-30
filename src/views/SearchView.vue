<template>
  <v-app>
    <SearchForm />
    <VideoList v-if="hasResults" />
  </v-app>
</template>

<script>
import SearchForm from '@/components/SearchForm';
import VideoList from '@/components/VideoList';
import { EventBus } from '@/event-bus.js';
export default {
  name: 'App',
  components: {
    SearchForm,
    VideoList
  },
  data() {
    return {
      hasResults: false
    }
  },
  mounted() {
    EventBus.$on('received-results', () => {
      this.hasResults = true;
    })
  }
};
</script>