<template>
  <div id="container">
    <v-stage ref="v_stage" :width="width" :height="height">
      <v-layer ref="v_layer" style="display: flex; justify-content: center;">
        <v-image ref="v_image" v-if="image" @dragstart="handleDragStart" @dragend="handleDragEnd"
          :config="{ draggable: true, x: (width - image_width) / 2, y: (height - image_height) / 2, width: image_width, height: image_height, image: image }"
          @mousemove="position" />
      </v-layer>
    </v-stage>
  </div>
</template>
<script setup lang="ts">
import { onMounted, ref, watch } from "vue";
const props = defineProps<{ width: number; height: number; image: HTMLImageElement }>();
let hoverTimer: any = null;
const v_stage = ref<any>(null);
const v_image = ref<any>(null);
const v_layer = ref<any>(null);
let image_scale = ref(1);
let image_width = ref(0);
let image_height = ref(0);
watch(
  () => props.image,
  (newVal) => {
    if (newVal instanceof HTMLImageElement) {
      const { width: imgWidth, height: imgHeight } = newVal;
      if (imgHeight > props.height || imgWidth > props.width) {
        image_scale.value = Math.min(props.height / imgHeight, props.width / imgWidth);
      } else {
        image_scale.value = 1;
      }
      image_width.value = imgWidth * image_scale.value;
      image_height.value = imgHeight * image_scale.value;
      console.log(`${image_width.value}, ${image_height.value}`);
    } else {
      console.warn("image.value is not HTMLImageElement");
    }
  }
);
const position = () => {
  if (hoverTimer) {
    clearTimeout(hoverTimer);
  }
  hoverTimer = setTimeout(function () {
    const mousePos = v_stage.value.getNode().getPointerPosition();
    const imageRect = v_image.value.getNode().getClientRect();
    // 将画布坐标转换为图片坐标
    const imagePos = v_layer.value.getNode().getAbsoluteTransform().invert().point(mousePos);
    if (imagePos.x < imageRect.x || imagePos.x > imageRect.x + imageRect.width || imagePos.y < imageRect.y || imagePos.y > imageRect.y + imageRect.height) {
      return;
    }
    console.log('Mouse position in image: (' + ((imagePos.x - imageRect.x) / image_scale.value) + ', ' + ((imagePos.y - imageRect.y) / image_scale.value) + ')');
  }, 50);
};

const handleDragStart = () => {

};
const handleDragEnd = () => {
};
</script>
  
<style scoped>
#container {
  margin: 0 auto;
  margin-top: 20px;
  border-radius: 5px;
  box-shadow: 1px -1px 3px 2px #eee;
}

#upload {
  display: flex;
  align-items: center;
}

#uploadBtn input {
  display: none;
}
</style>
  