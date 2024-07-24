<template>
    <div class="box">
        <canvas ref="canvas"></canvas>
    </div>
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

    camera.position.set(0, 0, 10)

    // 创建渲染器
    const renderer = new THREE.WebGLRenderer({ canvas: canvas.value })
    renderer.setSize(window.innerWidth, window.innerHeight)

    //创建一个球体几何体和材质
    const geometry = new THREE.SphereGeometry(1, 32, 32)
    const material = new THREE.MeshBasicMaterial({ color: 0xffff00 })
    const sphere = new THREE.Mesh(geometry, material)
    sphere.position.set(20, 20, 0)
    scene.add(sphere)

    const cube = new THREE.Mesh(new THREE.BoxGeometry(10, 10, 10), new THREE.MeshBasicMaterial({ color: 0x00ff00, wireframe: true }))
    scene.add(cube)


    // 添加轨道控制器
    const controls = new OrbitControls(camera, renderer.domElement)

    // 渲染循环
    const animate = () => {
        cube.rotation.x += 0.01
        cube.rotation.y += 0.01

        controls.update()
        renderer.render(scene, camera)
        requestAnimationFrame(animate)
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

<style lang="scss" scoped>
.box {
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    width: 100vw;
}
</style>
