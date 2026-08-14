<script setup lang="ts">
import { UseDraggable as Draggable } from '@vueuse/components'
import { ref } from "vue";
import { Tiles } from '@/data/tiles.js'

const tiles = ref(Tiles)

const TILE_SPACING_X = 30
const TILE_SPACING_Y = 45
const TILES_PER_ROW = 14

function getInitializedPosition(index : number) {
  const col = index % TILES_PER_ROW;
  const row = Math.floor(index / TILES_PER_ROW);
  return {
    x: 10 + col * TILE_SPACING_X,
    y: 60 + row * TILE_SPACING_Y
  }
}

</script>

<template>
  <h1>Mahjong Test</h1>

  <div style="position: relative";>
    <Draggable
        v-for="(tile, index) in tiles"
        :key="tile.id"
        class="tile"
        :initial-value="getInitializedPosition(index)"
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
