<script setup lang="ts">
const props = defineProps({
  class: {
    type: String,
  },
  layoutClass: {
    type: String,
  },
})
</script>

<template>
  <div class="slidev-layout two-cols-header w-full h-full" :class="layoutClass">
    <div class="col-header">
      <slot />
    </div>
    <div class="col-content">
      <slot name="content" />
    </div>
    <div class="col-left" :class="props.class">
      <slot name="left" />
    </div>
    <div class="col-right" :class="props.class">
      <slot name="right" />
    </div>
  </div>
</template>

<style scoped>
.two-cols-header {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  grid-template-rows: 0.2fr 1fr 2fr;
  gap: 0.5rem;
}

.col-header { grid-area: 1 / 1 / 1 / 3; }
.col-content { grid-area: 2 / 1 / 2 / 3; }
.col-left { grid-area: 3 / 1 / 3 / 1; }
.col-right { grid-area: 3 / 2 / 3 / 2; }
</style>