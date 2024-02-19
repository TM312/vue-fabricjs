<script setup lang="ts">
import { ref, onMounted, watch } from 'vue'
import { Canvas, FabricText } from 'fabric'

const props = defineProps({
  canvasText: String,
  exportToPng: Boolean,
  backgroundColor: {
    type: String,
    default: 'blue',
  }
})

const emit = defineEmits(['update:exportToPng'])

let canvas: Canvas | null = null
const canvasRef = ref<HTMLElement | null>(null)

function exportToPng() {
  if (!canvas) {
    console.error('Canvas not found')
    return
  }
  const dataURL = canvas.toDataURL({
    format: 'png',
    quality: 1.0,
    multiplier: 1,
  })
  const link = document.createElement('a')
  link.href = dataURL
  link.download = 'canvas.png'
  link.click()
}

function changeBackgroundColor() {
  if (!canvas) {
    console.error(`Canvas not found: ${canvas}`)
    return
  }
  canvas.backgroundColor = props.backgroundColor
  canvas.renderAll();
}

function setupCanvas() {
  if (!canvasRef.value) {
    console.error('Canvas frame not found')
    return
  }

  canvas = new Canvas('canvas', {
    width: 500,
    height: 500,
    backgroundColor: props.backgroundColor,
  })

  const text = new FabricText('Text before input', {
    left: 100,
    top: 100,

    fill: 'black',
  })
  canvas.add(text)
  canvas.renderAll();
}

watch(() => props.canvasText, () => {
  console.log(`Canvas text updated: ${props.canvasText}`)
  updateText()
})

watch(() => props.exportToPng, () => {
  console.log(`Export to PNG: ${props.exportToPng}`)
  if (props.exportToPng) {
    exportToPng()
    emit('update:exportToPng', false)
  }
})

watch(() => props.backgroundColor, () => {
  console.log(`Background color updated: ${props.backgroundColor}`)
  changeBackgroundColor()
})


function updateText() {
  if (!canvas) {
    console.error('Canvas not found')
    return
  }
  const objects = canvas?.getObjects('text')[0];
  if (!!objects) {
    objects.set('text', props.canvasText)
    canvas.renderAll()
    console.log('Text updated')
  } else {
    console.error(`Text object not found | canvas of type: ${typeof canvas}`)
  }
}

onMounted(() => {
  if (!canvasRef.value) {
    console.error('Canvas frame not found')
    return
  }
  setupCanvas()

  if (!canvas) {
    console.error('Canvas not found')
    return
  }
  console.log('Canvas mounted')
})
</script>

<template>
  <canvas ref="canvasRef" id="canvas" :key="'fixedCanvasKey'" />
</template>