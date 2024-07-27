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
    camera.position.x = -72
    camera.position.y = 2
    camera.position.z = 145

    // 创建渲染器
    const renderer = new THREE.WebGLRenderer({
        canvas: canvasRef.value,
        antialias: true // 启用抗锯齿
    })
    renderer.setSize(window.innerWidth, window.innerHeight)




    // 添加灯光
    const ambientLight = new THREE.AmbientLight(0xffffff, 2)
    scene.add(ambientLight)

    // 添加点光源
    const pointLight = new THREE.PointLight(0xff0000, 1, 100)
    pointLight.position.set(20, 0, 100)
    pointLight.castShadow = true // 启用阴影
    scene.add(pointLight)

    // 检查光源的阴影设置
    pointLight.shadow.mapSize.width = 1024
    pointLight.shadow.mapSize.height = 1024
    pointLight.shadow.camera.near = 0.5
    pointLight.shadow.camera.far = 500

    // 添加辅助工具以可视化光源
    const helper = new THREE.PointLightHelper(pointLight, 5)
    scene.add(helper)

    const cube = new THREE.BoxGeometry(10, 10, 10)
    const cubeMaterial = new THREE.MeshStandardMaterial({ color: 0xffff00 })
    const cubeMesh = new THREE.Mesh(cube, cubeMaterial)
    cubeMesh.position.set(20, 0, 100)
    scene.add(cubeMesh)



    const geometry = new THREE.BufferGeometry()

    // 定义顶点
    const vertices = new Float32Array([
        // 第一个面
        0, 0, 0,
        0, 0, 1.25,
        0, 11.7, 1.25,
        0, 11.7, 0,

        0, 0, 1.25,
        14.5, 0, 1.25,
        14.5, 11.7, 1.25,
        0, 11.7, 1.25,

        14.5, 0, 1.25,
        14.5, 0, 0,
        14.5, 11.7, 0,
        14.5, 11.7, 1.25,


        14.5, 0, 0,
        14.25, 0, 0,
        14.25, 11.7, 0,
        14.5, 11.7, 0,


        0.25, 0, 0,
        0, 0, 0,
        0, 11.7, 0,
        0.25, 11.7, 0,


        0, 11.7, 1.25,
        14.5, 11.7, 1.25,
        14.5, 11.7, 0,
        0, 11.7, 0,



        0, 0, 0,
        14.5, 0, 0,
        14.5, 0, 1.25,
        0, 0, 1.25,

        14.5, 0, 0,
        0, 0, 0,
        0, 0.25, 0,
        14.5, 0.25, 0,


        14.5, 11.45, 0,
        0, 11.45, 0,
        0, 11.7, 0,
        14.5, 11.7, 0,





    ])

    // 定义索引
    const indices = new Uint16Array([

        0, 1, 2,
        0, 2, 3,

        4, 5, 6,
        4, 6, 7,

        8, 9, 10,
        8, 10, 11,



        12, 13, 14,
        12, 14, 15,


        16, 17, 18,
        16, 18, 19,


        20, 21, 22,
        20, 22, 23,


        24, 25, 26,
        24, 26, 27,


        28, 29, 30,
        28, 30, 31,

        32, 33, 34,
        32, 34, 35,






    ])

    // 定义 UV 坐标
    const uvs = new Float32Array([


        0.25 / 17, 1.5 / 14.2,
        1.5 / 17, 1.5 / 14.2,
        1.5 / 17, 12.7 / 14.2,
        0.25 / 17, 12.7 / 14.2,

        1.5 / 17, 1.5 / 14.2,
        15.5 / 17, 1.5 / 14.2,
        15.5 / 17, 12.7 / 14.2,
        1.5 / 17, 12.7 / 14.2,

        15.5 / 17, 1.5 / 14.2,
        16.75 / 17, 1.5 / 14.2,
        16.75 / 17, 12.7 / 14.2,
        15.5 / 17, 12.7 / 14.2,






        16.75 / 17, 1.5 / 14.2,
        17 / 17, 1.5 / 14.2,
        17 / 17, 12.7 / 14.2,
        16.75 / 17, 12.7 / 14.2,

        0, 1.5 / 14.2,
        0.25 / 17, 1.5 / 14.2,
        0.25 / 17, 12.7 / 14.2,
        0, 12.7 / 14.2,


        1.5 / 17, 12.7 / 14.2,
        15.5 / 17, 12.7 / 14.2,
        15.5 / 17, 13.95 / 14.2,
        1.5 / 17, 13.95 / 14.2,


        0.25 / 17, 0,
        15.5 / 17, 0,
        15.5 / 17, 1.5 / 14.2,
        0.25 / 17, 1.5 / 14.2,


        1.5 / 17, 0,
        15.5 / 17, 0,
        15.5 / 17, 0.25 / 14.2,
        1.5 / 17, 0.25 / 14.2,


        1.5 / 17, 13.95 / 14.2,
        15.5 / 17, 13.95 / 14.2,
        15.5 / 17, 14.2 / 14.2,
        1.5 / 17, 14.2 / 14.2,



    ])

    // 设置几何体的属性
    geometry.setAttribute('position', new THREE.BufferAttribute(vertices, 3))
    geometry.setIndex(new THREE.BufferAttribute(indices, 1))
    geometry.setAttribute('uv', new THREE.BufferAttribute(uvs, 2))

    // 创建材质
    const material = new THREE.MeshStandardMaterial({
        color: 0xffffff,
        side: THREE.DoubleSide,
        roughness: 1.0, // 高粗糙度以模拟棉布效果
        metalness: 0.0 // 非金属
    })
    // 创建网格
    const mesh = new THREE.Mesh(geometry, material)

    // 加载纹理
    const textureLoader = new THREE.TextureLoader()
    const texture = textureLoader.load('/public/test.webp')

    texture.minFilter = THREE.LinearFilter
    texture.magFilter = THREE.LinearFilter
    texture.anisotropy = renderer.capabilities.getMaxAnisotropy()

    // 为材质添加纹理
    material.map = texture
    material.needsUpdate = true // 确保材质更新

    //将网格移至中心
    mesh.position.set(-7.25, -5.85, -0.625)

    // 将网格添加到场景
    scene.add(mesh)















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
        // controls.update()
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
