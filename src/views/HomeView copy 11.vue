<template>
    <canvas ref="container"></canvas>
</template>

<script setup>
import { onMounted, ref } from 'vue'
import * as THREE from 'three'

const container = ref(null)

onMounted(() => {
    // 创建场景
    const scene = new THREE.Scene()

    // 添加光源
    const directionalLight = new THREE.DirectionalLight(0xffffff, 1)
    scene.add(directionalLight)

    // 创建相机
    const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000)
    camera.position.z = 5

    // 创建渲染器
    const renderer = new THREE.WebGLRenderer()
    renderer.setSize(window.innerWidth, window.innerHeight)
    container.value.appendChild(renderer.domElement)

    // 加载纹理
    const textureLoader = new THREE.TextureLoader()
    const texture = textureLoader.load('/public/earth.jpg')

    // 创建几何体
    const geometry = new THREE.SphereBufferGeometry(1, 32, 32)

    // 创建材质并应用纹理
    const material = new THREE.MeshBasicMaterial({ map: texture })

    // 创建网格并添加到场景
    const mesh = new THREE.Mesh(geometry, material)
    scene.add(mesh)

    // 渲染函数
    function animate () {
        mesh.rotation.y += 0.01
        requestAnimationFrame(animate)
        renderer.render(scene, camera)
    }
    animate()
})
</script>

<style>
/* 确保容器占满整个窗口 */
div {
    width: 100vw;
    height: 100vh;
    overflow: hidden;
}
</style>
