<template>
  <div id="app" class="container">
    <header>
      <h1>Vue 3 Dynamic Components</h1>
      <p>Components generated from configuration files</p>
    </header>

    <div class="components-grid">
      <component
        v-for="comp in loadedComponents"
        :key="comp.name"
        :is="comp.module"
        :title="comp.config.title"
        :description="comp.config.description"
      />
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";

const loadedComponents = ref([]);

onMounted(async () => {
  console.log("onMounted function entry point");
  debugger;
  try {
    // Dynamically import all generated components
    // const componentModules = import.meta.glob("../generated/*.vue", {
    const componentModules = import.meta.glob("../../../generated/*.vue", {
      eager: true,
    });
    console.log("onMounted componentModules imported");

    loadedComponents.value = Object.entries(componentModules).map(
      ([path, module]) => {
        const name = path.split("/").pop().replace(".vue", "");
        return {
          name,
          module: module.default,
          config: module.default,
        };
      },
    );

    console.log(`Loaded ${loadedComponents.value.length} components`);
  } catch (error) {
    console.error("Error loading components:", error);
  }
});
</script>

<style scoped>
#app {
  max-width: 1200px;
  margin: 0 auto;
  padding: 2rem;
  font-family:
    -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
}

header {
  text-align: center;
  margin-bottom: 2rem;
  padding-bottom: 1rem;
  border-bottom: 2px solid #667eea;
}

header h1 {
  margin: 0 0 0.5rem 0;
  color: #333;
}

header p {
  margin: 0;
  color: #666;
}

.components-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 2rem;
  margin-top: 2rem;
}
</style>
