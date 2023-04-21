<template>
  <div id="container">
    <v-stage ref="v_stage" :width="width" :height="height">
      <v-layer ref="v_layer" style="display: flex; justify-content: center;">
        <v-image ref="v_image" v-if="image" @dragstart="handleDragStart" @dragend="handleDragEnd" :config="image_config"
          @mousemove="position" @wheel="resize" />
        <v-line v-for="label in labels"
          :config="{ points: label.points, stroke: label.color, closed: true, draggable: true, }" />
      </v-layer>
    </v-stage>
  </div>
</template>
<script setup lang="ts">
import { onMounted, ref, watch } from "vue";
const props = defineProps<{ width: number; height: number; image: HTMLImageElement | null; labels: any }>();
let hoverTimer: any = null;
const v_stage = ref<any>(null);
const v_image = ref<any>(null);
const v_layer = ref<any>(null);
let image_config = ref({ draggable: true, x: 0, y: 0, width: 0, height: 0, image: new Image(), scaleX: 1, scaleY: 1 });

watch(
  () => props.image,
  (newVal) => {
    if (newVal instanceof HTMLImageElement) {
      const { width: imgWidth, height: imgHeight } = newVal;
      if (imgHeight > props.height || imgWidth > props.width) {
        let image_scale = Math.min(props.height / imgHeight, props.width / imgWidth);
        image_config.value = { draggable: true, x: (props.width - imgWidth * image_scale) / 2, y: (props.height - imgHeight * image_scale) / 2, width: imgWidth, height: imgHeight, image: newVal, scaleX: image_scale, scaleY: image_scale };
      } else {
        image_config.value = { draggable: true, x: (props.width - imgWidth) / 2, y: (props.height - imgHeight) / 2, width: imgWidth, height: imgHeight, image: newVal, scaleX: 1, scaleY: 1 };
      }
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
    console.log('Mouse position in image: (' + ((imagePos.x - imageRect.x) / image_config.value.scaleX) + ', ' + ((imagePos.y - imageRect.y) / image_config.value.scaleY) + ')');
  }, 50);
};
const resize = (e: any) => {
  e.evt.preventDefault();
  const scaleBy = 1.01;
  if (e.evt.deltaY > 0) {
    image_config.value.scaleX /= scaleBy;
    image_config.value.scaleY /= scaleBy;
  } else {
    image_config.value.scaleX *= scaleBy;
    image_config.value.scaleY *= scaleBy;
  }
}
const handleDragStart = () => {
  console.log(v_image.value.getNode())

};
const handleDragEnd = () => {
  console
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
  