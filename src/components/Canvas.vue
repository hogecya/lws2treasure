<template>
  <div style="position: relative;">
    <canvas ref="canvas" width="1050" height="1050"></canvas>
    <div ref="tooltip" style="position: absolute; background-color: white; border: 1px solid black; padding: 2px; font-size: 10px; display: none;"></div>
  </div>
</template>

<script lang="ts" setup>
import { onMounted, ref } from 'vue';
import { coordinates } from './coordinates.ts'; // 座標を読み込む
import { treasures } from './getTreasure.ts'; // 座標を読み込む
import { failtreasures } from './failTerasure.ts'; // 座標を読み込む

const canvas = ref<HTMLCanvasElement | null>(null);
const tooltip = ref<HTMLDivElement | null>(null);

const drawGrid = (ctx: CanvasRenderingContext2D) => {
  const rows = 13;
  const cols = 13;
  const cellWidth = 75;
  const cellHeight = 75;
  const specialWidth = 100;
  const specialHeight = 100;

  // スタイリッシュな背景色を設定
  ctx.fillStyle = '#f0f0f0'; // 背景色を薄いグレーに設定
  ctx.fillRect(0, 0, canvas.value!.width, canvas.value!.height);

  // スタイリッシュな線色と太さを設定
  ctx.strokeStyle = '#007acc'; // 青色
  ctx.lineWidth = 1; // 線の太さを1に設定

  // 軸目盛のスタイル
  ctx.fillStyle = '#000000'; // 目盛の色を黒に設定
  ctx.font = '12px Arial';

  for (let i = 0; i <= rows; i++) {
    for (let j = 0; j <= cols; j++) {
      // 横線を描画
      if (j < cols) {
        ctx.beginPath();
        ctx.moveTo(j * cellWidth + (j >= 7 ? specialWidth - cellWidth : 0), i * cellHeight + (i >= 7 ? specialHeight - cellHeight : 0));
        ctx.lineTo((j + 1) * cellWidth + (j >= 6 ? specialWidth - cellWidth : 0), i * cellHeight + (i >= 7 ? specialHeight - cellHeight : 0));
        ctx.stroke();
      }

      // 縦線を描画
      if (i < rows) {
        ctx.beginPath();
        ctx.moveTo(j * cellWidth + (j >= 7 ? specialWidth - cellWidth : 0), i * cellHeight + (i >= 7 ? specialHeight - cellHeight : 0));
        ctx.lineTo(j * cellWidth + (j >= 7 ? specialWidth - cellWidth : 0), (i + 1) * cellHeight + (i >= 6 ? specialHeight - cellHeight : 0));
        ctx.stroke();
      }

      // 縦軸目盛を描画
      const yPos = i * cellHeight + (i >= 7 ? specialHeight - cellHeight : 0);
      if (j === 0 && yPos <= 1000) {
        ctx.fillText((1000 - yPos).toString(), 2, yPos + 12); // 縦軸の目盛位置を調整
      }

      // 横軸目盛を描画
      const xPos = j * cellWidth + (j >= 7 ? specialWidth - cellWidth : 0);
      if (i === 0 && xPos !== 0) {
        ctx.fillText(xPos.toString(), xPos + 2, canvas.value!.height - 8); // 横軸の目盛をキャンバスの直下に配置
      }
    }
  }

  // 1000の目盛が見えるように追加
  ctx.fillText('1000', 2, 12);
};

const drawPoints = (ctx: CanvasRenderingContext2D, points: { x: number, y: number }[]) => {
  ctx.fillStyle = 'blue';
  points.forEach(point => {
    ctx.fillRect(point.x - 1, 1000 - point.y - 1, 3, 3); // 左下を基準に座標を描画
  });
};

const drawFailTreasure = (ctx: CanvasRenderingContext2D, points: { x: number, y: number }[]) => {
  ctx.fillStyle = 'gray';
  points.forEach(point => {
    ctx.fillRect(point.x - 4, 1000 - point.y - 4, 9, 9); 
  });
};

const drawGetTreasure = (ctx: CanvasRenderingContext2D, points: { x: number, y: number }[]) => {
  ctx.fillStyle = 'red';
  points.forEach(point => {
    ctx.fillRect(point.x - 1, 1000 - point.y - 1, 3, 3); // 左下を基準に座標を描画
  });
};

const showTooltip = (event: MouseEvent) => {
  const rect = canvas.value!.getBoundingClientRect();
  const x = event.clientX - rect.left;
  const y = event.clientY - rect.top;
  const adjustedY = 1000 - y;

  tooltip.value!.style.left = `${x + 10}px`;
  tooltip.value!.style.top = `${y + 10}px`;
  tooltip.value!.textContent = `(${Math.round(x)}, ${Math.round(adjustedY)})`;
  tooltip.value!.style.display = 'block';
};

const hideTooltip = () => {
  tooltip.value!.style.display = 'none';
};

onMounted(() => {
  const ctx = canvas.value!.getContext('2d')!;
  drawGrid(ctx);
  drawPoints(ctx, coordinates); // 座標を読み込んで描画
  drawGetTreasure(ctx, treasures);
  drawFailTreasure(ctx, failtreasures);

  canvas.value!.addEventListener('mousemove', showTooltip);
  canvas.value!.addEventListener('mouseleave', hideTooltip);
});
</script>

<style scoped>
/* 必要に応じてスタイルを追加 */
</style>
