<script setup lang="ts">
import { ref } from "vue"

defineProps<{ modelValue: boolean, iconType: string }>()

const count = ref(0)
</script>

<template>
  <button
    type="button"
    class="icon-button"
    :class="iconType"
    @click="count++"
  >
    <slot></slot>
  </button>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
