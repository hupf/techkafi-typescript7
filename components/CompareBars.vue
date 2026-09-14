<script setup lang="ts">
interface Row {
  name: string;
  sub?: string;
  before: number;
  after: number;
  speedup: string;
}

const props = withDefaults(
  defineProps<{
    rows: Row[];
    unit?: string;
    labelBefore?: string;
    labelAfter?: string;
  }>(),
  {
    unit: "s",
    labelBefore: "TypeScript 6",
    labelAfter: "TypeScript 7",
  },
);

// Each row is normalised against its own "before" value, so codebases of very
// different sizes stay comparable on one chart.
const widthOf = (row: Row) => `${(row.after / row.before) * 100}%`;
const format = (value: number) => `${value}${props.unit}`;
</script>

<template>
  <div class="compare-bars">
    <div class="legend">
      <span class="swatch before"></span>{{ labelBefore }}
      <span class="swatch after"></span>{{ labelAfter }}
    </div>

    <div v-for="row in rows" :key="row.name" class="row">
      <div class="label">
        <div class="name">{{ row.name }}</div>
        <div v-if="row.sub" class="sub">{{ row.sub }}</div>
      </div>

      <div class="bars">
        <div class="track">
          <div class="bar before">
            <span>{{ format(row.before) }}</span>
          </div>
        </div>
        <div class="track">
          <div class="bar after" :style="{ width: widthOf(row) }">
            <span>{{ format(row.after) }}</span>
          </div>
          <span class="speedup">{{ row.speedup }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.compare-bars {
  --c-before: #1b2d53;
  --c-after: #238bca;

  font-size: 0.8rem;
}

.legend {
  display: flex;
  align-items: center;
  gap: 0.4rem;
  margin-bottom: 0.9rem;
  padding-left: 8.5rem;
  font-family: var(--slidev-code-font-family, monospace);
  font-size: 0.85em;
}

.legend .swatch {
  width: 0.85em;
  height: 0.85em;
  border-radius: 0.2em;
}

.legend .swatch + .swatch {
  margin-left: 1.2rem;
}

.swatch.before {
  background: var(--c-before);
}

.swatch.after {
  background: var(--c-after);
}

.row {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  margin-bottom: 0.85rem;
}

.label {
  flex: 0 0 7.75rem;
  text-align: right;
  line-height: 1.15;
}

.label .name {
  font-weight: 600;
}

.label .sub {
  font-size: 0.8em;
  opacity: 0.5;
}

.bars {
  flex: 1;
  min-width: 0;
  display: flex;
  flex-direction: column;
  gap: 0.3rem;
}

.track {
  display: flex;
  align-items: center;
  gap: 0.6rem;
}

.bar {
  display: flex;
  align-items: center;
  justify-content: flex-end;
  height: 1.7em;
  padding: 0 0.6em;
  border-radius: 0.3em;
  color: white;
  font-family: var(--slidev-code-font-family, monospace);
  font-weight: 600;
  white-space: nowrap;
}

.bar.before {
  width: 100%;
  background: var(--c-before);
}

.bar.after {
  /* Keep the value label readable even when the ratio is tiny. */
  min-width: 4.5em;
  background: var(--c-after);
}

.speedup {
  color: var(--c-after);
  font-family: var(--slidev-code-font-family, monospace);
  font-weight: 600;
}
</style>
