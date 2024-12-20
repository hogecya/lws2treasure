<template>
    <div class="grid">
      <div v-for="row in rows" :key="row" class="tile">{{ row }}</div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        rows: Array.from({ length: 13 * 13 }, (_, i) => i + 1)
      };
    }
  };
  </script>
  
  <style scoped>
  .grid {
    display: grid;
    grid-template-columns: repeat(6, 75px) 100px repeat(6, 75px); /* 7列目を100pxに設定 */
    grid-template-rows: repeat(6, 75px) 100px repeat(6, 75px);
    gap: 0;
  }
  
  .tile {
    display: flex;
    background-color: #add8e6; /* Light blue */
    text-align: center;
    border: 0.5px dashed #00008b; /* Navy blue dashed border */
    justify-content: center; /* 水平方向の中央揃え */
    align-items: center; /* 垂直方向の中央揃え */
    color: #808080; /* Grey text color */
    transition: background-color 0.3s; /* スムーズな色の変化を追加 */
  }
  
  .tile:hover {
    background-color: #ffa07a; /* Light salmon 色 */
  }
  </style>
  