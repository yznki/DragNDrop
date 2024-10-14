<template>
  <div>
    <ul class="list-none pl-0">
      <li
        v-for="(item, index) in items"
        :key="index"
        :draggable="true"
        @dragstart="dragStart(index)"
        @dragover.prevent="dragOver(index)"
        @drop="drop"
        :class="[
          'flex items-center p-2 mb-1 border rounded bg-white transition-all duration-200 ease-in-out',
          {
            'opacity-50': draggingIndex === index,
          },
        ]"
        :style="{
          height: '50px',
        }"
      >
        <span class="cursor-grab mr-2">⠿</span>
        {{ item }}
        <button
          @click="moveUp(index)"
          :disabled="index === 0"
          class="ml-2 bg-gray-200 text-gray-800 px-2 py-1 rounded disabled:opacity-50"
        >
          Up
        </button>
        <button
          @click="moveDown(index)"
          :disabled="index === items.length - 1"
          class="ml-2 bg-gray-200 text-gray-800 px-2 py-1 rounded disabled:opacity-50"
        >
          Down
        </button>
      </li>
    </ul>
  </div>
</template>

<script setup>
  import {ref} from "vue"

  const items = ref(["Item 1", "Item 2", "Item 3", "Item 4"])

  const draggingIndex = ref(null)
  const targetIndex = ref(null)

  const moveUp = (index) => {
    if (index > 0) {
      const temp = items.value[index]
      items.value[index] = items.value[index - 1]
      items.value[index - 1] = temp
    }
  }

  const moveDown = (index) => {
    if (index < items.value.length - 1) {
      const temp = items.value[index]
      items.value[index] = items.value[index + 1]
      items.value[index + 1] = temp
    }
  }

  const dragStart = (index) => {
    draggingIndex.value = index
    event.dataTransfer.effectAllowed = "move"
  }

  const dragOver = (index) => {
    if (draggingIndex.value !== null && draggingIndex.value !== index) {
      const temp = items.value[draggingIndex.value]
      items.value.splice(draggingIndex.value, 1)
      items.value.splice(index, 0, temp)
      draggingIndex.value = index
    }
  }

  const drop = () => {
    draggingIndex.value = null
  }
</script>
