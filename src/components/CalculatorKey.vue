<script setup>
const props = defineProps(['label'])
const emit = defineEmits(['keyPressed'])
</script>

<template>
  <button
    class="key"
    :class="{
      commandKey: props.label === 'del' || props.label === 'reset',
      resetKey: props.label === 'reset',
      equalKey: props.label === '=',
    }"
    @click.prevent="emit('keyPressed', props.label)"
  >
    <span class="text">
      {{ props.label }}
    </span>
  </button>
</template>

<style scoped>
.key {
  background-color: var(--color-bg-grayish-orange);
  border: none;
  border-radius: 5px;
  padding: 0.375rem 1.25rem;
  box-shadow: 0 2px var(--color-bg-shadow-grayish-orange);
  color: var(--color-darkest-blue);
}

.key:active {
  background-color: var(--color-bg-shadow-grayish-orange);
}

.commandKey {
  background-color: var(--color-bg-dark-blue);
  box-shadow: 0 2px var(--color-bg-shadow-dark-blue);
  color: var(--color-white);
  text-transform: uppercase;
  font-size: 0.75rem;
}

.commandKey:active {
  background-color: var(--color-bg-shadow-dark-blue);
}

.commandKey .text {
  font-size: 0.875rem;
}

.resetKey {
  grid-column: 1 / 3;
}

.equalKey {
  background-color: var(--color-bg-red);
  box-shadow: 0 2px var(--color-bg-shadow-red);
  color: var(--color-white);
  grid-column: 3 / 5;
}

.equalKey:active {
  background-color: var(--color-bg-shadow-red);
}

.text {
  font-size: 1.25rem;
  font-family: 'League Spartan', sans-serif;
  font-weight: bold;
  display: block;
}
</style>
