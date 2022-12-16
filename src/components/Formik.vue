<script setup>
import { reactive, ref, provide } from "vue";

const values = ref(props.initialValues);

const errors = reactive({});

const isSubmitting = ref(false);

const handleSubmit = (e) => {
  e.preventDefault();
  const [valuesForm, errorsForm] = [values.value, props.validate(values.value)];
  if (Object.keys(errors).length) {
    errors.value = errorsForm;
  } else {
    this.isSubmitting = true;
    props.onSubmit(values);
  }
  values[e.target.name] = e.target.value;
};

provide("values", values);
</script>

<template>
  <slot
    :values="values"
    :errors="errors"
    :handleSubmit="handleSubmit"
    :isSubmitting="isSubmitting"
  ></slot>
</template>
