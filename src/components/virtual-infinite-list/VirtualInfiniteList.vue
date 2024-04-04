<template>
  <div :ref="containerProps.ref" @scroll="containerProps.onScroll" :style="containerProps.style">
    <div id="wrapper" v-bind="wrapperProps">
      <DataLoader v-if="loadPreviousPage" :loading="isLoadingPreviousPage" />
      <div v-for="item in list">
        <slot v-bind="{ item }"></slot>
      </div>
      <DataLoader v-if="loadNextPage" :loading="isLoadingNextPage" />
    </div>
  </div>
</template>

<script setup lang="ts">
import { useVirtualList, useScroll, type UseScrollReturn } from '@vueuse/core'
import { onMounted, ref } from 'vue'
import DataLoader from './DataLoader.vue';

const props = defineProps<{
  items: any[]
  itemHeight: number
  loadPreviousPage?: () => Promise<void>
  loadNextPage?: () => Promise<void>
}>()

const isLoadingPreviousPage = ref(false)
const isLoadingNextPage = ref(false)

const { list, containerProps, wrapperProps } = useVirtualList(props.items, {
  itemHeight: props.itemHeight
})

let scroll: UseScrollReturn | undefined = undefined

onMounted(() => {
  scroll = useScroll(containerProps.ref, {
    async onStop() {
      if (!scroll) return
      const { top, bottom } = scroll.arrivedState
      if (!top && !bottom) return

      if (bottom) {
        if (!props.loadNextPage) return
        isLoadingNextPage.value = true
        await props.loadNextPage()
        isLoadingNextPage.value = false
      }
      else if (top) {
        if (!props.loadPreviousPage) return
        isLoadingPreviousPage.value = true
        await props.loadPreviousPage()
        isLoadingPreviousPage.value = false
      }
    }
  })
})
</script>

<style scoped lang="scss">
.loader {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 1rem;
}
</style>
