<template>
  <v-app>
    <v-main>
      <Deck :deck-info="deckInfo" @currentCard="onCurrentCard" @shuffle="onShuffle" />
      <v-snackbar v-model="showShuffled" color="success">The deck was shuffled</v-snackbar>
    </v-main>
  </v-app>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import Deck from '@/components/Deck.vue'

const deckInfo = ref({
  color: 'red',
  cover: { title: 'Loading' },
  cards: [],
})
const showShuffled = ref(false)

onMounted(() => {
  fetch(import.meta.env.VITE_DECK_DATA)
    .then(r => r.json())
    .then(data => { deckInfo.value = data })
})

function onShuffle() {
  const arr = deckInfo.value.cards
  for (let i = arr.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1))
    ;[arr[i], arr[j]] = [arr[j], arr[i]]
  }
  showShuffled.value = true
}

function onCurrentCard(i) {
  deckInfo.value.currentCard = i
}
</script>
