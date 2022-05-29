<template>
  <div class="word-picker">
    <p>
      Type the answer below and press Submit to start the game. Answers can only
      contain English alphabets and spaces.
    </p>
    <div class="picker-form">
      <input type="text" v-model="answer" />
      <button
        :disabled="isAnswerInvalid"
        @click="
          emit('submitAnswer', answer.toUpperCase());
          answer = '';
        "
      >
        Submit
      </button>
      <button @click="emit('resetGame')">Reset</button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, defineEmits, computed } from "vue";

const answer = ref<string>("");
const isAnswerInvalid = computed(() => {
  return answer.value === "" || /[^a-zA-Z ]/.test(answer.value);
});

const emit = defineEmits({
  submitAnswer: (word: string): boolean => {
    return true;
  },
  resetGame: null,
});
</script>

<style scoped lang="scss"></style>
