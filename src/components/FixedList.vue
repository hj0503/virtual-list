<script setup lang="ts">
import { ref, onMounted, nextTick } from 'vue'

const containerRef = ref<Element | null>(null)
const itemSize = ref(40)
const itemCount = ref(1000)
const contentHeight = ref(itemSize.value * itemCount.value)
const buffer = ref(4)
const startIndex = ref(0)
const endIndex = ref(0)
const bufferStartIndex = ref(0)
const bufferEndIndex = ref(0)
const data = new Array(itemCount.value).fill(0).map((_, i) => i)
const currentData = ref<number[]>([])

const setIndexData = (el: any) => {
  const { scrollTop, offsetHeight } = el
  // 可视区起始索引
  startIndex.value = Math.floor(scrollTop / itemSize.value)
  // 上缓冲区起始索引
  bufferStartIndex.value = Math.max(0, startIndex.value - buffer.value)
  // 可视区结束索引
  endIndex.value = startIndex.value + Math.ceil(offsetHeight / itemSize.value)
  // 下缓冲区结束索引
  bufferEndIndex.value = Math.min(itemCount.value, endIndex.value + buffer.value)

  currentData.value = data.slice(bufferStartIndex.value, bufferEndIndex.value)
}
const handleScroll = (event: any) => {
  setIndexData(event.target)
}
const listenerContainerScroll = () => {
  if (!containerRef.value) return
  containerRef.value.addEventListener('scroll', handleScroll)
}

onMounted(() => {
  setIndexData(containerRef.value)
  listenerContainerScroll()
})
</script>

<template>
  <div ref="containerRef" class="container">
    <div class="content" :style="{ height: contentHeight + 'px' }">
      <div
        v-for="(item, index) in currentData"
        :key="item"
        class="item"
        :style="{
          height: itemSize + 'px',
          position: 'absolute',
          width: '100%',
          top: (bufferStartIndex + index) * itemSize + 'px'
        }"
      >
        {{ item }}
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.container {
  position: relative;
  overflow: auto;
  width: 200px;
  height: 300px;
  border: 1px solid #000;
  .content {
    width: 100%;
  }
}
</style>
