<script setup lang="ts">
import { ref, computed } from 'vue';

const props = defineProps<{
  colour?: string;
  index?: number;
}>();

const emit = defineEmits<{
  dragStart: [index: number]
  drop: [draggedIndex: number, droppedIndex: number]
}>();

const isActive = ref(true);
const isDragOver = ref(false);
const baseClasses = "drag-el rounded min-h-20"
const activeStyle = "shadow-lg";
//const inActiveStyle = "bg-gray-400/50 outline-dashed outline-2 outline-offset-4";
const dragOverStyle = "bg-blue-200 border-2 border-blue-500";

const combinedClasses = computed(() => {
  const classes = [baseClasses];

  if (isDragOver.value) {
    classes.push(dragOverStyle);
  } else if (isActive.value) {
    classes.push(activeStyle);
    if (props.colour) {
      classes.push(props.colour);
    }
  }

  return classes
})

function handleClick() {
  isActive.value = !isActive.value;
}

function handleDragStart(event: DragEvent) {
  console.log('handleDragStart called');
  event.dataTransfer!.effectAllowed = 'move';
  event.dataTransfer!.setData('text/plain', props.index?.toString() || '0');
  emit('dragStart', props.index || 0);
}

function handleDragOver(event: DragEvent) {
  event.preventDefault();
  event.dataTransfer!.dropEffect = 'move';
  isDragOver.value = true;
}

function handleDragLeave() {
  isDragOver.value = false;
}

function handleDrop(event: DragEvent) {
  event.preventDefault();
  isDragOver.value = false;

  const draggedIndex = parseInt(event.dataTransfer!.getData('text/plain'));
  const droppedIndex = props.index || 0;

  if (draggedIndex !== droppedIndex) {
    emit('drop', draggedIndex, droppedIndex);
  }
}

</script>

<template>
  <div :class="combinedClasses" @click="handleClick" @dragstart="handleDragStart" @dragover="handleDragOver"
    @dragleave="handleDragLeave" @drop="handleDrop" draggable="true">
    <!-- <p class="text-center align-middle  ">Click Me!</p>
    <p>State: {{ isActive ? 'Active' : 'Inactive' }}</p>
    <p v-if="props.colour">Parent Colour: {{ props.colour }}</p>  -->
  </div>
</template>
