<template>
  <main class="min-vh-100 py-5 d-flex align-items-center justify-content-center overflow-hidden">
    <div class="container">
      <header class="header p-3 px-4 mb-5 rounded-4 d-flex align-items-center justify-content-between w-75 mx-auto">
        <img src="/images/logo.svg" alt="">
        <div class="score-box rounded-2 d-flex align-items-center flex-column justify-content-center">
          <span class="text-uppercase">Score</span>
          <h1 class="fw-bold mb-0">{{ score }}</h1>
        </div>
      </header>
      <div class="game py-5 d-flex justify-content-center align-items-center flex-wrap" v-if="!picked">
        <img class="triangle position-absolute" src="/images/bg-triangle.svg" alt="">
        <div class="w-25 d-flex align-items-center justify-content-end">
          <div
            class="box paper bg-white position-relative rounded-circle d-flex align-items-center justify-content-center"
            @click="play('paper')">
            <img src="/images/icon-paper.svg" alt="">
          </div>
        </div>
        <div class="w-25 d-flex align-items-center justify-content-start">
          <div
            class="box scissors bg-white position-relative rounded-circle d-flex align-items-center justify-content-center"
            @click="play('scissors')">
            <img src="/images/icon-scissors.svg" alt="">
          </div>
        </div>
        <div class="w-75 d-flex align-items-center justify-content-center">
          <div class="box rock bg-white position-relative rounded-circle d-flex align-items-center justify-content-center"
            @click="play('rock')">
            <img src="/images/icon-rock.svg" alt="">
          </div>
        </div>
      </div>
      <div class="playing p-5" v-else>
        <div class="d-flex justify-content-evenly align-items-center">
          <div class="wrapper">
            <h3 class="text-uppercase text-light text-center mb-5 pb-5">You picked</h3>
            <div :class="`boxes my-pick ${finalResult === 'win' ? 'win' : finalResult === 'draw' ? 'draw' : ''}`">
              <div
                class="box paper bg-white position-relative rounded-circle d-flex align-items-center justify-content-center"
                v-if="myChoice === 'paper'">
                <img src="/images/icon-paper.svg" alt="">
              </div>
              <div
                class="box scissors bg-white position-relative rounded-circle d-flex align-items-center justify-content-center"
                v-if="myChoice === 'scissors'">
                <img src="/images/icon-scissors.svg" alt="">
              </div>
              <div
                class="box rock bg-white position-relative rounded-circle d-flex align-items-center justify-content-center"
                v-if="myChoice === 'rock'">
                <img src="/images/icon-rock.svg" alt="">
              </div>
            </div>
          </div>
          <div class="results w-25 mt-5 pt-5" v-if="finalResult !== null">
            <h1 class="text-uppercase text-center mb-3 text-light">
              {{ finalResult === 'win' ? 'you win' : finalResult === 'lose' ? 'you lose' : finalResult === 'draw' ? 'draw'
                : '' }}
            </h1>
            <button class="text-uppercase w-75 mx-auto d-block rounded-2 p-2 px-3" @click="playAgain">Play Again</button>
          </div>
          <div class="wrapper">
            <h3 class="text-uppercase text-light text-center mb-5 pb-5">the house picked</h3>
            <div :class="`boxes computer-pick ${finalResult === 'lose' ? 'lose' : finalResult === 'draw' ? 'draw' : ''}`">
              <div class="place-holder" v-if="computerChoice === null"></div>
              <div
                class="box paper bg-white position-relative rounded-circle d-flex align-items-center justify-content-center"
                v-else-if="computerChoice === 'paper'">
                <img src="/images/icon-paper.svg" alt="">
              </div>
              <div
                class="box rock bg-white position-relative rounded-circle d-flex align-items-center justify-content-center"
                v-else-if="computerChoice === 'rock'">
                <img src="/images/icon-rock.svg" alt="">
              </div>
              <div
                class="box scissors bg-white position-relative rounded-circle d-flex align-items-center justify-content-center"
                v-else-if="computerChoice === 'scissors'">
                <img src="/images/icon-scissors.svg" alt="">
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="rules-butn text-center text-md-end mt-5">
        <button @click="openPopup = true" class="rounded-3 p-2 px-5 fw-bold text-uppercase">Rules</button>
      </div>
    </div>
    <RulesPopup @close-popup="openPopup = false" :openPopup="openPopup"></RulesPopup>
  </main>
