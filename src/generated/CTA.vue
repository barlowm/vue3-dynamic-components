<template>
  <div class="component-cta">
    <h2>Call to Action</h2>
    <p>Ready to get started?</p>
    <div class="content">
      <p>Modify the .txt files in src/config/ and run npm run generate-components</p>
    </div>
    <div class="meta">
      <small>Generated: 2026-05-20T15:27:41.049Z</small>
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
.component-cta {
  padding: 1rem;
  border: 1px solid #ddd;
  border-radius: 8px;
  background: linear-gradient(135deg, #4facfe, #00f2fe);
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
