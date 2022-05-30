<template>
  <form @submit.prevent="validate">
    <form-input
      label="Username"
      name="username"
      v-model="inputs.username"
      @update:modelValue="validate"
      :errors="res.getErrors('username')"
      :pending="usernameLoading"
      :class="cn('username')"
    ></form-input>
    <form-input
      label="Password"
      name="password"
      v-model="inputs.password"
      @update:modelValue="validate"
      :errors="res.getErrors('password')"
      :warnings="res.getWarnings('password')"
      :class="cn('password')"
    ></form-input>
    <form-input
      label="Confirm Password"
      name="confirm"
      v-model="inputs.confirm"
      @update:modelValue="validate"
      :errors="res.getErrors('confirm')"
      :class="cn('confirm')"
    ></form-input>
    <form-checkbox
      :class="cn('tos')"
      name="tos"
      v-model="inputs.tos"
      @update:modelValue="validate"
      label="I have read and agreed to the terms of service"
    ></form-checkbox>
    <form-submit :disabled="!res.isValid()" />
  </form>
</template>

<script>
import FormInput from "./components/FormInput.vue";
import FormCheckbox from "./components/FormCheckbox.vue";
import FormSubmit from "./components/FormSubmit.vue";
import suite from "./suite";
import classnames from "vest/classnames";

export default {
  name: "App",
  components: { FormInput, FormSubmit, FormCheckbox },
  data() {
    return {
      inputs: {
        username: "",
        password: "",
        confirm: "",
        tos: false,
      },
      usernameLoading: false,
      res: suite.get(),
    };
  },
  methods: {
    validate(value, name) {
      this.res = suite(this.inputs, name);
      console.log(this.res.tests.username.errors);

      if (name === "username") {
        this.usernameLoading = true;
      }

      this.res.done((res) => {
        this.usernameLoading = false;
        this.res = res;
      });
    },
  },
  computed: {
    cn() {
      return classnames(this.res, {
        valid: "success",
        warning: "warning",
        invalid: "error",
      });
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Baloo+Chettan+2:wght@400;600&display=swap");
* {
  font-family: "Baloo Chettan 2", cursive;
}
body {
  font-size: 18px;
  width: 100vw;
  height: 100vh;
  background: #2f4976;
  background: linear-gradient(191deg, #2f4976 19%, #1a1e41 100%);
}
#app {
  width: 100vw;
  height: 100vh;
  margin-top: 3em;
}
form {
  --color-error: rgb(245, 137, 137);
  --color-warning: rgb(245, 202, 137);
  --color-success: rgb(121, 196, 114);
  --color-content-active: #5081a6;
  --color-content-semi: #89b8dd;
  --color-content-inactive: #cfe3f0;
  color: #999;
  width: 500px;
  padding: 1.5em;
  margin: 0 auto;
  border-radius: 10px;
  background: #fff;
  box-shadow: 0 10px 25px rgba(27, 37, 46, 0.5);
}
</style>
