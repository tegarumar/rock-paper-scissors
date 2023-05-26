<template>
  <div class="rpc">
    <div class="container-xxl">
      <h1 class="fw-bold">Rock Paper Scissors</h1>
      <section class="d-flex justify-content-center">
        <div class="score--board mt-2 mt-md-3">
          <h2 class="sb__title fw-bold">SCORE</h2>
          <span class="sb__score">{{ score }}</span>
        </div>
      </section>
      <main>
        <template v-if="!is_over">
          <h2 class="fw-bold mt-3">Your turn to pick</h2>
          <ul class="rpc__list d-flex flex-column flex-md-row justify-content-center align-items-center mt-5">
            <li v-for="(val, index) in data" :key="index">
              <button class="btn button--rps" :class="val.class" type="button" @click="youPick(val)">
                <div class="game__svg-container">
                  <img :src="val.image" :alt="val.name" class="rpc--img">
                </div>
              </button>
            </li>
          </ul>
        </template>
        <template v-else>
          <div class="game--result d-md-flex align-items-center justify-content-center mt-5 d-none d-md-block">
            <div class="gr--you">
              <h2 class="mb-3 fw-bold">YOU PICKED</h2>
              <button class="btn button--rps" :class="playerChoice.class" type="button">
                <div class="game__svg-container">
                  <img :src="playerChoice.image" :alt="playerChoice.name" class="rpc--img">
                </div>
              </button>
            </div>
            <div class="gr--action">
              <template v-if="result">
                <h3 class="fw-bold">{{ result }}</h3>
                <button class="btn btn-pa" @click="resetGame">PLAY AGAIN</button>
              </template>
              <template v-else>
                <h3>Wait....</h3>
              </template>
            </div>
            <div class="gr--bot">
              <template v-if="computerChoice">
                <h2 class="mb-3 fw-bold">THE BOT PICKED</h2>
                <button class="btn button--rps" :class="computerChoice.class" type="button">
                  <div class="game__svg-container">
                    <img :src="computerChoice.image" :alt="computerChoice.name" class="rpc--img">
                  </div>
                </button>
              </template>
              <template v-else>
                <h2>Wait....</h2>
              </template>
            </div>
          </div>
          <div class="game--result mt-5  d-block d-md-none">
            <div class="inner__game-result d-flex align-items-center justify-content-center">
              <div class="gr--you">
                <button class="btn button--rps" :class="playerChoice.class" type="button">
                  <div class="game__svg-container">
                    <img :src="playerChoice.image" :alt="playerChoice.name" class="rpc--img">
                  </div>
                </button>
                <h3 class="mt-3 fw-bold">YOU</h3>
              </div>
              <div class="gr--bot">
                <template v-if="computerChoice">
                  <button class="btn button--rps" :class="computerChoice.class" type="button">
                    <div class="game__svg-container">
                      <img :src="computerChoice.image" :alt="computerChoice.name" class="rpc--img">
                    </div>
                  </button>
                  <h3 class="mt-3 fw-bold">BOT</h3>
                </template>
                <template v-else>
                  <h3>Wait....</h3>
                </template>
              </div>
            </div>
            <div class="gr--action mt-4">
              <template v-if="result">
                <h2 class="fw-bold">{{ result }}</h2>
                <button class="btn btn-pa" @click="resetGame">PLAY AGAIN</button>
              </template>
            </div>
          </div>
        </template>
      </main>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      result: '',
      is_over: false,
      score: 0,
      playerChoice: null,
      computerChoice: null,
      data : [
        {
          image : require('@/assets/image/rock_left.png'),
          name : 'rock',
          class : 'rps--rock'
        },
        {
          image : require('@/assets/image/paper_left.png'),
          name : 'paper',
          class : 'rps--paper'
        },
        {
          image : require('@/assets/image/scissors_left.png'),
          name : 'scissors',
          class : 'rps--scissors'
        },
      ]
    }
  },
  methods: {
    youPick(val) {
      this.is_over = true
      this.playerChoice = val
      setTimeout(() => {
        this.computerChoice = this.generateComputerChoice()
        this.calculateResult()
      }, 700)
    },
    generateComputerChoice() {
      const randomIndex = Math.floor(Math.random() * this.data.length);
      return this.data[randomIndex];
    },
    calculateResult() {
      if (this.playerChoice.name === this.computerChoice.name) {
        this.result = "DRAW";
      } else if (
        (this.playerChoice.name === 'rock' && this.computerChoice.name === 'scissors') ||
        (this.playerChoice.name === 'paper' && this.computerChoice.name === 'rock') ||
        (this.playerChoice.name === 'scissors' && this.computerChoice.name === 'paper')
      ) {
        this.result = "You win!";
        this.score++
      } else {
        this.result = "BOT wins!";
        this.score--
      }
    },
    resetGame() {
      this.playerChoice = null
      this.computerChoice = null
      this.result = null;
      this.is_over = false
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.rpc {
  height: 100vh;
  background: radial-gradient( circle at 50% 0%, hsl(214, 47%, 23%) 10%, hsl(237, 49%, 15%) 100% );;
  color: white;
  padding: 3rem;
}

.rpc__list {
  gap: 25px;
  padding: 0;
}

.score--board {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  background-color: #fff;
  padding: 10px;
  width: 140px;
  height: 140px;
  border-radius: 10px;
}

.sb__title {
  font-size: 16px;
  color: hsl(229, 64%, 46%);
  margin: 0;
}

.sb__score {
  font-size: 60px;
  color: hsl(217, 16%, 45%);
  font-weight: 900;
  line-height: 1;
}

.rpc__list li {
  list-style-type: none;
}

.rpc--img {
  width: 85%;
  height: 85%;
  position: relative;
  z-index: 2;
}

.rps--rock::before {
  background: linear-gradient(0deg, hsl(230, 89%, 62%) 50%, hsl(230, 89%, 65%) 100%);
  box-shadow: 0 1em 0 #2740be;
}

.rps--paper::before {
  background: linear-gradient(0deg, hsl(39, 89%, 49%) 50%, hsl(40, 84%, 53%) 100%);
  box-shadow: 0 1em 0 #c96d1d;
}

.rps--scissors::before {
  background: linear-gradient(0deg, hsl(349, 71%, 52%) 50%, hsl(349, 70%, 56%) 100%);
  box-shadow: 0 1em 0 #9e152e
}

.button--rps {
  width: 12.5rem;
  height: 12.5rem;
  border: none;
  box-shadow: none;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: row;
  position: relative;
  grid-row: 1;
  max-width: 12.6875rem;
  border-radius: 50%;
  font-size: 5px;
  animation: fadeIn .5s cubic-bezier(.175,.885,.32,1.275);
}

.button--rps::before {
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  content: "";
  border-radius: inherit;
  /* background: inherit; */
  z-index: 0;
}

.button--rps:active, .rps--rock:active, .rps--paper:active, .rps--rock:active, .rps--scissors:active {
  box-shadow: none;
  transform: scale(0.98);
}

.game__svg-container {
    width: 78%;
    height: 78%;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: row;
    border-radius: 50%;
    background-color: #e4e4e4;
    position: relative;
    z-index: 1;
    pointer-events: none;
    transition: background-color .15s linear;
    box-shadow: inset 0 7px 0 rgba(96,110,133,.25);
}

.gr--bot, .gr--you {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 280px;
}

.game--result {
  gap: 30px;
}

.gr--action {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.btn-pa {
  color: black;
  background-color: #fff;
  border-radius: 8px;
  padding: 7px 30px;
}
.btn-pa:hover {
  color: rgba(231, 11, 11, 0.856);
}

.v-enter-active,
.v-leave-active {
  transition: opacity 0.5s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}

.inner__game-result {
  gap: 50px;
}

@media only screen and (max-width: 800px) {
  .rpc {
    min-height: 100vh;
  }

  .button--rps {
    width: 8rem;
    height: 8rem;
  }
}

@media only screen and (max-width: 376px) {
  .rpc {
    min-height: 135vh;
  }
}
</style>
