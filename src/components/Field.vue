<script setup>
import { inject, reactive, ref, defineProps } from "vue";
const props = defineProps({
  name: {
    type: String,
    required: true,
  },
  as: { default: "input", type: String, required: true },
});

const values = inject("values");
</script>

<template>
  <component></component>
</template>
