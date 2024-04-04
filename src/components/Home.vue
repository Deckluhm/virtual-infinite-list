<template>
  <VirtualInfiniteList
    :items="items"
    :item-height="50"
    :load-next-page="loadNextPage"
    class="container"
  >
    <template v-slot="{ item }">
      <div class="item">
        {{ item.data }}
      </div>
    </template>
  </VirtualInfiniteList>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import VirtualInfiniteList from './virtual-infinite-list/VirtualInfiniteList.vue'
import { faker } from '@faker-js/faker'

function generateElements(n: number) {
  return [...Array(25).keys()].map(() => faker.person.fullName())
}

const items = ref(generateElements(25))

// Just to emulate API call
function sleep(ms: number) {
  return new Promise(resolve => setTimeout(resolve, ms))
}

const loadNextPage = async () => {
  await sleep(1_000)
  items.value.push(...generateElements(5))
}
</script>

<style scoped lang="scss">
.container {
  overflow: scroll;
  width: 500px;
  height: 525px;
  border-top: lightgrey 1px solid;
  background-color: white;

  .item {
    display: flex;
    align-items: center;
    height: 50px;
    padding: 1rem;
    border-color: lightgrey;
    border-width: 0 1px 1px;
    border-style: solid;
  }
}
</style>
./virtual-infinite-list/VirtualInfiniteList.vue