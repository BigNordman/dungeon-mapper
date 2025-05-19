<template>
  <div class="app-container">
    <!-- Сайдбар -->
    <div class="sidebar" :class="{ 'sidebar-collapsed': isSidebarCollapsed }">
      <div class="sidebar-content">
        <h3 v-if="!isSidebarCollapsed">Меню</h3>
        <ul>
          <li v-if="!isSidebarCollapsed">Элемент 1</li>
          <li v-if="!isSidebarCollapsed">Элемент 2</li>
          <li v-if="!isSidebarCollapsed">Элемент 3</li>
        </ul>
      </div>
      <button class="toggle-btn" @click="toggleSidebar">
        {{ isSidebarCollapsed ? '>' : '<' }}
      </button>
    </div>

    <!-- Основное содержимое -->
    <div class="main-content" :class="{ 'expanded': isSidebarCollapsed }">
      <h1>Рабочая область</h1>
      <p>Основное содержимое вашего приложения</p>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import { invoke } from "@tauri-apps/api/core";

const greetMsg = ref("");
const name = ref("");

async function greet() {
  // Learn more about Tauri commands at https://tauri.app/develop/calling-rust/
  greetMsg.value = await invoke("greet", { name: name.value });
}

export default defineComponent({
  name: 'App',
  setup() {
    const isSidebarCollapsed = ref(false);

    const toggleSidebar = () => {
      isSidebarCollapsed.value = !isSidebarCollapsed.value;
    };

    return {
      isSidebarCollapsed,
      toggleSidebar,
    };
  },
});
</script>

<style>
/* Базовые стили */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.app-container {
  display: flex;
  height: 100vh;
  font-family: Arial, sans-serif;
}

/* Стили сайдбара */
.sidebar {
  width: 250px;
  background-color: #2c3e50;
  color: white;
  transition: width 0.3s ease;
  position: relative;
  display: flex;
  flex-direction: column;
}

.sidebar-collapsed {
  width: 50px;
}

.sidebar-content {
  padding: 20px;
  flex-grow: 1;
  overflow-y: auto;
}

.sidebar h3 {
  margin-bottom: 20px;
}

.sidebar ul {
  list-style: none;
}

.sidebar li {
  padding: 10px 0;
  cursor: pointer;
}

.sidebar li:hover {
  background-color: #34495e;
}

/* Кнопка переключения */
.toggle-btn {
  background-color: #34495e;
  color: white;
  border: none;
  padding: 10px;
  cursor: pointer;
  width: 100%;
  text-align: center;
  transition: background-color 0.3s;
}

.toggle-btn:hover {
  background-color: #3d566e;
}

/* Основное содержимое */
.main-content {
  flex-grow: 1;
  padding: 20px;
  background-color: #ecf0f1;
  transition: margin-left 0.3s ease;
  overflow-y: auto;
}

.main-content.expanded {
  margin-left: -200px;
}

/* Адаптивность */
@media (max-width: 768px) {
  .sidebar {
    width: 200px;
  }
  
  .sidebar-collapsed {
    width: 40px;
  overflow: hidden;
  }
}
</style>