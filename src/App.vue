<template>
  <h1 class="col-md-6"> Test </h1>

  <Renderer :pointer="{ intersectRecursive: true }" ref="rendererC" antialias
    :orbit-ctrl="{ enableDamping: false, enabled: false }" resize="window">

    <Camera ref="camera" :position="{ z: 5, x: 1, y: 1 }" />

    <Scene ref="sceneC" background="#F2CD5C">
      <AmbientLight color="white" :intensity="0.2" :position="{ z: 400 }" />
      <PointLight ref="pointLightBlue" :position="{ z: propertyY, x: propertyX }" color="blue" :intensity="1" />
      <PointLight :position="{ z: 10, x: 10 }" color="blue" :intensity="0.5" />
      <PointLight :position="{ z: 50, x: -80 }" color="red" :intensity="0.6" />
      <PointLight :position="{ z: -50, x: -50 }" color="white" :intensity="0.3" />
      <PointLight :position="{ z: 80, x: 80 }" color="white" :intensity="0.1" />

      <GltfModel @click="onClick" :position="{x: 1, y:0.3, z:1}" :rotation="{y: modelRotationY}"  ref="model" src="/galva2.gltf">
      </GltfModel>

    </Scene>
  </Renderer>

</template>

<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount, onBeforeMount } from 'vue'
import { Box, Camera, PointLight, Renderer, Scene, Texture, AmbientLight, GltfModel, MeshPublicInterface, RendererPublicInterface, LambertMaterial, } from 'troisjs'
import { Mesh, PerspectiveCamera } from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls';

const rendererC = ref()
const camera = ref()
const meshC = ref()
const model = ref()
const sceneC = ref()
const pointLightBlue = ref()
const propertyY = ref(10)
const propertyX = ref(100)
const modelRotationY = ref(0)

const winSize = () => {
  return {
    width: window.innerWidth,
    height: window.innerHeight
  }
}

const winSizeCheck = () => {
  if (winSize().width <= 768) {
    model.value.position.z = 1.5
    model.value.position.x = 1.2
    model.value.rotation.y = -0.7
  }
}

const onClick = (event: any) => {
    // console.log(event);
    modelRotationY.value += 0.1
  }

onMounted(() => {

  // winSizeCheck()
  // model.value.position.z = 1
  // model.value.position.x = 1.2
  // model.value.position.y = 0.3
  // model.value.rotation.y = -0.4
  //get win size with vue3



//create onclikc event for model
  

    // console.log('click');
    
    // propertyY.value += 100
  




  //if winsize equal or lower than 768px then set camera position to 0,0,0





  // console.log(meshC.value);

  // model.value.geometry.center()
  // model.value.scene.position.x = 2
  // console.log(model.value);


  // model.value?.setRotationFromQuaternion(new Quaternion(10, 0, 10, 1))
  // const modelC = model.value as ConvexGeometry
  // modelC.convex


  const renderer = rendererC.value as RendererPublicInterface
  renderer.onBeforeRender(() => {
// modelRotationY.value += 0.01  
  })
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
