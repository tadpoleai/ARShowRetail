<template>
  <div class="app">
    <div ref="canvasRef"></div>
  </div>
</template>
​
<script setup>
import * as THREE from "three";
import { FBXLoader } from "three/examples/jsm/loaders/FBXLoader";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
// 引入gui.js库
import { GUI } from "three/examples/jsm/libs/lil-gui.module.min.js";
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
camera.position.z = 100;

// 渲染器
const renderer = new THREE.WebGLRenderer();
renderer.setClearColor("#f2f2f2");
// renderer.setSize(1000, 500);
renderer.setSize(window.innerWidth, window.innerHeight);

// 光源
const light = new THREE.HemisphereLight("#ffffff", "#000000", 5);
scene.add(light);

// 鼠标控件
new OrbitControls(camera, renderer.domElement);

// gui
const gui = new GUI();

gui.add(light, "intensity", 1, 10).name("灯光"); // change光照
gui.add(camera.position, "z", 50, 800).name("相机距离"); // change相机距离

const obj = {
  url: "/ar/b/Ace_Bee",
};
gui.add(obj, "x", 1, 100);
gui
  .add(obj, "url", {
    鞋: "/ar/b/Ace_Bee",
    包: "/ar/a/735145uulbg9683",
  })
  .name("产品")
  .onChange((val) => {
    loadAsset(val);
  });

let fbxObj;
const loader = new FBXLoader();

function loadAsset(url) {
  loader.load(url + ".fbx", (fbx) => {
    if (fbxObj) {
      scene.remove(fbxObj);
    }
    scene.add(fbx);
    fbxObj = fbx;
  });
}

// 初次执行
loadAsset('/ar/b/Ace_Bee')

function render() {
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
}
</style>
