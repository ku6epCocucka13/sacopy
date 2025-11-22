<template>
  <div class="home-page">
    <header class="page-header">
      <h1>Калькулятор сетей</h1>
      <p>Расчет параметров IP-сетей и подсетей</p>
    </header>
    
    <main class="page-content">
      <SubnetCalculator />
    </main>
  </div>
</template>

<script setup lang="ts">
import SubnetCalculator from '../components/SubnetCalculator.vue'
</script>

<style scoped>
.home-page {
  min-height: 100vh;
  background: var(--bg-primary);
}

.page-header {
  text-align: center;
  padding: 40px 20px;
  background: var(--bg-secondary);
}

.page-header h1 {
  color: var(--text-primary);
  margin-bottom: 12px;
  font-size: 2.5rem;
}

.page-header p {
  color: var(--text-secondary);
  font-size: 1.1rem;
}

.page-content {
  padding: 40px 20px;
}
</style>