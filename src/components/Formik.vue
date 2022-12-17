<script setup>
import { reactive, provide } from "vue";
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

function handleChange() {
  errors = props.validate(values);

  if (Object.keys(errors).length == 0) {
    props.onSubmit();
  }
}

provide(valuesKey, values);
</script>

<template>
  <slot
    :values="values"
    :errors="errors"
    :handleChange="handleChange"
    :isSubmitting="isSubmitting"
  ></slot>
</template>

<style scoped></style>
