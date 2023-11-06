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
  <div class="slidev-layout three-cols-header w-full h-full" :class="layoutClass">
    <div class="col-header">
      <slot />
    </div>
    <div class="col-content">
      <slot name="content" />
    </div>
    <div class="col-left" :class="props.class">
      <slot name="left" />
    </div>
    <div class="col-center" :class="props.class">
      <slot name="center" />
    </div>
    <div class="col-right" :class="props.class">
      <slot name="right" />
    </div>
  </div>
</template>

<style scoped>
.three-cols-header {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: 0.2fr 1fr 2fr;
  gap: 1rem;
}

.col-header { grid-area: 1 / 1 / 1 / 4; }
.col-content { grid-area: 2 / 1 / 2 / 4; }
.col-left { grid-area: 3 / 1 / 3 / 1; }
.col-center { grid-area: 3 / 2 / 3 / 2; }
.col-right { grid-area: 3 / 3 / 3 / 3; }
</style>