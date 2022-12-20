<script setup>
import { provide, reactive, defineEmits } from "vue";

const props = defineProps({
  initialValues: {
    type: Object,
    default: {},
  },
  validate: {
    type: Function,
    default: null,
  },
  onSubmit: {
    type: Function,
    required: true,
  },
});


const state = reactive({
  values: props.initialValues,
  errors: {},
  isSubmitting: false,
  handleSubmit: async (e) => {
    state.isSubmitting = true;
    state.errors = await props.validate(state.values);
    
    if (Object.keys(state.errors).length === 0)
      props.onSubmit(state.values)

    state.isSubmitting = false;
  },
  updateValue: (name, value) => {
    state.values[name] = value;
  },
});

provide("data", state);

</script>

<template>
  <form @submit.prevent="state.handleSubmit">
    <slot 
      :values="state.values" 
      :errors="state.errors" 
      :isSubmitting="state.isSubmitting"
    >
    </slot>
  </form>
</template>