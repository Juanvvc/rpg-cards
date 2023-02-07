<template>
  <v-app>
    <v-main>
      <Deck :deck-info="deckInfo" @currentCard="currentCard" @shuffle="shuffle"/>
      <v-snackbar v-model="showShuffled" color="success">The deck was shuffled</v-snackbar>
    </v-main>
  </v-app>
</template>

<script setup>
  import Deck from '@/components/Deck.vue'
</script>

<script>
  export default {
    data () {
      return {
        deckInfo: {
          color: 'red',
          cover: {title: 'Loading'},
          cards: [],
        },
        showShuffled: false
      }
    },

    mounted () {
      fetch(import.meta.env.VITE_DECK_DATA)
      .then((response) => response.json())
      .then((data) => {
        this.deckInfo = data
      });
    },

    methods: {
      shuffle () {
        let arr = this.deckInfo.cards
        for (let i = arr.length - 1; i > 0; i--) {
          const j = Math.floor(Math.random() * (i + 1));
          [arr[i], arr[j]] = [arr[j], arr[i]];
        }
        this.showShuffled = true
      },

      currentCard (i) {
        this.deckInfo.currentCard = i;
      }
    }
  }
</script>
