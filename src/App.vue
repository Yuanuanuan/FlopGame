<template>
  <main>
    <Game />
  </main>
</template>

<script setup lang="ts">
import Game from "./components/Game.vue";
</script>

<style>
main {
  width: 100vw;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
</style>
