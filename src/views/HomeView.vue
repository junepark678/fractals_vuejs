<template>
  <div class="home">
    <h1>Fractals!</h1>
    <input type="number" v-model="depth" max="5" min="1" />
    <br />
    <FractalCanvas :depth="depth" />
  </div>
</template>

<script lang="ts" setup>
import FractalCanvas from "@/components/FractalCanvas.vue";
import { ref } from "vue";
let depth = ref(2);
</script>

<style lang="scss">
input {
  margin: 2rem !important;
}
</style>
