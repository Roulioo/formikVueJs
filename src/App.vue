<script setup>
import Formik from "./components/Formik.vue";
import Field from "./components/Field.vue";
import { reactive, ref } from "vue";

const initialValues = reactive({
  name: "",
  email: "",
  color: "",
});

let hasErrors = ref(true);
const errors = reactive({});

function validate(values) {
  const emailPattern = /^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,}$/i;

  if (!values.name || values.name.length == 0) {
    errors.name = "Required";
    hasErrors.value = true;
  } else if (values.name.length > 15) {
    errors.name = "Must be 15 characters or less";
    hasErrors.value = true;
  } else {
    hasErrors.value = false;
  }

  if (!values.email || values.email.length == 0) {
    errors.email = "Required";
    hasErrors.value = true;
  } else if (!emailPattern.test(values.email)) {
    errors.email = "Invalid email address";
    hasErrors.value = true;
  } else {
    hasErrors.value = false;
  }

  if (!values.color || values.color.length == 0) {
    errors.color = "Required";
    hasErrors.value = true;
  } else {
    hasErrors.value = false;
  }

  // if (Object.keys(errors).length == 0) {
  //   onSubmit();
  // }

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
        <p>
          <Field type="text" name="name" as="input" placeholder="Enter name" />
        </p>
        <div v-if="hasErrors">
          <span v-if="errors?.name">{{ errors.name }}</span>
        </div>
        <p>
          <Field
            type="email"
            name="email"
            as="input"
            placeholder="Enter email"
          />
        </p>
        <div v-if="hasErrors">
          <span v-if="errors?.email">{{ errors.email }}</span>
        </div>
        <p>
          <Field name="color" as="select">
            <template #default>
              <option value="">Select a color</option>
              <option value="red">Red</option>
              <option value="green">Green</option>
              <option value="blue">Blue</option>
            </template>
          </Field>
        </p>
        <div v-if="hasErrors">
          <span v-if="errors?.color">{{ errors.color }}</span>
        </div>
        <button type="submit">Submit</button>
      </form>
    </template>
  </Formik>
</template>

<style scoped></style>
