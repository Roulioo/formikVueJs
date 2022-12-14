<script setup>
import Formik from "./components/Formik.vue";
import Field from "./components/Field.vue";
import { reactive } from "vue";

const initialValues = reactive({
  name: "",
  email: "",
});

function validate(values) {
  console.log("Validate trigger >>>", values);
}

function onSubmit(values) {
  console.log("On submit trigger >>>", values);
}
</script>

<template>
  <Formik
    :initialValues="initialValues"
    :validate="validate"
    :onSubmit="onSubmit"
  >
    <template #default="{ values, errors, handleChange, handleSubmit }">
      <form @submit.prevent="handleSubmit">
        <p>
          <Field type="text" name="name" as="input" placeholder="Enter name" />
          <!-- <span v-if="errors.name">{{ errors.name }}</span> -->
        </p>
        <p>
          <Field
            type="email"
            name="email"
            as="input"
            placeholder="Enter email"
          />
          <!-- <span v-if="errors.email">{{ errors.email }}</span> -->
        </p>
        <p>
          <Field name="color" as="select">
            <template #default>
              <option value="red">Red</option>
              <option value="green">Green</option>
              <option value="blue">Blue</option>
            </template>
          </Field>
        </p>
        <button type="submit">Submit</button>
      </form>
    </template>
  </Formik>
</template>

<style scoped></style>
