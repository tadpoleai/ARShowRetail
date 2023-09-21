<template>
  <div class="app">
    <div ref="canvasRef"></div>
  </div>
</template>
​
<script setup>
import * as THREE from "three";
import { FBXLoader } from "three/examples/jsm/loaders/FBXLoader";
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader";
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
const renderer = new THREE.WebGLRenderer({antinalias:true});

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
//gui.add(camera.position, "z", 50, 800).name("相机距离"); // change相机距离

const obj = {
  url: "/assets/xie",
};
//gui.add(obj, "x", 1, 100);
gui
  .add(obj, "url", {
    鞋: "/assets/xie",
    包: "/assets/bao",
  })
  .name("产品")
  .onChange((val) => {
    loadAsset(val);
  });

let fbxObj;
//const loader = new FBXLoader();
const loader = new GLTFLoader();


let modelObj;
function loadAsset(url) {
  loader.load(url + ".glb", (gltf) => {
    const model = gltf.scene;
    if (modelObj) {
      scene.remove(modelObj);
    }
    scene.add(model);
    modelObj = model;
  }, undefined, (error) => {
    console.error("An error occurred while loading the glTF model:", error);
  });
}

// 初次执行
loadAsset('/assets/xie')

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
