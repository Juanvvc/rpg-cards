<template>
  <v-card elevation="24" :max-width="deckInfo.width" class="mx-auto">
    <v-carousel
      :continuous="deckInfo.cycle"
      show-arrows="hover"
      hide-delimiters
      hide-delimiter-background
      delimiter-icon="mdi-square"
      :height="deckInfo.height"
      progress="primary"
      v-model="currentCard"
      @update:model-value="emit('currentCard', currentCard)"
    >
      <v-carousel-item v-for="(card, i) in availableCards" :key="i">
        <Card
          :style="getStyle(i)"
          :title="availableCards[i].title"
          :text="availableCards[i].text"
          :footer="availableCards[i].footer"
        />
      </v-carousel-item>
    </v-carousel>

    <div class="d-flex justify-space-around align-center py-4">
      <v-tooltip text="Previous card" v-if="currentCard > 0 || deckInfo.cycle">
        <template v-slot:activator="{ props }">
          <v-btn variant="text" icon="mdi-skip-previous" v-bind="props" @click="goToPosition(currentCard - 1)" />
        </template>
      </v-tooltip>

      <v-tooltip v-if="currentCard > 0" text="Go to cover">
        <template v-slot:activator="{ props }">
          <v-btn variant="text" icon="mdi-home-floor-1" v-bind="props" @click="goToCover()" />
        </template>
      </v-tooltip>

      {{ currentCard === 0 ? maxCards + ' cards in deck' : currentCard + '/' + maxCards }}

      <v-tooltip text="Shuffle cards">
        <template v-slot:activator="{ props }">
          <v-btn variant="text" icon="mdi-shuffle" v-bind="props" color="red" @click="shuffle()" />
        </template>
      </v-tooltip>

      <v-tooltip text="Next card" v-if="currentCard < maxCards || deckInfo.cycle">
        <template v-slot:activator="{ props }">
          <v-btn variant="text" icon="mdi-skip-next" v-bind="props" @click="goToPosition(currentCard + 1)" />
        </template>
      </v-tooltip>
    </div>
  </v-card>
</template>

<script setup>
import { ref, computed } from 'vue'
import Card from '@/components/Card.vue'

const props = defineProps({
  deckInfo: { required: true }
})

const emit = defineEmits(['currentCard', 'shuffle'])

const currentCard = ref(0)

const maxCards = computed(() => props.deckInfo.cards.length)

const availableCards = computed(() => {
  if (props.deckInfo.cover)
    return [props.deckInfo.cover, ...props.deckInfo.cards]
  return [...props.deckInfo.cards]
})

function goToCover() {
  currentCard.value = 0
  emit('currentCard', 0)
}

function goToLast() {
  currentCard.value = maxCards.value
}

function goToPosition(newCard) {
  if (newCard > maxCards.value && props.deckInfo.cycle) {
    goToCover()
    return
  }
  if (newCard < 0 && props.deckInfo.cycle) {
    goToLast()
    return
  }
  currentCard.value = newCard
  emit('currentCard', currentCard.value)
}

function shuffle() {
  goToCover()
  emit('shuffle')
}

function getStyle(i) {
  const style = availableCards.value[i].style
  return style ?? props.deckInfo.style
}
</script>
