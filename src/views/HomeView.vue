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
    camera.position.x = -61.6
    camera.position.y = 6
    camera.position.z = 149

    // 创建渲染器
    const renderer = new THREE.WebGLRenderer({
        canvas: canvasRef.value,
        antialias: true // 启用抗锯齿
    })
    renderer.setSize(window.innerWidth, window.innerHeight)
    renderer.shadowMap.enabled = true // 启用阴影映射
    renderer.setClearColor(0xffffff)

    // 添加轨道控制器
    const controls = new OrbitControls(camera, renderer.domElement)

    // 添加环境光
    const ambientLight = new THREE.AmbientLight(0xffffff, 1)
    scene.add(ambientLight)

    //添加点光源
    // const pointLight = new THREE.PointLight(0xffffff, 1000)
    const pointLight = new THREE.DirectionalLight(0xffffff, 2)
    pointLight.position.set(12, 8, 30)
    pointLight.castShadow = true // 启用阴影
    scene.add(pointLight)

    // 调整点光源的阴影设置
    pointLight.shadow.mapSize.width = 1024
    pointLight.shadow.mapSize.height = 1024
    pointLight.shadow.camera.near = 0.5
    pointLight.shadow.camera.far = 500

    //光源辅助器
    const helper = new THREE.PointLightHelper(pointLight, 1)
    scene.add(helper)

    // 添加坐标辅助器
    const axesHelper = new THREE.AxesHelper(500)
    scene.add(axesHelper)




    const geometry = new THREE.BufferGeometry()

    // 定义顶点
    const vertices = new Float32Array([
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
    // 计算法线
    geometry.computeVertexNormals()

    // const material = new THREE.MeshToonMaterial({

    // 加载纹理
    const textureLoader = new THREE.TextureLoader()
    const texture = textureLoader.load('/public/test.webp')


    const material = new THREE.MeshPhysicalMaterial({
        map: texture,
        // color: 0xffffff,
        side: THREE.DoubleSide,
        roughness: 1.0, // 高粗糙度以模拟棉布效果
        metalness: 0.0 // 非金属
    })

    // 创建网格
    const mesh = new THREE.Mesh(geometry, material)

    texture.minFilter = THREE.LinearFilter
    texture.magFilter = THREE.LinearFilter
    texture.anisotropy = renderer.capabilities.getMaxAnisotropy()

    // 为材质添加纹理
    // material.map = texture
    material.needsUpdate = true // 确保材质更新

    //将网格移至中心
    mesh.position.set(-7.25, -5.85, -0.625)

    scene.add(mesh)


    // 渲染循环
    function animate () {
        requestAnimationFrame(animate)
        //材质参数渐变
        // material.roughness = (Math.sin(Date.now() / 1000) + 1) / 2
        // material.metalness = (Math.cos(Date.now() / 1000) + 1) / 2
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
