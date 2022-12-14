<script setup>
defineProps({
  name: {
    type: String,
    required: true,
  },
  type: {
    type: String,
    required: false,
    default: "text",
  },
  as: {
    type: String,
    required: false,
  },
});
</script>

<template>
  <component :is="as" :type="type" :name="name"> <slot></slot></component>
</template>

<style scoped></style>
