<template>
  <v-app>
    <v-main>
      <DeckList v-if="!selectedDeckPath" :paths="deckPaths" @select="openDeck" />

      <template v-else>
        <div class="pa-4">
          <v-btn icon="mdi-arrow-left" variant="text" class="mb-4" @click="closeDeck" />
        </div>
        <Deck :deck-info="deckInfo" @currentCard="onCurrentCard" @shuffle="onShuffle" />
      </template>

      <v-snackbar v-model="showShuffled" color="success">The deck was shuffled</v-snackbar>
    </v-main>
  </v-app>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import Deck from '@/components/Deck.vue'
import DeckList from '@/components/DeckList.vue'

const deckPaths = ref([])
const selectedDeckPath = ref(null)
const deckInfo = ref({ cover: { title: 'Loading' }, cards: [] })
const showShuffled = ref(false)

onMounted(() => {
  fetch(import.meta.env.VITE_DECK_INDEX)
    .then(r => r.json())
    .then(data => { deckPaths.value = data })
})

function openDeck(path) {
  fetch(path)
    .then(r => r.json())
    .then(data => {
      deckInfo.value = data
      selectedDeckPath.value = path
    })
}

function closeDeck() {
  selectedDeckPath.value = null
  deckInfo.value = { cover: { title: 'Loading' }, cards: [] }
}

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
