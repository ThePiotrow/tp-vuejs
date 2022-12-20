<script setup>
import Formik from "./components/Formik.vue"
import Field from "./components/Field.vue"
const initialValues = {
  email: "johndoe@example.com",
  password: "",
};
const validate = (values) => {
  const pwdRegexp = /^(?=.*[a-z])(?=.*[A-Z])(?=.*[0-9])(?=.{8,})/;

  // Check if string contains numbers
  const errors = {}
  if (!values.email) {
    errors.email = "Aucun email renseigné";
  } else {
    if (!/^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$/.test(values.email)) {
      errors.email = "L'email n'est pas valide";
    }
  }
  
  if (!values.password) {
    errors.password = ["Aucun mot de passe renseigné"];
  } else {
    if (!pwdRegexp.test(values.password)) {
      const pwdErrors = [];
      if (!/\d/.test(values.password)) { // hasNumeric
        pwdErrors.push("Le mot de passe doit contenir au moins un chiffre.");
      }
      if (!/[a-z]/.test(values.password)) { // hasLowercase
        pwdErrors.push("Le mot de passe doit contenir au moins une minuscule.");
      }
      if (!/[A-Z]/.test(values.password)) { // hasUppercase
        pwdErrors.push("Le mot de passe doit contenir au moins une majuscule.");
      }

      errors.password = pwdErrors;
    }
  }
  return errors;
};

const onSubmit = (data) => {
  let result = [];
  for (const [key, value] of Object.entries(data)) {
    result.push(`${key}: ${value}`);
  }
  alert(result.join("\n"));
}
</script>

<template>
  <Formik 
    style="display: flex; flex-direction: column; align-items: start; justify-content: space-around" 
    v-slot="{
      values,
      errors,
      isSubmitting,
      handleSubmit,
    }" 
    :initialValues="initialValues" 
    :validate="validate"
    @submit="onSubmit"
  >
    <Field name="email" label="Email :" />
    
    <Field name="password" type="password" label="Mot de passe :" />
    
    <Field name="description" :as="'textarea'" label="Description"/>
    <br>
    <button :disabled="isSubmitting">Envoyer</button>
  </Formik>
</template>