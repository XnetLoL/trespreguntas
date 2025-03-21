<script setup lang="ts">
import { onUnmounted } from 'vue'
import { dispose } from '@tresjs/core'
import { useGLTF, useAnimations } from '@tresjs/cientos'

const props = defineProps(['position', 'path'])


const path = props.path;

const { scene: model, animations } = await useGLTF(path)

const { actions } = useAnimations(animations, model)

console.log("Loaded model", path);

// Play the first animation found
const actionKeys = Object.keys(actions);
console.log("Found actions:", actionKeys);
const firstActionKey = actionKeys.length > 0 ? actionKeys[0] : null;
const currentAction = firstActionKey ? actions[firstActionKey] : null;
if (currentAction) currentAction.play()

// Set position from component prop
model.position.set(props.position[0], props.position[1], props.position[2]);

onUnmounted(() => {
  dispose(model)
})
</script>

<template>
  <primitive :object="model" />
</template>
