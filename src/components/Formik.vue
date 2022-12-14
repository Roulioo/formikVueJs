<script setup>
import { reactive, provide } from "vue";
import {
  values as valuesKey,
  errors as errorsKey,
} from "./providers/FormikProviderKeys.js";

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
const errors = reactive({});

provide(valuesKey, values);
provide(errorsKey, errors);
</script>

<template>
  <div>
    <slot
      :values="values"
      :errors="errors"
      :handleChange="handleChange"
      :handleSubmit="handleSubmit"
    ></slot>
  </div>
</template>

<style scoped></style>
