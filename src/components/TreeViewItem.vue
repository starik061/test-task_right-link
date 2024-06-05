<template>
  <div>
    <div>{{ item.name }}</div>
    <div v-if="children.length > 0" class="children">
      <TreeViewItem v-for="child in children" :key="child.id" :item="child" :all-items="allItems" />
    </div>
  </div>
</template>

<script setup>
import { computed, defineProps } from 'vue';

const props = defineProps({
  item: Object,
  allItems: Array
});

const children = computed(() => {
  return props.allItems.filter(i => i.parent_id === props.item.id);
});
</script>

<style scoped>
.children {
  margin-left: 20px;
}
</style>
