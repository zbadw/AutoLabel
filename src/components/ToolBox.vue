<template>
    标注工具
    <div id="toolbox">
    <button>多边形</button>
    <button>矩形</button>
    <button>自动识别</button>
    </div>
    
</template>

<script setup lang="ts">
import { ref } from 'vue'

defineProps<{ width: number, height: number }>()
</script>

<style scoped>
#toolbox {
    display: flex;
    align-items: center;
}
</style>