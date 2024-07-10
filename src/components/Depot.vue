<script setup>
import { inject, ref } from "vue";
import MaterialContainer from "@/components/MaterialContainer.vue";
import NewEventForm from "@/components/NewEventForm.vue";
import NewOperatorForm from "@/components/NewOperatorForm.vue";

const isNewEventFormOpen = ref(false);
const isNewOperatorFormOpen = ref(false);
const data = inject("data");
const stackMaterials = inject("stackMaterials");

setTitleTextWidth();
function handelNewEvent() {
  isNewEventFormOpen.value = false;
  setTitleTextWidth();
}

function setTitleTextWidth() {
  const targetHeight = 80; // Zielhöhe in Pixeln
  const maxTestWidth = 1000; // Maximale Testbreite für Wrapping

  const temporaryElement = document.createElement("div");
  document.body.appendChild(temporaryElement);
  temporaryElement.style.position = "absolute";
  temporaryElement.style.visibility = "hidden";
  temporaryElement.style.whiteSpace = stackMaterials.value
    ? "normal"
    : "nowrap";

  let maxWidth = 0;

  Object.values(data.value.events).forEach((event) => {
    temporaryElement.textContent = event.name;

    if (stackMaterials.value) {
      // Set height only when stackMaterials.value is true
      temporaryElement.style.height = targetHeight + "px";

      for (let width = 10; width <= maxTestWidth; width += 10) {
        temporaryElement.style.width = width + "px";
        if (temporaryElement.scrollHeight <= targetHeight) {
          maxWidth = Math.max(maxWidth, temporaryElement.scrollWidth);
          break;
        }
      }
    } else {
      // Reset height to auto if stackMaterials.value is false
      temporaryElement.style.height = "auto";
      const elementWidth = temporaryElement.offsetWidth;
      maxWidth = Math.max(maxWidth, elementWidth);
    }
  });

  document.body.removeChild(temporaryElement);

  // Set the CSS variable
  document.documentElement.style.setProperty(
    "--title-text-width",
    maxWidth + 10 + "px"
  );
}
function expendButton(event) {
  event.expended = !event.expended;
  setTitleTextWidth();
}
function stackButton() {
  stackMaterials.value = !stackMaterials.value;
  setTitleTextWidth();
}
function newEventButton() {
  document.body.classList.add("no-scroll");
  isNewEventFormOpen.value = true;
}
function newOperatorButton() {
  document.body.classList.add("no-scroll");
  isNewOperatorFormOpen.value = true;
}
</script>

<template>
  <NewEventForm @newEvent="handelNewEvent()" :open="isNewEventFormOpen" />
  <!-- <NewOperatorForm @newEvent="handelNewOperator()" :open="isNewOperatorFormOpen" /> -->
  <div class="container">
    <div class="button-container">
      <span @click="stackButton()"> {{ stackMaterials ? "🚦" : "🚥" }} </span>
      <span @click="newEventButton()"> ➕ </span>
    </div>
    <div style="display: inline-flex">
      <h1 class="title-text">Depot</h1>
      <div>
        <MaterialContainer typ="image" />
        <MaterialContainer typ="value" :values="data.depot" self="depot" />
      </div>
    </div>
  </div>
  <div class="container" v-for="event in data.events">
    <div class="button-container">
      <span @click="expendButton(event)">
        {{ event.expended ? "🔺" : "🔻" }}
      </span>
      <span @click="newOperatorButton()"> ➕ </span>
    </div>
    <div style="display: inline-flex" v-if="!event.expended">
      <div class="title-text">{{ event.name }}</div>
      <MaterialContainer typ="value" :values="{}" />
    </div>
    <div style="display: inline-flex" v-else>
      <div class="title-text">Rewards</div>
      <MaterialContainer typ="value" :values="event.rewards" :self="event.id" />
    </div>
  </div>
  <p>{{ data }}</p>
</template>
<style>
:root {
  --title-text-width: 250px;
}
</style>
<style scoped>
.title-text {
  /* outline: 1px solid red; */
  width: var(--title-text-width);
  /* width: max-content; */
  margin: 2px;
  margin-top: 10px;
  user-select: none;
}
.container {
  /* outline: 2px solid red; */
  position: relative;
  border: solid;
  border-radius: 5px;
  display: inline-flex;
  padding: 4px;
  margin-bottom: 4px;
}
.button-container {
  cursor: pointer;
  position: absolute;
  top: 0px;
  left: 0px;
}
.button-container span {
  cursor: pointer;
}
</style>
