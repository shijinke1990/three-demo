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
    const camera = new THREE.PerspectiveCamera(5, window.innerWidth / window.innerHeight, 0.1, 1000)


    // 创建渲染器
    const renderer = new THREE.WebGLRenderer({
        canvas: canvasRef.value,
        antialias: true // 启用抗锯齿
    })
    renderer.setSize(window.innerWidth, window.innerHeight)








    const cube = new THREE.BoxGeometry(2, 2, 2)
    const cubeMaterial = new THREE.MeshBasicMaterial({ color: 0xffff00 })
    const cubeMesh = new THREE.Mesh(cube, cubeMaterial)
    cubeMesh.position.set(20, 0, 100)
    scene.add(cubeMesh)





    // 创建 OrbitControls
    const controls = new OrbitControls(camera, renderer.domElement)
    controls.autoRotate = true
    controls.autoRotateSpeed = 1

    //打印当前相机位置
    console.log(camera.position)


    const axesHelper = new THREE.AxesHelper(500)
    scene.add(axesHelper)

    // 渲染函数
    function animate () {
        requestAnimationFrame(animate)
        controls.update()
        renderer.render(scene, camera)

    }
    animate()

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
    position: relative;
}

.fullscreen-btn {
    position: absolute;
    top: 10px;
    right: 10px;
    padding: 10px 20px;
    background-color: rgba(0, 0, 0, 0.5);
    color: white;
    border: none;
    cursor: pointer;
    z-index: 1;
}

.fullscreen-btn:hover {
    background-color: rgba(0, 0, 0, 0.7);
}

/* 确保 GUI 显示在正确的位置 */
.lil-gui {
    position: absolute;
    top: 10px;
    left: 10px;
    z-index: 2;
}
</style>
