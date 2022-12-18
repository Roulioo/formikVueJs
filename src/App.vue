<script setup>
import Formik from "./components/FormikComponent.vue";
import Field from "./components/FieldComponent.vue";
import { reactive } from "vue";

const initialValues = reactive({});
const errors = reactive({});

function validate(values) {
  const emailPattern = /^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,}$/i;

  if (!values.name || values.name.length == 0) {
    errors.name = "The field name is required";
  } else if (values.name.length > 15) {
    errors.name = "Name must be 15 characters or less";
  } else {
    delete errors.name;
  }

  if (!values.email || values.email.length == 0) {
    errors.email = "The field email is required";
  } else if (!emailPattern.test(values.email)) {
    errors.email = "Invalid email address";
  } else {
    delete errors.email;
  }

  if (!values.color || values.color.length == 0) {
    errors.color = "The field color is required";
  } else {
    delete errors.color;
  }

  return errors;
}

function onSubmit() {
  console.log("Submit form", initialValues);
}
</script>

<template>
  <Formik
    :initialValues="initialValues"
    :validate="validate"
    :onSubmit="onSubmit"
  >
    <template #default="{ values, errors, handleSubmit, isSubmitting }">
      <form @submit.prevent="handleSubmit">
        <div class="mb-4">
          <Field
            type="text"
            name="name"
            as="input"
            placeholder="Enter name"
            v-model="values.name"
          />
          <!-- DEBUG -->
          <!-- NAME ==> {{ values.name }} -->
        </div>
        <span v-if="errors?.name" class="error mb-4">{{ errors.name }}</span>
        <div class="mb-4">
          <Field
            type="email"
            name="email"
            as="input"
            placeholder="Enter email"
            v-model="values.email"
          />
          <!-- DEBUG -->
          <!-- EMAIL ==> {{ values.email }} -->
        </div>
        <span v-if="errors?.email" class="error mb-4">{{ errors.email }}</span>
        <div class="mb-4">
          <Field name="color" as="select" v-model="values.color">
            <template #default>
              <option value="">Select a color</option>
              <option value="red">Red</option>
              <option value="green">Green</option>
              <option value="blue">Blue</option>
            </template>
          </Field>
          <!-- DEBUG -->
          <!-- COLOR ==> {{ values.color }} -->
        </div>
        <span v-if="errors?.color" class="error mb-4">{{ errors.color }}</span>
        <button type="submit" :class="{ isDisabled: !isSubmitting }">
          Submit
        </button>
      </form>
    </template>
  </Formik>
</template>

<style scoped>
.mb-4 {
  margin-bottom: 1rem;
}

.error {
  display: block;
  color: red;
}

.isDisabled {
  opacity: 0.5;
  pointer-events: none;
}
</style>
