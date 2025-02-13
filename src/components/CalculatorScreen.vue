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
  align-items: center;
  justify-content: flex-end;
  text-transform: uppercase;
}
</style>
