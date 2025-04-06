<script setup>
useHead({
  title: "Список продуктів"
});

const products = [
  { id: 1, name: 'Яблуко', quantity: '1 кг' },
  { id: 2, name: 'Молоко', quantity: '1 літр' },
  { id: 3, name: 'Сир', quantity: '200 г' },
  { id: 4, name: 'Хліб', quantity: '1 шт' },
  { id: 5, name: 'Яйця', quantity: '10 шт' }
];
</script>

<template>
  <div class="p-6">
    <h1 class="text-2xl font-bold mb-4">Список продуктів</h1>
    <ul class="space-y-2">
      <li
          v-for="product in products"
          :key="product.id"
          class="p-4 border rounded-lg shadow-sm bg-white"
      >
        <strong>{{ product.name }}</strong> — {{ product.quantity }}
      </li>
    </ul>
  </div>
</template>
