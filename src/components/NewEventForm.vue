<script setup>
import { inject, ref } from "vue";

const emit = defineEmits(["newEvent"]);
const { open } = defineProps(["open"]);
const data = inject("data");
const newEventInput = ref("");
const showNewEvent = ref(false);

function createNewEvent(create) {
  document.body.classList.remove("no-scroll");
  if ((newEventInput.value.length > 0) & create) {
    const key = getCurrentTime();
    data.value["events"][key] = {
      id: key,
      name: newEventInput.value,
      rewards: {},
      operators: {},
      expended: false,
    };
  }
  newEventInput.value = "";
  showNewEvent.value = false;
  emit("newEvent");
}

function getCurrentTime() {
  const now = new Date();
  const year = now.getFullYear();
  const month = String(now.getMonth() + 1).padStart(2, "0"); // Monate sind 0-basiert
  const day = String(now.getDate()).padStart(2, "0");
  const hours = String(now.getHours()).padStart(2, "0");
  const minutes = String(now.getMinutes()).padStart(2, "0");
  const seconds = String(now.getSeconds()).padStart(2, "0");
  const milliseconds = String(now.getMilliseconds()).padStart(3, "0");

  // Format: YYYYMMDDHHMMSSMMM
  return `${year}${month}${day}${hours}${minutes}${seconds}${milliseconds}`;
}

function focusInput() {
  document.getElementById("new_event_input").focus();
}
</script>

<template>
  <div
    class="form_container"
    id="new_event_bg"
    :open="open"
    @click.self="createNewEvent(false)"
    @mouseenter="focusInput()"
  >
    <input
      id="new_event_input"
      v-model.trim="newEventInput"
      type="text"
      placeholder="event name..."
    />
    <div @click="createNewEvent(true)">Create</div>
  </div>
</template>

<style scoped>
.form_container {
  justify-content: center;
  align-items: center;
  gap: 5px;
  z-index: 1;
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 150vw;
  height: 150vh;
  background-color: rgba(0, 0, 0, 0.7);
  font-size: 2rem;

  display: none;
  opacity: 0;
  translate: 25vw 0;
  transition-property: display, opacity, translate;
  transition-duration: 0.3s;
  transition-behavior: allow-discrete;
}

.form_container[open="true"] {
  display: flex;
  opacity: 1;
  translate: 0 0;

  @starting-style {
    opacity: 0;
    translate: 0 -25vh;
  }
}

.form_container input {
  width: 40%;
  height: 50px;
  font-size: inherit;
}

.form_container div {
  width: 10%;
  height: 50px;
  border: 0.1em solid;
  font-size: 0.8em;
  text-align: center;
  background-color: #222;
  cursor: pointer;
}
</style>
