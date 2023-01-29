<template>
  <h1 class="col-md-6"> Test </h1>

  <Renderer ref="renderer" antialias
    :orbit-ctrl="{ enableDamping: false, enabled: false, autoRotate: true, autoRotateSpeed: 0.5, }" resize="window">

    <Camera ref="camera" :position="{ z: 5, x: 1, y: 1 }"/>

    <Scene background="#F2CD5C">
      <AmbientLight color="white" :intensity="0.2" :position="{ z: 400 }" />
      <PointLight :position="{ z: 10, x: 100 }" color="blue" :intensity="1" />
      <PointLight :position="{ z: 10, x: 10 }" color="blue" :intensity="0.5" />
      <PointLight :position="{ z: 50, x: -80 }" color="red" :intensity="0.6" />
      <PointLight :position="{ z: -50, x: -80 }" color="white" :intensity="0.3" />

      <GltfModel ref="model" :position="{ x:2 }" src="/galva2.gltf" @load="">
      </GltfModel>
    </Scene>

  </Renderer>

</template>

<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount } from 'vue'
import { Box, Camera, PointLight, Renderer, Scene, Texture, AmbientLight, GltfModel, MeshPublicInterface, RendererPublicInterface, } from 'troisjs'
import { PerspectiveCamera } from 'three';

const renderer = ref()
const camera = ref()
const meshC = ref()
const model = ref()
// camera.value.camera.fov = 20




onMounted(() => {


model.value.position.x = 0
console.log(model.value.position.x);

  // const renderer = rendererC.value as RendererPublicInterface
  // const mesh = (meshC.value as MeshPublicInterface).mesh
  // renderer.onBeforeRender(() => {
  //   mesh!.rotation.x += 0.01
  // })
})


</script>

<style>
body,
html {
  margin: 0;
  /* height: 100%; */
}

h1 {
  z-index: 1;
  /* color: white; */
}


canvas {
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  left: 0;
  z-index: -9999;
}
</style>
