<template>
  <v-container class="py-8">
    <div class="text-h4 text-center mb-8">Choose a deck</div>

    <v-row v-if="loading" justify="center">
      <v-progress-circular indeterminate color="primary" size="64" />
    </v-row>

    <v-row v-else justify="center">
      <v-col v-for="(deck, i) in loadedDecks" :key="i" cols="6" sm="4" md="3">
        <v-card
          height="270"
          :style="coverStyle(deck)"
          class="d-flex align-center justify-center cursor-pointer"
          elevation="6"
          @click="$emit('select', paths[i])"
        >
          <span
            v-if="deck.cover?.title"
            class="text-h5 text-center pa-2"
            style="color: white; text-shadow: 1px 1px 4px black;"
          >
            {{ deck.cover.title }}
          </span>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
  paths: { required: true },
})
defineEmits(['select'])

const loadedDecks = ref([])
const loading = ref(true)

watch(() => props.paths, async (paths) => {
  if (paths.length === 0) return
  loading.value = true
  loadedDecks.value = await Promise.all(
    paths.map(p => fetch(p).then(r => r.json()))
  )
  loading.value = false
}, { immediate: true })

function coverStyle(deck) {
  return deck.cover?.style ?? deck.style ?? ''
}
</script>
