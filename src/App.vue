<script setup lang="ts">
import GridItem from './components/GridItem.vue';

const colourArray = [
  'bg-red-500 shadow-red-500/50',
  'bg-green-500 shadow-green-500/50',
  'bg-yellow-500 shadow-yellow-500/50',
  'bg-purple-500 shadow-purple-500/50',
];

function handleDragStart(index: number) {
  console.log('Drag started from index:', index);
}

function handleDrop(draggedIndex: number, droppedIndex: number) {
  console.log(`Moving item from ${draggedIndex} to ${droppedIndex}`);

  // Swap the items
  const draggedItem = colourArray.value[draggedIndex];
  const droppedItem = colourArray.value[droppedIndex];

  colourArray.value[draggedIndex] = droppedItem;
  colourArray.value[droppedIndex] = draggedItem;
}

</script>

<template>
  <header>
    <h1 class="text-3xl font-bold text-center p-2">Grids Demo</h1>
  </header>

  <main class="container mx-auto p-4 mt-4">
    <div class="grid grid-cols-4 grid-rows-4 gap-4 drop-zone">
      <!-- Row 1 -->
      <div v-for="colour, index in colourArray" :key="`${colour}-${index}`">
        <GridItem :colour="colour" :index="index" @drag-start="handleDragStart" @drop="handleDrop" />
      </div>

      <!-- Row 2 -->
      <div v-for="colour in colourArray" :key="colour">
        <GridItem :colour="colour" />
      </div>
      <!-- Row 3 -->
      <div v-for="colour in colourArray" :key="colour">
        <GridItem :colour="colour" />
      </div>
      <!-- Row 4 -->
      <div v-for="colour in colourArray" :key="colour">
        <GridItem :colour="colour" />
      </div>

    </div>
  </main>
</template>

<style scoped></style>
