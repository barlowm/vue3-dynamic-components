<template>
  <div id="app" class="container">
    <header>
      <h1>Vue 3 Dynamic Components</h1>
      <p>Components generated from configuration files</p>
    </header>

    <div v-if="loadedComponents.length === 0" class="debug-info">
      <p style="color: red; font-weight: bold;">⚠️ No components loaded</p>
      <pre style="background: #f0f0f0; padding: 1rem; border-radius: 4px; overflow-x: auto;">{{ debugInfo }}</pre>
    </div>

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

// Move import.meta.glob to module-level scope (required by Vite)
const componentModules = import.meta.glob("../generated/*.vue", {
  eager: true,
});

const loadedComponents = ref([]);
const debugInfo = ref("");

onMounted(() => {
  console.log("=== onMounted Debug Info ===");
  console.log("componentModules keys:", Object.keys(componentModules));
  console.log("componentModules:", componentModules);
  
  let debug = `Glob paths found: ${Object.keys(componentModules).length}\n\n`;
  
  try {
    if (Object.keys(componentModules).length === 0) {
      debug += "❌ No components found by import.meta.glob!\n";
      debug += "Expected path: ../generated/*.vue\n";
      debug += "Make sure generated directory has .vue files";
      debugInfo.value = debug;
      console.error(debug);
      return;
    }

    loadedComponents.value = Object.entries(componentModules).map(
      ([path, module]) => {
        console.log(`Processing: ${path}`, module);
        const name = path.split("/").pop().replace(".vue", "");
        
        if (!module.default) {
          console.warn(`⚠️ No default export for ${path}`);
        }
        
        if (module.componentConfig) {
          console.log(`✅ Config found for ${name}:`, module.componentConfig);
        } else {
          console.warn(`⚠️ No componentConfig export for ${name}`);
        }
        
        return {
          name,
          module: module.default,
          config: module.componentConfig || { title: name, description: "Auto-generated component" },
        };
      },
    );

    console.log(`✅ Loaded ${loadedComponents.value.length} components`);
    console.log("loadedComponents:", loadedComponents.value);
    
    debug += `✅ Successfully loaded ${loadedComponents.value.length} components\n`;
    loadedComponents.value.forEach(comp => {
      debug += `  - ${comp.name}: ${comp.config.title}\n`;
    });
  } catch (error) {
    console.error("❌ Error loading components:", error);
    debug += `❌ Error: ${error.message}\n${error.stack}`;
  }
  
  debugInfo.value = debug;
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

.debug-info {
  background: #fff3cd;
  border: 2px solid #ff9800;
  border-radius: 8px;
  padding: 1.5rem;
  margin-bottom: 2rem;
}

.components-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 2rem;
  margin-top: 2rem;
}
</style>
