<script setup lang="ts">
import { ref, useTemplateRef } from "vue";
import { Tiles } from '@/data/tiles'
import TileDraggable from "@/components/TileDraggable.vue";
import type {Tile} from "@/types.ts";

const boardRef = useTemplateRef('board')
const tiles = ref(
    Tiles.map((t, index) => {
      const pos = getInitializedPosition(index)
      return {
        ...t,
        rotationY: 0,
        isFaceUp: false,
        x: pos.x,
        y: pos.y,
      }
    })
)

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

function flipTile(tile : Tile) {
  tile.isFaceUp = !tile.isFaceUp
  tile.rotationY = tile.isFaceUp ? 0: 180
}

function bringToFront(tile : Tile) {
  const index = tiles.value.indexOf(tile);
  tiles.value.splice(index, 1)
  tiles.value.push(tile)
}

function orderTiles() {
  tiles.value.forEach(tile => {
    const pos = getInitializedPosition(tile.id)
    tile.rotationY = 0
    tile.isFaceUp = false
    tile.x = pos.x
    tile.y = pos.y
  })
}

function resetGame() {
  const board = boardRef.value;
  const boardWidth = board ? board.clientWidth : 800;
  const boardHeight = board ? board.clientHeight : 1000;
  const tileWidth = 25, tileHeight = 36

  shuffleAllTiles(tiles.value)

  tiles.value.forEach(tile => {
    tile.isFaceUp = false
    tile.rotationY = 180
    tile.x = Math.random() * (boardWidth - tileWidth)
    tile.y = Math.random() * (boardHeight - tileHeight)
  })

}

function shuffleAllTiles(arr: Array<Tile | undefined>) {
// Fisher-Yates Shuffle
  for (let i = arr.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [arr[i], arr[j]] = [arr[j], arr[i]]
  }
}

</script>

<template>
  <h1>Mahjong Test</h1>

  <button @click="resetGame()">Shuffle Tiles</button>
  <button @click="orderTiles()">Order Tiles</button>

  <div ref="board" class="board">
    <TileDraggable
        v-for="tile in tiles"
        :key="tile.id"
        :container-ref="boardRef"
        class="tile"
        :initial-x="tile.x"
        :initial-y="tile.y"
        :prevent-default="true"
        @click="flipTile(tile)"
        @pick="bringToFront(tile)"
    >
      <div
          class="tile-3d"
          :style="{ transform: `rotateY(${tile.rotationY}deg)` }"
      >
        <div class="face front">
          <img :src="tile.src" :alt="tile.name" />
        </div>
        <div class="face back"></div>
      </div>
    </TileDraggable>
  </div>

</template>

<style scoped>
.board {
  position: relative;
  perspective: 800px;
  min-height: 100vh;
}
.tile {
  position: absolute;
  width: 25px;
  height: 36px;
}
.tile-3d {
  position: relative;
  width: 100%;
  height: 100%;
  transform-style: preserve-3d;
  transition: transform 0.4s;
  cursor: pointer;
}
.face {
  position: absolute;
  inset: 0;
  border-radius: 3px;
  background: center / contain no-repeat;
  backface-visibility: hidden;
  box-shadow: 1px 1px rgba(0, 0, 0, 0.3);
}

.front {
  background-image: url('/img/tiles/Front.svg');
  padding: 5px;
}

.front img {
  width: 100%;
  height: 100%;
}

.back {
  background-image: url('/img/tiles/Back.svg');
  transform: rotateY(180deg);
}
</style>
