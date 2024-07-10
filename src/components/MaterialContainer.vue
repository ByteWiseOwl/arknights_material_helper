<script setup>
import { inject, onBeforeUnmount, ref } from "vue";

const { typ, values, self } = defineProps(["typ", "values", "self"]);
const materials = inject("materials");
const highlightedComponents = inject("highlightedComponents");
const hoveredTier = inject("hoveredTier");
const stackMaterials = inject("stackMaterials");
const target = ref(null);

const combined_materials = Object.values(materials)
  .filter((item) => Array.isArray(item)) // Nur Arrays filtern
  .flat();

function onMouseOverValue(material) {
  highlightComponents(material);
  addWheelListener(material);
}

function onMouseLeaveValue(material) {
  clearHighlights();
  removeWheelListener(material.id);
}

function getComponents(name) {
  return combined_materials.find((material) => material.name === name)
    .components;
}

function highlightComponents(material) {
  highlightedComponents.value = getComponents(material.name);
  highlightedComponents.value.push(material.name);
  hoveredTier.value = material.tier;
}

function clearHighlights() {
  highlightedComponents.value = [];
  hoveredTier.value = 0;
}

function addWheelListener(material) {
  if (!self) {
    return;
  }
  target.value = material;
  document
    .getElementById(`${self}_${material.id}`)
    .addEventListener("wheel", onWheel, { passive: false });
}

function removeWheelListener(id) {
  if (!self) {
    return;
  }
  target.value = null;
  document
    .getElementById(`${self}_${id}`)
    .removeEventListener("wheel", onWheel);
}

function onWheel(event) {
  event.preventDefault(); // Verhindert das Scrollen des Fensters
  const key = target.value.name;
  const keyExists = values.hasOwnProperty(key); // Überprüft, ob der Schlüssel existiert

  if (event.deltaY < 0) {
    // Mausrad nach oben drehen
    if (keyExists) {
      values[key]++;
      if (values[key] > 9999) {
        values[key] = 9999; // Setzt den Wert auf das Maximum von 9999
      }
    } else {
      values[key] = 1; // Fügt den Schlüssel mit dem Wert 1 hinzu
    }
  } else {
    // Mausrad nach unten drehen
    if (keyExists) {
      values[key]--;
      if (values[key] < 1) {
        delete values[key]; // Entfernt den Schlüssel, wenn der Wert unter 1 fällt
      }
    }
  }
}

function getBgColorByTier(tier) {
  switch (tier) {
    case 1:
      return "rgba(255, 174, 0, 0.9)";
    case 2:
      return "rgba(219, 177, 219, 0.9)";
    case 3:
      return "rgba(0, 178, 246, 0.9)";
    default:
      return "rgba(150, 150, 150, 0.9)";
  }
}

function shouldHighlight(material) {
  return highlightedComponents.value.includes(material);
}

function shouldDim(material) {
  if (highlightedComponents.value.length === 0) {
    return false;
  }
  return !highlightedComponents.value.includes(material);
}

onBeforeUnmount(() => {
  // todo! sichergehen das alle Listener entfernt werden
});
</script>

<template>
  <div :class="{ h_box: !stackMaterials }">
    <div v-for="(tier, index) in materials" :key="index">
      <div class="materials-container">
        <div
          v-for="material in tier"
          :key="material.id"
          class="material-border-left"
        >
          <img
            class="material-image"
            v-if="typ === 'image'"
            :src="`../src/assets/materials/${material.name}.png`"
            :alt="material.name"
            :style="{ backgroundColor: getBgColorByTier(material.tier) }"
            @mouseover="highlightComponents(material)"
            @mouseleave="clearHighlights()"
            :class="{
              highlighted: shouldHighlight(material.name),
              dimed: shouldDim(material.name),
              normal_opacity: !shouldDim(material.name),
            }"
          />
          <p
            :id="`${self}_${material.id}`"
            class="material-value"
            v-if="typ === 'value'"
            @mouseover="onMouseOverValue(material)"
            @mouseleave="onMouseLeaveValue(material)"
            :class="{
              highlighted: shouldHighlight(material.name),
              dimed: shouldDim(material.name),
              normal_opacity: !shouldDim(material.name),
            }"
          >
            {{ values[material.name] ?? 0 }}
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
img p {
  /* z-index: 10; */
  opacity: 30%;
}
.materials-container {
  /* border: 1px solid red; */
  display: flex;
  align-items: center;
}
.material-border-left {
  /* border: 1px solid blue; */
  display: flex;
  align-items: center;
  border-left: 2px solid;
  padding: 1px;
}
.material-image,
.material-value {
  height: 45px;
  width: 45px;
  border-radius: 10px;
  padding: 2px;
  margin: 2px;
  display: flex;
  align-items: center;
  justify-content: center;
}
.material-value {
  height: 25px;
  font-size: large;
  background-color: rgba(120, 120, 120, 0.9);
  user-select: none;
}
@keyframes orbitShadow {
  0% {
    box-shadow: 4px -4px 10px white;
  }
  25% {
    box-shadow: 4px 4px 10px white;
  }
  50% {
    box-shadow: -4px 4px 10px white;
  }
  75% {
    box-shadow: -4px -4px 10px white;
  }
  100% {
    box-shadow: 4px -4px 10px white;
  }
}
@keyframes pulsate {
  0% {
    /* box-shadow: 4px -4px 15px white, 4px 4px 15px white, -4px 4px 15px white,
      -4px -4px 15px white; */
    box-shadow: 0 0 10px 5px white;
  }
  100% {
    /* box-shadow: 4px -4px 10px white, 4px 4px 10px white, -4px 4px 10px white,
      -4px -4px 10px white; */
    /* box-shadow: 4px -4px 10px var(--shadow-color-big),
      4px 4px 10px var(--shadow-color-big),
      -4px 4px 10px var(--shadow-color-big),
      -4px -4px 10px var(--shadow-color-big); */
    box-shadow: 0 0 10px 3px white;
  }
}
.highlighted {
  /* animation: orbitShadow 3s infinite linear; */
  animation: pulsate 0.8s infinite ease-in-out alternate;
}
.dimed {
  opacity: 30%;
  filter: blur(2px);
  transition-property: opacity, filter;
  transition-duration: 0.3s;
  transition-timing-function: ease-in-out;
}
.normal_opacity {
  opacity: 100%;
  filter: blur(0);
  transition-property: opacity, filter;
  transition-duration: 0.6s;
  transition-timing-function: ease-in-out;
}
.h_box {
  display: flex;
}
</style>
