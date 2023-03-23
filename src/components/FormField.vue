<script lang="ts" setup>
import { defineComponent } from 'vue';

defineComponent({
  name: 'FormField',
});

defineProps<{
  label: string;
}>();
</script>

<template>
  <div class="mb-4 flex flex-col">
    <label class="mb-1 text-[16px] font-normal">{{ label }}</label>
    <slot />
  </div>
</template>
