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
    <div class="col-left" :class="props.class">
      <slot />
    </div>
    <div class="col-right m-auto" :class="props.class">
      <slot name="right" />
    </div>
  </div>
</template>

<style scoped>
.two-cols-header {
  display: flex;
  flex-direction: row;
  gap: 1em;
}

.col-left {
  flex: 3;
}

.col-right {
  flex: 4;
}
</style>