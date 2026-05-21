<template>
  <div class="component-hero">
    <h2>Hero Component</h2>
    <p>Welcome to the dynamic component system</p>
    <div class="content">
      <p>This component was automatically generated from a text configuration file!</p>
    </div>
    <div class="meta">
      <small>Generated: 2026-05-20T15:27:41.054Z</small>
    </div>
  </div>
</template>

<script setup>
defineProps({
  title: String,
  description: String
});
</script>

<style scoped>
.component-hero {
  padding: 1rem;
  border: 1px solid #ddd;
  border-radius: 8px;
  background: linear-gradient(135deg, #667eea, #764ba2);
  color: white;
}

h2 {
  margin-top: 0;
  font-size: 1.5rem;
}

.meta {
  margin-top: 1rem;
  opacity: 0.8;
}
</style>
