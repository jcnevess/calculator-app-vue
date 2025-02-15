<script setup>
const props = defineProps(['screenDisplay'])
</script>

<template>
  <div class="screen">{{ props.screenDisplay }}</div>
</template>

<style scoped>
.screen {
  background-color: var(--color-bg-screen);
  color: var(--color-text-primary);
  font-weight: bold;
  padding: 1rem;
  border-radius: 5px;
  display: flex;
  justify-content: flex-end;
  text-transform: uppercase;
  word-break: break-all;
  text-align: end;
  height: 3ch;
  line-height: 2ch;
  scroll-behavior: smooth;
  overflow-y: auto;
  scrollbar-width: thin;
  scrollbar-color: var(--color-text-primary) transparent;
}
</style>