</template>
<script setup>
import { ref, watch, onMounted } from 'vue';
import RulesPopup from './components/RulesPopup.vue'

const openPopup = ref(false)

const picked = ref(false)
const myChoice = ref(null)
const computerChoice = ref(null)
const finalResult = ref(null)

const score = ref(0)

const gameOutput = ref({
  rock: {
    rock: 'draw',
    paper: 'lose',
    scissors: 'win'
  },
  paper: {
    rock: 'win',
    paper: 'draw',
    scissors: 'lose'
  },
  scissors: {
    rock: 'lose',
    paper: 'win',
    scissors: 'draw'
  },
})

const play = (myPick) => {
  picked.value = true
  myChoice.value = myPick
  const choices = ['rock', 'paper', 'scissors']
  const randomPick = Math.floor(Math.random() * choices.length)
  setTimeout(() => {
    computerChoice.value = choices[randomPick]
  }, 2000)
  setTimeout(() => {
    finalResult.value = gameOutput.value[myChoice.value][computerChoice.value]
  }, 3000)
}


const playAgain = () => {
  picked.value = false
  myChoice.value = null
  computerChoice.value = null
  finalResult.value = null
}

watch(finalResult, newVal => {
  console.log(newVal);
  if (newVal === 'draw') {
    return score.value
  } else if (newVal === 'win') {
    score.value += 1
    localStorage.setItem('score', score.value)
  } else if (newVal === 'lose') {
    score.value -= 1
    if (score.value < 0) {
      score.value = 0
    }
    localStorage.setItem('score', score.value)
  }
})

onMounted(() => {
  if (localStorage.getItem('score')) {
    score.value = parseInt(localStorage.getItem('score'))
  }
})

</script>
<style lang="scss">
* {
  box-sizing: border-box;
}

:root {
  // ### Primary
  --Scissors-Gradient: linear-gradient(to right, hsl(39, 89%, 49%), hsl(40, 84%, 53%));
  --Paper-Gradient: linear-gradient(to right, hsl(230, 89%, 62%), hsl(230, 89%, 65%));
  --Rock-Gradient: linear-gradient(to right, hsl(349, 71%, 52%), hsl(349, 70%, 56%));
  --Lizard-Gradient: linear-gradient(to right, hsl(261, 73%, 60%), hsl(261, 72%, 63%));
  --Cyan: linear-gradient(to right, hsl(189, 59%, 53%), hsl(189, 58%, 57%));

  // ### Neutral
  --Dark-Text: hsl(229, 25%, 31%);
  --Score-Text: hsl(229, 64%, 46%);
  --Header-Outline: hsl(217, 16%, 45%);

  // ### Background
  --Radial-Gradient: linear-gradient(to bottom, hsl(214, 47%, 23%), hsl(237, 49%, 15%));
}

body {
  font-family: 'Barlow Semi Condensed', sans-serif;
  background: var(--Radial-Gradient);
}

ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

