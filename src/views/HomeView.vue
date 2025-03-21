<script setup lang="ts">
import { onMounted } from 'vue'
import { TresCanvas } from '@tresjs/core'
import { CameraControls} from '@tresjs/cientos'
import TheModel from '@/components/TheModel.vue'
import RenderControl from '@/components/RenderControl.vue'
const size = 10
const divisions = size * 4

onMounted(() => {
})
</script>

<template>
  <TresCanvas clear-color="#0c0c0c" window-size >
    <RenderControl />
    <TresPerspectiveCamera :position="[3, 2, 5]" :look-at="[0, 5, 0]" />
    <CameraControls />

    <TresDirectionalLight :intensity="2" :position="[3, 3, 3]" cast-shadow />
    <TresAmbientLight :intensity="1" />

    <Suspense>
      <TheModel 
        :position="[-1, 0, -2]"
        :scale="[.1, .1, .1,]"
        :path="'cubito.glb'" />
    </Suspense>
    <Suspense>
      <TheModel 
      :position="[-1, 0, 0]"
      :scale="[.1, .1, .1,]"
      :path="'cubito.glb'" />
    </Suspense>
    <Suspense>
      <TheModel :position="[1, 0, 0]"
      :scale="[.1, .1, .1,]"
      :path="'cubito.glb'" />
    </Suspense>
    <Suspense>
      <TheModel :position="[2, 0, 0]"
      :scale="[.1, .1, .1,]"
      :path="'cubito.glb'" />
    </Suspense>
    
    <TresGridHelper :args="[size, divisions]" />
  

    <TresFog v-bind="{ color: '#fff', near: '50', far: '80' }" />
  </TresCanvas>
  <div id="testContainer" ref="testContainer"></div>
</template>

<style scoped>
#canvas {
  width: 100%;
  height: 100%;
}
</style>
