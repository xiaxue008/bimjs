<script setup>
import { ref } from "vue";
import * as THREE from "three";
import { onMounted } from "vue";
// 引入轨道控制器扩展库OrbitControls.js
import { OrbitControls } from "three/addons/controls/OrbitControls.js";
import { DRACOLoader } from "three/examples/jsm/loaders/DRACOLoader.js";
import { GLTFLoader } from "three/addons/loaders/GLTFLoader.js";
// 定义变量
let scene, camera, renderer;
let axesHelper;
let controls;

window.addEventListener("resize", function () {
  camera.aspect = window.innerWidth / window.innerHeight;
  camera.updateProjectionMatrix();
  renderer.setSize(window.innerWidth, window.innerHeight);
});
function initScene() {
  scene = new THREE.Scene();
  scene.background = new THREE.Color(0xededed);
}
function initAxesHelper() {
  axesHelper = new THREE.AxesHelper(5);
  scene.add(axesHelper);
}
function initLight() {
  const pointLight = new THREE.AmbientLight(0xffffff, 1.0);
  pointLight.position.set(50, 50, 50);
  const pointLightHelper = new THREE.PointLightHelper(pointLight, 0);
  scene.add(pointLightHelper);
  scene.add(pointLight);
}
function initMesh() {}
function initCamera() {
  camera = new THREE.PerspectiveCamera(
    45,
    window.innerWidth / window.innerHeight,
    0.1,
    500
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
  // 初始化控制器后,设置相机的最大最小缩放
  controls.minDistance = 1; // 相机的最小距离----也就是放大
  controls.maxDistance = 200; // 相机的最大距离----也就是缩小
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
  loader.load("../../static/gltf/test.glb", function (gltf) {
    var model = gltf.scene;
    scene.add(model);
    console.log(gltf);

    // 计算模型的边界盒
    var boundingBox = new THREE.Box3().setFromObject(model);

    // 获取模型的尺寸
    var size = boundingBox.getSize(new THREE.Vector3());
    var center = boundingBox.getCenter(new THREE.Vector3());

    // 假设你希望模型的最大尺寸为1个单位
    var maxDimension = Math.max(size.x, size.y, size.z);
    var desiredMaxSize = 50; // 假设的最大尺寸
    var scaleRatio = desiredMaxSize / maxDimension;

    // 应用缩放变换
    model.scale.multiplyScalar(scaleRatio);

    // 调整模型位置，使其位于原点
    // 计算模型当前的中心偏移量，并将模型移动到场景的中心
    var displacement = center.multiplyScalar(-1);
    displacement.multiplyScalar(scaleRatio); // 保证位移量与缩放比例一致
    model.position.add(displacement);
  });
});
</script>

<template>
  <div id="container"></div>
</template>

<style scoped>
</style>
