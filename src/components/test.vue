<script setup>
import { ref } from "vue";
import * as THREE from "three";
import { onMounted } from "vue";
// 引入轨道控制器扩展库OrbitControls.js
import { OrbitControls } from "three/addons/controls/OrbitControls.js";
import { GLTFLoader } from "three/addons/loaders/GLTFLoader.js";
// 定义变量
let scene, camera, renderer;
let axesHelper;
let hesLight, dirLight, sportLight;
let controls;

window.addEventListener("resize", function () {
  camera.aspect = window.innerWidth / window.innerHeight;
  camera.updateProjectionMatrix();
  renderer.setSize(window.innerWidth, window.innerHeight);
});
function initScene() {
  scene = new THREE.Scene();
  scene.background = new THREE.Color(0xa0a0a0);
}
function initAxesHelper() {
  axesHelper = new THREE.AxesHelper(5);
  scene.add(axesHelper);
}
function initLight() {
  const pointLight = new THREE.PointLight(0xffffff, 1.0);
  pointLight.position.set(500, 500, 500);
  const pointLightHelper = new THREE.PointLightHelper(pointLight, 10);
  scene.add(pointLightHelper);
  scene.add(pointLight);
}
function initMesh() {}
function initCamera() {
  camera = new THREE.PerspectiveCamera(
    45,
    window.innerWidth / window.innerHeight,
    0.1,
    100
  );
  camera.position.set(50, 50, 50);
}
function initRenderer() {
  renderer = new THREE.WebGLRenderer({ antialias: true });
  renderer.setPixelRatio(window.devicePixelRatio);
  renderer.setSize(window.innerWidth, window.innerHeight);
  document.body.appendChild(renderer.domElement);
}
function initControls() {
  controls = new OrbitControls(camera, renderer.domElement);
}
function animate() {
  requestAnimationFrame(animate);
  renderer.render(scene, camera);
}
onMounted(() => {
  // 初始化场景
  initScene();
  // 初始化辅助轴
  initAxesHelper();
  // 初始化灯光
  initLight();
  // 初始化mesh
  initMesh();
  // 初始化相机
  initCamera();
  // 初始化渲染器
  initRenderer();
  // 循环动画
  animate();
  // 初始化轨道控制器
  initControls();
  const loader = new GLTFLoader();
  loader.load("../../static/gltf/bangonglou.gltf", function (gltf) {
    console.log(gltf);
    // scene.add(gltf.scene);
    var model = gltf.scene;
    // var boundingBox = new THREE.Box3().setFromObject(model);
    // var size = boundingBox.getSize(new THREE.Vector3());
    // // var maxDimension = Math.max(size.x, size.y, size.z);

    // // 根据需求设置缩放比例
    // var scaleRatio = 1; // 这里示意缩小为原始大小的一半

    // // 应用缩放变换
    // model.scale.multiplyScalar(scaleRatio);

    // 将模型添加到场景中
    scene.add(model);
  });
  renderer.outputEncoding = THREE.sRGBEncoding;
});
</script>

<template>
  <div id="container"></div>
</template>

<style scoped>
</style>
