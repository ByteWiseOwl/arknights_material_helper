<script setup>
import { ref, watch } from "vue";

const isElementVisible = ref(false);

// Watch the visibility of the element and toggle the no-scroll class
watch(isElementVisible, (newValue) => {
  if (newValue) {
    document.body.classList.add("no-scroll");
  } else {
    document.body.classList.remove("no-scroll");
  }
});

function toggleVisibility() {
  isElementVisible.value = !isElementVisible.value;
}
</script>

<template>
  <button @click="toggleVisibility">Toggle Element</button>
  <div v-if="isElementVisible" class="fixed-element">I am a fixed element.</div>
  <p>lorem500</p>
</template>

<style scoped>
.fixed-element {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background: lightblue;
  padding: 20px;
  border: 2px solid blue;
}

.no-scroll {
  overflow: hidden;
}
</style>
