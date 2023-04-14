<template>
  <!-- 上传图片 -->
  <div id="upload">
    <input type="file" id="file" accept="image/*" @change="upload" />
  </div>
  <div id="container">
    <v-stage :width="width" :height="height">
      <v-layer style="display: flex; justify-content: center;">
        <v-image
          :config="{ x: (width - image_width) / 2, y: (height - image_height) / 2, width: image_width, height: image_height, image: image }" />
      </v-layer>
    </v-stage>
  </div>
</template>
<script setup lang="ts">
import { onMounted, ref, watch } from "vue";
const props = defineProps<{ width: number; height: number }>();
const image = ref<HTMLImageElement | null>(null);
const image_width = ref(0);
const image_height = ref(0);
const scale = ref(1);
const upload = () => {
  const file = document.getElementById("file") as HTMLInputElement;
  const fileObj = file.files?.[0] as Blob;
  const reader = new FileReader();
  reader.readAsDataURL(fileObj);
  reader.onload = function (e) {
    const url = e.target?.result;
    image.value = new Image();
    image.value.src = url as string;
    image.value.onload = function () {
      if (image.value instanceof HTMLImageElement) {
        const { width: imgWidth, height: imgHeight } = image.value;
        if (imgHeight > props.height || imgWidth > props.width) {
          scale.value = Math.min(props.height / imgHeight, props.width / imgWidth);
        } else {
          scale.value = 1;
        }
        image_width.value = imgWidth * scale.value;
        image_height.value = imgHeight * scale.value;
        console.log(`${image_width.value}, ${image_height.value}`);
      }
      else {
        console.warn('image.value is not HTMLImageElement');
      }
    };
  };
};
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
  