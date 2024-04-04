<template>
  <div class="loader" :class="{ visible: loading }">Loading...</div>
</template>

<script setup lang="ts">
defineProps<{
  loading: boolean
}>()
</script>

<style scoped lang="scss">
.loader {
  display: flex;
  visibility: hidden;
  align-items: center;
  justify-content: center;
  padding: 1rem;
  border-color: lightgrey;
  border-width: 0 1px 1px;
  border-style: solid;
  background-color: aliceblue;

  &.visible {
    visibility: visible;
  }
}
</style>
