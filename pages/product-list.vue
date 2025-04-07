<script setup lang="ts">
import { h } from 'vue'
import { getPaginationRowModel } from '@tanstack/vue-table'
import type { TableColumn } from '@nuxt/ui'

interface Product {
  id: number
  title: string
  description: string
  price: number
  rating: number
  brand: string
  category: string
  thumbnail: string
}

useHead({
  title: "Список продуктів"
})

const { data, status } = await useFetch<Product[]>('https://dummyjson.com/products', {
  key: 'products-list',
  transform: (response: any) => response.products || []
})

const columns: TableColumn<Product>[] = [
  {
    accessorKey: 'title',
    header: ({ column }) => h('button', {
      class: 'text-left font-bold',
      onClick: () => column.toggleSorting(column.getIsSorted() === 'asc')
    }, 'Назва'),
    cell: ({ row }) => row.getValue('title')
  },
  {
    accessorKey: 'description',
    header: 'Опис',
    cell: ({ row }) => row.getValue('description')
  },
  {
    accessorKey: 'price',
    header: 'Ціна',
    cell: ({ row }) => `$${row.getValue('price')}`
  },
  {
    accessorKey: 'rating',
    header: 'Оцінка',
    cell: ({ row }) => {
      const rating = row.getValue('rating') as number
      return h('span', {
        class: rating < 4.5 ? 'text-red-500' : 'text-green-500'
      }, rating.toString())
    }
  },
  {
    accessorKey: 'brand',
    header: 'Бренд',
    cell: ({ row }) => row.getValue('brand')
  },
  {
    accessorKey: 'category',
    header: 'Категорія',
    cell: ({ row }) => row.getValue('category')
  },
  {
    accessorKey: 'thumbnail',
    header: 'Фото',
    cell: ({ row }) => h('img', {
      src: row.getValue('thumbnail'),
      alt: row.getValue('title'),
      class: 'w-[100px] h-[100px] object-cover'
    })
  }
]

const pagination = ref({
  pageIndex: 0,
  pageSize: 5
})

const filter = ref('')
const table = useTemplateRef('table')

const tableData = computed(() => data.value || [])
</script>

<template>
  <div class="p-6">
    <h1 class="text-2xl font-bold mb-4">Список продуктів</h1>

    <UInput
        v-model="filter"
        placeholder="Пошук за назвою..."
        class="mb-4 max-w-sm"
        @update:model-value="table?.tableApi?.getColumn('title')?.setFilterValue($event)"
    />

    <UTable
        ref="table"
        :data="tableData"
        :columns="columns"
        :loading="status === 'pending'"
        v-model:pagination="pagination"
        :pagination-options="{ getPaginationRowModel: getPaginationRowModel() }"
        class="flex-1"
    />

    <div class="flex justify-center mt-4">
      <UPagination
          :default-page="(table?.tableApi?.getState().pagination.pageIndex || 0) + 1"
          :items-per-page="table?.tableApi?.getState().pagination.pageSize"
          :total="table?.tableApi?.getFilteredRowModel().rows.length"
          @update:page="(p) => table?.tableApi?.setPageIndex(p - 1)"
      />
    </div>
  </div>
</template>