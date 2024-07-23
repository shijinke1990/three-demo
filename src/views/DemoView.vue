<template>
    <div>
        <canvas id="canvas" width="400" height="400"></canvas>

    </div>
</template>

<script setup>
import { onMounted } from 'vue'
onMounted(() => {
    const ctx = document.getElementById('canvas')
    const gl = ctx.getContext('webgl')
    // const gl = document.getElementById('canvas').getContext('webgl')

    gl.clearColor(0.0, 0.0, 0.0, 1.0)

    gl.clear(gl.COLOR_BUFFER_BIT)
})


</script>


<style lang="scss" scoped>
// #canvas {
//     width: 100px;
//     height: 100px;
//     display: block;
// }
</style>
