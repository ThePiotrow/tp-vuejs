<script setup>
import { defineProps, inject } from "vue";

const data = inject("data")

const props = defineProps({
  as: {
    type: String || Object,
    required: false,
    default: "input"
  },
  name: {
    type: String,
    required: true,
  }
});

</script>

<template>
  <component 
    :is="props.as" 
    :name="name" 
    :value="data.values[name]"
    @input="(event) => data.updateValue(name, event.target.value)"
  >
    <slot></slot>
  </component>
</template>

