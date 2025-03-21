<script setup lang="ts">
import { onUnmounted } from 'vue'
import { dispose, useRenderLoop } from '@tresjs/core'
import { useGLTF, GLTFModel, useAnimations } from '@tresjs/cientos'


const props = defineProps(['position', 'path', 'scale'])

const path = props.path;

// Load model
const { scene: model, animations } = await useGLTF(path)
console.log("Loaded model", path, model);

// Get material just for the skeleton model
let skelMat;
if (path.includes("skel")){
  model.traverse((child) => {
    if (!skelMat && child.material) {
      skelMat = child.material;
      console.log("Skeleton Material:", skelMat)
    }; 
  });

}
// Create update handler for this component, 
// only if the model is the skeleton 
const { onLoop } = useRenderLoop()
if (skelMat) {
  onLoop(({ delta, elapsed, clock }) => {
      let t = elapsed * 10; // multiply it so it variates faster
      let norm_sin = Math.sin(t) * 0.5 + 0.5;
      skelMat.emissive.b = norm_sin;
    });
}
  
// Inspect animation and play the first one
const { actions } = useAnimations(animations, model)
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


// Click handler 
function printClickInfo(event){
  if (event.intersections) {
    // just print the position of all the intersections
    event.intersections.forEach(intersection => {
      console.log(`Click hit ${intersection.object.name} at`, intersection.point);
    });
  }
}

// Cleanup model when component removed from scene
onUnmounted(() => {
  dispose(model)
})

</script>


<template>
  <primitive :object="model"/> 
  
  <!-- @click seems to be creating performance issues, probably activates
  onHover raycasting intersection tests that slows down the scene 
  when polycount is not negligible -->
  <!-- <primitive :object="model" @click="printClickInfo"/> -->
  <!-- <GLTFModel :object="model" @click="printClickInfo" draco /> -->
</template>
