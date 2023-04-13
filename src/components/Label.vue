<template>
<div v-for="label in labels">
  {{ label.id }}
</div>  
</template>

<script setup lang="ts">
import { defineProps,ref } from 'vue'
const props = defineProps<{ labels: any }>()
</script>