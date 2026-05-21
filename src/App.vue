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
import fs from 'fs';
import path from 'path';

const loadedComponents = ref([]);
const debugInfo = ref("");

onMounted(async () => {
  console.log("=== onMounted Debug Info ===");
  
  let debug = "";
  
  try {
    // Dynamically import all .vue files from generated directory
    const modules = import.meta.glob("../generated/*.vue", {
      eager: true,
      import: "default"
    });
    
    console.log("Glob modules:", modules);
    console.log("Module keys:", Object.keys(modules));
    
    if (Object.keys(modules).length === 0) {
      debug += "❌ Glob found 0 modules\n";
      debug += "Trying alternative loading method...\n";
      
      // Fallback: try importing specific files
      const componentNames = ["Hero", "Features", "CTA"];
      const loadedModules = [];
      
      for (const name of componentNames) {
        try {
          const module = await import(`../generated/${name}.vue`);
          console.log(`✅ Loaded ${name}.vue`);
          loadedModules.push({
            name,
            module: module.default,
            config: module.componentConfig || { title: name, description: "Auto-generated component" }
          });
          debug += `✅ Loaded ${name}.vue\n`;
        } catch (err) {
          console.warn(`⚠️ Failed to load ${name}.vue:`, err);
          debug += `⚠️ Failed to load ${name}.vue: ${err.message}\n`;
        }
      }
      
      loadedComponents.value = loadedModules;
    } else {
      // Use glob results
      loadedComponents.value = Object.entries(modules).map(([path, module]) => {
        const name = path.split("/").pop().replace(".vue", "");
        console.log(`Processing: ${name}`, module);
        
        return {
          name,
          module: module,
          config: module.componentConfig || { title: name, description: "Auto-generated component" }
        };
      });
      
      debug += `✅ Glob found ${Object.keys(modules).length} components\n`;
      loadedComponents.value.forEach(comp => {
        debug += `  - ${comp.name}\n`;
      });
    }

    console.log(`✅ Loaded ${loadedComponents.value.length} components`);
    console.log("loadedComponents:", loadedComponents.value);
    
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
