<script setup lang="ts">
import 'normalize.css'
import CanvasVue from './components/Canvas.vue'
</script>

<template>
  <h4>Treasure Hunters of [544]UGO</h4>
  <div class="divstyle">[残]L7:10 / L6:40 / L5:80 / L4:57 / L3:73 / L2:196 / L1:318 @2024/12/24 23:00</div>
  <div style="margin:5px">
    <CanvasVue />
  </div>
</template>

<style scoped>
h4 {
  position: relative;
  padding: 0.5rem 0.5rem;
  text-align: center;
  color: #0075a9;
  background: #d8ecf5;
  margin: 0;
}

.divstyle {
 padding: 4px 8px; 
 font-size: 10px; 
 color: #333;
 text-decoration: underline;
}
</style>
