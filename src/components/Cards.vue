<template>
  <div>
    <div class="clicks">
      <div class="title">Cards showings: &nbsp; <span>{{ this.numberOfClicks }}</span></div>
      <div v-if="bestResult" class="title">Best result: &nbsp; <span>{{ this.bestResult }}</span></div>
    </div>

    <div class="cards_wrapper">
      <div class="cards">
        <app-card @click="onCardClick(card)" v-for="(card, i) in shuffledNumbers" :card="card" :key="i"></app-card>
      </div>

      <div v-if="endOfTheGame" class="congratulations">Congratulations!
        <span v-if="newBestResult">New best result!</span>
      </div>
      <div @click="restartTheGame" class="btn">Restart</div>

    </div>
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
      newCurrentId: null,
      endOfTheGame: false,
      numberOfClicks: 0,
      bestResult: null,
      newBestResult: false
    }
  },

  computed: {
    checkRestCards() {
      return this.shuffledNumbers.filter(item => {
        return !item.open;
      });
    }
  },

  methods: {
    onCardClick(card) {
      if (card.clickable && !card.open) {
        card.backSide = false;
        card.animated = true;
        this.numberOfClicks++;

        if (!this.currentCard) {
          this.currentCard = card.number;
          this.currentId = card.id;
        } else {
          this.newCurrentCard = card.number;
          this.newCurrentId = card.id;

          this.shuffledNumbers.map(item => {
            item.clickable = false;
          });
        }

        if (this.currentCard === this.newCurrentCard && this.currentId !== this.newCurrentId) {
          this.lockOpenedCardsWithNumber(card.number);
        } else {
          this.returnCardsToBackSide(card);
        }
      }
    },

    lockOpenedCardsWithNumber(number) {
      this.shuffledNumbers.map(item => {
        if (item.number === number && item.number === this.newCurrentCard) {
          item.clickable = false;
          item.backSide = false;
          item.open = true;

          if (this.checkRestCards.length === 0) {
            this.endOfTheGame = true;

            if (!this.bestResult || this.numberOfClicks < this.bestResult) {
              this.bestResult = this.numberOfClicks;
              this.newBestResult = true;
            }
          }
        }
      })
    },

    returnCardsToBackSide(card) {
      setTimeout(() => {
        if (card.number !== this.newCurrentCard) {
          card.backSide = true;
          card.animated = false;
        }
        this.currentCard = null;
        this.newCurrentCard = null;

        this.shuffledNumbers.map(item => {
          item.clickable = true;
        });
      }, 2000)
    },

    shuffleCards() {
      this.shuffledNumbers = this.cards.sort(() => Math.random() - 0.5);
    },

    restartTheGame() {
      this.cards.map(item => {
        item.backSide = true;
        item.clickable = true;
        item.open = false;
        item.animated = false;
      });

      this.shuffleCards();
      this.endOfTheGame = false;
      this.numberOfClicks = 0;
      this.newBestResult = false;
    }
  },

  mounted() {
    this.shuffleCards();
  }
}
</script>

<style lang="scss">
@import "@/assets/style/variables";

.cards_wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  transition: .9s all;
  height: calc(100vh - 90px);
  width: 360px;

  @media (max-width: 500px) {
    width: 320px;
  }
}

.cards {
  display: flex;
  flex-wrap: wrap;
  transition: .9s all;
}

.congratulations {
  display: flex;
  flex-direction: column;
  margin-top: 30px;
  font-size: 34px;
  font-weight: bold;
  color: $main;
  animation: fadeUp .9s ease-in-out;

  span {
    margin-top: 10px;
    font-size: 22px;
    background: $gradient-blue-violet;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
  }
}

.btn {
  box-sizing: border-box;
  width: calc(100% - 10px);
  margin-top: 20px;
  border: 1px solid $main;
  border-radius: 4px;
  padding: 15px;
  cursor: pointer;
  font-size: 14px;
  line-height: 100%;
  letter-spacing: 2px;
  text-transform: uppercase;
  color: $main;
  transition: .4s all ease-in-out;

  &:hover {
    background-color: $secondary;
    border: 1px solid $secondary;
    color: white;
    transition: .6s all ease-in-out;

    @media (max-width: 500px) {
      background-color: transparent;
      color: $main;
      border: 1px solid $main;
    }
  }

  &:active {
    border: 1px solid $secondary;
    transition: .2s all ease-in-out;
    transform: scale(.99);
    letter-spacing: 1.3px;

    @media (max-width: 500px) {
      background-color: transparent;
      color: $secondary;
      border: 1px solid $secondary;
    }
  }
}

.title {
  display: flex;
  justify-content: flex-start;
  align-items: flex-start;
  font-size: 20px;
  color: $main;

  &:first-child {
    margin-top: 18px;
  }

  &:last-child {
    margin-top: 5px;
  }

  span {
    font-weight: bold;
  }
}

@keyframes fadeUp {
  from {
    transform: translateY(40px);
    opacity: 0;
  }
  to {
    transform: translateY(0);
    opacity: 1;
  }
}
</style>
