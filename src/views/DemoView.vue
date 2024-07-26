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
    camera.position.x = 10
    camera.position.y = 5
    camera.position.z = 40

    // 创建渲染器
    const renderer = new THREE.WebGLRenderer({
        canvas: canvasRef.value
    })
    renderer.setSize(window.innerWidth, window.innerHeight)


    // 添加灯光
    const ambientLight = new THREE.AmbientLight(0xffffff, 0.5)
    scene.add(ambientLight)




    // // 创建材质
    // const boxMaterial = new THREE.MeshStandardMaterial({
    //     color: 0xffffff,
    //     map: texture
    // })

    // // 创建正四面体
    // const vertices = new Float32Array([
    //     0, 0, 0,
    //     1, 0, 0,
    //     0, 1, 0,
    //     0, 0, 1
    // ])

    // const indices = new Uint16Array([
    //     0, 1, 2, // 面1
    //     0, 2, 3, // 面2
    //     0, 3, 1, // 面3
    //     1, 3, 2  // 面4
    // ])

    // const uv = new Float32Array([
    //     0.5, 1,  // 顶点1的UV坐标
    //     0, 0,    // 顶点2的UV坐标
    //     1, 0,    // 顶点3的UV坐标
    //     0.5, 0.5 // 顶点4的UV坐标
    // ])

    // function createMesh (vertices, indices, uv, position = new THREE.Vector3(0, 0, 0)) {
    //     const geometry = new THREE.BufferGeometry()
    //     geometry.setAttribute('position', new THREE.BufferAttribute(vertices, 3))
    //     geometry.setIndex(new THREE.BufferAttribute(indices, 1))
    //     geometry.setAttribute('uv', new THREE.BufferAttribute(uv, 2))
    //     const mesh = new THREE.Mesh(geometry, boxMaterial)
    //     mesh.position.copy(position)
    //     return mesh
    // }

    // const tetrahedronMesh = createMesh(vertices, indices, uv)
    // scene.add(tetrahedronMesh)

    const geometry = new THREE.BufferGeometry()
    const vertices = new Float32Array([
        0, 0, 0,
        1, 0, 0,
        0, 1, 0,
        1, 0, 0,
        0, 1, 0,
        1, 1, 0
    ])
    geometry.setAttribute('position', new THREE.BufferAttribute(vertices, 3))
    const textureLoader = new THREE.TextureLoader()
    const texture = textureLoader.load('/public/9.jpeg')
    const material = new THREE.MeshStandardMaterial({
        color: 0x8899ee,
        side: THREE.DoubleSide,
        // // wireframe: true,
        map: texture
    })
    const plane = new THREE.Mesh(geometry, material)
    scene.add(plane)











    // 创建 OrbitControls
    const controls = new OrbitControls(camera, renderer.domElement)
    controls.autoRotate = true
    controls.autoRotateSpeed = 1


    const axesHelper = new THREE.AxesHelper(500)
    scene.add(axesHelper)

    // 渲染函数
    function animate () {
        requestAnimationFrame(animate)
        // controls.update()
        renderer.render(scene, camera)
        //打印当前相机位置
        // console.log(camera.position)
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
