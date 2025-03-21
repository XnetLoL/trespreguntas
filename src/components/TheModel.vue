<script setup lang="ts">
import { onUnmounted } from 'vue'
import { dispose } from '@tresjs/core'
import { useGLTF, useAnimations } from '@tresjs/cientos'

const props = defineProps(['position', 'path', 'scale'])


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

// Set position and scale from component prop
model.position.set(props.position[0], props.position[1], props.position[2]);
const scale = props.scale || [1, 1, 1];

model.scale.set(scale[0], scale[1], scale[2]);

onUnmounted(() => {
  dispose(model)
})

function printClickInfo(event){
  if (event.intersections) {
    event.intersections.forEach(intersection => {
      console.log(`Click hit ${intersection.object.name} at`, intersection.point);
    });
  }
}
</script>


<template>
  <primitive :object="model" @click="printClickInfo"/>
</template>
