<script setup>
import { Canvas, Rect, Image, IText } from 'fabric';
import { onMounted, shallowRef, ref } from 'vue';
import { saveAs } from "file-saver";


const canvas = shallowRef();
onMounted(
  () => {
    canvas.value = new Canvas('canvas');
  }
)

const handleLoadImg = async () => {
  const arrFileHandle = await window.showOpenFilePicker({
    types: [{
      accept: {
        'image/*': ['.png', '.gif', '.jpeg', '.jpg', '.webp']
      }
    }],
    multiple: true
  });
  for (const fileHandle of arrFileHandle) {
    // 获取文件内容
    const fileData = await fileHandle.getFile();
    // 读文件数据
    const buffer = await fileData.arrayBuffer();
    // 转成Blod url地址
    let src = URL.createObjectURL(new Blob([buffer]));
    // 加载图片
    Image.fromURL(src).then(
      (img) => {
        canvas.value.add(img);
      }
    )
  }
}

const handleInsertText = () => {
  const itext = new IText('实例文字');
  canvas.value.add(itext);
}


const handleExportImg = () => {
  saveAs(canvas.value.toDataURL('png'), "image.png");
}

</script>

<template>
  <div>
    <button @click="handleLoadImg">加载图片</button>
    <button @click="handleInsertText">加载文字</button>
    <button @click="handleExportImg">导出图片</button>
    <canvas id="canvas" width="375" height="750" style="border: 1px solid #ccc;"></canvas>
  </div>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
