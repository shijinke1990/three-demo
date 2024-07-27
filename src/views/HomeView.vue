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

    // const geometry = new THREE.BufferGeometry()
    // const vertices = new Float32Array([
    //     0, 0, 0,
    //     1, 0, 0,
    //     1, 1, 0,
    //     0, 1, 0
    // ])
    // geometry.setAttribute('position', new THREE.BufferAttribute(vertices, 3))
    // const indices = new Uint16Array([
    //     0, 1, 2,
    //     0, 2, 3
    // ])
    // geometry.setIndex(new THREE.BufferAttribute(indices, 1))

    // geometry.addGroup(0, 3, 0)
    // geometry.addGroup(3, 3, 1)

    // const textureLoader = new THREE.TextureLoader()
    // const texture = textureLoader.load('/public/9.jpeg')
    // const material0 = new THREE.MeshStandardMaterial({
    //     color: 0x8899ee,
    //     side: THREE.DoubleSide,
    //     // // wireframe: true,
    //     // map: texture
    // })
    // const material1 = new THREE.MeshStandardMaterial({
    //     color: 0x88ee99,
    //     side: THREE.DoubleSide,
    //     // // wireframe: true,
    //     // map: texture
    // })

    // const mesh = new THREE.Mesh(geometry, [material0, material1])
    // scene.add(mesh)


    // const geometry = new THREE.BoxGeometry(1, 1, 1)

    // const geometry = new THREE.PlaneGeometry(1, 1, 1, 1)
    // const vertices = new Float32Array([
    //     0, 0, 0,
    //     1, 0, 0,
    //     1, 1, 0,
    //     0, 1, 0
    // ])
    // geometry.setAttribute('position', new THREE.BufferAttribute(vertices, 3))

    // geometry.clearGroups()

    // 创建 BufferGeometry
    const geometry = new THREE.BufferGeometry()

    // 定义顶点
    // 定义顶点
    const vertices = new Float32Array([
        // 第一个面
        0, 0, 0,
        0, 0, 1.25,
        0, 12.7, 1.25,
        0, 12.7, 0,

        // 第二个面
        0, 0, 1.25,
        15.5, 0, 1.25,
        15.5, 12.7, 1.25,
        0, 12.7, 1.25,

        //  第三个面
        15.5, 0, 1.25,
        15.5, 0, 0,
        15.5, 12.7, 0,
        15.5, 12.7, 1.25,

        //  第四个面
        15.5, 0, 0,
        0, 0, 0,
        0, 12.7, 0,
        15.5, 12.7, 0,

        //  第五个面
        0, 12.7, 1.25,
        15.5, 12.7, 1.25,
        15.5, 12.7, 0,
        0, 12.7, 0,

        //  第六个面
        0, 0, 0,
        15.5, 0, 0,
        15.5, 0, 1.25,
        0, 0, 1.25



    ])

    // 定义索引
    const indices = new Uint16Array([
        // 第一个面
        0, 1, 2,
        0, 2, 3,
        // // 第二个面
        4, 5, 6,
        4, 6, 7,
        // // 第三个面
        8, 9, 10,
        8, 10, 11,

        // // 第四个面
        12, 13, 14,
        12, 14, 15,

        // // 第五个面
        16, 17, 18,
        16, 18, 19,


        // // 第六个面

        20, 21, 22,
        20, 22, 23






    ])

    // 定义 UV 坐标
    const uvs = new Float32Array([
        // // 第一个面显示纹理的左上部分
        // 0.0, 0.0,
        // 0.5, 0.0,
        // 0.5, 0.5,
        // 0.0, 0.5,
        // // 第二个面显示纹理的右下部分
        // 0.5, 0.5,
        // 1.0, 0.5,
        // 1.0, 1.0,
        // 0.5, 1.0
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

        1.5 / 17, 12.7 / 14.2,
        15.5 / 17, 12.7 / 14.2,
        15.5 / 17, 13.95 / 14.2,
        1.5 / 17, 13.95 / 14.2,

        0.25 / 17, 0,
        15.5 / 17, 0,
        15.5 / 17, 1.5 / 14.2,
        0.25 / 17, 1.5 / 14.2

    ])

    // 设置几何体的属性
    geometry.setAttribute('position', new THREE.BufferAttribute(vertices, 3))
    geometry.setIndex(new THREE.BufferAttribute(indices, 1))
    geometry.setAttribute('uv', new THREE.BufferAttribute(uvs, 2))

    // 创建材质
    const material = new THREE.MeshBasicMaterial({ color: 0xffffff, side: THREE.DoubleSide })

    // 创建网格
    const mesh = new THREE.Mesh(geometry, material)

    // 加载纹理
    const textureLoader = new THREE.TextureLoader()
    const texture = textureLoader.load('/public/test.webp')

    // 为材质添加纹理
    material.map = texture
    material.needsUpdate = true // 确保材质更新

    // 将网格添加到场景
    scene.add(mesh)

    // 清除默认分组
    // geometry.clearGroups()

    // // 添加自定义分组
    // geometry.addGroup(0, 18, 0) // 前面
    // // geometry.addGroup(6, 6, 1) // 后面
    // // geometry.addGroup(12, 6, 2) // 顶面
    // geometry.addGroup(18, 6, 1) // 底面
    // geometry.addGroup(24, 6, 2) // 左面
    // geometry.addGroup(30, 6, 3) // 右面

    // const material0 = new THREE.MeshBasicMaterial({ color: 0x00ff00 })
    // const material1 = new THREE.MeshBasicMaterial({ color: 0xff0000 })
    // const material2 = new THREE.MeshBasicMaterial({ color: 0x0000ff })
    // const material3 = new THREE.MeshBasicMaterial({ color: 0xffff00 })
    // const material4 = new THREE.MeshBasicMaterial({ color: 0xff00ff })
    // const material5 = new THREE.MeshBasicMaterial({ color: 0x00ffff })

    // const materials = [material0, material1, material2, material3]


    //为cubeMesh添加纹理
    // 为每个材质添加纹理
    // materials.forEach(material => {
    //     material.map = texture
    //     material.needsUpdate = true // 确保材质更新
    // })
















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
