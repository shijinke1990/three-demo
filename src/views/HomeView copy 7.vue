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
    const textureLoader = new THREE.TextureLoader()
    const texture = textureLoader.load('/public/test.webp', () => {
        // 创建长方体
        const boxGeometry = new THREE.BoxGeometry(50, 50, 20)

        // 手动设置 UV 映射
        const uvAttribute = boxGeometry.attributes.uv
        const uvMapping = [
            // Front face
            0, 0, 1, 0, 1, 1, 0, 1,
            // Back face
            1, 0, 1, 1, 0, 1, 0, 0,
            // Top face
            0, 1, 0, 0, 1, 0, 1, 1,
            // Bottom face
            1, 1, 0, 1, 0, 0, 1, 0,
            // Right face
            1, 0, 1, 1, 0, 1, 0, 0,
            // Left face
            0, 0, 0, 1, 1, 1, 1, 0
        ]
        for (let i = 0; i < uvAttribute.count; i++) {
            uvAttribute.setXY(i, uvMapping[i * 2], uvMapping[i * 2 + 1])
        }

        // 创建材质
        const material = new THREE.MeshBasicMaterial({ map: texture })
        const box = new THREE.Mesh(boxGeometry, material)
        scene.add(box)

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
