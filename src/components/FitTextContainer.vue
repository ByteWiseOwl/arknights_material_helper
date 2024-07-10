<script setup>
import { onMounted } from "vue";
const { text } = defineProps(["text"]);

onMounted(() => {
  const container = document.getElementById("containerId");
  const textElement = document.getElementById("textId");

  let fontSize = 1;
  textElement.style.fontSize = fontSize + "px";

  while (
    textElement.scrollWidth <= container.clientWidth &&
    textElement.scrollHeight <= container.clientHeight &&
    fontSize < 1000
  ) {
    fontSize++;
    textElement.style.fontSize = fontSize + "px";
  }

  textElement.style.fontSize = fontSize - 1 + "px"; // Use the last fitting font size
});
</script>

<template>
  <div class="container-text" id="containerId">
    <div class="text" id="textId">{{ text }}</div>
  </div>
</template>

<style scoped>
.container-text {
  width: 120px;
  margin: 2px;
  /* padding-right: 20px; */
  /* border: 1px solid black; */
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: hidden;
  cursor: default;
}
.text {
  width: 100%;
  height: 100%;
  text-align: left;
  display: flex;
  justify-content: center;
  align-items: center;
  /* white-space: nowrap; */
}
</style>
