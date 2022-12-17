<script setup>
import { reactive, ref, provide } from "vue";
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
  console.log("handleChange function values", values);
  errors = props.validate(values);
  console.log("handleChange function errors", errors);
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
