<template>
  <v-card
    elevation="24"
    :max-width="deckInfo.width"
    class="mx-auto"
  >
  <!-- Current card -->
  <v-carousel
      :continuous="deckInfo.cycle"
      show-arrows="hover"
      hide-delimiters
      hide-delimiter-background
      delimiter-icon="mdi-square"
      :height="deckInfo.height"
      progress="primary"
      v-model="currentCard"
      @update:model-value="$emit('currentCard', currentCard)"
    >
      <v-carousel-item
        v-for="(card, i) in availableCards"
        :key="i"
      >
        <Card :style="getStyle(i)" :title="availableCards[i].title" :text="availableCards[i].text" :footer="availableCards[i].footer" />
      </v-carousel-item>
    </v-carousel>
    <!-- Navigation -->
    <div class="d-flex justify-space-around align-center py-4">
      <v-tooltip text="Previous card" v-if="currentCard > 0 || deckInfo.cycle">
        <template v-slot:activator="{ props }">
          <v-btn
            variant="text"
            icon="mdi-skip-previous"
            v-bind="props"
            @click="goToPosition(currentCard-1)"
          ></v-btn>
        </template>
      </v-tooltip>
      <v-tooltip v-if="currentCard > 0" text="Go to cover">
        <template v-slot:activator="{ props }">
          <v-btn
            variant="text"
            icon="mdi-home-floor-1"
            v-bind="props"
            @click="goToCover()"
          ></v-btn>
        </template>
      </v-tooltip>      
      {{ currentCard === 0?maxCards + ' cards in deck':currentCard + '/' + maxCards }}
      <v-tooltip text="Shuffle cards">
        <template v-slot:activator="{ props }">
          <v-btn
            variant="text"
            icon="mdi-shuffle"
            v-bind="props"
            color="red"
            @click="shuffle()"
          ></v-btn>
        </template>
      </v-tooltip>
      <v-tooltip text="Next card" v-if="currentCard < maxCards || deckInfo.cycle">
        <template v-slot:activator="{ props }">
          <v-btn
            variant="text"
            icon="mdi-skip-next"
            v-bind="props"
            @click="goToPosition(currentCard+1)"
          ></v-btn>
        </template>
      </v-tooltip>
      
    </div>
  </v-card>
</template>

<script setup>
  import Card from '@/components/Card.vue'
</script>


<script>
  export default {
    props: {
      deckInfo: { mandatory: true }
    },

    data () {
      return {
        currentCard: 0
      }
    },

    computed: {
      maxCards () {
        return this.deckInfo.cards.length
      },

      availableCards () {
        if (this.deckInfo.cover)
          return [this.deckInfo.cover, ...this.deckInfo.cards]
        return [...this.deckInfo.cards]
      }
    },

    methods: {
      shuffle () {
        this.goToCover()
        this.$emit('shuffle')
      },

      goToCover () {
        this.currentCard = 0
        this.$emit('currentCard', 0)
      },

      goToLast () {
        this.currentCard = this.maxCards
      }, 

      goToPosition(newCard) {
        if (newCard > this.maxCards && this.deckInfo.cycle) {
          this.goToCover()
          return false
        }
        if (newCard < 0 && this.deckInfo.cycle) {
          this.goToLast()
          return false
        }
        this.currentCard = newCard
        this.$emit('currentCard', this.currentCard)
        return true
      },

      getStyle(i) {
        const style = this.availableCards[i].style
        if (style) {
          return style
        }
        return this.deckInfo.style
      },
    }
  }
</script>