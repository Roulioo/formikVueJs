<script setup>
import { reactive, ref, provide, watch } from "vue";
import { values as valuesKey } from "./providers/FormikProviderKeys.js";

const props = defineProps({
  initialValues: {
    type: Object,
    required: true,
  },
  validate: {
    type: Function,
    required: false,
  },
  onSubmit: {
    type: Function,
    required: true,
  },
});

const values = reactive(props.initialValues);
let errors = reactive({});
let isSubmitting = ref(false);

watch([values], () => checkValidation());

function checkValidation() {
  errors = props.validate(values);

  if (Object.keys(errors).length == 0) {
    isSubmitting.value = true;
  } else {
    isSubmitting.value = false;
  }
}

function handleSubmit() {
  props.onSubmit();
}

provide(valuesKey, values);
</script>

<template>
  <slot
    :values="values"
    :errors="errors"
    :handleSubmit="handleSubmit"
    :isSubmitting="isSubmitting"
  ></slot>
</template>

<style scoped></style>
