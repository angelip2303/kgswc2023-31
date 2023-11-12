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
    <div :class="props.class">
      <slot name="content" />
    </div>
    <div class="col-footer">
      <slot name="footer" />
    </div>
  </div>
</template>

<style scoped>
.two-cols-header {
  display: flex;
  flex-direction: column;
}

.cols {
  display: flex;
}

.col-left {
  flex: 3;
  border-right: 1px solid #181818;
  padding-right: 1em;
}

.col-right {
  flex: 1;
  padding-left: 1em;
}
</style>