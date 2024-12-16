<script setup lang="ts">
import { onMounted, ref } from 'vue';
import {init as initDynamsoft, initDocDetectModule} from './dynamsoft.config';
import Scanner from './components/Scanner.vue';

const initializing = ref(false);
const initialized = ref(false);
const isScanning = ref(false);

const toggleScanning = () => {
  console.log("toggleScanning")
  isScanning.value = !isScanning.value;
}

onMounted(()=>{
  if (!initializing.value && !initialized.value) {
    initializing.value = true;
    initialize();
  }
})

const initialize = async () => {
  await initDynamsoft();
  await initDocDetectModule();
  initialized.value = true;
}

</script>

<template>
  <h1>Scan to PDF</h1>
  
  <button v-on:click="toggleScanning" v-if="initialized">
    {{isScanning ? "Stop Scanning" : "Start Scanning"}}
  </button>
  <div v-if="!initialized">Initializing...</div>
  <div id="scanner" v-if="isScanning">
    <Scanner></Scanner>
  </div>
  <div style="margin-top:2em">
    Powered by <a href='https://www.dynamsoft.com' target='_blank'>Dynamsoft</a>
  </div>
</template>

<style scoped>
#scanner {
  position: relative;
  width: 100%;
  height: 480px;
}
</style>
