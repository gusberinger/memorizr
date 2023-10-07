<script setup lang="ts">
import { ref, computed, watch } from "vue"
import { Cog8ToothIcon } from "@heroicons/vue/24/solid"

const selectedTab = ref(1)
const inputText = ref(localStorage.getItem("inputText") || "")
watch(inputText, () => {
  localStorage.setItem("inputText", inputText.value)
})
// const outputText = computed(() => inputText.value)

// output keeps only first letter of each word using regex
const outputText = computed(() => {
  let newText = inputText.value.replace(/(\w)\w*/g, "$1")
  if (settings.value.capitalize) {
    newText = newText.toUpperCase()
  }
  if (settings.value.removeCommas) {
    newText = newText.replace(/,/g, "")
  }
  return newText
  
})

const settingsOpen = ref(false)

const defaultSettings = ref({
  capitalize: false,
  removeCommas: false,
  disableSpellCheck: false,
})

const settings = ref(JSON.parse(localStorage.getItem("settings") || JSON.stringify(defaultSettings.value)))

const dialogRef = ref<HTMLDialogElement | null>(null)
console.log(dialogRef.value)

watch(settingsOpen, () => {
  console.log(settingsOpen.value, dialogRef.value)
  if (settingsOpen.value) {
    dialogRef.value?.showModal()
  } else {
    dialogRef.value?.close()
  }
})

watch([settings.value], () => {
  localStorage.setItem("settings", JSON.stringify(settings.value))
})
</script>

<template>
  <div class="max-w-2xl m-auto">
    <div role="tablist" aria-labelledby="tablist-1" class="flex flex-row items-center w-full my-4">
      <div class="flex flex-row flex-grow justify-start items-center">
        <button
          id="tab-1"
          type="button"
          role="tab"
          :aria-selected="selectedTab === 1"
          aria-controls="tabpanel-1"
          @click="selectedTab = 1"
        >
          <span class="focus">Input</span>
        </button>
        <button
          id="tab-2"
          type="button"
          role="tab"
          :aria-selected="selectedTab === 2"
          aria-controls="tabpanel-2"
          tabindex="-1"
          @click="selectedTab = 2"
        >
          <span class="focus">Output</span>
        </button>
      </div>
      <div class="flex justify-center items-center">
        <Cog8ToothIcon class="h-8 w-8 mt-2 text-gray-500 hover:text-gray-700 cursor-pointer" @click="settingsOpen = true" />
      </div>
    </div>

    <div
      id="tabpanel-1"
      role="tabpanel"
      tabindex="0"
      aria-labelledby="tab-1"
      :class="{ 'is-hidden': selectedTab !== 1 }"
    >
      <textarea :spellcheck="!settings.disableSpellCheck" v-model="inputText"></textarea>
    </div>
    <div
      id="tabpanel-2"
      role="tabpanel"
      tabindex="0"
      aria-labelledby="tab-2"
      :class="{ 'is-hidden': selectedTab !== 2 }"
    >
      <textarea :spellcheck="!settings.disableSpellCheck" v-model="outputText" readonly></textarea>
    </div>
    <dialog ref="dialogRef" class="bg-white py-8 px-8">
      <div class="flex flex-col items-center justify-center">
        <h2 class="text-2xl font-bold">Settings</h2>
        <div class="flex flex-col">
          <div class="flex flex-row gap-x-2">
            <input type="checkbox" id="capitalize" v-model="settings.capitalize" />
            <label @click="settings.capitalize = !settings.capitalize" for="capitalize">Capitalize</label>
          </div>
          <div class="flex flex-row gap-x-2">
            <input type="checkbox" id="capitalize" v-model="settings.removeCommas" />
            <label for="capitalize">Remove Commas</label>
          </div>
          <div class="flex flex-row gap-x-2">
            <input type="checkbox" id="capitalize" v-model="settings.disableSpellCheck" />
            <label for="capitalize">Disable Spell Check</label>
          </div>

        </div>
        <button class="mt-4" @click="settingsOpen = false">Close</button>
      </div>
    </dialog>
  </div>
</template>

<style scoped>
body {
  font-size: 16px;
}

[role="tablist"] {
  min-width: 100%;
}

[role="tab"],
[role="tab"]:focus,
[role="tab"]:hover {
  display: inline-block;
  position: relative;
  z-index: 2;
  top: 2px;
  font-size: 20px;
  padding-inline: 10px;
  font-weight: bold;
  max-width: 22%;
  overflow: hidden;
  text-align: left;
  cursor: pointer;
}

[role="tab"][aria-selected="true"] {
  border-bottom: 8px solid theme("colors.red.600");
  transition: 0.1s ease-in-out;
}

[role="tab"][aria-selected="false"] {
  transition: 0.1s ease-in-out;
  border-bottom: 6px solid theme("colors.gray.200");
}

[role="tabpanel"] {
  padding: 5px;
  border: 2px solid hsl(219deg 1% 72%);
  border-radius: 0 5px 5px;
  background: hsl(220deg 43% 99%);
  height: 100%;
  width: 100%;
  overflow: auto;
}

[role="tabpanel"] > textarea {
  height: 80vh;
  width: 100%;
  resize: none;
}

[role="tabpanel"] > textarea:focus {
  outline: none;
}

#tabpanel-1:focus-within {
  border: 2px solid hsl(240, 6%, 25%)
}

[role="tabpanel"].is-hidden {
  display: none;
}

[role="tabpanel"] p {
  margin: 0;
}
</style>
