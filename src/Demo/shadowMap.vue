<template>
    <canvas ref="canvasRef"></canvas>
</template>

<script setup>
import { onMounted, ref } from 'vue'
import * as THREE from 'three'
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls'

const canvasRef = ref(null)

onMounted(() => {
    // 创建场景
    const scene = new THREE.Scene()

    // 创建相机
    const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000)
    camera.position.set(0, 0, 5) // 设置相机位置

    // 创建渲染器
    const renderer = new THREE.WebGLRenderer({
        canvas: canvasRef.value,
        antialias: true // 启用抗锯齿
    })
    renderer.setSize(window.innerWidth, window.innerHeight)
    renderer.shadowMap.enabled = true // 启用阴影映射

    // 添加轨道控制器
    const controls = new OrbitControls(camera, renderer.domElement)

    // 添加环境光
    const ambientLight = new THREE.AmbientLight(0xffffff, 0.5)
    scene.add(ambientLight)

    // 添加点光源
    const pointLight = new THREE.PointLight(0xffffff, 100)
    pointLight.position.set(5, 5, 5)
    pointLight.castShadow = true // 启用阴影
    scene.add(pointLight)

    // 调整点光源的阴影设置
    pointLight.shadow.mapSize.width = 1024
    pointLight.shadow.mapSize.height = 1024
    pointLight.shadow.camera.near = 0.5
    pointLight.shadow.camera.far = 500

    // 添加坐标辅助器
    const axesHelper = new THREE.AxesHelper(500)
    scene.add(axesHelper)

    // 创建一个简单的几何体
    const geometry = new THREE.BoxGeometry()
    const material = new THREE.MeshStandardMaterial({ color: 0x00ff00 })
    const cube = new THREE.Mesh(geometry, material)
    cube.castShadow = true // 启用投射阴影
    cube.receiveShadow = true // 启用接收阴影
    //将网格移至中心
    cube.position.set(-7.25, -5.85, -0.625)

    scene.add(cube)


    const plan = new THREE.PlaneGeometry(10, 10)
    const planMaterial = new THREE.MeshStandardMaterial({
        color: 0xffffff,
        side: THREE.DoubleSide
    })
    const planMesh = new THREE.Mesh(plan, planMaterial)
    planMesh.rotation.x = -Math.PI / 2
    planMesh.position.y = -1
    planMesh.receiveShadow = true // 启用接收阴影


    scene.add(planMesh)






    // 渲染循环
    function animate () {
        requestAnimationFrame(animate)
        // planMesh.rotation.x += 0.01
        controls.update()
        renderer.render(scene, camera)
    }

    animate()
})
</script>

<style>
canvas {
    display: block;
}
</style>
