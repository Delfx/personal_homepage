<template>


    <h1 class="loading vh-100 d-flex align-items-center justify-content-center">
      Loading...
    </h1>












  <Transition v-if="show" name="fadePresentation" appear>
    <div class="presentation col-12 col-md-12 col-xl-9">
      <Presentation />
    </div>
  </Transition>

  <Renderer :pointer="{ intersectRecursive: true }" ref="rendererC" antialias
    :orbit-ctrl="{ enableDamping: true, enabled: true }" resize="window">

    <Camera ref="camera" :position="{ z: 5, x: 1, y: 1 }" />

    <Scene ref="sceneC" background="#F2CD5C">
      <AmbientLight color="white" :intensity="0.2" :position="{ z: 400 }" />
      <PointLight :position="{ z: 10, x: 100 }" :color="lightColorRight" :intensity="1" />
      <PointLight :position="{ z: 10, x: 10 }" color="blue" :intensity="0.5" />
      <PointLight :position="{ z: 50, x: -80 }" :color="lightColorLeft" :intensity="0.6" />
      <PointLight :position="{ z: -50, x: -50 }" color="white" :intensity="0.3" />
      <PointLight :position="{ z: 80, x: 80 }" color="white" :intensity="0.1" />

      <GltfModel @click="changeColorOnClick" :position="{ x: 1.3, y: 0.4, z: 1 }" :rotation="{ y: modelRotationY }"
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
const model = ref()
const sceneC = ref()
const modelRotationY = ref(5)
const lightColorRight = ref('blue');
const lightColorLeft = ref('red');
const show = ref(false)


const winSize = () => {
  return {
    width: window.innerWidth,
    height: window.innerHeight
  }
}






const randomColor = () => {
  return '#' + Math.floor(Math.random() * 16777215).toString(16);
}

const changeColorOnClick = (event: any) => {
  lightColorRight.value = randomColor();
  lightColorLeft.value = randomColor();
  // modelRotationY.value = 5.5}
}

const opacityChange = () => {
  let opacity = 0
  const renderer = rendererC.value as RendererPublicInterface

  renderer.onBeforeRender(() => {
    if (opacity < 1) {
      opacity += 0.01
      console.log(opacity);

    }
  })
}


const onReady = () => {
  console.log('ready');
  // opacityChange()
}


onMounted(() => {
  let speed = 0.002
  const renderer = rendererC.value as RendererPublicInterface
  const scene = model.value as LoadingManager


  scene.onLoad = function () {

    console.log('Loading complete!');
    show.value = true

    //modify css fade2 property new property opacity: 0
    const loading = document.querySelector('.loading')
    loading?.classList.add('fadeOut')
    // create timeout to remove element from DOM
    setTimeout(() => {
      loading?.classList.add('d-none')
    }, 400)
  };

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
      // modelRotationY.value = 0
      if (speed > 0.002) {
        speed -= 0.0001
      }
      modelRotationY.value += speed
    }

  })
})

</script>

<style>
body,
html {
  /* margin: 0; */
  width: 70%;
  /* height: 100%; */
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

.fadePresentation-enter-active,
.fadePresentation-leave-active {
  transition: opacity 2.5s ease;
}

.fadePresentation-enter-from,
.fadePresentation-leave-to {
  opacity: 0;
}

.loadingFade-enter-active,
.loadingFade-leave-active {
  transition: opacity 1.5s ease;
}

.loadingFade-enter-from,
.loadingFade-leave-to {
  opacity: 0;
}

/* create opacity animation on page load for fade2 class */

.fadeOut {
  opacity: 0;
  animation: fadeOut 0.3s ease-in-out;
  /* display: none; */
  /* visibility: hidden; */

}

@keyframes fadeOut {
  0% {
    opacity: 1;
  }

  100% {
    opacity: 0;
  }
}

.fadeId {
  opacity: 1;
  animation: fadeId 0.5s ease-in-out;
}

@keyframes fadeId {
  0% {
    opacity: 0;
  }

  100% {
    opacity: 1;
  }
}
</style>
