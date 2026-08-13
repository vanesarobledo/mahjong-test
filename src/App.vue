<script setup lang="ts">
import {UseDraggable as Draggable} from '@vueuse/components'
import {ref} from "vue";
import {Tiles} from '@/data/tiles.js'

const tiles = ref(Tiles)

// computed property for positions

</script>

<template>
  <h1>Mahjong Test</h1>

  <div style="display: flex">
  <Draggable
      v-for="tile in tiles"
      :key="tile.id"
      class="tile"
      :initial-value="{ x: 10, y: 60}"
      :prevent-default="true"
  >
    <img :src="tile.src" alt="tile" />
  </Draggable>
  </div>

</template>

<style scoped>
.tile {
  position: fixed;
  width: 25px;
  height: 36px;
  border-radius: 3px;
  padding: 5px;
  background: center / contain no-repeat url('/img/tiles/Front.svg');
}
.tile img {
  width: 100%;
}
</style>
