<template>
  <UApp>
    <header>
      <nav>
        <a href="/">Головна</a>
        <a href="/product-list">Список продуктів</a>
      </nav>
    </header>
    <NuxtRouteAnnouncer />
    <NuxtPage />
  </UApp>
</template>

<style scoped>
header {
  padding: 1rem;
  background-color: #f8f8f8;
}
nav a {
  margin-right: 1rem;
}
</style>