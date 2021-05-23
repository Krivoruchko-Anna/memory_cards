<template>
  <div class="cards">
    <app-card @click="onCardClick(card)" v-for="(card, i) in shuffledNumbers" :card="card" :key="i"></app-card>
  </div>
</template>

<script>
import AppCard from "./Card";

export default {
  components: {
     AppCard
  },

  data() {
    return {
      cards: [
        { number: 1, backSide: true, clickable: true, animated: false, open: false, id: 1 },
        { number: 2, backSide: true, clickable: true, animated: false, open: false, id: 2 },
        { number: 3, backSide: true, clickable: true, animated: false, open: false, id: 3 },
        { number: 4, backSide: true, clickable: true, animated: false, open: false, id: 4 },
        { number: 5, backSide: true, clickable: true, animated: false, open: false, id: 5 },
        { number: 6, backSide: true, clickable: true, animated: false, open: false, id: 6 },
        { number: 7, backSide: true, clickable: true, animated: false, open: false, id: 7 },
        { number: 8, backSide: true, clickable: true, animated: false, open: false, id: 8 },
        { number: 1, backSide: true, clickable: true, animated: false, open: false, id: 9 },
        { number: 2, backSide: true, clickable: true, animated: false, open: false, id: 10 },
        { number: 3, backSide: true, clickable: true, animated: false, open: false, id: 11 },
        { number: 4, backSide: true, clickable: true, animated: false, open: false, id: 12 },
        { number: 5, backSide: true, clickable: true, animated: false, open: false, id: 13 },
        { number: 6, backSide: true, clickable: true, animated: false, open: false, id: 14 },
        { number: 7, backSide: true, clickable: true, animated: false, open: false, id: 15 },
        { number: 8, backSide: true, clickable: true, animated: false, open: false, id: 16 },
      ],

      shuffledNumbers: [],

      currentCard: null,
      currentId: null,
      newCurrentCard: null,
      newCurrentId: null
    }
  },

  methods: {
    onCardClick(card) {

      if (card.clickable && !card.open) {
        card.backSide = false;
        card.animated = true;

        if (!this.currentCard) {
          this.currentCard = card.number;
          this.currentId = card.id;
        } else {
          this.newCurrentCard = card.number;
          this.newCurrentId = card.id;

          this.shuffledNumbers.map(item => {
            item.clickable = false;
          })
        }

        if (this.currentCard === this.newCurrentCard && this.currentId !== this.newCurrentId) {
          this.lockOpenedCardsWithNumber(card.number);
        } else {
          this.returnsCardsToBackSide(card);
        }
      }
    },

    lockOpenedCardsWithNumber(number) {
      this.shuffledNumbers.map(item => {
        if (item.number === number) {
          item.clickable = false;
          item.backSide = false;
          item.open = true;
        }
      })
    },

    returnsCardsToBackSide(card) {
      setTimeout(() => {
        if (card.number !== this.newCurrentCard) {
          card.backSide = true;
          card.animated = false;
        }
        this.currentCard = null;
        this.newCurrentCard = null;

        this.shuffledNumbers.map(item => {
          item.clickable = true;
        })
      }, 2000)
    }
  },

  mounted() {
    this.shuffledNumbers = this.cards.sort(() => Math.random() - 0.5);
  }
}
</script>

<style lang="scss">
.cards {
  display: flex;
  flex-wrap: wrap;
  width: 400px;
}
</style>
