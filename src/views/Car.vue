<template lang="pug">
#car
</template>

<script setup>
import Stats from 'stats.js';
import { ref, onMounted } from 'vue'
import { useRouter } from 'vue-router';
import * as THREE from 'three';
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls';
const router = useRouter();

onMounted(() => {
  // 建立FPS監測器
  const stats = new Stats();
  stats.showPanel(0); // 0: fps, 1: ms, 2: memory
  document.body.appendChild(stats.dom);

  // 創建場景
  const scene = new THREE.Scene();

  // 創建坐標線
  const axesHelper = new THREE.AxesHelper(150);
  scene.add(axesHelper);

  // 創建網格地面
  const gridHelper = new THREE.GridHelper(20, 20);
  scene.add(gridHelper);

  // 創建相機
  const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
  camera.position.set(0, 1, 5);

  // 創建渲染器
  const renderer = new THREE.WebGLRenderer({ antialias: true });
  renderer.setSize(window.innerWidth, window.innerHeight);
  document.body.appendChild(renderer.domElement);

  // 添加光源
  const light = new THREE.DirectionalLight(0xffffff, 1);
  light.position.set(5, 5, 5).normalize();
  scene.add(light);

  const ambientLight = new THREE.AmbientLight(0xffffff);
  scene.add(ambientLight);

  // 創建控制器
  const controls = new OrbitControls(camera, renderer.domElement);
  controls.target.set(0, 0.5, 0);
  controls.update();

  // 創建地面
  const geometry = new THREE.BoxGeometry(1000, 0.001, 1000)
  const material = new THREE.MeshBasicMaterial({color: 0x2b2b2b})
  const cube = new THREE.Mesh(geometry, material);
  cube.position.set(0, -0.1, 0);
  scene.add(cube);

  // 加載glTF模型
  const loader = new GLTFLoader();
  loader.load('/3d-models/lamborghini/scene.gltf', function (gltf) {
    const car = gltf.scene;
    car.position.set(0, 0, 0);
    scene.add(car);
  }, undefined, function (error) {
    console.error(error);
  });

  // 處理窗口大小調整
  window.addEventListener('resize', function () {
    camera.aspect = window.innerWidth / window.innerHeight;
    camera.updateProjectionMatrix();
    renderer.setSize(window.innerWidth, window.innerHeight);
  });

  // 動畫循環
  function animate() {
    requestAnimationFrame(animate);
    stats.begin();
    renderer.render(scene, camera);
    stats.end();
  }
  animate();
})
</script>

<style lang="sass" scoped>
</style>
