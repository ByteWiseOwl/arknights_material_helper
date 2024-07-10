<script setup>
    import { defineProps, inject } from 'vue';

    const {material, values, typ} = defineProps(['material', 'values', 'typ'])

    const highlightedComponents = inject('highlightedComponents');
    const hoveredTier = inject('hoveredTier');
    const materials = inject('materials');
    const combinet_materials = materials.tier1.concat(materials.tier2).concat(materials.tier3);

    function getComponents(name) {
        return combinet_materials.find(material => material.name == name).components;
    }

    function highlightComponents(material) {
        highlightedComponents.value = getComponents(material.name);
        hoveredTier.value = material.tier;
    }

    function clearHighlights() {
        highlightedComponents.value = [];
        hoveredTier.value = 0;
    }
    
    function getBgColorByTier(tier) {
        switch (tier) {
            case 1: return "rgba(255, 174, 0, 0.9)";
            case 2: return "rgba(219, 177, 219, 0.9)";
            case 3: return "rgba(0, 178, 246, 0.9)";
            default: return "rgba(150, 150, 150, 0.9)";
        }
    }
</script>

<template>
    <img v-if="typ == 'image'"
        :src="`../src/assets/materials/${material.name}.png`"
        :alt="material.name" 
        :style="{ backgroundColor: getBgColorByTier(material.tier) }"
        @mouseover="highlightComponents(material)"
        @mouseleave="clearHighlights()"
        :class="{ highlighted: highlightedComponents.some(comp => comp === material.name) }"
    >
    <p v-if="typ == 'value'"
        @mouseover="highlightComponents(material)"
        @mouseleave="clearHighlights()"
        :class="{ highlighted: highlightedComponents.some(comp => comp === material.name) }"
    ></p>
</template>

<style scoped>
    img {
        height: 50px;
        width: 50px;
        border-radius: 10px;
        padding: 2px;
        margin: 2px;
        display: flex;
        align-items: center;
    }
    img:hover {
        box-shadow: 4px -4px 10px rgb(255, 255, 255), -4px 4px 10px rgb(255, 255, 255);
    }
    .highlighted {
        box-shadow: 4px -4px 10px rgb(255, 255, 255), -4px 4px 10px rgb(255, 255, 255);
    }
</style>