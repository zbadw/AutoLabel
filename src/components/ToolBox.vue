<template>
    标注工具
    <div id="toolbox">
    <button @click="stateChange('polygon')">多边形</button>
    <button @click="stateChange('rectangle')">矩形</button>
    <button @click="stateChange('auto')">自动识别</button>
    </div>
    
</template>

<script setup lang="ts">
import { ref } from 'vue'

const props = defineProps<{ width: number, height: number }>()
const emit = defineEmits(['stateClick'])
const stateChange = (state:string) => {
    console.log(state)
    emit('stateClick', state)
}
</script>

<style scoped>
#toolbox {
    display: flex;
    align-items: center;
}
</style>