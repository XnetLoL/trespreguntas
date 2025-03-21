<script setup lang="ts">
import { onMounted } from 'vue'
import { TresCanvas } from '@tresjs/core'
import { CameraControls} from '@tresjs/cientos'
import TheModel from '@/components/TheModel.vue'
import RenderControl from '@/components/RenderControl.vue'
const size = 100
const divisions = size * 4

onMounted(() => {
})
</script>

<template>
  <TresCanvas clear-color="#FFFFFF" window-size >
    <RenderControl />
    <TresPerspectiveCamera :position="[3, 2, 5]" :look-at="[0, 5, 0]" />
    <CameraControls />

    <TresDirectionalLight :intensity="2" :position="[3, 3, 3]" cast-shadow />
    <TresAmbientLight :intensity="1" />

    <Suspense>
      <TheModel :position="[1, 0, 0]" />
    </Suspense>
    <Suspense>
      <TheModel :position="[-1, 0, 0]" />
    </Suspense>
    
    <TresGridHelper :args="[size, divisions]" />
  

    <!-- <TresFog v-bind="{ color: '#F78B3D', near: '10', far: '60' }" /> -->
  </TresCanvas>
  <div id="testContainer" ref="testContainer"></div>
</template>

<style scoped>
#canvas {
  width: 100%;
  height: 100%;
}
</style>
