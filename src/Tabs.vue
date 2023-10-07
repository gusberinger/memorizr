<template>
  <div class="tabs">
    <h3 :id="'tablist-' + uniqueId">{{ title }}</h3>
    <div
      :role="'tablist'"
      :aria-labelledby="'tablist-' + uniqueId"
      class="automatic"
    >
      <button
        v-for="(tab, index) in tabs"
        :key="index"
        :id="'tab-' + (index + 1)"
        type="button"
        :role="'tab'"
        :aria-selected="selectedTab === index + 1"
        :aria-controls="'tabpanel-' + (index + 1)"
        :tabindex="index === selectedTab - 1 ? 0 : -1"
        @click="selectedTab = index + 1"
      >
        <span class="focus">{{ tab.label }}</span>
      </button>
    </div>

    <div
      v-for="(tab, index) in tabs"
      :key="index"
      :id="'tabpanel-' + (index + 1)"
      role="tabpanel"
      tabindex="0"
      :aria-labelledby="'tab-' + (index + 1)"
      :class="{ 'is-hidden': selectedTab !== index + 1 }"
    >
      <p>{{ tab.content }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue"

const selectedTab = ref(1)

const props = {
  title: String,
  tabs: Array,
  uniqueId: {
    type: String,
    default: Math.random().toString(36).substring(7),
  },
}
</script>

<style scoped>
/* Add your styling here */
</style>
