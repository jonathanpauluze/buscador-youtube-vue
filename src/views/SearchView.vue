<template>
  <v-app>
    <SearchForm />
    <VideoList />
    <DetailsModal />
  </v-app>
</template>

<script>
import SearchForm from '@/components/SearchForm';
import VideoList from '@/components/VideoList';
import DetailsModal from '@/components/DetailsModal';

export default {
  name: 'App',
  components: {
    SearchForm,
    VideoList,
    DetailsModal
  }
};
</script>