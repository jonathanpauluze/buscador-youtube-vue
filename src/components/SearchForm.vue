<template>
  <v-layout justify-center align-center>
    <v-flex xs12 sm10 md8>
      <v-container>
        <v-row>
          <v-col cols="12">
            <v-form ref="form" lazy-validation>
              <v-text-field
                v-model="search.term"
                :rules="search.rules"
                @keypress.13.prevent="makeSearch"
                type="text"
                placeholder="Pesquisar"
                color="#3e206d"
              />
              <v-btn
                color="#3e206d"
                class="mt-3"
                dark
                block
                depressed
                large
                @click.prevent="makeSearch"
              >
                Buscar
              </v-btn>
            </v-form>
          </v-col>
        </v-row>
      </v-container>
    </v-flex>
  </v-layout>
</template>

<script>
// import axios from 'axios';
import { EventBus } from '@/event-bus.js';

export default {
  name: 'SearchForm',
  data() {
    return {
      search: {
        term: '',
        rules: [v => !!v || 'Digite algo para pesquisar']
      }
    }
  },
  methods: {
    makeSearch() {
      if(this.$refs.form.validate()) {
        EventBus.$emit('receive-term', this.search.term);
      }
    }
  }
}
</script>

<style lang="sass" scoped>
</style>