<script>
export default {
  data() {
    return {
      cards: [
        {
          key: 1,
          src: 'https://smile-emoji.ru/wp-content/uploads/site-images/discord/d8ccf3571d3ad7c6ca8ccad88daa29b6.png',
          isOpen: false,
          done: false,
        },
        {
          src: 'https://smile-emoji.ru/wp-content/uploads/site-images/discord/d8ccf3571d3ad7c6ca8ccad88daa29b6.png',
          isOpen: false,
          key: 1,
          done: false,
        },
        {
          src: 'https://kartinkof.club/uploads/posts/2022-07/1659163341_2-kartinkof-club-p-kartinki-smailiki-nastroeniya-2.png',
          isOpen: false,
          key: 2,
          done: false,
        },
        {
          src: 'https://kartinki.pics/uploads/posts/2022-02/1645708376_18-kartinkin-net-p-krasivie-kartinki-smailiki-19.jpg',
          isOpen: false,
          key: 4,
          done: false,
        },
        {
          src: 'https://otkritkis.com/wp-content/uploads/2021/11/CstB171WEAQGpJ8.jpg',
          isOpen: false,
          key: 3,
          done: false,
        },
        {
          src: 'https://kartinki.pics/uploads/posts/2022-02/1645708376_18-kartinkin-net-p-krasivie-kartinki-smailiki-19.jpg',
          isOpen: false,
          key: 4,
          done: false,
        },
        {
          src: 'https://otkritkis.com/wp-content/uploads/2021/11/CstB171WEAQGpJ8.jpg',
          isOpen: false,
          key: 3,
          done: false,
        },
        {
          src: 'https://kartinkof.club/uploads/posts/2022-07/1659163341_2-kartinkof-club-p-kartinki-smailiki-nastroeniya-2.png',
          isOpen: false,
          key: 2,
          done: false,
        },
      ],
      currentItems: { key: '', index: '' },
      count: { all: 0, max: 0 },
      game: { end: false, guessed: 0 },
    }
  },
  methods: {
    onClick(index, key) {
      if (!this.cards[index].isOpen && this.count.max != 2) {
        this.count.max++
        this.count.all++
        this.cards[index].isOpen = !this.cards[index].isOpen
        if (this.currentItems.key == key && this.currentItems.index != index) {
          setTimeout(() => {
            this.game.guessed++
            this.count.max = 0
            this.count.max = 0
            this.cards[index].done = true
            this.cards[this.currentItems.index].done = true
          }, 900)
        } else {
          if (this.count.max !== 2) {
            this.currentItems = { key, index }
          } else {
            setTimeout(() => {
              this.count.max = 0
              this.cards[index].isOpen = false
              this.cards[this.currentItems.index].isOpen = false
            }, 1000)
          }
        }
      }
    },
    gameOver(cards) {
      if (this.game.guessed == cards.length / 2) {
        return false
      }
      return true
    },
  },
  computed: {
    dynamicStylesFront() {
      return (index) => ({
        transform: this.cards[index].isOpen
          ? 'perspective(600px) rotateY(-180deg)'
          : 'perspective(600px) rotateY(0deg)',
        pointerEvents: 'none',
      })
    },
    dynamicStylesBack() {
      return (index) => ({
        transform: this.cards[index].isOpen
          ? 'perspective(600px) rotateY(0deg)'
          : 'perspective(600px) rotateY(180deg)',
        pointerEvents: 'none',
      })
    },
  },
}
</script>

<template>
  <section class="game">
    <div class="container">
      <div class="cards-game" v-if="gameOver(cards)">
        <div
          class="cards-fild"
          v-for="(card, index) in cards"
          :key="index"
          @click="onClick(index, card.key)"
        >
          <div class="item" v-if="!card.done">
            <div class="front" :style="dynamicStylesFront(index)">
              <img
                src="https://clipart-library.com/images/yckrpRKKi.jpg"
                alt="Обратная сторона карточки"
              />
            </div>
            <div class="back" :style="dynamicStylesBack(index)">
              <img v-bind:src="card.src" alt="Смайл" />
            </div>
          </div>
        </div>
      </div>
      <div v-else class="results">
        <p>Поздравляю!</p>
        <p>Количество ходов: {{ count.all }}</p>
        <p>Рекорд: {{ cards.length }}</p>
      </div>
    </div>
  </section>
</template>

<style scoped>
.game {
  height: 100%;
  background-color: rgb(222, 234, 230);
}
.cards-game {
  display: grid;
  grid-template-columns: repeat(4, minmax(200px, 1fr));
  gap: 20px;
  padding: 30px 0;
}
.item {
  position: relative;
  height: 375px;
  max-width: 250px;
  cursor: pointer;
  /* overflow: hidden; */
  text-align: center;
  border-radius: 30px;
}
.results {
  height: 100vh;
  text-align: center;
  padding: 30px 10px;
}
.results p {
  font-size: 16vh;
  font-weight: 500;
}
.front,
.back {
  height: inherit;
  position: absolute;
  backface-visibility: hidden;
  overflow: hidden;
  transition: transform 0.6s linear;
}
.dn {
  display: none;
}

.back {
  background-color: #f1f1f1;
}
</style>
