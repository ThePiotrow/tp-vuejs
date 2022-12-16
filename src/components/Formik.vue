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
});

const emit = defineEmits(["onSubmit"]);

const state = reactive({
  values: props.initialValues,
  errors: {},
  isSubmitting: false,
  handleSubmit: async (e) => {
    state.isSubmitting = true;
    state.errors = await props.validate(state.values);

    if (Object.keys(state.errors).length === 0) {
      emit("onSubmit", state.values);
    }
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
    <slot :values="state.values" :errors="state.errors" :handleSubmit="state.handleSubmit"
      :isSubmitting="state.isSubmitting">
    </slot>
  </form>
</template>