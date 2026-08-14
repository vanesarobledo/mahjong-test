<script setup lang="ts">
import {useDraggable} from "@vueuse/core";
import {useTemplateRef, watch} from "vue";

const props = defineProps<{
  initialX: number
  initialY: number
  containerRef: HTMLElement | null
}>()

const emit = defineEmits<{ (e: 'pick'): void }>()

const el = useTemplateRef('el')
const { x, y, position, style } = useDraggable(el, {
  initialValue: { x: props.initialX, y: props.initialY },
  containerElement: () => props.containerRef,
  preventDefault: true,
  onStart: () => emit('pick'),
})

watch(
    [() => props.initialX, () => props.initialY],
    ([newX, newY]) => {
      position.value = { x: newX, y: newY}
    }
)

</script>

<template>
  <div ref="el" :style="style" class="tile-wrapper">
    <slot />
  </div>
</template>=