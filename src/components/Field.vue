<script setup>
import { inject } from "vue";
import { values as valuesKey } from "./providers/FormikProviderKeys.js";

const props = defineProps({
  name: {
    type: String,
    required: true,
  },
  as: {
    type: String,
    required: false,
  },
});

const values = inject(valuesKey);

function handleChange(event) {
  values[props.name] = event.target.value;
}
</script>

<template>
  <component :is="props.as" :name="props.name" @input="handleChange">
    <slot></slot>
  </component>
</template>

<style scoped></style>
