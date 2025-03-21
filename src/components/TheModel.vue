<script setup lang="ts">
import { onUnmounted } from 'vue'
import { dispose } from '@tresjs/core'
import { useGLTF, useAnimations } from '@tresjs/cientos'

const props = defineProps(['position'])

console.log(props.position)

const path = 'https://raw.githubusercontent.com/XnetLoL/test/main/breakdance.glb'

const { scene: model, animations } = await useGLTF(path)

const { actions } = useAnimations(animations, model)

// Note: mixer update is being called somewhere

console.log('actions:', actions)
const currentAction = actions['Armature|mixamo.com|Layer0']
// .Myo_animation
model.position.set(props.position[0], props.position[1], props.position[2])

currentAction.play()

onUnmounted(() => {
  dispose(model)
})
</script>

<template>
  <primitive :object="model" />
</template>
