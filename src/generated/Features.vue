<template>
  <div class="component-features">
    <h2>Features</h2>
    <p>Key features of this application</p>
    <div class="content">
      <p>Components are generated automatically whenever you change configuration files</p>
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
.component-features {
  padding: 1rem;
  border: 1px solid #ddd;
  border-radius: 8px;
  background: linear-gradient(135deg, #f093fb, #f5576c);
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
