<template>
  <!-- 上传图片 -->
  <div id="upload">
    <input type="file" id="file" accept="image/*" @change="upload" />
  </div>
  <div id="container">
    <v-stage :width="1000" :height="500">
      <v-layer>
        <!-- <v-image x="0" y="0" width="100" height="100" image="https://konvajs.org/assets/lion.png" /> -->
      </v-layer>
    </v-stage>
  </div>
</template>
<script setup lang="ts">
import { onMounted, ref, defineEmits, watch } from "vue";


</script>
  
<style scoped>
#container {
  border: 1px solid #ccc;
  margin: 0 auto;
  margin-top: 20px;
}

#upload {
  display: flex;
  align-items: center;
}

#uploadBtn input {
  display: none;
}
</style>
  