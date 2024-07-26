<template>
    <canvas ref="canvasRef">
    </canvas>
</template>

<script setup>
import { onMounted, ref } from 'vue'
import * as THREE from 'three'
// import { GUI } from "three/examples/jsm/libs/lil-gui.module.min.js"

//import { GUI } from "three/examples/jsm/libs/lil-gui.module.min.js"

import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls'

const canvasRef = ref(null)








onMounted(() => {
    const scene = new THREE.Scene()
    // 创建相机
    const camera = new THREE.PerspectiveCamera(30, window.innerWidth / window.innerHeight, 0.1, 1000)
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

    //坐标辅助器

    const axesHelper = new THREE.AxesHelper(500)
    scene.add(axesHelper)

    // 创建太阳
    const sunGeometry = new THREE.SphereGeometry(12, 32, 32)
    const sunMaterial = new THREE.MeshBasicMaterial({ color: 0xffff00 })
    const sunMesh = new THREE.Mesh(sunGeometry, sunMaterial)
    scene.add(sunMesh)


    // const meshSize = 5
    // function createMesh (vertices, uv, position = new THREE.Vector3(0, 0, 0)) {
    //     const geometry = new THREE.BufferGeometry()
    //     geometry.setAttribute('position', new THREE.BufferAttribute(vertices, 3))
    //     geometry.setAttribute('uv', new THREE.BufferAttribute(uv, 2))
    //     const mesh = new THREE.Mesh(geometry, boxMaterial)
    //     mesh.position.copy(position)
    //     return mesh
    // }

    // const meshSize = 5

    // // 定义平面的顶点和索引数据
    // const vertices = new Float32Array([
    //     -1.0, -1.0, 0.0, // 左下角
    //     1.0, -1.0, 0.0, // 右下角
    //     1.0, 1.0, 0.0, // 右上角
    //     -1.0, 1.0, 0.0  // 左上角
    // ])

    // const indices = new Uint16Array([
    //     0, 1, 2, // 第一个三角形
    //     0, 2, 3  // 第二个三角形
    // ])

    // const geometry = new THREE.BufferGeometry()
    // geometry.setAttribute('position', new THREE.BufferAttribute(vertices, 3))
    // geometry.setIndex(new THREE.BufferAttribute(indices, 1))

    const geometry = new THREE.BoxGeometry(10, 10, 10)

    const wireframe = new THREE.WireframeGeometry(geometry)
    const boxMesh = new THREE.LineSegments(wireframe)

    boxMesh.position.set(5, 5, 5)

    boxMesh.scale.set(4, 2, 1)

    const childGeometry = new THREE.BoxGeometry(2, 2, 2)
    const childMesh = new THREE.Mesh(childGeometry, new THREE.MeshBasicMaterial({ color: 0x00ff00 }))
    childMesh.position.set(0, 0, 0)
    childMesh.scale.set(10, 4, 1)
    boxMesh.add(childMesh)

    const earth = new THREE.SphereGeometry(1, 32, 32)
    const earthMaterial = new THREE.MeshLambertMaterial()
    const earthMesh = new THREE.Mesh(earth, earthMaterial)
    earthMesh.position.set(50, 0, 0) // 设置地球相对于太阳的位置

    const textureLoader = new THREE.TextureLoader()
    const texture = textureLoader.load('/public/earth.jpg')
    earthMaterial.map = texture

    earthMesh.rotation.z = 675 / 900
    scene.add(earthMesh)



    // scene.add(boxMesh)

    const controls = new OrbitControls(camera, renderer.domElement)
    // controls.enableDamping = true
    // controls.dampingFactor = 0.05
    // controls.autoRotate = true
    // controls.autoRotateSpeed = 1


    // 渲染函数
    function animate () {
        earthMesh.rotateY(0.01)
        renderer.render(scene, camera)
        controls.update()
        requestAnimationFrame(animate)


    }
    animate()

    // 添加轨道控制

    // 处理窗口大小调整
    window.addEventListener('resize', () => {
        camera.aspect = window.innerWidth / window.innerHeight
        camera.updateProjectionMatrix()
        renderer.setSize(window.innerWidth, window.innerHeight)
    })

    // const gui = new GUI()
    // gui.domElement.style.right = '0px'
    // gui.domElement.style.width = '300px'
    // let folder = gui.addFolder('太阳')

    // folder.add(sunMesh.position, 'x').min(-100).max(100).step(0.1).name('太阳 x')
    // folder.add(sunMesh.position, 'y').min(-100).max(100).step(0.1).name('太阳 y')
    // folder.add(sunMesh.position, 'z').min(-100).max(100).step(0.1).name('太阳 z')

    // gui.add(camera.position, 'x').min(-100).max(100).step(0.1).name('相机 x')
    // gui.add(camera.position, 'y').min(-100).max(100).step(0.1).name('相机 y')
    // gui.add(camera.position, 'z').min(-100).max(100).step(0.1).name('相机 z')
    // gui.add(earthMesh.position, 'x').min(-100).max(100).step(0.1).name('地球 x')
    // gui.add(earthMesh.position, 'y').min(-100).max(100).step(0.1).name('地球 y')
    // gui.add(earthMesh.position, 'z').min(-100).max(100).step(0.1).name('地球 z')

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
