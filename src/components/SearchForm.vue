<template>
  <v-layout justify-center align-center>
    <v-flex xs12 sm10 md8>
      <v-container class="relative-container">
        <v-form ref="form" :class="(search.isSearched ? 'animate' : '')" lazy-validation>
          <v-row>
            <v-col cols="12" sm="9" class="no-mobile-y-padd">
              <v-text-field
                v-model="search.term"
                :rules="search.rules"
                @keypress.13.prevent="makeSearch"
                type="text"
                placeholder="Pesquisar"
                color="#3e206d"
              />
            </v-col>
            <v-col cols="12" sm="3" class="no-mobile-y-padd">
              <v-btn
                color="#3e206d"
                dark
                block
                depressed
                large
                @click.prevent="makeSearch"
              >
                Buscar
              </v-btn>
            </v-col>
          </v-row>
        </v-form>
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
        rules: [v => !!v || 'Digite algo para pesquisar'],
        isSearched: false
      }
    }
  },
  methods: {
    makeSearch() {
      if(this.$refs.form.validate()) {
        this.search.isSearched = true;
        EventBus.$emit('receive-term', this.search.term);
      }
    }
  }
}
</script>

<style lang="sass" scoped>
@keyframes smooth-up
  from
    top: 50%
    margin-top: 0
    transform: translate3d(0, -50%, 0)
  to
    top: 0
    margin-top: 20px
    transform: translate3d(0, 0, 0)

form
    position: absolute
    padding: 0 16px
    top: 50%
    left: 10%
    right: 10%
    transform: translate3d(0, -50%, 0)

.no-mobile-y-padd
  padding-top: 0
  padding-bottom: 0

.animate
  animation: smooth-up .3s forwards
  

// ===== Queries =====
@media only screen and (min-width: 600px)
  .no-mobile-y-padd
    padding-top: 12px
    padding-bottom: 12px


@media only screen and (min-width: 1000px)
  form
    left: 20%
    right: 20%
</style>