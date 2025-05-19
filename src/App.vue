<template>
  <div class="app-container" :class="{ 'collapsed': isSidebarCollapsed }">
    <!-- Сайдбар -->
    <div class="sidebar" :class="{ 'sidebar-collapsed': isSidebarCollapsed }">
      <!-- Кнопка переключения теперь всегда вверху -->
      <button class="toggle-btn" @click="toggleSidebar">
        {{ isSidebarCollapsed ? '>' : '<' }}
      </button>
      
      <div class="sidebar-content">
        <h3 v-if="!isSidebarCollapsed">Меню</h3>
        <ul v-if="!isSidebarCollapsed">
          <li>Элемент 1</li>
          <li>Элемент 2</li>
          <li>Элемент 3</li>
        </ul>
      </div>
    </div>

    <!-- Основное содержимое -->
    <div class="main-content">
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
  display: grid;
  grid-template-columns: 250px 1fr;
  height: 100vh;
  font-family: Arial, sans-serif;
  transition: grid-template-columns 0.3s ease;
}

.app-container.collapsed {
  grid-template-columns: 50px 1fr;
}

/* Стили сайдбара */
.sidebar {
  background-color: #2c3e50;
  color: white;
  display: flex;
  flex-direction: column;
  width: 100%;
}

/* Кнопка переключения теперь вверху */
.toggle-btn {
  background-color: #34495e;
  color: white;
  border: none;
  padding: 10px;
  cursor: pointer;
  width: 100%;
  text-align: center;
  transition: background-color 0.3s;
  order: -1; /* Перемещаем кнопку в начало flex-контейнера */
}

.toggle-btn:hover {
  background-color: #3d566e;
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

/* Основное содержимое */
.main-content {
  padding: 20px;
  background-color: #ecf0f1;
  overflow-y: auto;
}

/* Адаптивность */
@media (max-width: 768px) {
  .app-container {
    grid-template-columns: 200px 1fr;
  }
  
  .app-container.collapsed {
    grid-template-columns: 40px 1fr;
  }
}
</style>