main {

  header {
    border: 2px solid var(--Header-Outline);

    .score-box {
      background-color: white;
      width: 140px;
      height: 120px;

      span {
        color: var(--Score-Text);
        letter-spacing: 2px;
      }

      h1 {
        font-size: 60px;
        color: var(--Dark-Text);
      }
    }
  }

  .box {

    width: 130px;
    height: 130px;
    box-shadow: inset 0px 7px 1px #0000004d;
    cursor: pointer;
    transition: 0.2s;

    &.paper::before {
      background: var(--Paper-Gradient);
    }

    &.rock::before {
      background: var(--Rock-Gradient);
    }

    &.scissors::before {
      background: var(--Scissors-Gradient);
    }

    &::before {
      content: '';
      position: absolute;
      width: 175px;
      height: 175px;
      z-index: -1;
      border-radius: 50%;
      box-shadow: inset -1px -7px 1px #0000004d;
    }
  }

  .game {
    gap: 100px;
    row-gap: 75px;

    .triangle {
      z-index: -10;
    }
  }

  .playing {
    .place-holder {
      width: 200px;
      height: 200px;
      border-radius: 50%;
      position: relative;
      background-color: rgba(0, 0, 0, 0.459);
      opacity: 0.3;

      &::before {
        content: '';
        position: absolute;
        width: 270px;
        height: 270px;
        left: 50%;
        top: 50%;
        transform: translate(-50%, -50%);
        border-radius: 50%;
      }
    }

    .boxes {
      position: relative;

      &.win,
      &.lose,
      &.draw {

        &::before,
        &::after {
          display: block;
        }
      }

      &::before,
      &::after {
        content: '';
        position: absolute;
        display: none;
        width: 300px;
        height: 300px;
        border-radius: 50%;
        background-color: rgba(255, 255, 255, 0.644);
        opacity: 0.3;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        z-index: -1;
        animation: waves 1s ease-out infinite;
      }

      &::after {
        animation: waves 1s ease-out infinite 0.5s;
      }
    }

    .box {
      width: 200px;
      height: 200px;
      cursor: initial;

      img {
        width: 50%;
      }

      &::before {
        width: 270px;
        height: 270px;
      }
    }

    h3 {
      color: white;
    }

    .results {
      h1 {
        font-size: 50px;
      }

      button {
        background-color: white;
        outline: none;
        border: none;
        letter-spacing: 2px;
        color: var(--Dark-Text);
        transition: 0.2s;

        &:hover {
          color: rgb(216, 23, 23);
        }
      }
    }
  }

  .rules-butn {
    button {
      border: 2px solid var(--Header-Outline);
      background: none;
      outline: none;
      color: white;
      letter-spacing: 3px;
    }
  }

  @media (max-width: 767px) {
    .header {
      width: 100% !important;

      img {
        width: 35%;
      }

      .score-box {
        width: 90px;
        height: 80px;

        span {
          font-size: 10px;
        }

        h1 {
          font-size: 35px;
        }
      }
    }

    .box {
      width: 90px;
      height: 90px;
      box-shadow: inset 0px 4px 1px rgba(0, 0, 0, 0.3019607843);

      img {
        width: 50%;
      }

      &::before {
        box-shadow: inset 0px -4px 1px rgba(0, 0, 0, 0.3019607843);
        width: 120px;
        height: 120px;
      }
    }

    .game {
      .triangle {
        width: 60%;
      }
    }

    .playing {
      padding: 20px 30px !important;
      display: flex;
      justify-content: flex-end;
      flex-direction: column-reverse;
      height: 450px;
      
      &>div {
        flex-wrap: wrap;
        margin-bottom: 0 !important;
        justify-content: space-between !important;

        h3 {
          font-size: 15px !important;
        }
      }
      .wrapper {
        display: flex;
        flex-direction: column-reverse;
        height: 175px;
        justify-content: space-between;
        align-items: center;
        &:first-of-type {
          width: 41%;
        }
        h3 {
          padding-bottom: 0 !important;
          margin-bottom: 0 !important;
        }
      }

      .place-holder {
        width: 90px;
        height: 90px;
      }

      .boxes {

        &::before,
        &::after {
          width: 100px;
          height: 100px;
        }
      }

      .box {
        width: 90px;
        height: 90px;
        &::before {
          width: 120px;
          height: 120px;
        }
      }

      .results {
        width: 85% !important;
        margin: auto;
        order: 3;
        padding-top: 20px !important;
      }
    }
  }
}


@keyframes waves {
  100% {
    transform: translate(-50%, -50%) scale(1.9);
    opacity: 0;
  }
}
</style>