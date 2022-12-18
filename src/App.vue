<script setup>
import Formik from "./components/FormikComponent.vue";
import Field from "./components/FieldComponent.vue";
import SayHello from "./components/SayHelloComponent.vue";
import { reactive } from "vue";
import Captcha from "../../vue-project/src/components/lib/Captcha.vue";

const initialValues = reactive({});
const errors = reactive({});
const userData = reactive({ captcha: null });
const captchaOptions = Array.from({ length: 9 }, (_, i) => ({
  id: i,
  href: `https://picsum.photos/200?random=${i}`,
}));

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
        <fieldset>
          <legend class="legend">Input examples</legend>
          <div class="input-examples">
            <div class="mb-4">
              <Field
                type="text"
                name="name"
                as="input"
                placeholder="Enter name"
                v-model="values.name"
              />
              <!-- DEBUG -->
              <!-- <p>NAME ==> {{ values.name }}</p> -->
            </div>
            <span v-if="errors?.name" class="error mb-4">{{
              errors.name
            }}</span>
            <div class="mb-4">
              <Field
                type="email"
                name="email"
                as="input"
                placeholder="Enter email"
                v-model="values.email"
              />
              <!-- DEBUG -->
              <!-- <p>EMAIL ==> {{ values.email }}</p> -->
            </div>
            <span v-if="errors?.email" class="error mb-4">{{
              errors.email
            }}</span>
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
              <!-- <p>COLOR ==> {{ values.color }}</p> -->
            </div>
            <span v-if="errors?.color" class="error mb-4">{{
              errors.color
            }}</span>
            <button type="submit" :class="{ isDisabled: !isSubmitting }">
              Submit
            </button>
          </div>
        </fieldset>
        <fieldset>
          <legend class="legend">Custom component example</legend>
          <div class="sayHello-example">
            <div class="mb-4">
              <Field name="World" :as="SayHello" />
            </div>
          </div>
        </fieldset>
        <fieldset>
          <legend class="legend">Captcha example</legend>
          <div class="captcha-example">
            <div class="mb-4">
              <Field
                name="captcha"
                v-model:value="userData.captcha"
                :options="captchaOptions"
                :as="Captcha"
              />
              <!-- DEBUG -->
              <!-- <p>CAPTCHA SELECTED ==> {{ userData.captcha }}</p> -->
            </div>
          </div>
        </fieldset>
      </form>
    </template>
  </Formik>
</template>

<style scoped>
.mb-4 {
  margin-bottom: 1rem;
}

.legend {
  font-size: 1.5rem;
  font-weight: bold;
}

.error {
  display: block;
  color: red;
}

.isDisabled {
  opacity: 0.5;
  pointer-events: none;
}

.input-examples,
.sayHello-example,
.captcha-example {
  padding: 5px;
  margin: 10px 0px;
}
</style>
