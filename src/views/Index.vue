<template lang="pug">
#index
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { useRouter } from 'vue-router';
import * as THREE from 'three';
const router = useRouter();

onMounted(() => {
  //创建场景和相机
  var scene = new THREE.Scene();
  var camera = new THREE.PerspectiveCamera(
    75,
    window.innerWidth / window.innerHeight,
    0.1,
    1000
  );

  //创建渲染器，设置尺寸为窗口尺寸，并将渲染后的元素添加到body
  var renderer = new THREE.WebGLRenderer();
  renderer.setSize(window.innerWidth, window.innerHeight);
  document.body.appendChild(renderer.domElement);

  //创建一个Mesh（绿色的3D立方体），并添加到场景中
  var geometry = new THREE.BoxGeometry();
  var material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
  var cube = new THREE.Mesh(geometry, material);
  scene.add(cube);

  //设置照相机的位置
  camera.position.z = 5;

  //浏览器每次渲染的时候更新立方体的旋转角度
  var animate = function () {
    requestAnimationFrame(animate);

    cube.rotation.x += 0.01;
    cube.rotation.y += 0.01;

    renderer.render(scene, camera);
  };

  animate();
})
</script>

<style lang="sass" scoped>
</style>
