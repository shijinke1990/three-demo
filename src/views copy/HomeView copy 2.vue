<template>
    <canvas ref="canvas"></canvas>
</template>

<script setup>
import { onMounted, onUnmounted, ref } from 'vue'
import * as THREE from 'three'
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls'

const canvas = ref(null)

onMounted(() => {
    // 创建场景
    const scene = new THREE.Scene()

    // 创建相机
    const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000)
    camera.position.z = 20

    // 创建渲染器
    const renderer = new THREE.WebGLRenderer({ canvas: canvas.value })
    renderer.setSize(window.innerWidth, window.innerHeight)

    // 创建太阳
    const sunGeometry = new THREE.SphereGeometry(2, 32, 32)
    const sunMaterial = new THREE.MeshLambertMaterial({ color: 0xffff00 })
    const sun = new THREE.Mesh(sunGeometry, sunMaterial)
    scene.add(sun)

    // 创建地球
    const earthGeometry = new THREE.SphereGeometry(1, 32, 32)
    const earthMaterial = new THREE.MeshLambertMaterial({ color: 0x0000ff })
    const earth = new THREE.Mesh(earthGeometry, earthMaterial)
    scene.add(earth)


    //添加灯光
    const spotLight = new THREE.SpotLight(0xffffff)
    spotLight.position.set(2, 2, 5)
    spotLight.castShadow = true
    scene.add(spotLight)


    // 添加轨道控制器
    const controls = new OrbitControls(camera, renderer.domElement)

    // 渲染循环
    const animate = () => {
        requestAnimationFrame(animate)

        // 地球围绕太阳旋转
        const time = Date.now() * 0.001
        earth.position.x = Math.cos(time) * 10
        earth.position.z = Math.sin(time) * 10

        controls.update()
        renderer.render(scene, camera)
    }
    animate()



    // 处理窗口大小调整
    const onWindowResize = () => {
        camera.aspect = window.innerWidth / window.innerHeight
        camera.updateProjectionMatrix()
        renderer.setSize(window.innerWidth, window.innerHeight)
    }
    window.addEventListener('resize', onWindowResize)

    // 清理函数
    onUnmounted(() => {
        window.removeEventListener('resize', onWindowResize)
        controls.dispose()
    })
})
</script>

<style lang="scss" scoped></style>
