<template>
    <div class="box">
        <canvas ref="canvas" id="canvas" width="800" height="600"></canvas>
        <div class="x"></div>
        <div class="y"></div>
    </div>
</template>

<script setup>
import { onMounted, onUnmounted, ref } from 'vue'
import useShader from '../composables/useShader'

const canvas = ref(null)

onMounted(() => {
    const ctx = document.getElementById('canvas')
    const gl = ctx.getContext('webgl')

    const VERTEX_SHADER_SOURCE = `
    attribute vec4 a_position;
    uniform mat4 mat;
    void main() {
        gl_Position = mat * a_position;
        gl_PointSize = 10.0;
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

    const a_position = gl.getAttribLocation(program, 'a_position')
    const mat = gl.getUniformLocation(program, 'mat')

    function getScaleMatrix (x = 1, y = 1, z = 1) {
        return new Float32Array([
            x, 0, 0, 0,
            0, y, 0, 0,
            0, 0, z, 0,
            0, 0, 0, 1
        ])
    }

    const points = new Float32Array([
        -0.8, 0.0,
        0.8, 0.5,
        0.5, -0.5,
    ])

    const buffer = gl.createBuffer()

    gl.bindBuffer(gl.ARRAY_BUFFER, buffer)
    gl.bufferData(gl.ARRAY_BUFFER, points, gl.STATIC_DRAW)
    gl.useProgram(program)
    gl.enableVertexAttribArray(a_position)
    gl.vertexAttribPointer(a_position, 2, gl.FLOAT, false, 0, 0)
    gl.clear(gl.COLOR_BUFFER_BIT)

    let x = -1
    function draw () {
        x += 0.01
        if (x > 1) {
            x = -1
        }
        const matrix = getScaleMatrix(x, x)
        gl.clearColor(0.0, 0.0, 0.0, 1.0)
        gl.clear(gl.COLOR_BUFFER_BIT)
        gl.uniformMatrix4fv(mat, false, matrix)
        gl.drawArrays(gl.TRIANGLES, 0, 3)
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
