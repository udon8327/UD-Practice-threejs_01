<template lang="pug">
#car
  #stats
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

  // 创建场景
  const scene = new THREE.Scene();

  // 创建相机
  const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
  camera.position.set(0, 1, 5);

  // 创建渲染器
  const renderer = new THREE.WebGLRenderer({ antialias: true });
  renderer.setSize(window.innerWidth, window.innerHeight);
  document.body.appendChild(renderer.domElement);

  // 添加光源
  const light = new THREE.DirectionalLight(0xffffff, 1);
  light.position.set(5, 5, 5).normalize();
  scene.add(light);

  const ambientLight = new THREE.AmbientLight(0x404040);
  scene.add(ambientLight);

  // 创建控制器
  const controls = new OrbitControls(camera, renderer.domElement);
  controls.target.set(0, 0.5, 0);
  controls.update();

  // 加载 glTF 模型
  const loader = new GLTFLoader();
  loader.load('/3d-models/lamborghini/scene.gltf', function (gltf) {
    scene.add(gltf.scene);
  }, undefined, function (error) {
    console.error(error);
  });

  // 处理窗口大小调整
  window.addEventListener('resize', function () {
    camera.aspect = window.innerWidth / window.innerHeight;
    camera.updateProjectionMatrix();
    renderer.setSize(window.innerWidth, window.innerHeight);
  });

  // 动画循环
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
#stats
  position: absolute
  left: 0
  top: 0
</style>
