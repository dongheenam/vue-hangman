<template>
  <main>
    <GameSettings @submit-answer="setAnswer" @reset-game="resetGame" />
    <div class="answer-display">
      <div v-for="(char, index) in answer" class="char-display">
        {{ guessStatus[index] ? char : "_" }}
      </div>
    </div>
    <div class="solver-display">
      <div class="keyboard">
        <div v-for="keyboardRow in alphabets" class="keyboard-row">
          <button
            v-for="letter in keyboardRow"
            @click="evaluateGuess(letter)"
            :disabled="isKeyDisabled(letter)"
          >
            {{ letter }}
          </button>
        </div>
      </div>
      <div class="nguess-display">
        <div>Guess(es) left: {{ numberOfChancesLeft }}</div>
      </div>
    </div>
  </main>
</template>

<script setup lang="ts">
import { ref } from "vue";
import GameSettings from "./GameSettings.vue";

import JSConfetti from "js-confetti";

const alphabets = ref<string[][]>([
  "QWERTYUIOP".split(""),
  "ASDFGHJKL".split(""),
  "ZXCVBNM ".split(""),
]);

const answer = ref<string>("");
const guessStatus = ref<boolean[]>([]);
const pastGuesses = ref<string[]>([" "]);
const numberOfChancesLeft = ref<number>(8);

function setAnswer(newAnswer: string) {
  answer.value = newAnswer;
  guessStatus.value = new Array(newAnswer.length).fill(false);
  // space is always revealed
  answer.value.split("").map((value, index) => {
    if (value === " ") {
      guessStatus.value[index] = true;
    }
  });
}
function isKeyDisabled(letter: string): boolean {
  return pastGuesses.value.includes(letter);
}
function isAnswerFound(): boolean {
  return guessStatus.value.reduce((pv, cv) => pv && cv);
}
function evaluateGuess(letter: string): void {
  if (answer.value === "") {
    alert("Submit an answer first!");
    return;
  }
  if (numberOfChancesLeft.value === 0) {
    alert("No more guesses allowed. Reset the game!");
    return;
  }
  if (isAnswerFound()) {
    return;
  }
  pastGuesses.value.push(letter);
  let letterIsInAnswer: boolean = false;
  answer.value.split("").map((value, index) => {
    if (value === letter) {
      guessStatus.value[index] = true;
      letterIsInAnswer = true;
    }
  });
  if (!letterIsInAnswer) {
    numberOfChancesLeft.value--;
  }
  if (isAnswerFound()) {
    const confetti = new JSConfetti();
    confetti.addConfetti();
  }
}

function resetGame() {
  answer.value = "";
  guessStatus.value = [];
  pastGuesses.value = [" "];
  numberOfChancesLeft.value = 8;
}
</script>

<style lang="scss">
main {
  width: clamp(400px, 85%, 800px);
  margin: 2rem auto;
}
</style>

<style lang="scss" scoped>
.answer-display {
  width: auto;
  margin: 1rem 0;
  display: flex;
  justify-content: center;

  .char-display {
    padding: 0 0.3rem;

    font-size: 2rem;
    font-family: monospace;
    font-weight: bold;
  }
}

.keyboard {
  display: flex;
  flex-direction: column;
  align-items: center;

  .keyboard-row {
    display: flex;
  }

  button {
    width: 2em;
    height: 2em;

    background-color: var(--vt-c-black-soft);
    border: 1px solid var(--vt-c-white);

    font-size: 1.2rem;
    color: var(--vt-c-white);

    &:hover {
      background-color: var(--vt-c-black-mute);
    }

    &:active {
      background-color: var(--vt-c-indigo);
    }

    &[disabled] {
      background-color: gray;
    }
  }
}
</style>
