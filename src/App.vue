<script setup lang="ts">
import {UseDraggable as Draggable} from '@vueuse/components'
import {ref} from "vue";

const tiles = ref(
    [
      {
        id: 0,
        src: '/img/tiles/Pin1.svg',
      },
      {
        id: 1,
        src: '/img/tiles/Pin2.svg',
      }
    ]
)

// computed property for positions

</script>

<template>
  <h1>Mahjong Test</h1>

  <Draggable
      v-for="tile in tiles"
      :key="tile.id"
      class="tile"
      :initial-value="{ x: 10, y: 60}"
      :prevent-default="true"
  >
    <img :src="tile.src" alt="tile" />
  </Draggable>

</template>

<style scoped>
.tile {
  position: fixed;
  width: 25px;
  height: 36px;
  border: 1px solid #ccc;
  border-radius: 2px;
  padding: 5px;
}
.tile img {
  width: 100%;
}
</style>
