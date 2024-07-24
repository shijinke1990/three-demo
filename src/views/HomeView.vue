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
    precision mediump float;
    uniform vec4 u_color;
    void main() {
        gl_FragColor = u_color;
    }
    `

    const program = useShader(gl, VERTEX_SHADER_SOURCE, FRAGMENT_SHADER_SOURCE)

    gl.useProgram(program)

    const a_position = gl.getAttribLocation(program, 'a_position')

    const u_color = gl.getUniformLocation(program, 'u_color')


    gl.uniform4f(u_color, 1.0, 0.0, 0.0, 1.0)

    const points = []


    ctx.onmousemove = (e) => {
        const { x, y } = translatePosition(e.offsetX, e.offsetY)
        points.push({ x, y })
    }

    function draw () {
        console.info('draw', points)
        gl.clear(gl.COLOR_BUFFER_BIT)
        points.forEach(point => {
            gl.vertexAttrib3f(a_position, point.x, point.y, 0.0)
            gl.drawArrays(gl.POINTS, 0, 1)
        })
        requestAnimationFrame(draw)
    }

    draw()




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
