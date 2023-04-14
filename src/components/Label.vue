<template>
  <div :key="label.id" v-for="label in labels">
    {{ label.id }}
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
const props = defineProps<{ labels: any }>()
</script>