<template>
    <div class="container">
        <div>
            <div id="upload">
                <input type="file" id="file" accept="image/*" @change="upload" />
            </div>
            <Stage :width="1000" :height="600" @polygonclick="addLabel" :labels="labels" :image="image" />
        </div>
        <div>
            <ToolBox :width="600" :height="12" />
            <Label :labels="labels" />
        </div>

    </div>
</template>
<script setup lang="ts">
import { ref } from "vue";
import Label from '@/components/Label.vue';
import ToolBox from '@/components/ToolBox.vue';
import Stage from '@/components/Stage/Stage.vue';
const labels = ref([]);
const addLabel = (label: any) => {
    // labels.value.push(label);
};

let image = ref<HTMLImageElement | null>(null);
const upload = () => {
    const file = document.getElementById("file") as HTMLInputElement;
    const fileObj = file.files?.[0] as Blob;
    const reader = new FileReader();
    reader.readAsDataURL(fileObj);
    reader.onload = function (e) {
        const url = e.target?.result;
        const tempImage = new Image();
        tempImage.src = url as string;
        tempImage.onload = function () {
            image.value = tempImage;
        };
    };
};
</script>

<style scoped>
.container {
    display: flex;
    position: relative;
    justify-content: space-around;
}
</style>
