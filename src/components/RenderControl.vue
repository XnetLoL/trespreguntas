<script setup lang="ts">
import { onUnmounted } from 'vue'
import { useLoop } from '@tresjs/core'
import { StatsGl } from '@tresjs/cientos'

const { render } = useLoop()

// only the last render is used
render(({ renderer, scene, camera }) => {
    renderer.render(scene, camera)
})

onUnmounted(() => {
})
</script>

<template>
    <StatsGl />
</template>
