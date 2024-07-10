<script setup>
import { ref } from "vue";
function setTitleTextWidth() {
  const targetHeight = 500; // Zielhöhe in Pixeln

  const temporaryElement = document.createElement("div");
  document.body.appendChild(temporaryElement);
  temporaryElement.style.position = "absolute";
  temporaryElement.style.visibility = "hidden";
  temporaryElement.style.height = targetHeight + "px";
  temporaryElement.style.whiteSpace = "normal"; // Um Zeilenumbruch zu ermöglichen

  const text =
    "Lorem, ipsum dolor sit amet consectetur adipisicing elit. Quae exercitationem distinctio temporibus quos rerum possimus quia, eius cupiditate eaque, eos quaerat veniam consequuntur similique odit officiis? Ea quo iusto distinctio sint voluptatum quod, sequi eos explicabo dicta quasi a. Maxime, officiis, eos qui fuga, dignissimos officia sapiente inventore adipisci non necessitatibus at temporibus velit voluptas nobis hic numquam earum nisi! Fuga quidem laudantium, amet obcaecati dolorum aut. Animi eveniet at qui consequatur expedita, repellendus sequi a minima reprehenderit saepe officiis architecto fugiat pariatur ratione? Repellat ducimus repudiandae deserunt fuga! Quod veniam numquam similique officia aliquam qui perferendis animi rem quidem.";
  let maxWidth = 0;

  // Object.values(data.events).forEach((event) => {
  temporaryElement.style.width = "auto"; // Setze die Breite auf automatisch, um neu zu berechnen
  temporaryElement.textContent = text;

  // Temporär eine feste Breite setzen, um die Höhe zu erreichen
  for (let width = 10; width <= 10000; width += 10) {
    temporaryElement.style.width = width + "px";
    if (temporaryElement.scrollHeight <= targetHeight) {
      const elementWidth = temporaryElement.scrollWidth;
      if (elementWidth > maxWidth) {
        maxWidth = elementWidth;
      }
      break;
    }
  }
  // });

  document.body.removeChild(temporaryElement);

  // Set the CSS variable
  document.documentElement.style.setProperty("--width", maxWidth + "px");
}
setTitleTextWidth();
</script>

<template>
  <div class="container">
    <div class="child">
      Lorem, ipsum dolor sit amet consectetur adipisicing elit. Quae
      exercitationem distinctio temporibus quos rerum possimus quia, eius
      cupiditate eaque, eos quaerat veniam consequuntur similique odit officiis?
      Ea quo iusto distinctio sint voluptatum quod, sequi eos explicabo dicta
      quasi a. Maxime, officiis, eos qui fuga, dignissimos officia sapiente
      inventore adipisci non necessitatibus at temporibus velit voluptas nobis
      hic numquam earum nisi! Fuga quidem laudantium, amet obcaecati dolorum
      aut. Animi eveniet at qui consequatur expedita, repellendus sequi a minima
      reprehenderit saepe officiis architecto fugiat pariatur ratione? Repellat
      ducimus repudiandae deserunt fuga! Quod veniam numquam similique officia
      aliquam qui perferendis animi rem quidem.
    </div>
  </div>
</template>
<style>
:root {
  --width: 200px;
}
</style>
<style scoped>
.container {
  border: 5px solid red;
  /* height: 80px;
  width: auto; */
}

.child {
  /* white-space: normal; */
  /* max-height: 80px; */
  width: var(--width);
}
</style>
