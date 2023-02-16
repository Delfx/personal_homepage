<template>
  <div class="presentation col-12 col-md-12 col-xl-9">
    <Presentation class="" />
  </div>

  <Renderer :pointer="{ intersectRecursive: true }" ref="rendererC" antialias
    :orbit-ctrl="{ enableDamping: true, enabled: false }" resize="window">

    <Camera ref="camera" :position="{ z: 5, x: 1, y: 1 }" />

    <Scene ref="sceneC" background="#F2CD5C">
      <AmbientLight color="white" :intensity="0.2" :position="{ z: 400 }" />
      <PointLight :position="{ z: 10, x: 100 }" :color="lightColorRight" :intensity="1" />
      <PointLight :position="{ z: 10, x: 10 }" color="blue" :intensity="0.5" />
      <PointLight :position="{ z: 50, x: -80 }" :color="lightColorLeft" :intensity="0.6" />
      <PointLight :position="{ z: -50, x: -50 }" color="white" :intensity="0.3" />
      <PointLight :position="{ z: 80, x: 80 }" color="white" :intensity="0.1" />

      <GltfModel @click="changeColorOnClick" :position="{ x: 1, y: 0.4, z: 1 }" :rotation="{ y: modelRotationY }"
        ref="model" src="/galva2.gltf">
      </GltfModel>

    </Scene>
</Renderer>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
import { Camera, PointLight, Renderer, Scene, AmbientLight, GltfModel, RendererPublicInterface, } from 'troisjs'
import Presentation from './components/Presentation.vue'
import { LoadingManager } from 'three';
// import { Scene } from 'three';


const rendererC = ref()
const camera = ref()
const meshC = ref()
const model = ref()
const sceneC = ref()
const modelRotationY = ref(0)
const lightColorRight = ref('blue');
const lightColorLeft = ref('red');

const winSize = () => {
  return {
    width: window.innerWidth,
    height: window.innerHeight
  }
}

//create to check if page loaded



//create random color generator
const randomColor = () => {
  return '#' + Math.floor(Math.random() * 16777215).toString(16);
}

const winSizeCheck = () => {
  if (winSize().width <= 768) {
    model.value.position.z = 1.5
    model.value.position.x = 1.2
    model.value.rotation.y = -0.6
  }
}

const changeColorOnClick = (event: any) => {
  // console.log(randomColor());
  lightColorRight.value = randomColor();
  lightColorLeft.value = randomColor();
  modelRotationY.value = 0
  // modelRotationY.value += 0.2
}

const pageLoaded = () => {
  if (document.readyState === 'complete') {

    console.log('ready');


  }
}












onMounted(() => {
  let speed = 0.002
  const renderer = rendererC.value as RendererPublicInterface
  const scene = model.value as LoadingManager

  scene.onProgress = function (url, itemsLoaded, itemsTotal) {

    console.log('Loading file: ' + url + '.\nLoaded ' + itemsLoaded + ' of ' + itemsTotal + ' files.');

  };


  scene.onLoad = function () {

    console.log('Loading complete!');

  };


  //create function gradually increace number from 0 to 6.28







  // console.log(sceneC.value.scene.children.length);

  // convert modelRotationY 2 symbol after dot and covert to number



  renderer.onBeforeRender(() => {
    if (Number(modelRotationY.value.toFixed(2)) == 6.29) {
      modelRotationY.value = 0
      modelRotationY.value += speed
    } else if (Number(modelRotationY.value.toFixed(2)) < 4.38 && Number(modelRotationY.value.toFixed(2)) > 1.5) {
      //gradually increase speed of rotation
      if (speed < 0.006) {
        speed += 0.0001
      }
      modelRotationY.value += speed
    } else {
      //gradually decrease speed of rotation
      if (speed > 0.002) {
        speed -= 0.0001
      }
      modelRotationY.value += speed
    }


  })
})


//create function to check if page loaded




//explain onMounted in vue 3




</script>

<style>
body,
html {
  /* margin: 0; */
  width: 70%;
}


@media screen and (max-width: 768px) {
  body,
  html {
    width: 70%;
  }
}


.presentation {
  margin-left: 0.6rem;
  margin-top: 13.5rem;
}

h1 {
  z-index: 1;
}

canvas {
  width: 100%;
  height: 100%;
  position: fixed;
  top: 0;
  left: 0;
  z-index: -1;
}
</style>
