<script setup lang="ts">
import { h } from 'vue'
import { getPaginationRowModel } from '@tanstack/vue-table'
import type { TableColumn } from '@nuxt/ui'

useHead({
  title: "Головна сторінка"
})

const products = ref([
  { id: 1, name: 'Яблуко', quantity: '1 кг' },
  { id: 2, name: 'Молоко', quantity: '1 літр' },
  { id: 3, name: 'Сир', quantity: '200 г' },
  { id: 4, name: 'Хліб', quantity: '1 шт' },
  { id: 5, name: 'Яйця', quantity: '10 шт' }
])

const columns: TableColumn<any>[] = [
  {
    accessorKey: 'id',
    header: 'ID',
    cell: ({ row }) => row.getValue('id')
  },
  {
    accessorKey: 'name',
    header: ({ column }) => h('button', {
      class: 'text-left font-bold',
      onClick: () => column.toggleSorting(column.getIsSorted() === 'asc')
    }, 'Назва'),
    cell: ({ row }) => row.getValue('name')
  },
  {
    accessorKey: 'quantity',
    header: 'Кількість',
    cell: ({ row }) => row.getValue('quantity')
  }
]

const pagination = ref({
  pageIndex: 0,
  pageSize: 3
})

const filter = ref('')
const table = useTemplateRef('table')
</script>

<template>
  <div class="p-6">
    <h1 class="text-2xl font-bold mb-4">Головна сторінка</h1>
    <p class="mb-4">Перейдіть до <a href="/product-list" class="text-blue-500 underline">Списку продуктів</a> (тут завдання 9 з API)</p>

    <h2 class="text-xl font-semibold mb-2">Приклад таблиці продуктів (це завдання 8)</h2>

    <UInput
        v-model="filter"
        placeholder="Пошук за назвою..."
        class="mb-4 max-w-sm"
        @update:model-value="table?.tableApi?.getColumn('name')?.setFilterValue($event)"
    />

    <UTable
        ref="table"
        :data="products"
        :columns="columns"
        v-model:pagination="pagination"
        :pagination-options="{ getPaginationRowModel: getPaginationRowModel() }"
        class="flex-1 mb-4"
    />

    <div class="flex justify-center">
      <UPagination
          :default-page="(table?.tableApi?.getState().pagination.pageIndex || 0) + 1"
          :items-per-page="table?.tableApi?.getState().pagination.pageSize"
          :total="table?.tableApi?.getFilteredRowModel().rows.length"
          @update:page="(p) => table?.tableApi?.setPageIndex(p - 1)"
      />
    </div>
  </div>
</template>