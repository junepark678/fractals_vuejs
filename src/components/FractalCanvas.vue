<template>
  <h3>하우스도르프의 차원: {{ calculateHausdorffDimension(props.depth) }}</h3>
  <div class="container"><div id="fractalcanvas"></div></div>
</template>

<script lang="ts" setup>
import { defineProps, onMounted, onUpdated } from "vue";
import Konva from "konva";
const props = defineProps<{
  depth: number;
}>();
let stage: Konva.Stage;
let layer: Konva.Layer;
onMounted(() => {
  stage = new Konva.Stage({
    container: "fractalcanvas",
    width: 800,
    height: 600,
  });
  layer = new Konva.Layer();
  stage.add(layer);
  drawKochFractal(200, 100, 400, 0, props.depth);
});

function calculateHausdorffDimension(depth: number) {
  // Base case: depth 0
  if (depth === 0) {
    return 1;
  }

  // Calculate the number of line segments at the given depth
  const numSegments = Math.pow(4, depth);

  // Calculate the scaling factor for each iteration of the fractal
  const scalingFactor = 1 / 3;

  // Calculate the Hausdorff dimension using the formula
  const hausdorffDimension =
    Math.log(numSegments) / Math.log(1 / scalingFactor);

  return hausdorffDimension;
}

function drawKochFractal(
  x: number,
  y: number,
  length: number,
  angle: number,
  iterations: number
) {
  if (iterations === 0) {
    const endX = x + length * Math.cos((angle * Math.PI) / 180);
    const endY = y + length * Math.sin((angle * Math.PI) / 180);

    const line = new Konva.Line({
      points: [x, y, endX, endY],
      stroke: "red",
      strokeWidth: 1,
    });

    layer.add(line);
  } else {
    const segmentLength = length / 3;

    const x1 = x + segmentLength * Math.cos((angle * Math.PI) / 180);
    const y1 = y + segmentLength * Math.sin((angle * Math.PI) / 180);

    const x2 = x1 + segmentLength * Math.cos(((angle + 60) * Math.PI) / 180);
    const y2 = y1 + segmentLength * Math.sin(((angle + 60) * Math.PI) / 180);

    const x3 = x2 + segmentLength * Math.cos(((angle - 60) * Math.PI) / 180);
    const y3 = y2 + segmentLength * Math.sin(((angle - 60) * Math.PI) / 180);

    drawKochFractal(x, y, segmentLength, angle, iterations - 1);

    drawKochFractal(x1, y1, segmentLength, angle + 60, iterations - 1);

    drawKochFractal(x2, y2, segmentLength, angle - 60, iterations - 1);

    drawKochFractal(x3, y3, segmentLength, angle, iterations - 1);
  }

  layer.batchDraw();
}
onUpdated(() => {
  layer.removeChildren();
  layer.batchDraw();
  drawKochFractal(200, 100, 400, 0, props.depth);
});
</script>

<style lang="scss">
.container {
  display: flex;
  justify-content: center;
}
</style>
