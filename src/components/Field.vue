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
</script>

<template>
  <component
    :is="props.as"
    :name="props.name"
    @input="values[props.name] = $event.target.value"
    ><slot></slot
  ></component>
</template>

<style scoped></style>
