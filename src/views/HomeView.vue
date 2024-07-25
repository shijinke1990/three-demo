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
    const camera = new THREE.PerspectiveCamera(20, window.innerWidth / window.innerHeight, 0.1, 1000)
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

    const textureLoader = new THREE.TextureLoader()
    const texture = textureLoader.load('/public/test.webp')

    // texture.repeat.x = 1
    // texture.repeat.y = 1
    // texture.repeat.set(1.1, 1.1) // 将纹理放大两倍
    // texture.offset.x = -0.1
    // texture.offset.y = -0.1
    texture.wrapS = THREE.RepeatWrapping
    texture.wrapT = THREE.RepeatWrapping
    texture.offset.x += 0.1
    texture.offset.y += 0.1
    // texture.wrapS = THREE.MirroredRepeatWrapping
    // texture.wrapT = THREE.MirroredRepeatWrapping

    const boxGeometry = new THREE.BoxGeometry(14.5, 11.7, 2.5)

    // 手动设置每个面的 UV 坐标
    const uvAttribute = boxGeometry.attributes.uv
    for (let i = 0; i < uvAttribute.count; i++) {
        const u = (i % 2 === 0) ? 0 : 1
        const v = (i % 4 < 2) ? 1 : 0
        uvAttribute.setXY(i, u, v)
    }
    uvAttribute.needsUpdate = true



    const boxMaterial = new THREE.MeshStandardMaterial({
        color: 0xffffff,
        map: texture, // 2.重点位置
    })
    const boxMesh = new THREE.Mesh(boxGeometry, boxMaterial)
    scene.add(boxMesh)

    // 渲染函数
    function animate () {

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
