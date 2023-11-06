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
  <div class="slidev-layout three-images w-full h-full" :class="layoutClass">
    <div class="col-left" :class="props.class">
      <slot name="left" />
    </div>
    <div class="col-top" :class="props.class">
      <slot name="top" />
    </div>
    <div class="col-bottom" :class="props.class">
      <slot name="bottom" />
    </div>
  </div>
</template>

<style scoped>
.three-images {
  display: grid;
  grid-template-columns: repeat(2, 50%);
  grid-template-rows: repeat(2, 50%);
  gap: 0.75rem;
  justify-items: center;
  align-items: center;
}

.col-left { grid-area: 1 / 1 / 3 / 1; }
.col-top { grid-area: 1 / 2 / 1 / 2; }
.col-bottom { 
  grid-area: 2 / 2 / 2 / 2;
  max-height: 1fr;
}
</style>