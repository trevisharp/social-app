<script setup lang="ts">
import { computed, ref } from 'vue';
import { RouterView } from 'vue-router'

const inConfigs = ref(false)
if (localStorage.getItem("api") === null) {
  localStorage.setItem("api", "http://localhost:5000")
}
const storePath = ref(localStorage.getItem("api"))

const apiPath = computed({
  get: () => storePath.value,
  set: (value: string) => 
  {
    storePath.value = value
    localStorage.setItem("api", value)
  }
})

const toggleConfigs = () =>
  inConfigs.value = !inConfigs.value;
</script>

<template>
  <main>
    <RouterView />
  </main>

  <div id="app-config">
    <img src="/server-icon.png" v-on:click="toggleConfigs">
  </div>

  <div v-if="inConfigs" id="configs">
    <h1>Configurações</h1>
    <div class="config-item">
      Backend URL:
      <el-input v-model="apiPath" style="width: 240px" placeholder="Input da url" />
    </div>
  </div>
</template>

<style>
.config-item {
  gap: 20px;
  display: flex;
  align-items: center;
  justify-content: flex-start;
}

#configs {
  gap: 20px;
  display: flex;
  align-items: center;
  justify-content: flex-start;
  flex-direction: column;
  position: absolute;
  left: 0;
  top: 0;
  width: calc(100vw - 40px);
  height: calc(100vh - 40px);
  margin: 20px;
  background-color: rgba(60, 60, 60, 0.95);
  border-radius: 10px;
  border-color: black;
  border-width: 2px;
}

#app-config {
  position: absolute;
  bottom: 0;
  right: 0;
  padding: 10px;
  margin: 10px;
  background-color: darkblue;
  border-radius: 180px;
  z-index: 1;
}

#app-config:hover {
  background-color: blue;
}

#app-config img {
  width: 40px;
  height: 40px;
}

#app {
  padding: 0;
  margin: 0;
}
</style>