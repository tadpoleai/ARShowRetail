<template>
  <div ref="canvasRef"></div>
</template>
​
<script setup>
import * as THREE from "three";
import { FBXLoader } from "three/examples/jsm/loaders/FBXLoader";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
import { ref, onMounted } from "vue";

const canvasRef = ref();

// 创建场景
const scene = new THREE.Scene();

// 创建相机
const camera = new THREE.PerspectiveCamera(
  30,
  window.innerWidth / window.innerHeight,
  0.1,
  1000
);
camera.position.z = 50;

// 渲染器
const renderer = new THREE.WebGLRenderer();
renderer.setClearColor("#f2f2f2");
// renderer.setSize(1000, 500);
renderer.setSize(window.innerWidth, window.innerHeight);

// 光源
const light = new THREE.HemisphereLight("#ffffff", "#000000", 8);
scene.add(light);

// 鼠标控件
new OrbitControls(camera, renderer.domElement);

// // 加载FBX模型
let mixer = null;
let actions;
const loader = new FBXLoader();
loader.load("/ar/b/Ace_Bee.fbx", (fbx) => {
  scene.add(fbx);
  // 动画
  // mixer = new THREE.AnimationMixer(fbx);
  // for (let i = 0; i < fbx.animations; i++) {
  //   actions[i] = mixer.clipAction(fbx.animations[i]);
  // }
});

// let clock = new THREE.Clock();
function render() {
  if (mixer) {
    mixer.update(clock.getDelta());
  }
  renderer.render(scene, camera);
}
renderer.setAnimationLoop(render);

onMounted(() => {
  canvasRef.value?.appendChild(renderer.domElement);
});
</script>

<style scoped>
.app {
  width: 100%;
  height: 100vh;
  border: 1px solid red;
}
</style>
