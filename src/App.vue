<template>
  <div class="container">
    <Header />
    <Game />
  </div>
</template>

<script setup lang="ts">
import Header from "./components/Header.vue";
import Game from "./components/Game.vue";
</script>

<style lang="scss">
@import "./assets/base.scss";

.container {
  margin: auto;

  display: grid;
  grid-template-rows: auto 1fr;
  background-color: var(--color-background-soft);
}
</style>
