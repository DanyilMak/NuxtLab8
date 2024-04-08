<script setup lang="ts">
import { ref, computed } from 'vue';
const { pending,  data } = await useLazyAsyncData<any>('products',()=> $fetch('https://dummyjson.com/products') as any);
useHead({
  title: "Products"
})
const products = data.value.products;
const columns = [{
  key: 'id',
  label: 'ID'
}, {
  key: 'title',
  label: 'Title'
}, {
  key: 'description',
  label: 'Description'
}, {
  key: 'price',
  label: 'Price'
}, {
  key: 'rating',
  label: 'Rating',
  slot: 'rating-data'
}, {
  key: 'brand',
  label: 'Brand'
}, {
  key: 'category',
  label: 'Category'
}, {
  key: 'thumbnail',
  label: 'Thumbnail',
  slot: 'thumbnail-data'
}]

const q = ref('')
const page = ref(1)
const pageCount = 5

const filteredRows = computed(() => {
  if (!q.value) {
    return products
  }

  return products.filter((product: { [s: string]: unknown; } | ArrayLike<unknown>) => {
    return Object.values(product).some((value) => {
      return String(value).toLowerCase().includes(q.value.toLowerCase())
    })
  })
})

const paginatedRows = computed(() => {
  const startIndex = (page.value - 1) * pageCount
  return filteredRows.value.slice(startIndex, startIndex + pageCount)
})
</script>

<template>
  <div>
    <div class="flex px-3 py-3.5 border-b border-gray-200 dark:border-gray-700">
      <UInput v-model="q" placeholder="Filter products..." />
    </div>

    <UTable :rows="paginatedRows" :columns="columns">
      <template #rating-data="{ row }">
        <span :style="{ color: row.rating > 4.5 ? 'green' : 'red' }">
          {{ row.rating }}
        </span>
      </template>
      <template #thumbnail-data="{ row }">
        <img class="w-[100px] h-[100px]" :src="row.thumbnail" alt="Thumbnail" />
      </template>
    </UTable>

    <div class="flex justify-end px-3 py-3.5 border-t border-gray-200 dark:border-gray-700">
      <UPagination v-model="page" :page-count="pageCount" :total="filteredRows.length" />
    </div>
  </div>
</template>
