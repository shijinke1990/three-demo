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
    const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000)
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
    const uvTexture = new THREE.TextureLoader().load('/public/2.jpg', () => {
        // 创建平面几何体
        const plain = new THREE.PlaneGeometry(20, 20, 5)

        // 创建材质
        const plainMaterial = new THREE.MeshBasicMaterial({ map: uvTexture })

        // 创建平面网格
        const plainMesh = new THREE.Mesh(plain, plainMaterial)

        // 添加平面网格到场景
        scene.add(plainMesh)

        // 动画循环
        const animate = () => {
            requestAnimationFrame(animate)
            renderer.render(scene, camera)
        }
        animate()
    })

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
body {
    margin: 0;
}

canvas {
    display: block;
}
</style>
