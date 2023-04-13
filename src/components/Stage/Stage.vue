<template>
  <!-- 上传图片 -->
  <div id="upload">
    <input type="file" id="file" accept="image/*" @change="upload" />
  </div>
  <div id="container"></div>
</template>
  <script setup lang="ts">
import { useAttrs, onMounted, ref, defineEmits ,onBeforeUpdate, watch} from "vue";

const props = defineProps<{ width: number; height: number; labels: any }>();
var hoverTimer: any = null;
var Stage: Konva.Stage = ref();
const Layer = ref(new Konva.Layer());
var image = ref();
var polygon = ref();
const emits = defineEmits(["polygonclick"]);
Layer.value.on("mousemove", function (event: any) {
  if (hoverTimer) {
    polygon.destroy();
    Layer.value.batchDraw();
    clearTimeout(hoverTimer);
  }
  hoverTimer = setTimeout(function () {
    var mousePos = Stage.value.getPointerPosition();
    var imageRect = image.getClientRect();
    // 将画布坐标转换为图片坐标
    var imagePos = Layer.value.getAbsoluteTransform().invert().point(mousePos);
    imagePos.x -= imageRect.x;
    imagePos.y -= imageRect.y;
    if (
      !(
        imagePos.x < 0 ||
        imagePos.x > imageRect.width ||
        imagePos.y < 0 ||
        imagePos.y > imageRect.height
      )
    ) {
      console.log(
        "Mouse position in image: (" + imagePos.x + ", " + imagePos.y + ")"
      );
      axios
        .post("http://127.0.0.1:5000/setpoint", {
          x: imagePos.x,
          y: imagePos.y,
        })
        .then(function (response: any) {
          console.log(response);
          polygon = new Konva.Line({
            points: response.data["counter"],
            stroke: "green",
            fill: "red",
            strokeWidth: 2,
            lineJoin: "round",
            closed: true,
            opacity: 0.3,
          });
        //   按空格键触发事件
          polygon.on("click", function (e) {
        console.log(e);
            emits("polygonclick", new Konva.Line({
              points: response.data["counter"],
              stroke: "green",
              fill: "red",
              strokeWidth: 2,
              lineJoin: "round",
              closed: true,
              opacity: 0.3,
            }));
          });
          Layer.value.add(polygon);
          image.moveToTop();
          for (let i = 0; i < props.labels.length; i++) {
            props.labels[i].moveToTop();
          }
          polygon.moveToTop();
          Layer.value.batchDraw();
        });
    }
  }, 100);
});
onMounted(() => {
  Stage = ref(
    new Konva.Stage({
      container: "container",
      width: props.width,
      height: props.height,
    })
  );
  Stage.value.add(Layer.value);

});
watch(props.labels, (newVal, oldVal) => {
    console.log(newVal);
    newVal.forEach((label: any) => {
        // 如果不在Layer中，添加
        if (!Layer.value.find(".polygon").includes(label)) {
            Layer.value.add(label);
        }
        
    });
    Layer.value.draw();
});
const upload = () => {
  const input = document.getElementById("file");
  const file = input.files[0];
  const reader = new FileReader();
  reader.onload = function (e) {
    const img = new Image();
    img.src = e.target.result;
    img.onload = function () {
      image = new Konva.Image({
        x: 0,
        y: 0,
        image: img,
        width: img.width,
        height: img.height,
      });
      console.log(Layer);
      Layer.value.add(image);
      Layer.value.draw();
    };
  };
  reader.readAsDataURL(file);
};
</script>
  
  <style scoped>
#container {
  border: 1px solid black;
}
#upload {
  display: flex;
  align-items: center;
}
#uploadBtn input {
  display: none;
}
</style>
  