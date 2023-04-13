<script setup lang="ts">
import { ref } from "vue";
import Label from '@/components/Label.vue';
import ToolBox from '@/components/ToolBox.vue';
import Stage from '@/components/Stage/Stage.vue';
const labels = ref([]);
const addLabel = (label: any) => {
    // labels.value.push(label);
};
</script>

<template>
    <Stage :width="1000" :height="600" @polygonclick="addLabel" :labels="labels" />
    <ToolBox :width="600" :height="12" />
    <Label :labels="labels" />
</template>

<style scoped></style>
