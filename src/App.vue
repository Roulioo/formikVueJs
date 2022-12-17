<script setup>
import Formik from "./components/Formik.vue";
import Field from "./components/Field.vue";
import { reactive, ref } from "vue";

const initialValues = reactive({
  name: "",
  email: "",
  color: "",
});

const errors = reactive({});

let hasErrors = ref(true);

function validate(values) {
  const emailPattern = /^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,}$/i;

  if (!values.name || values.name.length == 0) {
    errors.name = "The field name is required";
    hasErrors.value = true;
  } else if (values.name.length > 15) {
    errors.name = "Name must be 15 characters or less";
    hasErrors.value = true;
  } else {
    errors.name = "";
    hasErrors.value = false;
  }

  if (!values.email || values.email.length == 0) {
    errors.email = "The field email is required";
    hasErrors.value = true;
  } else if (!emailPattern.test(values.email)) {
    errors.email = "Invalid email address";
    hasErrors.value = true;
  } else {
    errors.email = "";
    hasErrors.value = false;
  }

  if (!values.color || values.color.length == 0) {
    errors.color = "The field color is required";
    hasErrors.value = true;
  } else {
    errors.color = "";
    hasErrors.value = false;
  }

  return errors;
}

function onSubmit() {
  console.log("Submit form");
}
</script>

<template>
  <Formik
    :initialValues="initialValues"
    :validate="validate"
    :onSubmit="onSubmit"
  >
    <template #default="{ values, errors, handleChange, isSubmitting }">
      <form @submit.prevent="handleChange">
        <div class="mb-4">
          <Field type="text" name="name" as="input" placeholder="Enter name" />
        </div>
        <template v-if="hasErrors">
          <span v-if="errors?.name" class="error mb-4">{{ errors.name }}</span>
        </template>
        <div class="mb-4">
          <Field
            type="email"
            name="email"
            as="input"
            placeholder="Enter email"
          />
        </div>
        <template v-if="hasErrors">
          <span v-if="errors?.email" class="error mb-4">{{
            errors.email
          }}</span>
        </template>
        <div class="mb-4">
          <Field name="color" as="select">
            <template #default>
              <option value="">Select a color</option>
              <option value="red">Red</option>
              <option value="green">Green</option>
              <option value="blue">Blue</option>
            </template>
          </Field>
        </div>
        <template v-if="hasErrors">
          <span v-if="errors?.color" class="error mb-4">{{
            errors.color
          }}</span>
        </template>
        <button type="submit">Submit</button>
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
</style>
