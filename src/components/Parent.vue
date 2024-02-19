<script lang="ts" setup>
import { ref } from 'vue'
import Child from '../components/Child.vue';

const canvasText = ref('Hello World')
const exportToPng = ref(false)
const backgroundColor = ref('red')

function setBackgroundColor() {
  if (backgroundColor.value === 'blue') {
    backgroundColor.value = 'red'
  } else {
    backgroundColor.value = 'blue'
  }
}
</script>

<template>
  <div>
    <input v-model="canvasText" style="{ border: 2px; }" />
    <button @click="exportToPng = !exportToPng">Export to PNG</button>
    <button @click="setBackgroundColor">Change background color</button>
    <p>This is the text of the input element: {{ canvasText }}</p>
    <p>Below is the Child element</p>
    <Child style="{ border: 2px; }" :canvas-text="canvasText" :export-to-png="exportToPng"
      :background-color="backgroundColor" /> @update:export-to-png="exportToPng = $event" />
  </div>
</template>