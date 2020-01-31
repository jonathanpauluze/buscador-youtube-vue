<template>
  <v-app>
    <SearchForm />
    <VideoList />
  </v-app>
</template>

<script>
import SearchForm from '@/components/SearchForm';
import VideoList from '@/components/VideoList';

export default {
  name: 'App',
  components: {
    SearchForm,
    VideoList
  }
};
</script>