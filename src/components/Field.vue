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
  },
  label: {
    type: String,
    required: false,
  },
  type: {
    type: String,
    required: false,
    default: "text"
  },
});


</script>

<template>
  <label :for="name" v-if="props.label">{{ props.label }}</label>
  <component 
    :is="props.as" 
    :name="name" 
    :value="data.values[name]"
    :type="props.type"
    @input="(event) => data.updateValue(name, event.target.value)"
  >
  </component>
  <span v-if="data.errors.hasOwnProperty(name) && typeof data.errors[name] == 'object'" v-for="error in data.errors[name]">{{ error }}</span>
  <span v-if="data.errors.hasOwnProperty(props.name) && typeof data.errors[name] == 'string'" >{{ data.errors[props.name] }}</span>
</template>

