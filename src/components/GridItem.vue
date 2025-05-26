<script setup lang="ts">
const props = defineProps<{
  colour?: string;
}>();
</script>

<template>
  <div class="bg-blue-300 rounded min-h-20 shadow-lg shadow-cyan-500/50" :class="[props.colour]"></div>
</template>
