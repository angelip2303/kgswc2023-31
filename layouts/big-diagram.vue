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
    <div class="cols">
      <div class="col-left" :class="props.class">
        <slot name="big" />
      </div>
      <div class="col-right my-auto mx-auto text-center" :class="props.class">
        <slot name="small" />
      </div>
    </div>
    <div class="col-footer">
      <slot name="bottom" />
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