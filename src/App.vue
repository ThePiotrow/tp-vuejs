<script setup>
import Formik from "./components/Formik.vue"
import Field from "./components/Field.vue"
const initialValues = {
  email: "johndoe@example.com",
  password: "",
  adult: false,
  gender: "",
};
const validate = (values) => {
  const pwdRegexp = /^(?=.*[a-z])(?=.*[A-Z])(?=.*[0-9])(?=.{8,})/;

  // Check if string contains numbers
  const errors = {}
  if (!values.email) {
    errors.email = "Aucun email renseigné";
  }
  if (!values.password) {
    errors.password = "Aucun mot de passe renseigné";
  }
  if (!pwdRegexp.test(values.password)) {
    const isNumbers = (str) => {
      return /\d/.test(str);
    };

    const isLowercase = (str) => {
      return /[a-z]/.test(str);
    };

    const isUppercase = (str) => {
      return /[A-Z]/.test(str);
    };

    const errors = [];
    if (!isNumbers(values.password)) {
      errors.push("Le mot de passe doit contenir au moins un chiffre");
    }
    if (!isLowercase(values.password)) {
      errors.push("Le mot de passe doit contenir au moins une minuscule");
    }
    if (!isUppercase(values.password)) {
      errors.push("Le mot de passe doit contenir au moins une majuscule");
    }

    console.log(errors);

    errors.password = errors.join("\n");
  }
  return errors;
};

const handleSubmit = (values) => {
  alert(values);
}
</script>

<template>
  <Formik 
    style="display: flex; flex-direction: column; align-items: start; justify-content: space-around" 
    v-slot="{
      values,
      errors,
      handleSubmit,
      isSubmitting,
    }" 
    :initialValues="initialValues" 
    :validate="validate"
  >
    <label for="email">Email :</label>
    <Field name="email" />
    <span>{{ errors.email }}</span>

    <label for="password">Mot de passe :</label>
    <Field name="password" type="password" />
    <span>{{ errors.password }}</span>
    
    <button @click="handleSubmit" :disabled="isSubmitting">Envoyer</button>
  </Formik>
</template>