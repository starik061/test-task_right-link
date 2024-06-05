<template>
   <div>
      <template v-if="!isError">
         <TreeViewItem v-for="item in rootItems" :key="item.id" :item="item" :all-items="items" />
      </template>

      <ErrorDescription v-else />
   </div>
</template>

<script setup>
import TreeViewItem from '@/components/TreeViewItem.vue';
import ErrorDescription from '@/components/ErrorDescription.vue';

import { ref, onMounted, computed } from 'vue';

const items = ref([
]);
const isError = ref(false);

const rootItems = computed(() => {
   return items.value.filter(i => i.parent_id === 0);
});


onMounted(async () => {
   const apiUrl = import.meta.env.VITE_API_URL;

   try {
      const response = await fetch(apiUrl);
      if (!response.ok) {
         throw new Error('Network response was not ok' + response.statusText);
      }
      items.value = await response.json();
      console.log(items.value);
   } catch (error) {
      isError.value = true;
   }
});
</script>
