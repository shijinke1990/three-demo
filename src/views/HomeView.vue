<template>
    <canvas ref="canvasRef"></canvas>

</template>

<script setup>
import { onMounted, ref } from 'vue'
import * as THREE from 'three'

import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls'

const canvasRef = ref(null)


onMounted(() => {
    const scene = new THREE.Scene()
    // 创建相机
    const camera = new THREE.PerspectiveCamera(5, window.innerWidth / window.innerHeight, 0.1, 1000)
    camera.position.z = 50

    // 创建渲染器
    const renderer = new THREE.WebGLRenderer({
        canvas: canvasRef.value
    })
    renderer.setSize(window.innerWidth, window.innerHeight)

    // 添加灯光
    const ambientLight = new THREE.AmbientLight(0xffffff, 0.5)
    scene.add(ambientLight)

    const directionalLight = new THREE.DirectionalLight(0xffffff, 1)
    directionalLight.position.set(5, 5, 5).normalize()
    scene.add(directionalLight)

    // 加载纹理


    const textureLoader = new THREE.TextureLoader()
    // 2.
    const texture = textureLoader.load('/public/test.webp')

    texture.repeat.x = 1
    texture.repeat.y = 1
    texture.offset.x = -0.1
    texture.offset.y = -0.1
    // texture.wrapS = THREE.RepeatWrapping
    // texture.wrapT = THREE.RepeatWrapping
    texture.wrapS = THREE.MirroredRepeatWrapping
    texture.wrapT = THREE.MirroredRepeatWrapping

    const boxGeometry = new THREE.BoxGeometry(2, 2, 2)
    const boxMaterial = new THREE.MeshStandardMaterial({
        color: 0xffffff,
        map: texture, // 2.重点位置
    })
    const boxMesh = new THREE.Mesh(boxGeometry, boxMaterial)
    scene.add(boxMesh)



    function animate () {
        texture.offset.x += 0.001
        // boxMesh.rotation.y += 0.01
        requestAnimationFrame(animate)
        renderer.render(scene, camera)
    }
    animate()

    // 添加轨道控制
    const controls = new OrbitControls(camera, renderer.domElement)

    // 处理窗口大小调整
    window.addEventListener('resize', () => {
        camera.aspect = window.innerWidth / window.innerHeight
        camera.updateProjectionMatrix()
        renderer.setSize(window.innerWidth, window.innerHeight)
    })
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
