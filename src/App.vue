<script setup>
import Depot from "@/components/Depot.vue";
import Test from "@/components/Test.vue";
import Test2 from "@/components/Test2.vue";
import Test3 from "@/components/Test3.vue";
import Test4 from "@/components/Test4.vue";

import materials from "@/data/materials.json";
import { ref, provide, watch } from "vue";

const highlightedComponents = ref([]);
const hoveredTier = ref(0);
const stackMaterials = ref(true);
const data = ref({ depot: {}, events: {} });

const hasData = window.localStorage.getItem("data");
if (hasData) {
  data.value = JSON.parse(hasData);
}

provide("highlightedComponents", highlightedComponents);
provide("hoveredTier", hoveredTier);
provide("stackMaterials", stackMaterials);
provide("materials", setIds(materials));
provide("data", data);

// window.localStorage.setItem("mats", JSON.stringify(removeIds(materials)));

watch(
  data,
  (val) => {
    window.localStorage.setItem("data", JSON.stringify(val));
  },
  { deep: true }
);

function setIds(materials) {
  let index = 0;
  Object.keys(materials).forEach((tier) => {
    materials[tier].forEach((item) => {
      item.id = index;
      index++;
    });
  });
  return materials;
}

// function removeIds(materials) {
//   Object.keys(materials).forEach((tier) => {
//     materials[tier].forEach((item) => {
//       delete item.id;
//     });
//   });
//   return materials;
// }

// onBeforeUnmount(() => {
//   window.localStorage.setItem("data", JSON.stringify(data))
// });
</script>

<template>
  <div class="container-main">
    <Depot />
    <!-- <Test /> -->
    <!-- <Test2 /> -->
    <!-- <Test3 /> -->
    <!-- <Test4 /> -->
  </div>
</template>

<style>
.container-main {
  padding: 20px;
  /* display: flex; */
  /* flex-direction: column; */
  /* justify-content: center;
  align-items: center;
  min-height: 100vh; */
}
.no-scroll {
  overflow: hidden;
}
</style>
