<template>
  <div class="content">
    <dynamic-list
      :data="list"
      :height="600"
      :width="300"
      :itemCount="1000"
      :itemEstimatedSize="10"
      :buffCount="50"
    >
      <template #slot-scope="{ slotProps }">
        <div class="li">{{ slotProps.data.text }}</div>
      </template>
    </dynamic-list>
  </div>
</template>

<script setup lang="ts">
import DynamicList from '../components/dynamic-list/index.vue'
import { ref, onMounted } from 'vue'

const list = ref<any>([])
onMounted(() => {
  for (let i = 0; i < 1000; i++) {
    list.value.push({ text: i + '这是滚动列表的数据！！！' })
  }
})
</script>

<style scoped>
.content {
  background: #fff;
  overflow: hidden;

  .li {
    height: 32px;
    line-height: 32px;
    color: #000;
    font-size: 14px;
  }
}
</style>
