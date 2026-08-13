<script setup lang="ts">
import {UseDraggable as Draggable} from '@vueuse/components'


</script>

<template>
  <h1>Mahjong Test</h1>
  <Draggable
      p="x-4 y-2"
      style="position: fixed;"
      :initial-value="{ x: 10, y: 60}"
      :prevent-default="true"
  >
    <img class="tile" src="/img/tiles/Pin1.svg" alt="Pin 1 tile"/>
  </Draggable>

</template>

<style scoped>
.tile {
  width: 25px;
  height: 36px;
  border: 1px solid #ccc;
  border-radius: 2px;
  padding: 5px;
}
</style>
