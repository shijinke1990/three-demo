<template>
    <div class="box">
        <canvas ref="canvas" id="canvas" width="800" height="600"></canvas>
        <div class="x"></div>
        <div class="y"></div>
    </div>
</template>

<script setup>
import { onMounted, ref } from 'vue'
import useShader from '../composables/useShader'

const canvas = ref(null)
const points = ref([])
const color = ref('red')



onMounted(() => {
    const ctx = document.getElementById('canvas')
    const gl = ctx.getContext('webgl')

    const translatePosition = (x, y) => {
        const halfWidth = ctx.width / 2
        const halfHeight = ctx.height / 2
        return {
            x: (x - halfWidth) / halfWidth,
            y: (halfHeight - y) / halfHeight
        }
    }

    const VERTEX_SHADER_SOURCE = `
    attribute vec4 a_position;
    void main() {
        gl_Position = a_position;
        gl_PointSize = 5.0;
    }
    `
    const FRAGMENT_SHADER_SOURCE = `
    uniform vec4 u_color;
    void main() {
        gl_FragColor = u_color;
    }
    `



    const program = useShader(gl, VERTEX_SHADER_SOURCE, FRAGMENT_SHADER_SOURCE)

    const a_position = gl.getAttribLocation(program, 'a_position')

    gl.vertexAttrib3f(a_position, 0.3, 0.0, 0.0)


    gl.useProgram(program)
    gl.drawArrays(gl.POINTS, 0, 1)

    let x = 0
    // setInterval(() => {
    //     x += 0.01
    //     if (x > 1) x = -1
    //     gl.vertexAttrib3f(a_position, x, 0.0, 0.0)
    //     gl.drawArrays(gl.POINTS, 0, 1)


    // }, 100)
    const draw = () => {
        x += 0.01
        if (x > 1) x = -1
        gl.vertexAttrib3f(a_position, x, 0.0, 0.0)
        gl.drawArrays(gl.POINTS, 0, 1)
        requestAnimationFrame(draw)
    }

    // requestAnimationFrame(() => {
    //     x += 0.01
    //     if (x > 1) x = -1
    //     gl.vertexAttrib3f(a_position, x, 0.0, 0.0)
    //     gl.drawArrays(gl.POINTS, 0, 1)


    // })

    // draw()

    ctx.onmousemove = (e) => {
        console.log('click')
        console.log(e.clientX)
        console.log(e.clientY)
        console.log(e.offsetX)
        console.log(e.offsetY)
        const domPosition = ctx.getBoundingClientRect()
        console.log(domPosition)
        const x = e.clientX - domPosition.left
        const y = e.clientY - domPosition.top

        console.log(x, y)

        const { x: tx, y: ty } = translatePosition(x, y)

        console.log(tx, ty)

        points.value.push({ x: tx, y: ty })

        for (let i = 0; i < points.value.length; i++) {
            gl.vertexAttrib3f(a_position, points.value[i].x, points.value[i].y, 0.0)
            gl.drawArrays(gl.POINTS, 0, 1)


        }



    }

})


</script>


<style lang="scss" scoped>
.box {
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 600px;
    width: 800px;

    .x {
        content: '2dfsaf';
        display: block;
        position: absolute;
        top: 0;
        left: 400px;
        background-color: red;
        width: 1px;
        height: 600px
    }

    .y {
        content: ' ';
        display: block;
        position: absolute;
        top: 300px;
        left: 0;
        background-color: red;
        width: 800px;
        height: 1px
    }

}

canvas {
    border: 1px solid #000;


}
</style>
