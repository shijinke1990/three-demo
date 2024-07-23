<template>
    <div>
        <canvas id="canvas" width="400" height="400"></canvas>

    </div>
</template>

<script setup>
import { onMounted } from 'vue'
import useShader from '../composables/useShader'
onMounted(() => {
    const ctx = document.getElementById('canvas')
    const gl = ctx.getContext('webgl')

    const VERTEX_SHADER_SOURCE = `
    void main() {
        gl_Position = vec4(0.0, 0.0, 0.0, 1.0);
        gl_PointSize = 10.0;
    }
    `
    const FRAGMENT_SHADER_SOURCE = `
    void main() {
        gl_FragColor = vec4(1.0, 0.0, 0.0, 1.0);
    }
    `

    const program = useShader(gl, VERTEX_SHADER_SOURCE, FRAGMENT_SHADER_SOURCE)

    gl.clearColor(0.0, 0.0, 0.0, 1.0)

    gl.clear(gl.COLOR_BUFFER_BIT)

    gl.useProgram(program)

    gl.drawArrays(gl.POINTS, 0, 1)



})


</script>


<style lang="scss" scoped>
// #canvas {
//     width: 100px;
//     height: 100px;
//     display: block;
// }
</style>
